# [LEKCJA 12 – Fluent Validation](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-8-od-widoku-do-modelu/lekcja-12-fluent-validation/)
FluentValidation to biblioteka .NET do budowania silnie typowanych reguł walidacji. Będziemy jej używać do sprawdzania poprawności danych pochodzących od użytkownika (czy są zgodne z oczekiwaniami). Aby z niej korzystać, musimy ją najpierw zainstalować.
## Instalacja FluentValidation
Biblioteka FluentValidation jest dostępna w paczkach NuGet. W projekcie _.Application_ zainstalujemy 3 paczki związane z tę biblioteką. Będą to _FluentValidation_, _FluentValidation.AspNetCore_ i _FluentValidation.DependencyInjectionExtensions_.
## Walidacja
Do tej pory podczas walidacji korzystaliśmy co najwyżej z adnotacji. Bezpośrednio do właściwości viewmodeli przypisywaliśmy odpowiednie atrybuty, np. `[EmailAddress]`, `[Phone]`, `[Range]`, `[Required]` i inne, zdefiniowane w przestrzeni nazw `System.ComponentModel.DataAnnotations`. Walidacja wbudowana w .NET Core jest oparta właśnie na adnotacjach. Kiedy w elementach widoku używaliśmy atrybutów `asp-validation-for`, `asp-validation-summary`, to silnik Razor tworzył na ich podstawie odpowiednie reguły. Do tego sposobu walidacji wrócimy jeszcze w kolejnej lekcji.

Biblioteka FluentValidation pozwala nam na definiowanie dokładnych reguł walidacji w osobnych klasach. Składnia biblioteki przypomina nieco składnię biblioteki Fluent Assertions, poznanej przy okazji testów jednostkowych (tydzień 4. lekcja 6.).

### 1. Konfiguracja
W konfiguracji naszych serwisów związanych z dependency injection (w pliku _Program.cs_ lub _Startup.cs_ projektu webowego) musimy dopisać konfigurację dla FluentValidation. Pomiędzy `builder.Services.AddControllersWithViews();` a `var app = builder.Build();` dopisujemy `builder.Services.AddFluentValidationAutoValidation().AddFluentValidationClientsideAdapters();`. Pierwsza metoda (`AddFluentValidationAutoValidation`), spowoduje dodanie fluent validation do potoku walidacji ASP.NET. Walidacja którą zaraz zaimplementujemy będzie się wówczas odbywać automatycznie. Ten sposób postępowania nie jest zalecany przez autora biblioteki, a ponieważ _FluentValidation.AspNetCore_ nie jest w tej chwili dłużej wspierane i rozwijane, może w przyszłości powodować powstawanie błędów, gdy zajdą jakieś zmiany w ASP.NET Core, ale na razie przy nim zostańmy. Druga metoda (`AddFluentValidationClientsideAdapters`) spowoduje dodanie walidacji po stronie klienta. Dzięki temu jeszcze przed przesłaniem danych na serwer, zostaną one wstępnie sprawdzone. Jeżeli zostaną wykryte jakieś błędy dane nie zostaną przesłane, a użytkownik otrzyma odpowiednie komunikaty o błędach. Nie wszystkie reguły, które za chwile utworzymy będą jednak sprawdzane po stronie klienta. Dodanie walidacji po stronie klienta poprawia działanie aplikacji (użytkownik szybciej dostaje informacje o błędach i jest większe prawdopodobieństwo, że otrzymane przez serwer dane są prawidłowe), jednak należy pamiętać, że jest ona łatwa do obejścia, więc otrzymane dane należy zawsze sprawdzić ponownie po stronie serwera. Powyższa konfiguracja spowoduje dodanie reguł walidacji stworzonych przez nas przy pomocy FluentValidation do tych tworzonych automatycznie przez .NET (przy pomocy adnotacji, specyfikatorów np. `required`, czy np. nieoznaczenia typu jako nullowalny). Przy czym .NETowa walidacja "ma pierwszeństwo". Jeżeli chcemy temu zapobiec możemy ustawić flagę `DisableDataAnnotationsValidation` na `true`. W wywołaniu funkcji konfigurującej auto-walidacje dopisujemy więc np. `fv => fv.DisableDataAnnotationsValidation = true`. Spowoduje to "wyłączenie" walidacji .NET po stronie serwera. Dalej będzie się ona jednak odbywać po stronie klienta. Nie znalazłam dobrego sposobu jak to zmienić. Jedyne co udało mi się zrobić, to wyczyścić kolekcję `ClientModelValidatorProviders` w opcjach widoków MVC. Dzięki temu nie będą się nam już wyświetlać komunikaty błędów związane z DataAnnotations, chyba, że te adnotacje powodują zmianę typu elementu html (np. adnotacja `[EmailAddress]` powoduje, że zostanie stworzony `<input type="email >`, chyba, że zadeklarowaliśmy inaczej), związane z typami html komunikaty o błędach nadal będą wyświetlane. Jeżeli chcemy więc usunąć/ignorować reguły walidacji dodawane przez .NET, to konfiguracja może wyglądać następująco:
```csharp
// ...
builder.Services.AddControllersWithViews().AddViewOptions(opt => { opt.ClientModelValidatorProviders.Clear(); });
builder.Services.AddFluentValidationAutoValidation(fv => fv.DisableDataAnnotationsValidation = true).AddFluentValidationClientsideAdapters();
// ...
var app = builder.Build();
// ...
```
### 2. Reguły walidacji
Aby stworzyć reguły walidacji dla naszego viewmodelu musimy stworzyć dla niego osobną klasę. Będzie ona dziedziczyć po klasie generycznej `AbstractValidator` należącej do biblioteki FluentValidation. Załóżmy, że mamy klasę `BookVM`, będącą viewmodelem. Reguły walidacji jej parametrów zdefiniujemy w klasie `BookValidator` dziedziczącej po klasie bazowej `AbstractValidator<BookVM>`. Wszystkie reguły zdefiniujemy w konstruktorze tej klasy. Najpierw będziemy wskazywać dla jakiej właściwości definiujemy regułę. Służy do tego metoda `RuleFor`. Np. polecenie `RuleFor(x => x.Title)` rozpocznie tworzenie reguł walidacji dla właściwości `Title` klasy `BookVM`. Dalej podajemy jakie warunki musi spełniać wartość tego parametru. FluentValidator dostarcza nam wiele reguł, z których możemy korzystać. Możemy sprawdzać np. czy coś nie jest nullem (`NotNull()`), albo czy nim jest (`Null()`), sprawdzać, czy string jest (`Empty()`) lub nie jest (`NotEmpty()`) pusty, czy wartość mieści się w jakimś zakresie (`ExclusiveBetween(wieksze_niz, mniejsze_niz)`, `InclusiveBetween(wieksze_rowne, mniejsze_rowne)`, `LessThan(mniejsze_niz)`, `LessThanOrEqualTo(mniejsze_rowne)`, `GreaterThan(wieksze_niz)`, `GreaterThanOrEqualTo(wieksze_rowne)`), jest określonej długości (`Length(dokladna_dlugosc)`, `Length(minimalna_dlugosc, maksymalna_dlugosc)`, `MinimumLength(minimalna_dlugosc)`, `MaximumLength(maksymalne_dlugosc)`) itd. Zobaczmy jak to może wyglądać na przykładzie.
```csharp
using FluentValidation;

namespace TitlesOrganizer.Application.ViewModels.BookVMs
{
    public class BookVM
    {
        public int Id { get; set; }
        public required string Title { get; set; }
        public string? OriginalTitle { get; set; }
        public string? OriginalLanguageCode { get; set; }
        public int? Year { get; set; }
        public string? Edition { get; set; }
        public string? Description { get; set; }
    }

    public class BookValidator : AbstractValidator<BookVM>
    {
        public BookValidator()
        {
            // Id nie może być nullem
            RuleFor(x => x.Id).NotNull();
            // Title powinien zawsze byc podany (nie null i nie pusty string), a jego maksymalna dlugosc jest w bazie danych ustawiona na 450 znakow
            RuleFor(x => x.Title).NotNull().NotEmpty().MaximumLength(450);
            // OriginalTitle nie musi byc podany, a jego maksymalna dlugosc jest w bazie danych rowniez ustawiona na 450 znakow
            RuleFor(x => x.OriginalTitle).MaximumLength(450);
            // OriginalLanguageCode jest trzyliterowym kodem jezyku, nie musi zostac podany, ale gdy juz jest, to musi skladac sie dokladnie z trzech liter
            RuleFor(x => x.OriginalLanguageCode).Length(3).Must(lang => lang.ToLower().All(c => c >= 'a' && c <= 'z')).Unless(x => string.IsNullOrEmpty(x.OriginalLanguageCode));
            // Year jest rokiem wydania ksiazki, nie musi byc podany, ale gdy jest, musi byc dodatni i nie wiekszy niz biezacy rok
            RuleFor(x => x.Year).GreaterThan(0).LessThanOrEqualTo(DateTime.Now.Year).When(x => x.Year.HasValue);
            // Edition nie musi byc podana, a jej maksymalna dlugosc w naszej bazie danych jest ustawiona na 50 znakow
            RuleFor(x => x.Edition).MaximumLength(50);
            // Description nie musi byc podany, a jego maksymalna dlugosc w bazie danych jest ustawiona na 4000 znakow
            RuleFor(x => x.Description).MaximumLength(4000);
        }
    }
}
```

Jak już wspominaliśmy nie wszystkie ze zdefiniowanych reguł będą sprawdzane po stronie klienta. Wspierane są tam tylko następujące reguły:
* `NotNull`/`NotEmpty`
* `Matches(regex)`
* `InclusiveBetween(range)`
* `CreditCard`
* `Email`
* `EqualTo(cross-property equality comparison)`
* `MaxLength`
* `MinLength`
* `Length`

Przy czym wszystkie reguły zdefiniowane z użyciem warunków (`When`, `Unless`), samodzielnie zdefiniowane warunki (`Custom`) lub odwołania do `Must` nie zostaną wykonane po stronie klienta. Z powyższego przykładu po stronie klienta zostaną sprawdzone jedynie właściwości `Id`, `Title`, `OriginalTitle`, `Edition` i `Description`.

### 3. Utworzenie serwisu dla Dependency Injection
Aby utworzony przez nas walidator zadziałał musimy jeszcze zdefiniować dla niego serwis do dependency injection. Dla przykładu z poprzedniego punktu musimy dodać do konfiguracji następującą instrukcję `services.AddTransient<IValidator<BookVM>, BookValidator>();`, gdzie `services` to obiekt `IServiceCollection` aplikacji webowej. Możemy dodać tą instrukcję do pliku _Program.cs_ (lub _Startup.cs_). Wówczas `services` zastąpimy najpewniej kodem `builder.Services`, a samą instrukcję wstawimy przed zbudowaniem aplikacji (`builder.Build()`). Jeżeli w projekcie _.Application_ tworzyliśmy osobną klasę `DependencyInjection` z metodą rozszerzającą dodającą do kolekcji wszystkie serwisy do wstrzykiwania zależności związane z tym projektem, to możemy tam dodać tą instrukcję. Będziemy musieli utworzyć analogiczny serwis dla każdego zdefiniowanego przez nas walidatora.

Teraz już wszystko powinno działać.
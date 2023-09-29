# [LEKCJA 13 – Data Annotations](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-8-od-widoku-do-modelu/lekcja-13-data-annotations/)
Zamiast walidacji przy pomocy biblioteki FluentValidation możemy użyć wbudowanego mechanizmu ASP.NET korzystającego z adnotacji (`System.ComponentModel.DataAnnotations`). Wybór sposobu walidacji zależy od indywidualnych preferencji. FluentValidation szczególnie w wersji manualnej jest łatwiejszy (możliwy) do debugowania i testowania (dobrze łączy się z biblioteką FluentAssertion której używaliśmy do testowania). Wykorzystanie `DataAnnotations` jest z kolei jak już wspomnieliśmy wbudowanym mechanizmem ASP.NET, nie wymaga więc instalowania żadnych dodatkowych bibliotek (nie jest od nich zależne). Można również łączyć oba podejścia, chociaż nie jest to zalecane, gdyż zmniejsza to czytelność kodu.
## Definiowanie reguł walidacji
Będziemy to robić podobnie jak w przypadku wszystkich używanych dotychczas adnotacji (np. `[DisplayName("Nazwa")]`). W klasie viewmodelu nad każdą właściwością dla której będziemy chcieli stworzyć regułę walidacji dopiszemy odpowiednie adnotacje. Mamy do dyspozycji m.in. atrybuty:
* `[Required]` - oznaczający, że dana właściwość jest wymagana,
* `[StringLength(maxLength)]` - oznacza, że wartość może składać się maksymalnie z podanej liczby znaków. Atrybut ma jeszcze poza tym właściwości m.in. `MinimumLength`, która umożliwia sprecyzowanie minimalnej liczby znaków,
* `[MaxLength(maxLength)]` - oznacza, że wartość tej właściwości może mieć maksymalnie podaną długość,
* `[MinLength(minLength)]` - oznacza, że wartość tej właściwości może mieć minimalnie podaną długość,
* `[Range(minValue, maxValue)]` - oznacza, że wartość tej właściwości może być minimum `minValue` i maksimum `maxValue`,
* `[Compare(otherProperty)]` - sprawdza, czy wartość tej właściwości jest taka sama jak innej właściwości (do użycia np. przy zmianie hasła do powtórzenia nowego hasła),
* `[DataType(type)]` - umożliwia dopasowanie sposobu prezentacji elementu związanego z tę właściwością na stronie. Przyjmuje jeden parametr. Może być to `string` wskazujący własny schemat pola lub `DataType`, będący enumem reprezentującym różne typy danych:
    * `CreditCard` (14) - numer karty kredytowej,
    * `Currency` (6) - wartość w walucie zgodnej z ustawioną kulturą (językiem),
    * `Custom` (0) - własny typ danych,
    * `Date` (2) - data,
    * `DateTime` (1) - data i czas,
    * `Duration` (4) - czas trwania,
    * `EmailAddress` (10) - adres email,
    * `Html` (8) - plik .html,
    * `ImageUrl` (13) - URL obrazka,
    * `MultilineText` (9) - tekst wieloliniowy,
    * `Password` (11) - hasło,
    * `PhoneNumber` (5) - numer telefonu,
    * `PostalCode` (15) - kod pocztowy,
    * `Text` (7) - wyświetlany tekst,
    * `Time` (3) - czas,
    * `Upload` (16) - typ danych przesyłania pliku,
    * `Url` (12) - wartość URL,
* `[RegularExpression(regex)]` - sprawdza, czy wartość właściwości jest zgodna z podanym wyrażeniem regularnym,
* `[CreditCard]` - sprawdza, czy wartość właściwości jest numerem karty kredytowej,
* `[EmailAddress]` - sprawdza, czy wartość właściwości jest adresem email,
* `[EnumDataType(type)]` - sprawdza, czy wartość właściwości należy do enuma podanego typu,
* `[FileExtensions]` - waliduje rozszerzenia plików,
* `[Phone]` - sprawdza, czy wartość właściwości jest poprawnie sformatowanym numerem telefonu,
* `[Url]` - dostarcza walidację URL.

Przy czym należy pamiętać, że parametry atrybutów muszą być wyrażeniami stałymi, wyrażeniami `typeof` lub wyrażeniami tworzącymi `array`.

Poza predefiniowanymi atrybutami możemy również zdefiniować własne, dziedziczące bezpośrednio po klasie `ValidationAttribute` lub np. po jednym z predefiniowanych typów.

Wszystkie atrybuty związane z walidacją posiadają m.in. właściwość związaną z komunikatem błędów. W adnotacjach można więc również podać własne komunikaty błędów, które zostaną wyświetlone użytkownikowi w przypadku wystąpienia błędu tej walidacji.
### Przykład
Dla przykładu, zapiszmy sobie reguły walidacji utworzone w poprzedniej lekcji, przy pomocy FluentValidation, używając tym razem adnotacji.
```csharp
using System.ComponentModel.DataAnnotations;

namespace TitlesOrganizer.Application.ViewModels.BookVMs
{
    public class BookVM
    {
        [Required] // Id jest wymagane, ale poniewaz jest typu nienullowalnego, wiec nie trzeba nawet podawac tego atrybutu, bo jest to wowczas zachowanie domyslne
        public int Id { get; set; }

        [Required]
        [StringLength(225, MinimumLength = 1)]
        public required string Title { get; set; }

        [StringLength(225)]
        public string? OriginalTitle { get; set; }

        [LanguageCode] // samodzielnie stworzony atrybut
        public string? OriginalLanguageCode { get; set; }

        [YearRange(1)] // samodzielnie stworzony atrybut
        public int? Year { get; set; }

        [StringLength(25)]
        public string? Edition { get; set; }

        [StringLength(2000)]
        public string? Description { get; set; }
    }
// Poniewaz walidacja OriginalLanguageCode jest bardziej zlozona, wiec musimy ja zdefiniowac w osobnej klasie
    public class LanguageCodeAttribute : ValidationAttribute // atrybuty zapewniajace walidacje musza dziedziczyc po klasie ValidationAttribute
    {
        // nadpisujac metode IsValid tworzymy wlasna logike walidacji
        public override bool IsValid(object? value)
        {
            string? code = value as string;
            if (string.IsNullOrEmpty(code))
            {
                return true;
            }
            else
            {
                return code.Length == 3 && code.ToLower().All(c => c >= 'a' && c <= 'z');
            }
        }
    }
// Podany rok chcemy porownac z rokiem biezacym.
// DateTime.Now.Year nie jest jednak const.
// Musimy wiec zdefiniowac wlasny atrybut.
// Tym razem jednak mozemy skorzystac z logiki RangeAttribute i dziedziczyc po tej wlasnie klasie.
// Jedyne co musimy zrobic to przekazac wartosc DateTime.Now.Year do konstruktora klasy bazowej.
    public class YearRangeAttribute : RangeAttribute
    {
        public YearRangeAttribute(int minimum) : base(minimum, DateTime.Now.Year)
        {
        }
    }
}
```
## Walidacja, a akcja kontrolera
Zdefiniowaliśmy już reguły walidacji. Walidacja przy użyciu `DataAnnotations` występuje jednak (częściowo tylko) po stronie serwera. Oznacza to, że akcja kontrolera może zostać wywołana również w przypadku wykrycia błędów w danych. Zanim więc wyślemy dane do warstwy aplikacji musimy jeszcze sprawdzić wynik walidacji. Walidacja `DataAnnotations` odbywa się przy użyciu `ModelState`. Jej wynik jest zapisywany we właściwości `IsValid`.
### Przykład
Dla przykładu zmodyfikujmy akcję użytą w poprzedniej lekcji w przykładzie dotyczącym walidacji manualnej.
```csharp
[HttpPost]
public ActionResult AddBook(BookVM book)
{
    if (ModelState.IsValid) // sprawdzamy wynik walidacji
    {
        int id = _bookService.AddBook(book);

        if (id > 0)
        {
            return RedirectToAction("Details", new { id = id });
        }
    }

    return View(book);
}
```
## `[ValidateAntiForgeryToken]`
Kiedy mówimy o adnotacjach, warto jeszcze wspomnieć o tokenach przeciw fałszerstwom. Są to specjalne tokeny generowane przez widoki i sprawdzane przez akcje kontrolera, które mają zweryfikować, czy otrzymane dane pochodzą z naszego widoku. Mają one za zadanie zabezpieczenie przed podmienieniem przez kogoś naszych widoków. Kiedyś (przed ASP.NET Core MVC) musieliśmy samodzielnie generować tokeny po stronie formularza. Obecnie istnieje do tego wbudowany mechanizm (wszystkie formularze automatycznie generują takie tokeny). Jedyne co musimy zrobić to oznaczyć akcję kontrolera, która ma wykorzystywać ten mechanizm, atrybutem `[ValidateAntiForgeryToken]`. Należy oznaczać tym atrybutem wszystkie akcje przyjmujące dane z formularza. Powinniśmy więc taką adnotację dopisać również do akcji z przykładu powyżej (nie zależnie od użytego sposobu walidacji).
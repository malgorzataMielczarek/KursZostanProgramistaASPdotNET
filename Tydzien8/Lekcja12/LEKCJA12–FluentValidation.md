# [LEKCJA 12 – Fluent Validation](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-8-od-widoku-do-modelu/lekcja-12-fluent-validation/)
FluentValidation to biblioteka .NET do budowania silnie typowanych reguł walidacji. Będziemy jej używać do sprawdzania poprawności danych pochodzących od użytkownika (czy są zgodne z oczekiwaniami). Aby z niej korzystać, musimy ją najpierw zainstalować.
## Instalacja FluentValidation
Biblioteka FluentValidation jest dostępna w paczkach NuGet. W projekcie _.Application_ zainstalujemy 3 paczki związane z tę biblioteką. Będą to _FluentValidation_, _FluentValidation.AspNetCore_ i _FluentValidation.DependencyInjectionExtensions_.
## Walidacja
Do tej pory podczas walidacji korzystaliśmy co najwyżej z adnotacji. Bezpośrednio do właściwości viewmodeli przypisywaliśmy odpowiednie atrybuty, np. `[EmailAddress]`, `[Phone]`, `[Range]`, `[Required]` i inne, zdefiniowane w przestrzeni nazw `System.ComponentModel.DataAnnotations`. Walidacja wbudowana w .NET Core jest oparta właśnie na adnotacjach. Kiedy w elementach widoku używaliśmy atrybutów `asp-validation-for`, `asp-validation-summary`, to silnik Razor tworzył na ich podstawie odpowiednie reguły. Do tego sposobu walidacji wrócimy jeszcze w kolejnej lekcji.

Biblioteka FluentValidation pozwala nam na definiowanie dokładnych reguł walidacji w osobnych klasach. Składnia biblioteki przypomina nieco składnię biblioteki Fluent Assertions, poznanej przy okazji testów jednostkowych (tydzień 4. lekcja 6.).

### 1. Konfiguracja
W konfiguracji naszych serwisów związanych z dependency injection (w pliku _Program.cs_ lub _Startup.cs_ projektu webowego) musimy dopisać konfigurację dla FluentValidation. 
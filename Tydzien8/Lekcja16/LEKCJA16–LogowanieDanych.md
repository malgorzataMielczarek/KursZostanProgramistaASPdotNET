# [LEKCJA 16 – Logowanie danych](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-8-od-widoku-do-modelu/lekcja-16-logowanie-danych/)
Logowanie danych, czyli zapisywanie co dzieje się w aplikacji. Kiedy użytkownik natrafi na jakiś błąd, to chcemy mu tylko wyświetlić informację, że wystąpił błąd i żeby skontaktować się z administratorem gdyby się powtarzał. Sami jednak chcielibyśmy mieć o takim błędzie jak najwięcej informacji. Chcemy jak najdokładniej wiedzieć w jakiej sytuacji występuje. Chcemy, aby takie informacje zostały odnotowane po stronie serwera, ale nie po stronie klienta. Po to właśnie tworzy logi. Logowanie danych jest obecnie wbudowanym mechanizmem frameworka .NET Core. Wcześniej programista musiał sam o to zadbać lub użyć zewnętrznych bibliotek. Teraz możemy do tego użyć loggerów (obiektów typu `ILogger<TController>`). Są one automatycznie tworzone przez framework dla naszej aplikacji w momencie utworzenia jej buildera. Loggery odpowiedniego typu tworzone są dla poszczególnych kontrolerów i wstrzykiwane do nich przez konstruktor. Mieliśmy np.
```csharp
public class BooksController : Controller
{
    private readonly ILogger<BooksController> _logger;
    // ...

    public BooksController(ILogger<BooksController> logger /*, ...*/)
    {
        _logger = logger;
        // ...
    }

    // ...
}
```
Loggery pozwalają na tworzenie czterech poziomów logów:
Poziom|Typ logu|Tworząca log metoda logger|Opis
--:|:--|:--|:--
1|informacyjny|`LogInformation(message)`|Najbardziej podstawowy typ, nie świadczy o niczym konkretnym, ma jedynie za zadanie przekazanie informacji. Raczej rzadko będziemy z niego korzystać.
2|debug|`LogDebug(message)`|Typ używany podczas tworzenia aplikacji, przeznaczony do śledzenia w którym miejscu kodu się znajdujemy, np. jakie akcje były kolejno wywoływane.
3|ostrzegawczy|`LogWarning(message)`|Typ używany do informowania, że może nie było jeszcze błędu, ale wydarzyło się coś co może nas zaniepokoić. Np. użytkownik poszedł nie do końca taką ścieżką jakiej oczekiwaliśmy, albo mamy jakieś miejsce w kodzie, które potencjalnie może się zepsuć, bo np. łączymy się z jakimś zewnętrznym serwerem, co do którego nie mamy pewności, że zawsze będzie dostępny.
4|błąd|`LogError(message)`|Typ używany do logowania informacji o wystąpieniu błędu.

Aby stworzyć log danego typu w odpowiednim miejscu kontrolera piszemy po prostu np. `_logger.LogInformation("Informacja, którą chcemy umieścić w logu");`.

## Gdzie logowane są dane?
Standardowo informacje z loggerów trafiają do konsoli. W przypadku aplikacji webowych nie jest ona na ogół widoczna. W Visual Studio możemy ją również znaleźć w zakładce _Output_. O ile możemy korzystać z takiej opcji podczas tworzenia aplikacji, to w przypadku, gdy jest już ona w fazie produkcyjnej, taka opcja nas już nie zadowoli. Nie zawsze będziemy mieć bowiem otwartą konsolę. Dlatego logi zapisujemy do plików lub bazy danych. Skupmy się na pierwszej opcji. Będziemy do tego potrzebować osobnej paczki, którą pobierzemy z NuGeta.
### _Serilog.Extensions.Logging.File_
Jest to paczka, która pozwoli nam na skonfigurowanie naszej aplikacji tak, aby logi były zapisywane do plików. Aby jej użyć musimy ją najpierw zainstalować dla naszego projektu UI. Standardowo możemy to zrobić za pomocą _NuGet Package Manager_. Kiedy już zainstalujemy paczkę, to przechodzimy do pliku _Program.cs_ (lub _Startup.cs_). Aby skonfigurować zapisywanie logów do pliku musimy dodać tylko jedną linijkę kodu. Po utworzeniu buildera (`var builder = WebApplication.CreateBuilder(args);`), ale przed jego zbudowaniem (`var app = builder.Build();`), musimy dopisać
```csharp
builder.Logging.AddFile("Logs/myLog-{Date}.txt");
```
Spowoduje to utworzenie w katalogu głównym projektu _.Web_ folderu _Logs_. Kiedy nasza aplikacja będzie działać w katalogu tym będą tworzone pliki o nazwach _myLog-{Date}.txt_, gdzie {Date} to aktualna data zapisana w postaci ciągu liczb. Dla każdego dnia działania aplikacji będzie więc tworzony nowy plik z logami.
## Filtrowanie logów
Jeżeli nasza aplikacja jest już na produkcji, a umieściliśmy w niej dużo logów informujących nas w którym miejscu kodu się znajdujemy, to może się okazać, że pliki z logami zaczynają szybko rosnąć i zajmują znaczną część miejsca. Dla tego możemy wskazać logi jakiego typu chcemy, aby były zapisywane. Podczas tworzenia aplikacji możemy korzystać ze wszystkich typów logów. Na tym etapie nie ma ich jeszcze bowiem tak dużo. Kiedy przechodzimy do fazy testowania zostawiamy tylko logi typu debug i powyżej (odrzucamy logi informacyjne). W fazie produkcyjnej pozostawiamy tylko logi z błędami i ostrzeżeniami, albo wręcz wyłącznie te z błędami. Nie musimy jednak usuwać logów z naszego kodu. Wystarczy odpowiednio skonfigurować aplikację. W pliku _appsettings.json_ znajdziemy m.in. następujące ustawienia:
```json
"Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
}
```
W powyższej konfiguracji mamy ustawione logowanie wszystkich logów informacyjnych i powyżej (`"Default": "Information"`), czyli wszystkich logów. Jeżeli będziemy w fazie testowej, to musimy w tym miejscu zmienić ustawienia na `"Default": "Debug"`, a w fazie produkcyjnej na `"Default": "Warning"` lub `"Default": "Error"`, jeżeli chcemy rejestrować wyłącznie informacje o błędach.
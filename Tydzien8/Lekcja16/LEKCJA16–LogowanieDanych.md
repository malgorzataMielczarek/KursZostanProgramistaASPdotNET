# [LEKCJA 16 – Logowanie danych](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-8-od-widoku-do-modelu/lekcja-16-logowanie-danych/)
Logowanie danych, czyli zapisywanie co dzieje się w aplikacji. Kiedy użytkownik natrafi na jakiś błąd, to chcemy mu tylko wyświetlić informację, że wystąpił błąd i żeby skontaktować się z administratorem gdyby się powtarzał. Sami jednak chcielibyśmy mieć o takim błędzie jak najwięcej informacji. Chcemy jak najdokładniej wiedzieć w jakiej sytuacji występuje. Chcemy, aby takie informacje zostały odnotowane po stronie serwera, ale nie po stronie klienta. Po to właśnie tworzy logi. Logowanie danych jest obecnie wbudowanym mechanizmem frameworka .NET Core. Wcześniej programista musiał sam o to zadbać. Teraz możemy do tego użyć loggerów (obiektów typu `ILogger<TController>`), które wstrzykiwaliśmy do naszych kontrolerów. Mieliśmy np.
```csharp
private readonly ILogger<BooksController> _logger;
// ...

public BooksController(ILogger<BooksController> logger /*, ...*/)
{
    _logger = logger;
    // ...
}
```
Loggery pozwalają na tworzenie czterech poziomów logów:
Poziom|Typ logu|Tworząca log metoda logger|Opis
--:|:--|:--|:--
1|informacyjny|`LogInformation(message)`|Najbardziej podstawowy typ, nie świadczy o niczym konkretnym, ma jedynie za zadanie przekazanie informacji. Raczej rzadko będziemy z niego korzystać.
2|debug|`LogDebug(message)`|Typ używany podczas tworzenia aplikacji, przeznaczony do śledzenia w którym miejscu kodu się znajdujemy, np. jakie akcje były kolejno wywoływane.
3|ostrzeżegawczy|`LogWarning`|
4|błąd|`LogError`|
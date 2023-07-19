# [LEKCJA 9 – Dependency Injection](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-6-aplikacje-webowe-w-asp-net-core/lekcja-9-dependency-injection/)
Wzorzec projektowy _Dependency Injection_ (_DI_ - wstrzykiwanie zależności) jest to technika pozwalająca na osiągnięcie _Inversion of Control_ (_IoC_ - odwrócenie sterowania) pomiędzy klasami i ich zależnościami.

 ## _Inversion of Control_
 Inaczej _Dependency inversion_ (odwrócenie zależności), to zasada architektoniczna bazująca na interfejsach. Mówi ona, że klasa zależna od innej klasy, powinna bazować na jej interfejsie, a nie implementacji. Normalnie zależności czasu kompilacji płyną w kierunku wykonywania, tworząc schemat bezpośredniej zależności.

 ![Schemat bezpośredniej zależności](https://learn.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/media/image4-1.png)<br />
_Rysunek 1. Schemat bezpośredniej zależności<br/>
Źródło: https://learn.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/architectural-principles#dependency-inversion_

Jak widać klasa A wywołuje metodę klasy B, a klasa B zależy od klasy C. Gdyby teraz od klasy B zależało więcej klas i chcielibyśmy jej implementację podmienić na jakąś inną, to musielibyśmy te zmiany wprowadzić we wszystkich tych klasach. Zastosowanie zasady odwróconej zależności pozwala klasie A wywoływać metodę abstrakcji (interfejsu lub klasy abstrakcyjnej, bazowej), którą implementuje klasa B. Dzięki temu w czasie wykonywania klasa A może wywoływać metodę klasy B, jednak w czasie kompilacji zarówno klasa A, jak i klasa B zależą od tej abstrakcji, a nie bezpośrednio od siebie (stąd odwrócenie zależności czasu kompilacji). Przebieg wykonywania programu pozostaje bez zmian, jednak wprowadzenie interfejsów oznacza, że można łatwo podłączyć ich różne implementacje. Czyli teraz, gdybyśmy chcieli zmienić implementację, musielibyśmy tylko podmienić implementację interfejsu, bez wprowadzania zmian w klasie A i innych klasach od niego zależnych.

 ![Schemat odwróconej zależności](https://learn.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/media/image4-2.png)<br />
_Rysunek 2. Schemat odwróconej zależności<br/>
Źródło: https://learn.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/architectural-principles#dependency-inversion_

_Dependency inversion_ jest kluczowe do budowania luźno powiązanych aplikacji, gdyż implementacje powstają na podstawie abstrakcji, a nie implementacji innych części aplikacji. Tak zbudowane aplikacje są łatwiejsze do przetestowania, modularne i łatwiejsze w utrzymaniu.

## _Dependency Injection_ w .NET
_Dependency Injection_ jest wbudowaną częścią frameworka .NET. Bazuje ona na użyciu interfejsów lub klas bazowych do wyabstrahowania implementacji zależności. Ponadto dostarcza wbudowany kontener na serwisy, `IServiceProvider`, do rejestrowania zależności. Serwisy są zwykle rejestrowane na początku aplikacji i dodawane do kolekcji `IServiceCollection`. Po dodaniu wszystkich serwisów, można użyć `BuildServiceProvider`, aby stworzyć kontener serwisów. Aby użyć serwisu w klasie, wstrzykujemy go do jej konstruktora (dodajemy jako parametr). Framework przyjmuje na siebie odpowiedzialność za tworzenie instancji zależności (serwisu) i usunięcie jej, gdy nie jest już potrzebna.
### Przykład
1. Tworzenie interfejsu
```csharp =
namespace DependencyInjection.Example;

public interface IMessageWriter
{
    void Write(string message);
}
```
2. Implementacja interfejsu
```csharp =
namespace DependencyInjection.Example;

public class MessageWriter : IMessageWriter
{
    public void Write(string message)
    {
        Console.WriteLine($"MessageWriter.Write(message: \"{message}\")");
    }
}
```
3. Utworzenie i dodanie serwisu i utworzenie kontenera
```csharp =
using DependencyInjection.Example;

// utworzenie obiektu HostApplicationBuilder, posiadajacego wlasciwosc Services, bedaca kolekcja IServiceCollection
HostApplicationBuilder builder = Host.CreateApplicationBuilder(args);

// utworzenie i dodanie serwisow
// dodanie do kolekcji serwisu typu Worker, jako serwisu zarzadzanego przez hosta aplikacji
builder.Services.AddHostedService<Worker>();
// dodanie do kolekcji serwisu typu IMessageWriter, implementowanego przez typ MessageWriter, jako serwisu z zakresem (określonym okresem uzytkowania, na czas pojedynczego zadania)
builder.Services.AddScoped<IMessageWriter, MessageWriter>(); // przypisanie konkretnej implementacji do interfejsu

// zbudowanie hosta aplikacji
using IHost host = builder.Build(); // ta metoda moze byc wywolana tylko raz

// uruchomienie aplikacji
host.Run();
```
4. Utworzenie klasy `Worker`, wykorzystującej metodę `Write` i wstrzyknięcie serwisu `IMessageWriter`.
```csharp =
namespace DependencyInjection.Example;

public sealed class Worker : BackgroundService
{
    private readonly IMessageWriter _messageWriter;
    // wstrzykniecie serwisu
    public Worker(IMessageWriter messageWriter) =>
        _messageWriter = messageWriter;

    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        while (!stoppingToken.IsCancellationRequested)
        {
            // uzycie metody interfejsu (serwisu)
            _messageWriter.Write($"Worker running at: {DateTimeOffset.Now}");
            await Task.Delay(1_000, stoppingToken);
        }
    }
}
```

Nie tworzymy nigdzie instancji klasy `MessageWriter`. Robi to kontener DI. Host zawiera `IServiceProvider`, do wstrzykiwania zależności. Zawiera również wszystkie inne serwisy wymagane do automatycznego utworzenia instancji klasy `Worker` i dostarczenia odpowiedniej implementacji interfejsu `IMessageWriter` jako argumentu.

## Dodawanie serwisu do kolekcji `ServiceCollection`
W powyższym przykładzie dodaliśmy serwis do kolekcji używając metody `AddScoped<TService, TImplementation>()`, tworzącej serwis zakresowy (ang. _scoped service_). Do dyspozycji mamy jeszcze dwie inne metody: `AddTransient<TService,TImplementation>()`, tworzącą serwis przejściowy (ang. _transient service_) i `AddSingleton<TService,TImplementation>()`, tworzącą serwis singletonowy (ang. _singleton service_). Używamy je analogicznie do metody `AddScoped<TService, TImplementation>()`.

### Scoped service
```csharp =
builder.Services.AddScoped<IMessageWriter, MessageWriter>();
```
W tym typie serwisu:
* tworzona jest nową instancja w każdym żądaniu HTTP
* ta sama instancja jest zapewniona na cały okres trwania danego żądania
    * np. gdy mamy kilka parametrów w kontrolerze zależnych od tego serwisu, wszystkie te obiekty będą zawierać tą samą instancję serwisu na czas trwania żądania

Wybieramy go, gdy chcemy utrzymać stan wewnątrz żądania.
![Schemat serwisu zakresowego](https://f4n3x6c5.stackpathcdn.com/article/differences-between-scoped-transient-and-singleton-service/Images/scoped_service.PNG)<br />
_Rysunek 3. Schemat serwisu zakresowego (scoped service)<br/>
Źródło: https://www.c-sharpcorner.com/article/differences-between-scoped-transient-and-singleton-service/_
### Transient service
```csharp =
builder.Services.AddTransient<IMessageWriter, MessageWriter>();
```
W tym typie serwisu:
* tworzona jest nową instancja dla każdego obiektu w żądaniu HTTP
* instancja jest tworzona za każdym razem, co zwiększa użycie pamięci i zasobów. To podejście może więc mieć niekorzystny wpływ na wydajność aplikacji.

Wybieramy go, gdy aplikacja działa wielowątkowo, gdyż dzięki temu obiekty są od siebie niezależne. Możemy go również wykorzystać do lekkich serwisów z niewielkim stanem lub bez stanu.
![Schemat serwisu przejściowego](https://f4n3x6c5.stackpathcdn.com/article/differences-between-scoped-transient-and-singleton-service/Images/tranient_service.PNG)<br />
_Rysunek 3. Schemat serwisu przejściowego (transient service)<br/>
Źródło: https://www.c-sharpcorner.com/article/differences-between-scoped-transient-and-singleton-service/_
### Singleton service
```csharp =
builder.Services.AddSingleton<IMessageWriter, MessageWriter>();
```
W tym typie serwisu:
* tworzona jest tylko jedna instancja na czas życia aplikacji
* ta sama instancja jest używana za każdym razem, gdy serwis jest potrzebny
* ewentualne wycieki pamięci będą się z czasem namnażać, ponieważ używamy cały czas tego samego obiektu
* zapewniona jest największa wydajność, jeśli chodzi o zużycie pamięci, gdyż serwis jest tworzony jednokrotnie i wszędzie jest używany ten sam obiekt
### Kiedy używać którego podejścia
Typ podejścia|Użycie
------------:|:-----
Singleton    |Serwis logowania, flagi funkcjonalności(_feature flag_) - do włączania i wyłączania modułu podczas wdrażania, serwis e-mail
Scope        |Gdy chcemy zachować stan na cały czas trwania żądania
Transient    |Do lekkich serwisów, z niewielkim stanem lub bez stanu. W podejściu wielowątkowym.
## _Property Dependencie Injection_
Powyżej omawialiśmy _Method Dependency Injection_, czyli wstrzykiwanie zależności przy użyciu metody, a dokładniej konstruktora. Właśnie tego podejścia będziemy też najczęściej używać. Może się jednak zdarzyć, że chcemy wstrzyknąć zależność do klasy bez modyfikacji jej konstruktora. Raczej nie zdarzy się to w naszej nowotworzonej aplikacji, ale gdy np. będziemy modyfikować jakąś rozbudowaną aplikację i do którejś z jej klas będziemy chcieli wstrzyknąć zależność. Obiekty tej klasy mogą być tworzone w wielu miejscach aplikacji, więc zmiana konstruktora wiązałaby się z wieloma zmianami w kodzie. Wtedy z pomocą może nam przyjść _Property Dependencie Injection_. Podobnie jak podczas wstrzykiwania przez konstruktor, w klasie korzystającej z naszego serwisu tworzymy prywatne pole, gdzie ten serwis będziemy przechowywać. Dodatkowo tworzymy jednak publiczną właściwość, przez którą będzie to pole obsługiwać. Teraz, zamiast przekazywać serwis w konstruktorze, zrobimy to przez seter publicznej właściwości. Takie podejście ma jednak zasadniczy minus, mianowicie narażamy się na NullReferenceException. Kiedy wstrzykiwaliśmy zależność przez konstruktor, mieliśmy pewność, że w każdym utworzonym obiekcie naszej klasy będzie istniał obiekt serwisu. W przypadku użycia property injection, może się zdarzyć, że przez nieuwagę będziemy chcieli skorzystać z obiektu serwisu, który nie został jeszcze utworzony. Musimy więc być czujni.

Poza trudnościami w zmianie istniejącego kodu _Property Dependencie Injection_ może nam się jeszcze przydać np. gdy z jakiegoś powodu musimy zmieniać serwis od którego zależy obiekt naszej klasy, podczas życia tego obiektu.
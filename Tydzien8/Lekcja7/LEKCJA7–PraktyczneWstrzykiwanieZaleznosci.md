# [LEKCJA 7 – Praktyczne wstrzykiwanie zależności](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-8-od-widoku-do-modelu/lekcja-7-praktyczne-wstrzykiwanie-zaleznosci/)
O wstrzykiwaniu zależności (ang. _Dependency Injection_ - DI) trochę już mówiliśmy (tydzień 6. lekcja 9.). Dla przypomnienia jest to mechanizm bazujący na interfejsach. Dzięki temu w naszych klasach możemy używać metod niezależnie od ich implementacji, a co za tym idzie możemy w łatwy sposób podmieniać użyte implementacje.

Zależności możemy wstrzykiwać poprzez:
1. metody (konstruktor) - prywatne pole readonly typu odpowiedniego interfejsu, któremu wartość przypisujemy w konstruktorze. Jest to najczęściej używany sposób. Mamy w nim pewność, że pole na wstrzykiwany obiekt nie będzie null.
2. właściwości - prywatne pole typu odpowiedniego interfejsu i publiczny setter. Używamy gdy nie chcemy modyfikować konstruktora (np. dodajemy wstrzykiwanie zależności do istniejącej klasy, której obiekty są tworzone w wielu miejscach kodu), albo istnieje konieczność zmiany instancji wstrzykiwanego obiektu. W tym sposobie istnieje niebezpieczeństwo null reference exception (pole na wstrzykiwany obiekt może być null, bo zapomnieliśmy przypisać mu wartość).

Dzięki wbudowanemu mechanizmowi DI frameworka .NET zrzucamy na nasz program tworzenie instancji klas implementujących używane w kodzie interfejsy. Mamy do dyspozycji trzy mechanizmy:
1. Scoped service - tworzy nową instancję dla danego zapytania,
2. Transient service - tworzy nową instancję dla każdego obiektu,
3. Singleton service - tworzy tylko jedną instancję na cały czas życia aplikacji.

Zajmijmy się teraz praktyczną stroną dependency injection w naszym projekcie.
## Jak wstrzykujemy zależności?
Jak już wspominaliśmy możemy to zrobić przez metodę lub przez właściwość. Wbudowany mechanizm DI ASP.NET Core wspiera jednak wyłącznie wstrzykiwanie zależności przez konstruktor (tylko wtedy będzie wstanie automatycznie tworzyć dla nas odpowiednie obiekty). Takiego też sposobu będziemy używać w naszym programie. Jeżeli jeszcze tego nie zrobiliśmy dopiszmy odpowiednie konstruktory, przez które będziemy wstrzykiwać używane w danej klasie interfejsy. Np. do serwisów warstwy aplikacji będziemy musieli wstrzyknąć odpowiednie repozytoria. Np.:
```csharp =
using AutoMapper;
using TitlesOrganizer.Application.Interfaces;
using TitlesOrganizer.Domain.Interfaces;

namespace TitlesOrganizer.Application.Services
{
    public class BookService : IBookService
    {
        private readonly IBookRepository _bookRepository;
        private readonly IMapper _mapper;

        public BookService(IBookRepository bookRepository, IMapper mapper)
        {
            _bookRepository = bookRepository;
            _mapper = mapper;
        }
        // implementacja serwisu
    }
}
```
## Gdzie tworzymy serwisy wstrzykujące zależności?
Wszystkie serwisy wstrzykujące zależności będziemy tworzyć, dodawać do kolekcji i budować w projekcie UI, a dokładniej w pliku _Program.cs_ (a jeśli istnieje to w _Startup.cs_). Używaliśmy już tego mechanizmu, kiedy w lekcji 5. tygodnia 7. konfigurowaliśmy nasz DbContext. Dla przypomnienia mamy w naszym pliku kod podobny do poniższego:
```csharp
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
var connectionString = builder.Configuration.GetConnectionString("DefaultConnection") ?? throw new InvalidOperationException("Connection string 'DefaultConnection' not found.");
builder.Services.AddDbContext<Context>(options =>  // <= TU wstrzykiwalismy nasz DbContext
    options.UseSqlServer(connectionString));
builder.Services.AddDatabaseDeveloperPageExceptionFilter();

builder.Services.AddDefaultIdentity<IdentityUser>(options => options.SignIn.RequireConfirmedAccount = true)
    .AddEntityFrameworkStores<Context>();
builder.Services.AddControllersWithViews();

var app = builder.Build();
```
Tworzymy tu kolekcję serwisów, które będą odpowiedzialne za tworzenie instancji naszych klas. Musimy więc podać jaka klasa ma w naszym programie implementować konkretny interfejs. Musimy więc przypisać implementacje do interfejsów z warstwy aplikacji i domenowej. Mamy więc np.:
```csharp
builder.Services.AddTransient<IBookService, BookService>()
    .AddTransient<ILanguageService, LanguageService>();

builder.Services.AddTransient<IBookRepository, BookRepository>()
    .AddTransient<ILanguageRepository, LanguageRepository>();
```
Jak już wspominaliśmy te serwisy mogą być typu transient, scoped, singleton. Tworzymy je odpowiednio przy pomocy metod `AddTransient`, `AddScoped` i `AddSingleton` analogicznie jak wykonano to w powyższym kodzie. Serwisów singletonowych nie używamy (chyba, że jest to naprawdę konieczne i uzasadnione).

Ważne, aby **serwisy dodawać przed zbudowaniem buildera** (przed linijką `var app = builder.Build();`).

W naszym programie może być dużo repozytoriów i serwisów, a co za tym idzie będziemy musieli stworzyć dużo serwisów do wstrzykiwania zależności. Spowoduje to dodanie dużej ilości linii kodu do pliku _Program.cs_ (_Startup.cs_), a tym samym zmniejszy jego czytelność. Dlatego w poszczególnych projektach (_.Application_ i _.Repository_) często tworzy się dodatkowe klasy statyczne z metodami rozszerzającymi `IServiceCollection`.
## Metody rozszerzające do wstrzykiwania zależności
W projektach _.Application_ i _.Infrastructure_ stwórzmy publiczne klasy statyczne. Nazwijmy je _DependencyInjection_. W każdej z nich stwórzmy publiczną statyczną klasę rozszerzającą interfejs `IServiceCollection` o nazwie wskazującej zależności z jakiego projektu będziemy wstrzykiwać, czyli odpowiednio `AddApplication` i `AddInfrastructure`. W każdej z tych metod będziemy tworzyć serwisy do dependency injection związane ze wstrzykiwaniem zależności związanych z tym projektem. Jeżeli korzystamy z biblioteki AutoMapper, to pamiętajmy, aby w metodzie warstwie aplikacji dodać również wstrzykiwanie związanych z nią zależności. Służy do tego metoda `AddAutoMapper` rozszerzająca interfejs `IServiceCollection`, dodana wraz z biblioteką AutoMappera. Jako argument, przyjmuje ona assembly naszej aplikacji. Kod klas z metodami rozszerzającymi może więc wyglądać np. jak w przykładach poniżej.
```csharp =
using Microsoft.Extensions.DependencyInjection;
using System.Reflection;
using TitlesOrganizer.Application.Interfaces;
using TitlesOrganizer.Application.Services;

namespace TitlesOrganizer.Application
{
    public static class DependencyInjection
    {
        public static IServiceCollection AddApplication(this IServiceCollection services)
        {
            services.AddTransient<IBookService, BookService>()
                .AddTransient<ILanguageService, LanguageService>();
            services.AddAutoMapper(Assembly.GetExecutingAssembly());

            return services;
        }
    }
}
```
```csharp =
using Microsoft.Extensions.DependencyInjection;
using TitlesOrganizer.Domain.Interfaces;
using TitlesOrganizer.Infrastructure.Repositories;

namespace TitlesOrganizer.Infrastructure
{
    public static class DependencyInjection
    {
        public static IServiceCollection AddInfrastructure(this IServiceCollection services)
        {
            services.AddTransient<IBookRepository, BookRepository>()
                .AddTransient<ILanguageRepository, LanguageRepository>();

            return services;
        }
    }
}
```
Kiedy już stworzymy nasze metody, to w projekcie webowym zamiast pojedynczo dodawać wszystkie serwisy związane ze wstrzykiwaniem zależności możemy wywołać te metody. Czyli będziemy mieć:
```csharp =
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
var connectionString = builder.Configuration.GetConnectionString("DefaultConnection") ?? throw new InvalidOperationException("Connection string 'DefaultConnection' not found.");
builder.Services.AddDbContext<Context>(options =>
    options.UseSqlServer(connectionString));
builder.Services.AddDatabaseDeveloperPageExceptionFilter();

builder.Services.AddDefaultIdentity<IdentityUser>(options => options.SignIn.RequireConfirmedAccount = true)
    .AddEntityFrameworkStores<Context>();

builder.Services.AddApplication(); // <= TU dodajemy serwisy zwiazane ze wstrzykiwaniem zaleznosci miedzy interfejsami a serwisami warstwy aplikacji oraz AutoMapperem (jesli uzywamy)
builder.Services.AddInfrastructure(); // <= TU dodajemy serwisy zwiazane ze wstrzykiwaniem zaleznosci miedzy interfejsami z warstwy domenowej a repozytoriami warstwy infrastruktury

builder.Services.AddControllersWithViews();

var app = builder.Build();
```
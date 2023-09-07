# [LEKCJA 7 – Praktyczne wstrzykiwanie zależności](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-8-od-widoku-do-modelu/lekcja-7-praktyczne-wstrzykiwanie-zaleznosci/)
O wstrzykiwaniu zależności (ang. _Dependency Injection_ - DI) trochę już mówiliśmy (tydzień 6. lekcja 9.). Dla przypomnienia jest to mechanizm bazujący na interfejsach. Dzięki temu w naszych klasach możemy używać metod niezależnie od ich implementacji. Zrzucamy na nasz program tworzenie instancji klas implementujących używane w kodzie interfejsy. Mamy do dyspozycji trzy mechanizmy:
1. Scoped service - tworzy nową instancję dla danego zapytania,
2. Transient service - tworzy nową instancję dla każdego obiektu,
3. Singleton service - tworzy tylko jedną instancję na cały czas życia aplikacji.

Zajmijmy się teraz praktyczną stroną dependency injection w naszym projekcie.
## Gdzie wstrzykujemy zależności?
Wszystkie zależności będziemy wstrzykiwać w projekcie UI, a dokładniej w pliku _Program.cs_ (a jeśli istnieje to w _Startup.cs_). Używaliśmy już tego mechanizmu, kiedy w lekcji 5. tygodnia 7. konfigurowaliśmy nasz DbContext. Dla przypomnienia mamy w naszym pliku kod podobny do poniższego:
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
Jak już wspominaliśmy te serwisy mogą być typu transient, scoped, singleton. Tworzymy je odpowiednio przy pomocy metod `AddTransient`, `AddScoped` i `AddSingleton` analogicznie jak wykonano to w powyższym kodzie.

Ważne, aby **serwisy dodawać przed zbudowaniem buildera** (przed linijką `var app = builder.Build();`).

W naszym programie może być dużo repozytoriów i serwisów, a co za tym idzie będziemy musieli stworzyć dużo serwisów do wstrzykiwania zależności. Spowoduje to dodanie dużej ilości linii kodu do pliku _Program.cs_ (_Startup.cs_), a tym samym zmniejszy jego czytelność. Dlatego w poszczególnych projektach (_.Application_ i _.Repository_) często tworzy się dodatkowe klasy statyczne z metodami rozszerzającymi `IServiceCollection`.
## Metody rozszerzające

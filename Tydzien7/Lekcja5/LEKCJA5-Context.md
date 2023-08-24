# [LEKCJA 5 – Context](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-5-context/)
Kontekst jest to specjalna klasa wymagana przez _Entity Framework Core_ definiująca tabele i relacje jakie mają zostać utworzone w bazie danych. Pozwala ona na zmapowanie stworzonych przez nas modeli domenowych na tabele bazodanowe. Tworzy sesję z bazą danych i umożliwia wysyłanie zapytań, pobieranie i modyfikację danych w bazie danych.

Kontekst zależy od typu używanego silnika bazodanowego (np. od paczki _Microsoft.EntityFrameworkCore.SqlServer_) oraz od użytego ORM-a (np. _Entity Framework Core_). Dlatego też utworzymy go w projekcie _.Infrastructure_ (bezpośrednio w projekcie, a nie w folderze). Dzięki temu nasze modele mogą być bez problemu używane przez aplikację, nawet jeśli w pewnym momencie zdecydujemy się na użycie innego silnika bazodanowego, czy innego ORM-a. Będziemy wówczas musieli jedynie podmienić nasz projekt _.Infrastructure_, bez wpływu na resztę aplikacji.

Stworzymy zwykłą publiczną klasę i nazwiemy ją `Context`. W przypadku _Entity Framework Core_ musi ona dziedziczyć po klasie _Microsoft.EntityFrameworkCore.DbContext_. Zawiera ona najbardziej podstawową implementację kontekstu. Ponieważ jednak zaznaczyliśmy, że chcemy przeprowadzać autoryzację użytkowników przy pomocy _Individual Accounts_, to nasza klasa `Context` będzie dziedziczyć po bardziej rozbudowanej klasie bazowej `Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext`. Poza implementacją podstawowych funkcjonalności kontekstu, zawiera ona odpowiednią implementację wspierającą ten typ autoryzacji. Stwórzmy więc odpowiednią klasę, np.:
```csharp =
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using TitlesOrganizer.Domain.Models;

namespace TitlesOrganizer.Infrastructure
{
    public class Context : IdentityDbContext
    {
    }
}
```
Będzie to wymagało doinstalowania jeszcze jednej paczki (_Microsoft.AspNetCore.Identity.EntityFrameworkCore_).

W naszej nowej klasie musimy utworzyć jeszcze odpowiedni konstruktor, który umożliwi inicjalizację podstawowych funkcjonalności kontekstu zaimplementowaną w konstruktorze bazowej klasy `DbContext`:
```csharp =
public Context(DbContextOptions options) : base(options)
{
}
```
Jeżeli nasza baza danych może być zmapowana wyłącznie przy użyciu konwencji _Entity Framework Core_ to wystarczy jeszcze tylko dodać właściwości z naszymi DbSetami.
## `DbSet<TEntity>`
Jest to klasa pozwalająca na zapytywanie i zapisywanie instancji `TEntity`. Zapytania LINQ wykonywane na `DbSet<TEntity>` będą tłumaczone na zapytania na bazie danych. Pozwala więc nam na utworzenie na podstawie modeli domenowych odpowiednich tabel w bazie danych i wykonywanie na nich operacji. W naszej klasie `Context` tworzymy więc właściwość `DbSet` dla każdej tabeli bazodanowej (dla każdego modelu domenowego). Np. jeśli mamy modele

`Book` (książka):
```csharp =
namespace TitlesOrganizer.Domain.Models
{
    public class Book
    {
        public int Id { get; set; }
        public string? Title { get; set; }
        public string? OriginalTitle { get; set; }
        public string? OriginalLanguage { get; set; }
        public int? Year { get; set; }
        public string? Edition { get; set; }
        public string? Description { get; set; }

        // relacja jeden do jeden
        public Cover? Cover { get; set; }

        // relacja jeden do wielu
        public int? SeriesId { get; set; }
        public Series? Series { get; set; }

        // relacje wiele do wielu
        public virtual ICollection<BookAuthor>? BookAuthors { get; set; }
        public virtual ICollection<BookGenre>? BookGenres { get; set; }
    }
}
```
`Cover` (okładka książki):
```csharp =
namespace TitlesOrganizer.Domain.Models
{
    public class Cover
    {
        public int Id { get; set; }
        public string? Name { get; set; }
        public string? Description { get; set; }

        // relacja jeden do wielu
        public int DesignerId { get; set; }
        public Designer Designer { get; set; }

        // relacja jeden do jednego
        public int BookId { get; set; }
        public Book Book { get; set; }
    }
}
```
`Designer` (twórca okładki):
```csharp =
namespace TitlesOrganizer.Domain.Models
{
    public class Designer
    {
        public int Id { get; set; }
        public string? Name { get; set; }
        public string? LastName { get; set; }
        public string? Pseudonim { get; set; }

        public virtual ICollection<Cover> Covers { get; set; }
    }
}
```
`Author` (autor książki):
```csharp =
namespace TitlesOrganizer.Domain.Models
{
    public class Author
    {
        public int Id { get; set; }
        public string? Name { get; set; }
        public string? LastName { get; set; }

        public virtual ICollection<BookAuthor>? BookAuthors { get; set; }
    }
}
```
`Genre` (gatunek do którego należy książka):
```csharp =
namespace TitlesOrganizer.Domain.Models
{
    public class Genre
    {
        public int Id { get; set; }
        public required string Name { get; set; }

        public virtual ICollection<BookGenre>? BookGenres { get; set; }
    }
}
```
`Series` (do tworzenia serii książek, np. trylogie):
```csharp =
namespace TitlesOrganizer.Domain.Models
{
    public class Series
    {
        public int Id { get; set; }
        public string? Title { get; set; }
        public string? Description { get; set; }
        public int? ExpectedLength { get; set; }

        public ICollection<Book>? Books { get; set; }
    }
}
```
oraz pomocnicze klasy do stworzenia relacji wiele do wielu między książką, a autorem (`BookAuthor`):
```csharp =
namespace TitlesOrganizer.Domain.Models
{
    public class BookAuthor
    {
        public int BookId { get; set; }
        public Book Book { get; set; }
        public int AuthorId { get; set; }
        public Author Author { get; set; }
    }
}
```
i książką a gatunkiem (`BookGenre`):
```csharp =
namespace TitlesOrganizer.Domain.Models
{
    public class BookGenre
    {
        public int BookId { get; set; }
        public Book Book { get; set; }
        public int GenreId { get; set; }
        public Genre Genre { get; set; }
    }
}
```
W klasie `Context` tworzymy dla nich odpowiednie właściwości `DbSet`:
```csharp =
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using TitlesOrganizer.Domain.Models;

namespace TitlesOrganizer.Infrastructure
{
    public class Context : IdentityDbContext
    {
        public DbSet<Author> Authors { get; set; }
        public DbSet<Book> Books { get; set; }
        public DbSet<BookAuthor> BookAuthor { get; set; }
        public DbSet<BookGenre> BookGenre { get; set; }
        public DbSet<Cover> Covers { get; set; }
        public DbSet<Designer> Designers { get; set; }
        public DbSet<Genre> Genres { get; set; }
        public DbSet<Series> Serieses { get; set; }

        public Context(DbContextOptions options) : base(options)
        {
        }
    }
}
```
Tradycyjnie nazwa właściwości `DbSet` odpowiada liczbie mnogiej nazwy modelu, którego dotyczy. Wyjątek stanowią modele pomocnicze do tworzenia relacji wiele do wielu, w których używamy liczby pojedynczej, właśnie, aby wyróżnić, że są to tabele z relacjami.

Gdyby wszystkie klasy i relacje były utworzone zgodnie z konwencją, to na tym moglibyśmy zakończyć tworzenie naszej klasy `Context`. Jak już jednak wspominaliśmy konwencja wspiera wyłącznie tworzenie relacji jeden do wielu (już nie tylko, ale nadal może istnieć konieczność skonfigurowania rzeczy niezgodnych z konwencją lub przez nią nieuwzględnionych, np. jednokierunkowa relacja wiele do wielu). W naszym przykładzie występują jednak również relacje jeden do jednego i wiele do wielu. Musimy więc je jeszcze zdefiniować (nie trzeba już jawnie konfigurować relacji jeden do jednego i wiele do wielu, ale ponieważ tu mamy utworzone modele dla pomocniczych tabeli łączących, więc musimy przynajmniej zdefiniować dla nich klucze złożone). Posłuży do tego tzw. _fluent API_.
## _Fluent API_
Tym mianem określamy metody klasy `ModelBuilder`. Pozwalają one na nadpisanie jakichkolwiek domyślnych konfiguracji mapowania _Entity Framework Core_. Zmiany wykonane przy pomocy _Fluent API_ są nadrzędne względem wszystkich innych konfiguracji. Modyfikacji `ModelBuilder`a dokonujemy przez nadpisanie metody `OnModelCreating(ModelBuilder modelBuilder)` klasy `DbContext`.
### `OnModelCreating`
Metodę nadpisuje się, gdy chce się dalej skonfigurować model utworzony przy pomocy konwencji na podstawie modeli domenowych wskazanych przez `DbSet`y. Ponieważ `IdentityDbContext` nadpisuje tą metodę, aby skonfigurować modele związane z użytkownikami, więc na początku metody musimy wywołać jej bazową wersję. Następnie przechodzimy do konfiguracji utworzonych przez nas modeli.
### `Entity`
Najpierw na naszym `modelBuilder`ze wywołujemy metodę:
```csharp =
public virtual Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder<TEntity> Entity<TEntity> () where TEntity : class;
```
Zwraca ona obiekt `EntityTypeBuilder<TEntity>`, który może zostać użyty do skonfigurowania modelu `TEntity`.
### `HasKey`
Metoda klasy `EntityTypeBuilder<TEntity>` pozwalająca na ustawienie właściwości modelu `TEntity`, które będą tworzyć klucz główny. Wywołujemy ją na obiekcie zwróconym przez metodę `Entity<TEntity>`.
### `HasOne<TRelatedEntity>`
Metoda klasy `EntityTypeBuilder<TEntity>` konfigurująca relację, gdzie `TEntity` posiada referencję wskazującą na pojedynczą instancję `TRelatedEntity`. Zwraca obiekt typu `ReferenceNavigationBuilder<TEntity,TRelatedEntity>`.
### `HasMany<TRelatedEntity>`
Metoda klasy `EntityTypeBuilder<TEntity>` konfigurująca relację, gdzie `TEntity` posiada kolekcję zawierającą instancje `TRelatedEntity`. Zwraca obiekt typu `CollectionNavigationBuilder<TEntity,TRelatedEntity>`.
### `WithOne`
Metoda klasy `ReferenceNavigationBuilder<TEntity,TRelatedEntity>` lub `CollectionNavigationBuilder<TEntity,TRelatedEntity>`. Konfiguruje ona odpowiednią relację i zwraca obiekt umożliwiający jej dalszą konfigurację.
Klasa|Relacja|Typ zwracany
---|---|---
`ReferenceNavigationBuilder <TEntity,TRelatedEntity>`|jeden do jednego|`ReferenceReferenceBuilder <TEntity,TRelatedEntity>`
`CollectionNavigationBuilder <TEntity,TRelatedEntity>`|jeden do wielu|`ReferenceCollectionBuilder <TEntity,TRelatedEntity>`
### `WithMany`
Metoda klasy `ReferenceNavigationBuilder<TEntity,TRelatedEntity>` lub `CollectionNavigationBuilder<TEntity,TRelatedEntity>`. Konfiguruje ona odpowiednią relację i zwraca obiekt umożliwiający jej dalszą konfigurację.
Klasa|Relacja|Typ zwracany
---|---|---
`ReferenceNavigationBuilder <TEntity,TRelatedEntity>`|jeden do wielu|`ReferenceCollectionBuilder <TRelatedEntity,TEntity>`
`CollectionNavigationBuilder <TEntity,TRelatedEntity>`|wiele do wielu|`CollectionCollectionBuilder <TRelatedEntity,TEntity>`
### `HasForeignKey`
Metoda klas `ReferenceCollectionBuilder<TPrincipalEntity,TDependentEntity>` i `ReferenceReferenceBuilder<TEntity,TRelatedEntity>`. Konfiguruje właściwość (właściwości), którą (które) należy użyć, jako klucza obcego tej relacji.
### Przykład
Skonfigurujmy więc relacje jeden do jednego i wiele do wielu, dla naszych przykładowych modeli. Zauważmy, że relacja wiele do wielu, jest w praktyce relacją jeden do wielu, pomiędzy modelami, a klasą pomocniczą. Czyli np. relacja wiele do wielu pomiędzy `Book` i `Author`, jest tak na prawdę relacją jeden do wielu między `Book` i `BookAuthor` oraz `Author` i `BookAuthor`. Zwróćmy jeszcze uwagę, że dla modeli pomocniczych musimy skonfigurować klucze główne, gdyż będą to tzw. klucze złożone, składające się z dwóch właściwości, będących kluczami obcymi. Nadpiszmy więc metodę `OnModelCreating`:
```csharp =
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using TitlesOrganizer.Domain.Models;

namespace TitlesOrganizer.Infrastructure
{
    public class Context : IdentityDbContext
    {
        public DbSet<Author> Authors { get; set; }
        public DbSet<Book> Books { get; set; }
        public DbSet<BookAuthor> BookAuthor { get; set; }
        public DbSet<BookGenre> BookGenre { get; set; }
        public DbSet<Cover> Covers { get; set; }
        public DbSet<Designer> Designers { get; set; }
        public DbSet<Genre> Genres { get; set; }
        public DbSet<Series> Serieses { get; set; }

        public Context(DbContextOptions options) : base(options)
        {
        }
        
        protected override void OnModelCreating(ModelBuilder builder)
        {
            // konfiguracja identity
            base.OnModelCreating(builder);

            // konfiguracja relacji wiele do wielu Book - Author
            builder.Entity<BookAuthor>()
                .HasKey(it => new { it.AuthorId, it.BookId }); // na klucz glowny entity BookAuthor skladaja sie wlasciwosci AuthorId i BookId

            builder.Entity<BookAuthor>()         // entity BookAuthor
                .HasOne<Book>(it => it.Book)     // jest w relacji z jednym entity Book (przechowywanym we wlasciwosci Book)
                .WithMany(i => i.BookAuthors)    // jest to relacja jeden do wielu (entity Book posiada kolekcje BookAuthors obiektow BookAuthor)
                .HasForeignKey(it => it.BookId); // kluczem obcym tworzacym ta releacje jest wlasciwosc BookId

            builder.Entity<BookAuthor>()            // entity BookAuthor
                .HasOne<Author>(it => it.Author)    // jest w relacji z jednym entity Author (przechowywanej we wlasciwosci Author)
                .WithMany(i => i.BookAuthors)       // jest to relacja typu jeden do wielu (entity Author posiada kolekcje BookAuthors obiektow BookAuthor)
                .HasForeignKey(it => it.AuthorId); // kluczem obcym tworzacym ta relacje jest wlasciwosc AuthorId

            // konfiguracja relacji wiele do wielu Book - Genre
            builder.Entity<BookGenre>()
                .HasKey(it => new { it.BookId, it.GenreId }); // na klucz glowny entity BookGenre skladaja sie wlasciwosci BookId i GenreId

            builder.Entity<BookGenre>()          // entity BookGenre
                .HasOne<Book>(it => it.Book)     // jest w relacji z jednym entity Book (przechowywanym we wlasciwosci Book)
                .WithMany(i => i.BookGenres)     // jest to relacja typu jeden do wielu (entity Book posiada kolekcje BookGenres obiektow BookGenre)
                .HasForeignKey(it => it.BookId); // kluczem obcym tworzacym ta relacje jest wlasciwosc BookId

            builder.Entity<BookGenre>()           // entity BookGenre
                .HasOne<Genre>(it => it.Genre)    // jest w relacji z jednym entity Genre (przechowywanym we wlasciwosci Genre)
                .WithMany(i => i.BookGenres)      // jest to relacja typu jeden do wielu (entity Genre posiada kolekcje BookGenres obiektow BookGenre)
                .HasForeignKey(it => it.GenreId); // kluczem obcym tworzacym ta relacje jest wlasciwosc GenreId

            // konfiguracja relacji jeden do jednego Book - Cover
            builder.Entity<Book>()                  // entity Book
                .HasOne(it => it.Cover)      // jest w relacji z jednym entity Cover (przechowywanym we wlaciwosci Cover)
                .WithOne(i => i.Book)               // jest to relacja jeden do jeden (entity Cover posiada wlasciwosc Book, przechowujaca obiekt Book)
                .HasForeignKey<Cover>(it => it.BookId); // kluczem obcym tworzacym ta relacje jest wlasciwosc BookId entity Cover
        }
    }
}
```
## Konfiguracja UI
Kiedy tworzyliśmy nasz projekt _.Web_ wybraliśmy opcję autoryzacji użytkowników przy pomocy _Individual Accounts_. Przez to, został dla nas automatycznie utworzony podstawowy `IdentityDbContext`. Znajduje się on w katalogu _Data_ projektu _.Web_. Ponieważ aplikacja MVC ma być tylko frontem naszej aplikacji, więc nie chcemy się w niej łączyć z bazą danych. Usuwamy więc cały katalog _Data_. Teraz, musimy jeszcze zaktualizować konfigurację naszej aplikacji. W pliku _Program.cs_ (lub _Startup.cs_, jeśli istnieje) aplikacji webowej podmieniamy usunięty przed chwilą kontekst (`ApplicationDbContext`), na ten przez nas utworzony (`TitlesOrganizer.Infrastructure.Context`).

Więc kod:
```csharp =
using TitlesOrganizer.Web.Data; // ZMIANIC REFERENCJE

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
var connectionString = builder.Configuration.GetConnectionString("DefaultConnection") ?? throw new InvalidOperationException("Connection string 'DefaultConnection' not found.");
builder.Services.AddDbContext<ApplicationDbContext>(options => // ZMIENIC TUTAJ
    options.UseSqlServer(connectionString));
builder.Services.AddDatabaseDeveloperPageExceptionFilter();

builder.Services.AddDefaultIdentity<IdentityUser>(options => options.SignIn.RequireConfirmedAccount = true)
    .AddEntityFrameworkStores<ApplicationDbContext>(); // ZMIENIC TUTAJ
builder.Services.AddControllersWithViews();

var app = builder.Build();
```
zamieniamy na:
```csharp =
using TitlesOrganizer.Infrastructure; // PO ZMIANIE

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
var connectionString = builder.Configuration.GetConnectionString("DefaultConnection") ?? throw new InvalidOperationException("Connection string 'DefaultConnection' not found.");
builder.Services.AddDbContext<Context>(options => // PO ZMIANIE
    options.UseSqlServer(connectionString));
builder.Services.AddDatabaseDeveloperPageExceptionFilter();

builder.Services.AddDefaultIdentity<IdentityUser>(options => options.SignIn.RequireConfirmedAccount = true)
    .AddEntityFrameworkStores<Context>(); // PO ZMIANIE
builder.Services.AddControllersWithViews();

var app = builder.Build();
```
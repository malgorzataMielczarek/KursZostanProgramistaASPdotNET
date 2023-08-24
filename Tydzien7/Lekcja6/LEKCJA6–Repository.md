# [LEKCJA 6 – Repository](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-6-repository/)
Kiedy mamy już utworzony kontekst ze wszystkimi naszymi tabelami bazodanowymi, możemy przejść do implementacji operacji na danych. Poprzednio, w aplikacji konsolowej, używaliśmy w tym celu serwisów. W aplikacji webowej stworzymy do tego celu repozytoria w naszym projekcie _.Infrastructure_.
## Czy trzeba tworzyć repozytoria?
Od kiedy powstał _Entity Framework Core_ takie operacje możemy wykonywać bezpośrednio na utworzonych w kontekście `DbSet`ach. Teoretycznie nie musielibyśmy więc tworzyć repozytoriów. Warto jednak oddzielnie zaimplementować operacje na danych z bazy danych. Pozwala nam to na kontrolę nad tym, jak te operacje są wykonywane, co zmniejsza ryzyko błędu. Poprawia to również podział logiczny aplikacji. Serwisy nie będą wykonywać operacji na bazie danych, a jedynie otrzymywać i przesyłać odpowiednie dane i wywoływać odpowiednie operacje.
## Jak tworzyć repozytoria?
Istnieją dwa podejścia do tworzenia repozytoriów. Po pierwsze możemy tworzyć osobne repozytorium dla każdej tabeli. Często jednak mamy powiązane z sobą tabele, które zawsze będziemy używać razem. Np. w jednej tabeli przechowujemy książki, a w drugiej autorów. Wyświetlając, tworząc lub edytując książki będziemy również podawać autorów. Samych autorów też nie będziemy raczej używać nie w odniesieniu do książek. Tworzenie osobnych repozytoriów dla obu tych tabel może nie mieć więc sensu. Zgodnie z drugim podejściem tworzymy więc repozytoria zgodnie z logiką biznesową. Wówczas w jednym repozytorium możemy wykonywać operacje na kilku `DbSet`ach.
## Tworzenie repozytoriów
Repozytoria tworzymy w naszym projekcie _.Infrastructure_ w katalogu _Repositories_. Będą to po prostu zwykłe klasy. Będziemy im nadawać nazwy z przyrostkiem _Repository_, np. _BookRepository_. Do każdego repozytorium będziemy musieli wstrzyknąć nasz kontekst, np.:
```csharp =
using TitlesOrganizer.Domain.Models;

namespace TitlesOrganizer.Infrastructure.Repositories
{
    public class BookRepository
    {
        // wstrzykujemy kontekst do repozytorium
        private readonly Context _context;

        public BookRepository(Context context)
        {
            _context = context;
        }
    }
}
```
Teraz będziemy już mogli odwoływać się w naszym repozytorium do `DbSet`ów naszego kontekstu i wykonywać na nich operacje. Robimy to przy użyciu LINQ, analogicznie jak to robiliśmy na listach w aplikacji konsolowej. Dodatkowo, kiedy wykonamy pożądane zmiany w naszym kontekście (dodamy/usuniemy/zmodyfikujemy dane w jednym lub kilku `DbSet`ach) i chcemy zapisać je w bazie danych musimy wywołać funkcję `SaveChanges` kontekstu. Konteksty działają na zasadzie transakcji. Tzn. zbierane są wszystkie informacje które zostały zmienione w kontekście. Są one wykonywane na bazie danych, ale jeżeli któraś z operacji na bazie się nie powiedzie, to wszystkie te zmiany zostaną cofnięte i nasza baza pozostanie bez zmian.

Kiedy tworzymy metodę pobierającą z bazy zestaw (kolekcję) danych, to powinna ona zawsze mieć zwracany typ danych `IQueryable<T>`. Dzięki temu metoda ta nie zwraca tak na prawdę fizycznej kolekcji obiektów, a formułuje jedynie zapytanie do bazy danych. Jeżeli później w serwisie będziemy wywoływać taką metodę i wykonywać jakieś dalsze filtrowanie, to nasze zapytanie zostanie zmodyfikowane i dopiero wtedy wykonane. W efekcie z bazy danych zostaną pobrane wyłącznie dane, których naprawdę potrzebujemy.

Dodając nowe obiekty do bazy nie nadajemy im numerów id (o ile są one kluczami głównymi), jak to robiliśmy w naszej aplikacji konsolowej. Zostaną one nadane automatycznie przez naszą bazę danych, przez autoinkrementację klucza głównego tabeli.
## Przykład
Wykorzystajmy kontekst utworzony w poprzedniej lekcji. Stwórzmy repozytorium związane z książkami. Zaimplementujmy w nim kilka podstawowych metod.
```csharp =
using TitlesOrganizer.Domain.Models;

namespace TitlesOrganizer.Infrastructure.Repositories
{
    public class BookRepository
    {
        private readonly Context _context;

        public BookRepository(Context context)
        {
            _context = context;
        }

        public int AddBook(Book book)
        {
            _context.Books.Add(book);
            _context.SaveChanges();

            return book.Id;
        }

        public void DeleteBook(int bookId)
        {
            Book? book = _context.Books.Find(bookId);
            if (book != null)
            {
                _context.Books.Remove(book);
                _context.SaveChanges();
            }
        }

        public Book? GetBookById(int bookId)
        {
            return _context.Books.FirstOrDefault(book => book.Id == bookId);
        }

        public IQueryable<Book> GetBooksByAuthor(int authorId)
        {
            return _context.Books.Where(b => b.BookAuthors.Select(ba => ba.AuthorId).Contains(authorId));
        }
    }
}
```
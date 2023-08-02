# [LEKCJA 7 – Abstract](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-7-abstract/)
Jak wspominaliśmy podczas omawiania architektury naszej aplikacji, w warstwie domenowej naszej aplikacji poza modelami będziemy mieć interfejsy, które będą implementowane przez repozytoria warstwy infrastruktury. Interfejsy te nazywamy kontraktami. Są one bardzo ważne, gdyż dzięki nim będziemy mogli w warstwie aplikacji korzystać z tych metod bez znajomości i zależności od implementacji, a co za tym idzie użytego silnika bazodanowego.
## Tworzenie kontraktów dla repozytoriów
W naszej aplikacji _.Domain_ w katalogu _Interfaces_ stwórzmy interfejs dla każdego repozytorium, będący jego szablonem. Są to zwyczajne interfejsy. Ich nazwy są zgodne z nazwą odpowiedniego repozytorium, poprzedzając je wielką literę "i", np. _IBookRepository_.

Po stworzeniu kontraktów pamiętajmy jeszcze, aby w repozytoriach dopisać jakie interfejsy implementują.
## Przykład
Stwórzmy kontrakt dla uproszczonego repozytorium z poprzedniej lekcji:
```csharp =
using TitlesOrganizer.Domain.Models;

namespace TitlesOrganizer.Domain.Interfaces
{
    public interface IBookRepository
    {
        int AddBook(Book book);

        void DeleteBook(int bookId);

        Book? GetBookById(int bookId);

        IQueryable<Book> GetBooksByAuthor(int authorId);
    }
}
```
Dopiszmy jeszcze do repozytorium jaki interfejs implementuje:
```csharp =
using TitlesOrganizer.Domain.Interfaces; // Dopisz odpowiedni using
using TitlesOrganizer.Domain.Models;

namespace TitlesOrganizer.Infrastructure.Repositories
{
    public class BookRepository : IBookRepository // dopisz implementowany interfejs
    {
        ...
    }
}
```
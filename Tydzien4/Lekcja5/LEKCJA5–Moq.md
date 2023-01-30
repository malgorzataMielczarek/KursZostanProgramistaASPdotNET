# [LEKCJA 5 – Moq](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-5-moq/)
Biblioteka służąca do stworzenia pewnej ilości danych, która będzie nam potrzebna do sprawdzenia funkcjonalności. Zazwyczaj używamy jej do zasymulowania działania pewnych metod, których nie chcemy sprawdzać w danym teście lub do zasymulowania danych z bazy danych. Zazwyczaj podczas testowania nie chcemy opierać się na danych z bazy danych, gdyż nie jesteśmy w stanie przewidzieć co się tam w danej chwili znajduje.
## Instalacja biblioteki _Moq_
Zanim będziemy mogli wykorzystać tą bibliotekę, musimy ją oczywiście zainstalować. Otwieramy _NuGet Manager_. W tym celu wybieramy _Tools_ -> _NuGet Package Manager_ -> _Manage NuGet Packages for Solution..._ i  w zakładce _Browse_ wyszukujemy _Moq_ (na przykład poprzez wpisanie tej nazwy w polu wyszukiwarki - Ctrl+L). Następnie wybieramy i instalujemy naszą bibliotekę.
## Wybór metody do przetestowania
Załóżmy, że mamy klasę `ItemManager`, o następującym konstruktorze:

```csharp =
public ItemManager(MenuActionService actionService, IService<Item> itemService)
{
    _itemService = itemService;
    _actionService = actionService;
}
```

W klasie tej znajduje się m.in. pole `_itemService` klasy `IService<Item>`. W klasie `ItemManager` zdefiniowana jest metoda `GetItemById`, którą chcemy przetestować.

```csharp =
public Item GetItemById(int id)
{
    var item = _itemService.GetItemById(id);
    return item;
}
```

## Test z użyciem biblioteki _Moq_
### Arrange
Metoda którą chcemy przetestować korzysta z klasy `Item`. Ma ona m.in. właściwość `Id` i trzyargumentowy konstruktor, w którym pierwszy argument inicjalizuje `Id`. Poza tym `GetItemById` korzysta również z metody `GetItemById` klasy `IService<Item>`. W naszym teście nie chcemy jednak sprawdzać poprawności działania klasy serwisowej. Zakładamy, że działa ona poprawnie. Musimy więc w teście zasymulować jej działanie. Do tego właśnie użyjemy biblioteki _Moq_. Stwórzmy więc potrzebne nam dane.

```csharp =
// Tworzymy jakis obiekt klasy Item, ktora tez mamy zdefiniowana w naszej aplikacji
Item item = new Item(1, "Apple", 2);
// Symulujemy działanie serwisu IService<Item> - testujemy go w osobnym teście
var mock = new Mock<IService<Item>>();
mock.Setup(s => s.GetItemById(1)).Returns(item);
// tworzymy obiekt klasy, którą będzie testować
var manager = new ItemManager(new MenuActionService(), mock.Object);
```

### Act
Mamy już przygotowane wszystkie potrzebne nam dane. Stwórzmy więc logikę naszego testu.

```csharp =
var returnedItem = manager.GetItemById(item.Id);
```

### Assert
Na koniec sprawdźmy uzyskany wynik. Spodziewamy się, że otrzymaliśmy taki sam obiekt `Item`, jak ten który na początku stworzyliśmy i którego `Id` przekazaliśmy jako parametr testowanej funkcji. Sprawdźmy więc, czy oba obiekty są sobie równe.

```csharp =
Assert.Equal(item, returnedItem);
```

### Cały test
Kod całego testu będzie więc wyglądał następująco:

```csharp =
using NazwaAplikacji.Domain.Entity;
using NazwaAplikacji.App.Abstract;
using NazwaAplikacji.App.Concrete;
using NazwaAplikacji.App.Managers;
using Xunit;

namespace NazwaAplikacji.Tests
{
    public class UnitTest1
    {
        [Fact]
        public void Test1()
        {
            //Arrange
            Item item = new Item(1, "Apple", 2);
            var mock = new Mock<IService<Item>>();
            mock.Setup(s => s.GetItemById(1)).Returns(item);
            var manager = new ItemManager(new MenuActionService(), mock.Object);
            //Act
            var returnedItem = manager.GetItemById(item.Id);
            //Assert
            Assert.Equal(item, returnedItem);
        }
    }
}
```

Jeżeli w naszej aplikacji mamy zdefiniowane wszystkie potrzebne klasy, to możemy teraz uruchomić test. Powinien zakończyć się on pozytywnie.

Oczywiście jest to najprostszy test, służący tylko przedstawieniu biblioteki _Moq_. W prawdziwym projekcie prawdopodobnie będziemy musieli zasymulować więcej obiektów, metod. Będziemy też przy zapewne sprawdzać więcej uzyskanych wartości. Biblioteki _Moq_ będziemy najczęściej używać do zasymulowania danych uzyskiwanych bezpośrednio z bazy danych. Dzięki temu niezależnie od niej będziemy mogli sprawdzić logikę biznesową naszej aplikacji.

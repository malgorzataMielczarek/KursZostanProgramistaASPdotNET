# [LEKCJA 6 – FluentAssertions](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-6-fluentassertions/)
Biblioteka rozszerzająca możliwości kroku _Assert_ testów[^*].
## Instalacja biblioteki _FluentAssertions_
Podobnie jak w poprzedniej lekcji, zanim będziemy mogli wykorzystać tą bibliotekę, musimy ją oczywiście zainstalować. Ponownie otwieramy _NuGet Manager_. W tym celu wybieramy _Tools_ -> _NuGet Package Manager_ -> _Manage NuGet Packages for Solution..._ i  w zakładce _Browse_ wyszukujemy _FluentAssertions_ (na przykład poprzez wpisanie tej nazwy w polu wyszukiwarki - Ctrl+L). Następnie wybieramy i instalujemy naszą bibliotekę.
## Test z wykorzystaniem biblioteki _FluentAssertions_
Wykorzystajmy test utworzony w poprzedniej lekcji.

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

Jak już wspomnieliśmy bibliotekę _FluentAssertions_ wykorzystujemy w kroku _Assert_. Będziemy więc modyfikować tylko ten krok. Posiada ona bardziej intuicyjną składnię, dzięki czemu łatwiej jest pisać testy i łatwiej sprawdzić wszystkie przypadki testowe.

Zastanówmy się więc jeszcze raz co chcieliśmy sprawdzić powyższym testem. Upewnialiśmy się, że metoda `GetItemById` menadżera zwraca poprawną, oczekiwaną przez nas wartość. Co to jednak znaczy?
### 1. Typ zwracanego obiektu
Po pierwsze spodziewamy się, że otrzymamy obiekt typu `Item`. Czyli obiekt `returnedItem` powinien być typu `Item`. To zdanie zapisane przy pomocy _FluentAssertions_ będzie wyglądać następująco:
```csharp =
returnedItem.Should().BeOfType(typeof(Item));
```

### 2. Wartość nie `null`
Po drugie oczekujemy, że otrzymamy rzeczywiście jakiś konkretny obiekt. Czyli obiekt `returnedItem` nie powinien być `null`. To zdanie zapisane przy pomocy _FluentAssertions_ będzie wyglądać następująco:
```csharp =
returnedItem.Should().NotBeNull();
```

### 3. Zwracamy obiekt utworzony na początku
I w końcu spodziewamy się otrzymać ten sam obiekt który na początku utworzyliśmy. Czyli obiekt `returnedItem` powinien być taki sam jak obiekt `item`. To zdanie zapisane przy pomocy _FluentAssertions_ będzie wyglądać następująco:
```csharp =
returnedItem.Should().BeSameAs(item);
```

### 4. Ulepszony test
Jak widać biblioteka _FluentAssertions_ pozwala nam pisać sprawdzane wartości prawie dokładnie tak jak byśmy mówili to w języku naturalnym. Dzięki temu testy są łatwiejsze w pisaniu i czytelniejsze. Z tego względu warto z niej korzystać.

Poprawmy więc nasz test:

```csharp =
using NazwaAplikacji.Domain.Entity;
using NazwaAplikacji.App.Abstract;
using NazwaAplikacji.App.Concrete;
using NazwaAplikacji.App.Managers;
using Xunit;
using FluentAssertions;

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
            returnedItem.Should().BeOfType(typeof(Item));
            returnedItem.Should().NotBeNull();
            returnedItem.Should().BeSameAs(item);
        }
    }
}
```

[^*]: Jeśli ktoś woli może również korzystać z podobnej biblioteki - _Shouldly_.

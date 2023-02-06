# [LEKCJA 9 – Testy integracyjne](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-9-testy-integracyjne/)
Pomimo, że nie są to testy jednostkowe, to testy tego rodzaju również często są pisane przez programistów.

**Testy integracyjne** - służą do testowania integracji pomiędzy warstwami naszej aplikacji np. menadżerami i serwisami. Piszemy je podobnie, jak testy jednostkowe, tylko jeżeli np. sprawdzamy integrację między menadżerami i serwisami, to nie mokujemy już serwisów, ale działami na rzeczywistych obiektach. Ponieważ serwisy przeprowadzają operacje na bazie danych, to używając rzeczywistych obiektów serwisowych, podczas wykonywania testów, wprowadzimy rzeczywiste zmiany w bazie danych. Dlatego testy integracyjne poza etapami _Arrange_-_Act_-_Assert_ posiadają na końcu jeszcze jeden etap, _Clean_, w którym będziemy "sprzątać" po teście, czyli przywracać bazę danych do stanu przed testem.

## Przykład
Stwórzmy przykładowy test integracyjny na podstawie testu jednostkowego z poprzedniej lekcji.
### Test jednostkowy

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
        public void CanDeleteItemWithProperId()
        {
            //Arrange
            Item item = new Item(1, "Apple", 2);
            var mock = new Mock<IService<Item>>();
            mock.Setup(s => s.GetItemById(1)).Returns(item);
            mock.Setup(m => m.RemoveItem(It.IsAny<Item>()));
            var manager = new ItemManager(new MenuActionService(), mock.Object);
            //Act
            manager.RemoveItemById(item.Id);
            //Assert
            mock.Verify(m => m.RemoveItem(item));
        }
    }
}
```

### Test integracyjny
Powyżej, dla przypomnienia, znajduje się test jednostkowy, który stworzyliśmy w poprzedniej lekcji. Jak już wspominaliśmy w teście integracyjnym nie sprawdzamy działania poszczególnych funkcji, a współdziałanie między poszczególnymi warstwami aplikacji.
#### _Arrange_
Nie będziemy więc teraz mokować naszego serwisu, a stworzymy jego faktyczny obiekt. Wypełnimy też go od razu jakąś daną, czyli naszym obiektem `item`.

```csharp =
Item item = new Item(1, "Apple", 2);
IService<Item> itemService = newService();
itemService.AddItem(item);
var manager = new ItemManager(new MenuActionService(), itemService);
```

#### _Act_
Krok _act_ pozostawiamy bez zmian.
#### _Assert_
Ponieważ nie mamy już obiektu `Mock`, krok _assert_ musimy zmodyfikować. Ponieważ w teście usuwaliśmy obiekt `item` z serwisu (bazy danych), to możemy sprawdzić, czy taki obiekt nadal w nim istnieje. Czyli oczekujemy, że próba odnalezienia obiektu o `Id` `item.Id`, da nam wynik `null`.

```csharp =
itemService.GetItemById(item.Id).Should().BeNull();
```

Możemy to również zapisać bardziej bezpośrednio, sprawdzając, czy na liście `Item`ów w serwisie istnieje element o `id` `item.Id`.

```csharp =
itemService.Items.FirstOrDefault(p => p.Id == item.Id).Should().BeNull();
```

#### _Clean_
W naszym przypadku, ponieważ w teście usuwaliśmy element, który na początku testu dodaliśmy, a w dodatku nie działamy jeszcze na bazie danych, więc nie mamy nic do czyszczenia.

#### Cały test

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
        public void CanDeleteItemWithProperId()
        {
            //Arrange
            Item item = new Item(1, "Apple", 2);
            IService<Item> itemService = newService();
            itemService.AddItem(item);
            var manager = new ItemManager(new MenuActionService(), itemService);
            //Act
            manager.RemoveItemById(item.Id);
            //Assert

            //itemService.GetItemById(item.Id).Should().BeNull();
            itemService.Items.FirstOrDefault(p => p.Id == item.Id).Should().BeNull();

            //Clean
        }
    }
}
```

Jeżeli mamy zaimplementowane wszystkie wymagane elementy aplikacji, to wykonanie takiego testu powinno zakończyć się pozytywnie.

## _Clean_
Jak już wspominaliśmy, kiedy podczas wykonywania testu wprowadzamy jakieś zmiany w bazie danych, to na jego zakończenie, musimy wszystkie te zmiany cofnąć. W naszym przykładzie nie mieliśmy nic do czyszczenia, gdyż nie pracujemy jeszcze na bazie danych. Gdybyśmy jednak taki test wykonywali na serwisie dodającym i usuwającym obiekty z bazy danych, to musimy uważać. Na początku testu dodaliśmy do bazy danych obiekt, który następnie usunęliśmy w kroku _act_. Mogłoby się więc wydawać, że wszystko powinno być w porządku. Jeżeli jednak nasza baza danych korzystna z mechanizmów auto-inkrementacji np. numerów id, to może się okazać, że zwiększyliśmy odpowiedzialny za nią znacznik i kolejny obiekt jaki dodamy do bazy danych będzie miał id o jedno większe od obiektu, który właśnie usunęliśmy, a nie od ostatniego obiektu znajdującego się w bazie danych (obiektu, który był ostatni, przed wykonaniem testu). Wówczas testując aplikacją będziemy tworzyć "dziury" w numerach id (lub innych wartościach). Jeżeli tabela na której pracujemy podczas testu posiada jakieś mechanizmy auto-inkrementacji, to musimy pamiętać, aby po zakończeniu testu, w etapie czyszczenia (_Clean_) przywrócić znacznikom odpowiednie wartości. Jest to oczywiście możliwe. Dlatego przed przestąpieniem do pisania testów integracyjnych należy zapoznać się z takimi mechanizmami, tak aby testowanie aplikacji, nie przynosiło więcej szkody niż pożytku.

# [LEKCJA 8 – TDD](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-8-tdd/)
TDD (ang. _Test Driven Development_) - metodologia w tworzeniu oprogramowania, polegająca na tym, że tworzenie oprogramowania rozpoczyna się od napisania testów.
## Podejście klasyczne (najpierw kod, potem testy)
Do tej pory tworzyliśmy testy na podstawie metod.
1. Na podstawie wymagań funkcjonalnych piszemy metodę.
2. Wiemy co chcieliśmy, aby ta metoda robiła i zwracała.
3. Piszemy test sprawdzający, czy w wyniku działania metody, rzeczywiście otrzymaliśmy spodziewane rezultaty.
4. Przeprowadzamy test.
## **TDD** (najpierw testy, potem kod)
Zgodnie z metodologią **TDD** postępujemy na odwrót.
1. Na podstawie wymagań funkcjonalnych określamy jakie wyniki chcemy otrzymać.
2. Na tej podstawie piszemy testy.
3. Implementujemy metody wykonujące odpowiednie przekształcenia.
4. Przeprowadzamy testy i sprawdzamy, czy przekształcenia dały oczekiwany rezultat.
## Przykład
### Test
Przypomnijmy sobie nasz ostatni test. Sprawdzaliśmy w nim, działanie funkcji menadżera, zwracającej obiekt `Item` na podstawie `Id`. Załóżmy teraz, że nasza aplikacja musi mieć również funkcjonalność usuwającą `Item` z bazy danych, na podstawie `Id`. Samo usuwanie obiektu z bazy danych będzie się odbywać oczywiście za pomocą serwisu i nie będziemy tej części testować. Chcemy jednak dokonać tego na podstawie `Id`. Nie mamy jeszcze zaimplementowanej takiej funkcjonalności, ale wiemy, że umieścimy ją w tym samym menadżerze, co metodę zwracającą obiekt na podstawie `Id`. Zanim jednak ją zaimplementujemy, zastanówmy się, czego oczekujemy w wyniku jej działania i co będzie nam potrzebne do wykonania testu. Zacznijmy od tej drugiej części.
#### Arrange
1. Musimy utworzyć jakiś obiekt `Item`, którego możliwość usunięcia będziemy sprawdzać. Np.:

```csharp =
Item item = new Item(1, "Apple", 2);
```

2. Na pewno nasza metoda będzie musiała sprawdzić, czy dany obiekt istnieje w bazie danych, zanim spróbuje go z niej usunąć. Operacjami na bazie danych zajmuje się odpowiedni serwis, więc metoda menadżera na pewno będzie musiała skorzystać z odpowiedniej metody serwisu. Ponieważ nie chcemy testować serwisu, ani wykonywać żadnych operacji bezpośrednio na bazie danych, więc taką metodę, będziemy musieli zamokować. Serwis będzie również zajmował się usuwaniem `Item`u z bazy danych, więc musimy zamokować również metodę `RemoveItem` przyjmującą jako parametr dowolny obiekt typu `Item`.

```csharp =
var mock = new Mock<IService<Item>>();
mock.Setup(m => m.GetItemById(1)).Returns(item);
mock.Setup(m => m.RemoveItem(It.IsAny<Item>()));
```

3. Oczywiście potrzebujemy również stworzyć obiekt menadżera.

```csharp =
var manager = new ItemManager(new MenuActionService(), mock.Object);
```

#### Act
Etapem _act_ będzie oczywiście wywołanie metody menadżera. Oczywiście ta metoda jeszcze nie istnieje. Po napisaniu poniższej linii kodu możemy ją od razu utworzyć korzystając z _Quick Actions_.

```csharp =
manager.RemoveItemById(item.Id);
```

#### Assert
Ostatnim etapem testów jest weryfikacja uzyskanych wyników. Jest ona w tym przypadku o tyle utrudniona, że ani metoda menadżera `RemoveItemById`, ani metoda serwisowa `RemoveItem` nie zwracają żadnych danych (są typu `void`). Dzieje się tak, gdyż ich zadanie polega jedynie na usunięciu odpowiedniego obiektu. Co możemy więc sprawdzić? Ponieważ nie testujemy samego procesu usuwania z bazy danych, możemy więc sprawdzić, czy przekazano do usunięcia oczekiwany obiekt. Robimy to przy pomocy metody `Verify` klasy `Mock`.

```csharp =
mock.Verify(m => m.RemoveItem(item));
```

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

Gdybyśmy teraz przeprowadzili test, to oczywiście wyjdzie on negatywny. Jeżeli podczas pisania testu utworzyliśmy naszą metodę przy pomocy _Quick Actions_ to zawiera ona wyłącznie wyrzucenie wyjątku o braku implementacji. Taką właśnie wiadomość uzyskamy w wyniku działania testu. Zaimplementujmy więc naszą metodę i przeprowadźmy test ponownie.

### Metoda
#### Automatycznie wygenerowany kod metody
Jeżeli utworzyliśmy naszą metodę przy pomocy _Quick Actions_ to powinna ona wyglądać następująco:

```csharp =
public void RemoveItemById(int id)
{
    throw new NotImplementedException();
}
```

#### Implementacja funkcjonalności
Nasza metoda powinna pobierać odpowiedni obiekt na podstawie podanego `id`, a następnie usuwać go z bazy danych. Zapiszmy więc to.

```csharp =
public void RemoveItemById(int id)
{
    var item = _itemService.GetItemById(id);
    _itemService.RemoveItem(item);
}
```

Jeżeli teraz ponownie przeprowadzimy test `CanDeleteItemWithProperId`, to powinien on wyjść pozytywnie (oczywiście jeśli mamy zaimplementowana wszystkie inne wymagane klasy i metody). Oznacza to, że do naszego testu napisaliśmy odpowiednią metodę, zachowującą się w sposób, jakiego oczekiwaliśmy.

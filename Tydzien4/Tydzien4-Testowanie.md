# TYDZIEŃ 4 – Testowanie
## Spis treści
### [LEKCJA 1 – Powitanie](#lekcja-1--powitanie-1)
### [LEKCJA 2 – Projekt z testami](#lekcja-2--projekt-z-testami-1)
### [LEKCJA 3 – Twój pierwszy test](#lekcja-3--twój-pierwszy-test-1)
### [LEKCJA 4 – Testy jednostkowe](#lekcja-4--testy-jednostkowe-1)
### [LEKCJA 5 – Moq](#lekcja-5--moq-1)
### [LEKCJA 6 – FluentAssertions](#lekcja-6--fluentassertions-1)
### [LEKCJA 7 – Pokrycie kodu testami](#lekcja-7--pokrycie-kodu-testami-1)
### [LEKCJA 8 – TDD](#lekcja-8--tdd-1)
### [LEKCJA 9 – Testy integracyjne](#lekcja-9--testy-integracyjne-1)
### [LEKCJA 10 – Błędy początkujących](#lekcja-10--błędy-początkujących-1)
### [LEKCJA 11 – Praca domowa](#lekcja-11--praca-domowa-1)

## [LEKCJA 1 – Powitanie](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-1-powitanie/)
W tym tygodniu nauczymy się tworzyć testy jednostkowe. Jest to bardzo ważne, gdyż pozwala na wyłapanie wielu błędów na wczesnym etapie tworzenia oprogramowania. Dodatkowo zmniejsza czas jaki spędzimy nad debugowaniem naszej aplikacji. Pod koniec tygodnia dodamy takie testy do naszej aplikacji.

## [LEKCJA 2 – Projekt z testami](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-2-projekt-z-testami/)
Aby dodać testy do naszej aplikacji musimy umieścić w niej osobny projekt, w którym te testy będą się znajdować.
### Dodanie projektu z testami
Analogicznie jak to robiliśmy wcześniej, w przypadku innych projektów, klikamy prawym przyciskiem myszki w naszą solucję w _Solution Explorer_ i wybieramy _Add_ -> _New Project..._. Możemy zawęzić listę wyszukiwania wybierając język _C#_ i typ _Test_. Dla .NET Core mamy do wyboru kilka opcji. Są to testy przy użyciu bibliotek:
1. _MSTest_
2. _NUnit_
3. _xUnit_
#### _MSTest_
Najstarsza biblioteka .NET do testowania. Stworzona przez Microsoft. Wbudowana w .NET Core. Nie jest jednak specjalnie rozwijana, więc ma pewne niedoskonałości.
#### _NUnit_ i _xUnit_
Biblioteki tworzone przez społeczność .NETową, przez osoby związane z testowaniem. Wybierając z której biblioteki chcemy korzystać przy pisaniu testów, powinniśmy rozważać właśnie którąś z tych bibliotek, gdyż są one najczęściej stosowane w komercyjnych projektach.

Zaczniemy od projektu używającego biblioteki _xUnit_ (_xUnit Test Project (.NET Core)_), gdyż ma on najprostszą składnię, jeśli chodzi o budowę samych testów i prawdopodobnie najlepsze wsparcie, jeśli chodzi o dodatkowe biblioteki. Wybierzmy więc ten typ projektu i nadajmy mu nazwę. Nazwa może być dowolna, ale dla porządku nazwijmy go analogicznie jak poprzednie stworzone przez nas projekty np. _NazwaAplikacji.Tests_ (oczywiście zamiast _NazwaAplikacji_ wpisujemy nazwę, jaką nadaliśmy naszej aplikacji).
### Podstawowa budowa testu
W nowo stworzonym projekcie z testami, mamy domyślnie stworzony plik _UnitTest1.cs_ z pustym testem. Zobaczmy na jego przykładzie podstawową budowę testu.
```csharp =
using System;
using Xunit;

namespace NazwaAplikacji.Tests
{
    public class UnitTest1
    {
        [Fact]
        public void Test1()
        {

        }
    }
}
```
Widzimy więc po pierwsze, że test to po prostu publiczna klasa. Będziemy w niej oczywiście korzystać z biblioteki `Xunit` (`using Xunit;`). Kolejną rzeczą na którą należy zwrócić uwagę jest adnotacja `[Fact]`, umieszczona powyżej pierwszej metody. Jest ona częścią biblioteki `Xunit` i wskazuje, że dana metoda jest metodą testową (powinna być uruchomiona przez _test runner_).
### Uruchamianie testów
Taki przykładowy test możemy sobie oczywiście uruchomić. Możemy to zrobić np. na dwa poniższe sposoby.
#### 1. Uruchomienie wszystkich testów
Po pierwsze w _Solution Explorer_, na naszym projekcie z testami, klikamy prawym przyciskiem myszki i wybieramy opcję _Run Tests_. Spowoduje to wyświetlenie okna eksploratora testów (_Test Explorer_) i uruchomienie wszystkich testów w tym projekcie. W _Test Explorer_ wylistowane są wszystkie testy utworzone w danym projekcie. Po zakończeniu ich wykonywania, przy każdym z testów mamy zaznaczone, czy zakończył się powodzeniem (!["ptaszek" w zielonym kółku](https://learn.microsoft.com/en-us/visualstudio/media/yes-icon.png?view=vs-2022)), ile czasu się wykonywał (_Duration_), ewentualne komunikaty błędów (_Error Message_) itd. Mamy też podsumowanie dla danej grupy testów (_Group Summary_). Po nazwie grupy mamy podaną liczbę testów w grupie (_Tests in group:_), całkowity czas wykonywania testów (_Total Duration:_) oraz wynik testów (ile testów zakończyło się sukcesem - _Passed_, ile niepowodzeniem - _Failed_ itd.)

![przykładowy wygląd eksploratora testów po zakończeniu ich wykonywania](https://learn.microsoft.com/en-us/visualstudio/test/media/vs-2022/test-explorer-17-0.png?view=vs-2022)<br/>
_Przykładowy  wygląd eksploratora testów po zakończeniu wykonywania testowania.<br/>
Źródło: https://learn.microsoft.com/en-us/visualstudio/test/run-unit-tests-with-test-explorer?view=vs-2022._

Oczywiście w przypadku naszego przykładowego testu powinien zakończyć się on sukcesem, gdyż jest pusty (nic się w nim nie wykonuje).

##### Używane w _Test Explorer_ ikony
Na obrazku poniżej przedstawiono ikony używane w _Test Explorer_ (ikony mogą się nieznacznie różnić w zależności np. od wersji Visual Studio). Są używane m.in. do oznaczania wyniku testowania w danej grupie (projekcie, przestrzeni nazw, klasie). Na poniższej ilustracji przedstawiono je w sposób hierarchiczny.
![Hierarchia ikon w Test Explorer](https://learn.microsoft.com/en-us/visualstudio/test/media/testex-hierarchy-icons.png?view=vs-2022)<br/>
_Hierarchia ikon użytych w Test Explorer do oznaczenia grup testowych_
###### 1. _if group contains failed tests_
Jeżeli przynajmniej jeden z testów w grupie zakończył się niepowodzeniem, to grupa jest oznaczona jako zakończona niepowodzeniem (_Failed_). Oznacza się tak również pojedynczy test zakończony niepowodzeniem.
###### 2. _if group contains passed tests and NO failed tests_
Jeżeli przynajmniej jeden z testów w grupie zakończył się sukcesem (_Passed_) i żaden nie zakończył się niepowodzeniem (_Failed_), to grupa jest oznaczona jako _Passed_. Oznacza się tak również pojedynczy test zakończony sukcesem.
###### 3. _if group contains skipped tests and NO failed or passed tests_
Jeżeli grupa nie zawiera żadnych testów zakończonych sukcesem lub porażką, a zawiera przynajmniej jeden pominięty test to jest oznaczana jako pominięta (_Skipped_). Oznacza się tak również pojedynczy pominięty test.
###### 4. _if group contains not run tests and NO failed, passed, or skipped tests_
Jeżeli grupa nie zawiera żadnych testów zakończonych sukcesem, porażką lub pominiętych, a zawiera przynajmniej jeden nie uruchomiony test to jest oznaczana jako nieuruchomiona (_Not run_). Oznacza się tak również pojedynczy nie uruchomiony test.
###### 5. _Passed tests that were not a part of the latest test run_
Test zakończony sukcesem, który nie był jednak przeprowadzony podczas ostatniego wykonywania testów (jego ostatni wynik był pozytywny, ale informacja ta może być już nieaktualna).
###### 6. _Failed tests that were not a part of the latest test run_
Test zakończony niepowodzeniem, który nie był jednak przeprowadzony podczas ostatniego wykonywania testów (jego ostatni wynik był negatywny, ale informacja ta może być już nieaktualna).
###### 7. _Ignored tests that were not a part of the latest test run_
Testy zignorowane podczas tego i poprzedniego przeprowadzania testów.
#### 2. Uruchamianie pojedynczych testów
Jeżeli dany test był już kiedykolwiek przeprowadzony, to wynik jego ostatniego wykonania jest zaznaczony w kodzie. Powyżej metody definiującej dany test znajduje się ikonka (analogiczna do tych używanych w _Test Explorer_). Kliknięcie w tą ikonkę wyświetli nam podsumowanie tego testu. Wybranie w rozwiniętym oknie opcji _Run_ umożliwia ponowne uruchomienie tego właśnie testu, bez konieczności korzystania z eksploratora testów lub wykonywania wszystkich zdefiniowanych przez nas testów.

## [LEKCJA 3 – Twój pierwszy test](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-3-twoj-pierwszy-test/)
W tej lekcji napiszemy nasz pierwszy test jednostkowy. Na razie będzie to najbardziej podstawowy test, nie powiązany w żaden sposób z tworzoną przez nas aplikacją. Oczywiście, w kontekście tworzonego przez nas programu, nie będzie on miał sensu. Zależy nam jednak jedynie, aby nauczyć się w jaki sposób powinny być tworzone testy jednostkowe.
### Struktura testu jednostkowego - **AAA**
Każdy test jednostkowy powinien być zbudowany zgodnie ze strukturą, nazywaną w skrócie **AAA** (ang. _Arrange - Act - Assert_).
#### 1. _Arrange_
Pierwszym krokiem testowym jest tzw. _Arrange_. Będziemy w nim przygotowywać dane, które będziemy testować.
#### 2. _Act_
Drugim krokiem testowym jest wykonanie działania logicznego, które ma zostać sprawdzone w tym teście jednostkowym.
#### 3. _Assert_
W ostatnim kroku testowym musi znajdować się dowód na to, że kod działa w sposób prawidłowy. Służy do tego klasa `Assert`, będąca częścią biblioteki `Xunit`. Każda biblioteka służąca do testowania posiada na ogół klasę `Assert`.
### Przykład
Stwórzmy dla przykładu prosty test sprawdzający, czy dodawanie dwóch liczb działa prawidłowo.
#### 1. _Arrange_
W pierwszym kroku zadeklarujemy dane testowe. W naszym przypadku będę to dwie zmienne przechowujące liczby, które będziemy dodawać. Np.:
```csharp =
int a = 5;
int b = 3;
```
#### 2. _Act_
Chcemy przetestować dodawanie dwóch zmiennych. Wykonajmy więc takie działanie, zapisując wynik w zmiennej `result`.
```csharp =
int result = a + b;
```
#### 3. _Assert_
Chcemy sprawdzić, czy w wyniku działań podjętych w poprzednim kroku, otrzymaliśmy oczekiwany rezultat. Wykorzystamy do tego jedną z metod klasy `Assert`, a mianowicie `Equal` (z ang. równy).
W naszym przykładzie testujemy dodawanie liczb, na przykładzie liczb 5 i 3. Ponieważ wiemy, że
<p align="center">5 + 3 = 8,</p>

więc oczekujemy, że wynik, który uzyskaliśmy na skutek działań podjętych w kroku _Act_, i zapisaliśmy w zmiennej `result`, wyniesie właśnie 8. Zapiszmy to więc

```csharp =
Assert.Equal(8, result);
```

W naszym przykładzie, chcieliśmy sprawdzić czy uzyskany wynik jest równy konkretnej liczbie. Oczywiście biblioteka `Assert` posiada wiele metod i umożliwia nie tylko porównywanie.
#### Cały kod przykładowego testu
Zapiszmy jeszcze łącznie cały kod testu.
```csharp =
using System;
using Xunit;

namespace NazwaAplikacji.Tests
{
    public class UnitTest1
    {
        [Fact]
        public void Test1()
        {
            // Arrange
            int a = 5;
            int b = 3;

            // Act
            int result = a + b;

            // Assert
            Assert.Equal(8, result);
        }
    }
}
```
Po utworzeniu testu możemy go uruchomić. Oczywiście z godnie z oczekiwaniami powinien zakończyć się on sukcesem.
### Przykładowe metody klasy `Assert`
Poza metodą `Equal`, klasa `Assert` posiada wiele innych metod. Ciekawymi przykładami są metody:
#### `Throws`
Służy ona do sprawdzenia, czy w określonej sytuacji nasz program zwrócił wyjątek. Sprawdza, czy aplikacja w odpowiedni sposób obsługuje błąd na który może natrafić.
#### `NotNull`
Służy do sprawdzenia, czy jakaś wartość nie jest nullem.
#### `NotEmpty`
Służy do sprawdzenia, czy dany `string` (lub kolekcja) nie jest pusty(a).
#### `StartsWith`
Służy do sprawdzenia, czy dany `string` (lub kolekcja) rozpoczyna się danym elementem.
#### `Contains`
Służy do sprawdzenia, czy jakiś `string` (lub kolekcja) zawiera jakiś element.
#### `DoesNotContain`
Służy do sprawdzenia, czy jakiś `string` (lub kolekcja) nie zawiera jakiegoś elementu.
### Pisanie testów
Pisząc testy chcemy sprawdzić nie tylko nasz idealny wariant, który z założenia powinien być poprawnie wykonany. Powinniśmy sprawdzić wszystkie możliwości dotyczące naszego kodu. Im więcej wariantów sprawdzimy, im więcej testów napiszemy, tym mniej będziemy później musieli debugować nasz kod. Kiedy większość sytuacji zostanie przewidziana i sprawdzona w testach, to zamiast debugować kod, będziemy mogli uruchomić testy. Jeżeli któryś z nich wypadnie negatywnie, to będziemy wiedzieć w której części kodu znajduje się błąd. Nie będziemy więc musieli przechodzić debugerem krok po kroku przez cały kod, a jedynie najwyżej przez fragment w którym testy wykazały błąd.

## [LEKCJA 4 – Testy jednostkowe](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-4-testy-jednostkowe/)
Testy jednostkowe, standardowo, mają sprawdzić, czy najmniejsza możliwa jednostka naszej aplikacji wykonywana jest w sposób prawidłowy i taki, jaki jesteśmy w stanie przewidzieć.
### Co testujemy
Najmniejszą możliwą jednostką, którą powinniśmy testować, jest metoda danej klasy. Większość tworzonych przez nas metod powinna mieć pokrycie w testach. Dokładniej o tym co i w jakiej ilości powinniśmy testować dowiemy się w kolejnych lekcjach.
### Kto pisze testy jednostkowe
W odróżnieniu od innych rodzajów testów, takich jak testy integracyjne, testy End-To-End, testy regresji itd., za testy jednostkowe zawsze powinien być odpowiedzialny programista piszący kod. Dzieje się tak, ponieważ testy jednostkowe tworzone są po to, aby ułatwić pracę programiście. Testerzy są natomiast odpowiedzialni za tworzenie testów wyższego poziomu. Mają za zadanie sprawdzić aplikację, przed jej wdrożeniem na serwer produkcyjny. Muszą sprawdzić czy nowe funkcjonalności nie niszczą starych funkcjonalności, czy połączenia pomiędzy funkcjonalnościami i aplikacjami działają prawidłowo itd. Sprawdzają aplikację jako całość oraz połączenia między różnymi aplikacjami, a nie wszystkie metody. Testy jednostkowe są natomiast sprawdzeniem dla samego programisty, czy jego kod wykonuje się w sposób jaki tego oczekuje. Mówi się, że programiści dzielą się na tych którzy już piszą testy i na tych, którzy będą pisać testy, albo na tych którzy piszą testy i na tych, którzy debugują.

## [LEKCJA 5 – Moq](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-5-moq/)
Biblioteka służąca do stworzenia pewnej ilości danych, która będzie nam potrzebna do sprawdzenia funkcjonalności. Zazwyczaj używamy jej do zasymulowania działania pewnych metod, których nie chcemy sprawdzać w danym teście lub do zasymulowania danych z bazy danych. Zazwyczaj podczas testowania nie chcemy opierać się na danych z bazy danych, gdyż nie jesteśmy w stanie przewidzieć co się tam w danej chwili znajduje.
### Instalacja biblioteki _Moq_
Zanim będziemy mogli wykorzystać tą bibliotekę, musimy ją oczywiście zainstalować. Otwieramy _NuGet Manager_. W tym celu wybieramy _Tools_ -> _NuGet Package Manager_ -> _Manage NuGet Packages for Solution..._ i  w zakładce _Browse_ wyszukujemy _Moq_ (na przykład poprzez wpisanie tej nazwy w polu wyszukiwarki - Ctrl+L). Następnie wybieramy i instalujemy naszą bibliotekę.
### Wybór metody do przetestowania
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

### Test z użyciem biblioteki _Moq_
#### Arrange
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

#### Act
Mamy już przygotowane wszystkie potrzebne nam dane. Stwórzmy więc logikę naszego testu.

```csharp =
var returnedItem = manager.GetItemById(item.Id);
```

#### Assert
Na koniec sprawdźmy uzyskany wynik. Spodziewamy się, że otrzymaliśmy taki sam obiekt `Item`, jak ten który na początku stworzyliśmy i którego `Id` przekazaliśmy jako parametr testowanej funkcji. Sprawdźmy więc, czy oba obiekty są sobie równe.

```csharp =
Assert.Equal(item, returnedItem);
```

#### Cały test
Kod całego testu będzie więc wyglądał następująco:

```csharp =
using NazwaAplikacji.Domain.Entity;
using NazwaAplikacji.App.Abstract;
using NazwaAplikacji.App.Concrete;
using NazwaAplikacji.App.Managers;
using Moq;
using Xunit;

namespace NazwaAplikacji.Tests
{
    public class UnitTest1
    {
        [Fact]
        public void Test1()
        {
            // Arrange
            Item item = new Item(1, "Apple", 2);
            var mock = new Mock<IService<Item>>();
            mock.Setup(s => s.GetItemById(1)).Returns(item);
            var manager = new ItemManager(new MenuActionService(), mock.Object);

            // Act
            var returnedItem = manager.GetItemById(item.Id);
            
            // Assert
            Assert.Equal(item, returnedItem);
        }
    }
}
```

Jeżeli w naszej aplikacji mamy zdefiniowane wszystkie potrzebne klasy, to możemy teraz uruchomić test. Powinien zakończyć się on pozytywnie.

Oczywiście jest to najprostszy test, służący tylko przedstawieniu biblioteki _Moq_. W prawdziwym projekcie prawdopodobnie będziemy musieli zasymulować więcej obiektów, metod. Będziemy też zapewne sprawdzać więcej uzyskanych wartości. Biblioteki _Moq_ będziemy najczęściej używać do zasymulowania danych uzyskiwanych bezpośrednio z bazy danych. Dzięki temu niezależnie od niej będziemy mogli sprawdzić logikę biznesową naszej aplikacji.

## [LEKCJA 6 – FluentAssertions](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-6-fluentassertions/)
Biblioteka rozszerzająca możliwości kroku _Assert_ testów[^*].
### Instalacja biblioteki _FluentAssertions_
Podobnie jak w poprzedniej lekcji, zanim będziemy mogli wykorzystać tą bibliotekę, musimy ją oczywiście zainstalować. Ponownie otwieramy _NuGet Manager_. W tym celu wybieramy _Tools_ -> _NuGet Package Manager_ -> _Manage NuGet Packages for Solution..._ i  w zakładce _Browse_ wyszukujemy _FluentAssertions_ (na przykład poprzez wpisanie tej nazwy w polu wyszukiwarki - Ctrl+L). Następnie wybieramy i instalujemy naszą bibliotekę.
### Test z wykorzystaniem biblioteki _FluentAssertions_
Wykorzystajmy test utworzony w poprzedniej lekcji.

```csharp =
using NazwaAplikacji.Domain.Entity;
using NazwaAplikacji.App.Abstract;
using NazwaAplikacji.App.Concrete;
using NazwaAplikacji.App.Managers;
using Moq;
using Xunit;

namespace NazwaAplikacji.Tests
{
    public class UnitTest1
    {
        [Fact]
        public void Test1()
        {
            // Arrange
            Item item = new Item(1, "Apple", 2);
            var mock = new Mock<IService<Item>>();
            mock.Setup(s => s.GetItemById(1)).Returns(item);
            var manager = new ItemManager(new MenuActionService(), mock.Object);

            // Act
            var returnedItem = manager.GetItemById(item.Id);
            
            // Assert
            Assert.Equal(item, returnedItem);
        }
    }
}
```

Jak już wspomnieliśmy bibliotekę _FluentAssertions_ wykorzystujemy w kroku _Assert_. Będziemy więc modyfikować tylko ten krok. Posiada ona bardziej intuicyjną składnię, dzięki czemu łatwiej jest pisać testy i łatwiej sprawdzić wszystkie przypadki testowe.

Zastanówmy się więc jeszcze raz co chcieliśmy sprawdzić powyższym testem. Upewnialiśmy się, że metoda `GetItemById` menadżera zwraca poprawną, oczekiwaną przez nas wartość. Co to jednak znaczy?
#### 1. Typ zwracanego obiektu
Po pierwsze spodziewamy się, że otrzymamy obiekt typu `Item`. Czyli obiekt `returnedItem` powinien być typu `Item`. To zdanie zapisane przy pomocy _FluentAssertions_ będzie wyglądać następująco:
```csharp =
returnedItem.Should().BeOfType(typeof(Item));
```

#### 2. Wartość nie `null`
Po drugie oczekujemy, że otrzymamy rzeczywiście jakiś konkretny obiekt. Czyli obiekt `returnedItem` nie powinien być `null`. To zdanie zapisane przy pomocy _FluentAssertions_ będzie wyglądać następująco:
```csharp =
returnedItem.Should().NotBeNull();
```

#### 3. Zwracamy obiekt utworzony na początku
I w końcu spodziewamy się otrzymać ten sam obiekt który na początku utworzyliśmy. Czyli obiekt `returnedItem` powinien być taki sam jak obiekt `item`. To zdanie zapisane przy pomocy _FluentAssertions_ będzie wyglądać następująco:
```csharp =
returnedItem.Should().BeSameAs(item);
```

#### 4. Ulepszony test
Jak widać biblioteka _FluentAssertions_ pozwala nam pisać sprawdzane wartości prawie dokładnie tak jak byśmy mówili to w języku naturalnym. Dzięki temu testy są łatwiejsze w pisaniu i czytelniejsze. Z tego względu warto z niej korzystać.

Poprawmy więc nasz test:

```csharp =
using NazwaAplikacji.Domain.Entity;
using NazwaAplikacji.App.Abstract;
using NazwaAplikacji.App.Concrete;
using NazwaAplikacji.App.Managers;
using Moq;
using Xunit;
using FluentAssertions;

namespace NazwaAplikacji.Tests
{
    public class UnitTest1
    {
        [Fact]
        public void Test1()
        {
            // Arrange
            Item item = new Item(1, "Apple", 2);
            var mock = new Mock<IService<Item>>();
            mock.Setup(s => s.GetItemById(1)).Returns(item);
            var manager = new ItemManager(new MenuActionService(), mock.Object);
            
            // Act
            var returnedItem = manager.GetItemById(item.Id);
            
            // Assert
            returnedItem.Should().BeOfType(typeof(Item));
            returnedItem.Should().NotBeNull();
            returnedItem.Should().BeSameAs(item);
        }
    }
}
```

[^*]: Jeśli ktoś woli może również korzystać z podobnej biblioteki - _Shouldly_.

## [LEKCJA 7 – Pokrycie kodu testami](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-7-pokrycie-kodu-testami/)
### Co powinniśmy testować?
Po tym, jak mówiliśmy jakie ważne są testy, chciałoby się powiedzieć, że wszystko. Nie jest to jednak prawda. Zarówno brak testów, jak i 100% pokrycie kodu testami nie są dobre dla naszej aplikacji. Tak na prawdę powinniśmy przetestować tylko konkretne rzeczy.
<table>
<tr>
<th style="color: green;">Testujemy:</th>
<th style="color: red;">Nie testujemy (zakładamy, że są poprawne, dobrze napisane):</th>
</tr>
<tr>
<td style="color: green;">
<img src="https://www.iconsdb.com/icons/preview/green/checkmark-xxl.png" style="height:1em;" title="-"/><b>Logikę biznesową</b> naszej aplikacji
<ul>
<li>Wszelkie metody, w których manipulujemy obiektami, przekształcamy je według wymagań użytkowników (klienta)</li>
</ul><br/>
<img src="https://www.iconsdb.com/icons/preview/green/checkmark-xxl.png" style="height:1em;" title="-"/>Warstwę aplikacji związaną z <b>interakcją z użytkownikiem</b><br/>
</td>
<td style="color: red;">
  <img src="https://www.citypng.com/public/uploads/preview/transparent-handdrawn-doodle-red-x-close-icon-31631916723qjvbg7xy7u.png" style="height: 1em;" title="-"/><b>Konstruktorów</b> - gdyż:
  <ul>
  <li>Nie powinny one posiadać żadnej logiki.</li>
  <li>Najczęściej znajdują się w nich tylko proste przypisania parametrów do obiektów w danej klasie, więc nie ma tam czego testować.</li>
  <li>Jeżeli już chcielibyśmy przetestować taką metodę i istniej w niej błąd, to istnieje duże prawdopodobieństwo, że powielimy go w teście. Wówczas będziemy mieć nie tylko błędny konstruktor, ale również test.</li>
  <li>Aby zmniejszyć możliwość pomyłki, możemy do ich tworzenia używać <i>snippetów</i> (piszemy <i>ctor</i> i klikamy dwa razy <b>Tab</b>, co tworzy nam podstawowy, bezparametrowy konstruktor), a jeżeli używamy <i>Resharpera</i> to również szablonów.</li>
  </ul><br/>
  <img src="https://www.citypng.com/public/uploads/preview/transparent-handdrawn-doodle-red-x-close-icon-31631916723qjvbg7xy7u.png" style="height:1em; color: red;" title="-"/>Prostych <b>get</b>erów, <b>set</b>erów <ul><li>Z podobnych powodów co konstruktorów</li></ul><br/>
  <img src="https://www.citypng.com/public/uploads/preview/transparent-handdrawn-doodle-red-x-close-icon-31631916723qjvbg7xy7u.png" style="height:1em; display: inline;" title="-"><label style="color: red;">Warstwy aplikacji wygenerowanych przez <b>zewnętrzne biblioteki</b>
  <ul>
  <li>Np. klasy stworzone przez bibliotekę <i>Entity Framework</i> (będziemy z niej korzystać w dalszej części kursu, do połączenia z bazą danych), na podstawie bazy danych.</li>
  <li>Zakładamy, że biblioteki zewnętrzne działają prawidłowo i nie testujemy ich działania. Dla tego ważne jest, aby korzystać z zaufanych bibliotek, przetestowanych przez społeczność programistów.</li>
  </ul></label><br/>
  <img src="https://www.citypng.com/public/uploads/preview/transparent-handdrawn-doodle-red-x-close-icon-31631916723qjvbg7xy7u.png" style="height:1em; color: red;" title="-"/>Warstwy aplikacji związanej bezpośrednio z operacjami na bazie danych (klas <b>CRUD</b>-owych).
  </td>
</tr>
</table>

## [LEKCJA 8 – TDD](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-8-tdd/)
TDD (ang. _Test Driven Development_) - metodologia w tworzeniu oprogramowania, polegająca na tym, że tworzenie oprogramowania rozpoczyna się od napisania testów.
### Podejście klasyczne (najpierw kod, potem testy)
Do tej pory tworzyliśmy testy na podstawie metod.
1. Na podstawie wymagań funkcjonalnych piszemy metodę.
2. Wiemy co chcieliśmy, aby ta metoda robiła i zwracała.
3. Piszemy test sprawdzający, czy w wyniku działania metody, rzeczywiście otrzymaliśmy spodziewane rezultaty.
4. Przeprowadzamy test.
### **TDD** (najpierw testy, potem kod)
Zgodnie z metodologią **TDD** postępujemy na odwrót.
1. Na podstawie wymagań funkcjonalnych określamy jakie wyniki chcemy otrzymać.
2. Na tej podstawie piszemy testy.
3. Implementujemy metody wykonujące odpowiednie przekształcenia.
4. Przeprowadzamy testy i sprawdzamy, czy przekształcenia dały oczekiwany rezultat.
### Przykład
#### Test
Przypomnijmy sobie nasz ostatni test. Sprawdzaliśmy w nim, działanie funkcji menadżera, zwracającej obiekt `Item` na podstawie `Id`. Załóżmy teraz, że nasza aplikacja musi mieć również funkcjonalność usuwającą `Item` z bazy danych, na podstawie `Id`. Samo usuwanie obiektu z bazy danych będzie się odbywać oczywiście za pomocą serwisu i nie będziemy tej części testować. Chcemy jednak dokonać tego na podstawie `Id`. Nie mamy jeszcze zaimplementowanej takiej funkcjonalności, ale wiemy, że umieścimy ją w tym samym menadżerze, co metodę zwracającą obiekt na podstawie `Id`. Zanim jednak ją zaimplementujemy, zastanówmy się, czego oczekujemy w wyniku jej działania i co będzie nam potrzebne do wykonania testu. Zacznijmy od tej drugiej części.
##### Arrange
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

##### Act
Etapem _act_ będzie wywołanie metody menadżera. Oczywiście ta metoda jeszcze nie istnieje. Po napisaniu poniższej linii kodu możemy ją od razu utworzyć korzystając z _Quick Actions_.

```csharp =
manager.RemoveItemById(item.Id);
```

##### Assert
Ostatnim etapem testów jest weryfikacja uzyskanych wyników. Jest ona w tym przypadku o tyle utrudniona, że ani metoda menadżera `RemoveItemById`, ani metoda serwisowa `RemoveItem` nie zwracają żadnych danych (są typu `void`). Dzieje się tak, gdyż ich zadanie polega jedynie na usunięciu odpowiedniego obiektu. Co możemy więc sprawdzić? Ponieważ nie testujemy samego procesu usuwania z bazy danych, możemy więc sprawdzić, czy przekazano do usunięcia oczekiwany obiekt. Robimy to przy pomocy metody `Verify` klasy `Mock`.

```csharp =
mock.Verify(m => m.RemoveItem(item));
```

##### Cały test

```csharp =
using NazwaAplikacji.Domain.Entity;
using NazwaAplikacji.App.Abstract;
using NazwaAplikacji.App.Concrete;
using NazwaAplikacji.App.Managers;
using Moq;
using Xunit;

namespace NazwaAplikacji.Tests
{
    public class UnitTest1
    {
        [Fact]
        public void CanDeleteItemWithProperId()
        {
            // Arrange
            Item item = new Item(1, "Apple", 2);
            var mock = new Mock<IService<Item>>();
            mock.Setup(s => s.GetItemById(1)).Returns(item);
            mock.Setup(m => m.RemoveItem(It.IsAny<Item>()));
            var manager = new ItemManager(new MenuActionService(), mock.Object);
            
            // Act
            manager.RemoveItemById(item.Id);
            
            // Assert
            mock.Verify(m => m.RemoveItem(item));
        }
    }
}
```

Gdybyśmy teraz przeprowadzili test, to oczywiście wyjdzie on negatywny. Jeżeli podczas pisania testu utworzyliśmy naszą metodę przy pomocy _Quick Actions_ to zawiera ona wyłącznie wyrzucenie wyjątku o braku implementacji. Taką właśnie wiadomość uzyskamy w wyniku działania testu. Zaimplementujmy więc naszą metodę i przeprowadźmy test ponownie.

#### Metoda
##### Automatycznie wygenerowany kod metody
Jeżeli utworzyliśmy naszą metodę przy pomocy _Quick Actions_ to powinna ona wyglądać następująco:

```csharp =
public void RemoveItemById(int id)
{
    throw new NotImplementedException();
}
```

##### Implementacja funkcjonalności
Nasza metoda powinna pobierać odpowiedni obiekt na podstawie podanego `id`, a następnie usuwać go z bazy danych. Zapiszmy więc to.

```csharp =
public void RemoveItemById(int id)
{
    var item = _itemService.GetItemById(id);
    _itemService.RemoveItem(item);
}
```

Jeżeli teraz ponownie przeprowadzimy test `CanDeleteItemWithProperId`, to powinien on wyjść pozytywnie (oczywiście jeśli mamy zaimplementowana wszystkie inne wymagane klasy i metody). Oznacza to, że do naszego testu napisaliśmy odpowiednią metodę, zachowującą się w sposób, jakiego oczekiwaliśmy.

## [LEKCJA 9 – Testy integracyjne](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-9-testy-integracyjne/)
Pomimo, że nie są to testy jednostkowe, to testy tego rodzaju również często są pisane przez programistów.

**Testy integracyjne** - służą do testowania integracji pomiędzy warstwami naszej aplikacji np. menadżerami i serwisami. Piszemy je podobnie, jak testy jednostkowe, tylko jeżeli np. sprawdzamy integrację między menadżerami i serwisami, to nie mokujemy już serwisów, ale działami na rzeczywistych obiektach. Ponieważ serwisy przeprowadzają operacje na bazie danych, to używając rzeczywistych obiektów serwisowych, podczas wykonywania testów, wprowadzimy rzeczywiste zmiany w bazie danych. Dlatego testy integracyjne poza etapami _Arrange_-_Act_-_Assert_ posiadają na końcu jeszcze jeden etap, _Clean_, w którym będziemy "sprzątać" po teście, czyli przywracać bazę danych do stanu przed testem.

### Przykład
Stwórzmy przykładowy test integracyjny na podstawie testu jednostkowego z poprzedniej lekcji.
#### Test jednostkowy

```csharp =
using NazwaAplikacji.Domain.Entity;
using NazwaAplikacji.App.Abstract;
using NazwaAplikacji.App.Concrete;
using NazwaAplikacji.App.Managers;
using Moq;
using Xunit;

namespace NazwaAplikacji.Tests
{
    public class UnitTest1
    {
        [Fact]
        public void CanDeleteItemWithProperId()
        {
            // Arrange
            Item item = new Item(1, "Apple", 2);
            var mock = new Mock<IService<Item>>();
            mock.Setup(s => s.GetItemById(1)).Returns(item);
            mock.Setup(m => m.RemoveItem(It.IsAny<Item>()));
            var manager = new ItemManager(new MenuActionService(), mock.Object);
            
            // Act
            manager.RemoveItemById(item.Id);
            
            // Assert
            mock.Verify(m => m.RemoveItem(item));
        }
    }
}
```

#### Test integracyjny
Powyżej, dla przypomnienia, znajduje się test jednostkowy, który stworzyliśmy w poprzedniej lekcji. Jak już wspominaliśmy w teście integracyjnym nie sprawdzamy działania poszczególnych funkcji, a współdziałanie między poszczególnymi warstwami aplikacji.
##### _Arrange_
Nie będziemy więc teraz mokować naszego serwisu, a stworzymy jego faktyczny obiekt. Wypełnimy też go od razu jakąś daną, czyli naszym obiektem `item`.

```csharp =
Item item = new Item(1, "Apple", 2);
IService<Item> itemService = newService();
itemService.AddItem(item);
var manager = new ItemManager(new MenuActionService(), itemService);
```

##### _Act_
Krok _act_ pozostawiamy bez zmian.
##### _Assert_
Ponieważ nie mamy już obiektu `Mock`, krok _assert_ musimy zmodyfikować. Ponieważ w teście usuwaliśmy obiekt `item` z serwisu (bazy danych), to możemy sprawdzić, czy taki obiekt nadal w nim istnieje. Czyli oczekujemy, że próba odnalezienia obiektu o `Id` `item.Id`, da nam wynik `null`.

```csharp =
itemService.GetItemById(item.Id).Should().BeNull();
```

Możemy to również zapisać bardziej bezpośrednio, sprawdzając, czy na liście `Item`ów w serwisie istnieje element o `id` `item.Id`.

```csharp =
itemService.Items.FirstOrDefault(p => p.Id == item.Id).Should().BeNull();
```

##### _Clean_
W naszym przypadku, ponieważ w teście usuwaliśmy element, który na początku testu dodaliśmy, a w dodatku nie działamy jeszcze na bazie danych, więc nie mamy nic do czyszczenia.

##### Cały test

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
            // Arrange
            Item item = new Item(1, "Apple", 2);
            IService<Item> itemService = newService();
            itemService.AddItem(item);
            var manager = new ItemManager(new MenuActionService(), itemService);
            
            // Act
            manager.RemoveItemById(item.Id);
            
            // Assert
            itemService.Items.FirstOrDefault(p => p.Id == item.Id).Should().BeNull();   // itemService.GetItemById(item.Id).Should().BeNull();

            // Clean
        }
    }
}
```

Jeżeli mamy zaimplementowane wszystkie wymagane elementy aplikacji, to wykonanie takiego testu powinno zakończyć się pozytywnie.

### _Clean_
Jak już wspominaliśmy, kiedy podczas wykonywania testu wprowadzamy jakieś zmiany w bazie danych, to na jego zakończenie, musimy wszystkie te zmiany cofnąć. W naszym przykładzie nie mieliśmy nic do czyszczenia, gdyż nie pracujemy jeszcze na bazie danych. Gdybyśmy jednak taki test wykonywali na serwisie dodającym i usuwającym obiekty z bazy danych, to musimy uważać. Na początku testu dodaliśmy do bazy danych obiekt, który następnie usunęliśmy w kroku _act_. Mogłoby się więc wydawać, że wszystko powinno być w porządku. Jeżeli jednak nasza baza danych korzystna z mechanizmów auto-inkrementacji np. numerów id, to może się okazać, że zwiększyliśmy odpowiedzialny za nią znacznik i kolejny obiekt jaki dodamy do bazy danych będzie miał id o jedno większe od obiektu, który właśnie usunęliśmy, a nie od ostatniego obiektu znajdującego się w bazie danych (obiektu, który był ostatni, przed wykonaniem testu). Wówczas testując aplikacją będziemy tworzyć "dziury" w numerach id (lub innych wartościach). Jeżeli tabela na której pracujemy podczas testu posiada jakieś mechanizmy auto-inkrementacji, to musimy pamiętać, aby po zakończeniu testu, w etapie czyszczenia (_Clean_) przywrócić znacznikom odpowiednie wartości. Jest to oczywiście możliwe. Dlatego przed przestąpieniem do pisania testów integracyjnych należy zapoznać się z takimi mechanizmami, tak aby testowanie aplikacji, nie przynosiło więcej szkody niż pożytku.

## [LEKCJA 10 – Błędy początkujących](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-10-bledy-poczatkujacych/)
Jeśli chodzi o testowanie, to możliwych błędów jest całkiem sporo. Pierwszy podstawowy to

### Brak testów
Wielu programistów w ogóle nie testuje swoich aplikacji, co jest pierwszym krokiem do ich klęski. Pamiętajmy, że każdy program powinien być odpowiednio przetestowany. Dzięki temu mamy pewność, że działa ona w taki sposób, jaki to zaplanowaliśmy. Dodatkowo, w przypadkach spornych (np. z administratorami sieci), pozwala nam to udowodnić, że nasz kod działa prawidłowo. W przypadku gdy natomiast coś cię wysypie, ułatwia to i przyśpiesza znalezienie źródła błędu. Pozwala nam również sprawdzić, że każda kolejna wersja aplikacji, działa w ten sam sposób.

### Testowanie nieodpowiednich elementów aplikacji
Po drugie, często testujemy nieodpowiednie elementy aplikacji.

**Nie powinniśmy testować:**
* kontrolerów (w przypadku programowania webowego)
* zewnętrznych bibliotek (zakładamy, że działają one poprawnie).

**Powinniśmy testować:**
* logikę biznesową aplikacji.

Skupmy się na testowaniu tego, nad czym mamy panowanie, co sami zaimplementowaliśmy i powinniśmy sprawdzić, czyli na logice biznesowej naszej aplikacji. W naszym przypadku będą to menadżery i być może serwisy. W tym momencie, prawdopodobnie nie używamy w nich jeszcze bibliotek zewnętrznych, więc warto je przetestować.

## [LEKCJA 11 – Praca domowa](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-11-praca-domowa/)
Pracą domową na ten tydzień jest dodanie do pisanej przez nas przez ostatnie tygodnie aplikacji konsolowej projektu z testami i napisanie testów do wszystkich, stworzonych w niej do tej pory, funkcjonalności. Ponieważ nasza aplikacja może wydawać się już dość duża, potraktujmy to jako dobre ćwiczenie na przyszłość, kiedy nasze aplikacje będą o wiele bardziej rozbudowane. Przećwiczmy więc tworzenie testów do kodu już istniejącego. Kiedy w kolejnych tygodniach będziemy tworzyć kolejną aplikację (aplikację internetową), to możemy pokusić się o stosowanie metody TDD (_Test Driven Development_). Wówczas tworzenie naszej aplikacji rozpoczniemy od napisania testów.


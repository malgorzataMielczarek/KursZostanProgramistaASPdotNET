# [LEKCJA 2 – Projekt z testami](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-2-projekt-z-testami/)
Aby dodać testy do naszej aplikacji musimy umieścić w niej osobny projekt, w którym te testy będą się znajdować.
## Dodanie projektu z testami
Analogicznie jak to robiliśmy wcześniej, w przypadku innych projektów, klikamy prawym przyciskiem myszki w naszą solucję w _Solution Explorer_ i wybieramy _Add_ -> _New Project..._. Możemy zawęzić listę wyszukiwania wybierając język _C#_ i typ _Test_. Dla .NET Core mamy do wyboru kilka opcji. Są to testy przy użyciu bibliotek:
1. _MSTest_
2. _NUnit_
3. _xUnit_
### _MSTest_
Najstarsza biblioteka .NET do testowania. Stworzona przez Microsoft. Wbudowana w .NET Core. Nie jest jednak specjalnie rozwijana, więc ma pewne niedoskonałości.
### _NUnit_ i _xUnit_
Biblioteki tworzone przez społeczność .NETową, przez osoby związane z testowaniem. Wybierając z której biblioteki chcemy korzystać przy pisaniu testów, powinniśmy rozważać właśnie którąś z tych bibliotek, gdyż są one najczęściej stosowane w komercyjnych projektach.

Zaczniemy od projektu używającego biblioteki _xUnit_ (_xUnit Test Project (.NET Core)_), gdyż ma on najprostszą składnię, jeśli chodzi o budowę samych testów i prawdopodobnie najlepsze wsparcie, jeśli chodzi o dodatkowe biblioteki. Wybierzmy więc ten typ projektu i nadajmy mu nazwę. Nazwa może być dowolna, ale dla porządku nazwijmy go analogicznie jak poprzednie stworzone przez nas projekty np. _NazwaAplikacji.Tests_ (oczywiście zamiast _NazwaAplikacji_ wpisujemy nazwę, jaką nadaliśmy naszej aplikacji).
## Podstawowa budowa testu
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
## Uruchamianie testów
Taki przykładowy test możemy sobie oczywiście uruchomić. Możemy to zrobić np. na dwa poniższe sposoby.
### 1. Uruchomienie wszystkich testów
Po pierwsze w _Solution Explorer_, na naszym projekcie z testami, klikamy prawym przyciskiem myszki i wybieramy opcję _Run Tests_. Spowoduje to wyświetlenie okna eksploratora testów (_Test Explorer_) i uruchomienie wszystkich testów w tym projekcie. _Test Explorer_ wylistowane są wszystkie testy utworzone w danym projekcie. Po zakończeniu ich wykonywania, przy każdym z testów mamy zaznaczone, czy zakończył się powodzeniem (!["ptaszek" w zielonym kółku](https://learn.microsoft.com/en-us/visualstudio/media/yes-icon.png?view=vs-2022)), ile czasu się wykonywał (_Duration_), ewentualne komunikaty błędów (_Error Message_) itd. Mamy też podsumowanie dla danej grupy testów (_Group Summary_). Po nazwie grupy mamy podaną liczbę testów w grupie (_Tests in group:_), całkowity czas wykonywania testów (_Total Duration:_) oraz wynik testów (ile testów zakończyło się sukcesem - _Passed_, ile niepowodzeniem - _Failed_ itd.)

![przykładowy wygląd eksploratora testów po zakończeniu ich wykonywania](https://learn.microsoft.com/en-us/visualstudio/test/media/vs-2022/test-explorer-17-0.png?view=vs-2022)<br/>
_Przykładowy  wygląd eksploratora testów po zakończeniu wykonywania testowania.<br/>
Źródło: https://learn.microsoft.com/en-us/visualstudio/test/run-unit-tests-with-test-explorer?view=vs-2022._

Oczywiście w przypadku naszego przykładowego testu powinien zakończyć się on sukcesem, gdyż jest pusty (nic się w nim nie wykonuje).

#### Używane w _Test Explorer_ ikony
Na obrazku poniżej przedstawiono ikony używane w _Test Explorer_ (ikony mogą się nieznacznie różnić w zależności np. od wersji Visual Studio). Są używane m.in. do oznaczania wyniku testowania w danej grupie (projekcie, przestrzeni nazw, klasie). Na poniższej ilustracji przedstawiono je w sposób hierarchiczny.
![Hierarchia ikon w Test Explorer](https://learn.microsoft.com/en-us/visualstudio/test/media/testex-hierarchy-icons.png?view=vs-2022)<br/>
_Hierarchia ikon użytych w Test Explorer do oznaczenia grup testowych_
##### 1. _if group contains failed tests_
Jeżeli przynajmniej jeden z testów w grupie zakończył się niepowodzeniem, to grupa jest oznaczona jako zakończona niepowodzeniem (_Failed_). Oznacza się tak również pojedynczy test zakończony niepowodzeniem.
##### 2. _if group contains passed tests and NO failed tests_
Jeżeli przynajmniej jeden z testów w grupie zakończył się sukcesem (_Passed_) i żaden nie zakończył się niepowodzeniem (_Failed_), to grupa jest oznaczona jako _Passed_. Oznacza się tak również pojedynczy test zakończony sukcesem.
##### 3. _if group contains skipped tests and NO failed or passed tests_
Jeżeli grupa nie zawiera żadnych testów zakończonych sukcesem lub porażką, a zawiera przynajmniej jeden pominięty test to jest oznaczana jako pominięta (_Skipped_). Oznacza się tak również pojedynczy pominięty test.
##### 4. _if group contains not run tests and NO failed, passed, or skipped tests_
Jeżeli grupa nie zawiera żadnych testów zakończonych sukcesem, porażką lub pominiętych, a zawiera przynajmniej jeden nie uruchomiony test to jest oznaczana jako nieuruchomiona (_Not run_). Oznacza się tak również pojedynczy nie uruchomiony test.
##### 5. _Passed tests that were not a part of the latest test run_
Test zakończony sukcesem, który nie był jednak przeprowadzony podczas ostatniego wykonywania testów (jego ostatni wynik był pozytywny, ale informacja ta może być już nieaktualna).
##### 6. _Failed tests that were not a part of the latest test run_
Test zakończony niepowodzeniem, który nie był jednak przeprowadzony podczas ostatniego wykonywania testów (jego ostatni wynik był negatywny, ale informacja ta może być już nieaktualna).
##### 7. _Ignored tests that were not a part of the latest test run_
Testy zignorowane podczas tego i poprzedniego przeprowadzania testów.
### 2. Uruchamianie pojedynczych testów
Jeżeli dany test był już kiedykolwiek przeprowadzony, to wynik jego ostatniego wykonania jest zaznaczony w kodzie. Powyżej metody definiującej dany test znajduje się ikonka (analogiczna do tych używanych w _Test Explorer_). Kliknięcie w tą ikonkę wyświetli nam podsumowanie tego testu. Wybranie w rozwiniętym oknie opcji _Run_ umożliwia ponowne uruchomienie tego właśnie testu, bez konieczności korzystania z eksploratora testów lub wykonywania wszystkich zdefiniowanych przez nas testów.

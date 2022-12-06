# [LEKCJA 18 – Piszemy aplikację](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-18-piszemy-aplikacje/)
W tej lekcji możemy zobaczyć początek implementacji przykładowej aplikacji. Kod programu możemy znaleźć [tutaj](https://github.com/szkoladotneta/warehouse/tree/tydz2).

## 1. Utworzenie projektu
Tworzenie programu rozpoczynamy oczywiście od utworzenia projektu w którym implementować będziemy naszą aplikację
## 2. Wypisanie funkcjonalności
Następnie, jeszcze zanim zaczniemy pisać jakiś kod dobrze jest wypisać sobie w komentarzach podstawowe funkcjonalności, które będziemy chcieli utworzyć w pierwszej kolejności. Można to zrobić np. wyobrażając sobie co po kolei powinno się dziać po uruchomieniu naszego programu.
## 3. Rozpisanie funkcjonalności krok po kroku
Poniżej w komentarzach dobrze jest wypisać poszczególne etapy z jakich będzie się składać dana funkcjonalność (w punktach).
## 4. Implementacja kolejnych funkcjonalności
Teraz możemy przejść do implementacji kolejnych funkcjonalności. Będziemy to robić najczęściej tworząc nowe klasy. Zazwyczaj będą to dwie klasy (podstawowa i serwisowa). Staramy się unikać powtarzania kodu. Dobrze, aby nasz kod był możliwie elastyczny. Wówczas być może będzie możliwe jego ponowne użycie. Dobrze również, aby ewentualne późniejsze zmiany (rozszerzenia funkcjonalności itp.) wymagały modyfikacji kodu w jak najmniejszej liczbie miejsc (a idealnie tylko w jednym). Jeżeli ktoś potrzebuje, może sobie najpierw rozpisać jakie klasy i metody będą mu potrzebne do implementacji poszczególnych etapów. Pamiętajmy również przy wymyślaniu nazw oraz implementacji, aby nasz kod był jak najbardziej czytelny i zrozumiały zarówno dla nas jak i dla innych. Starajmy się przestrzegać dobrych praktyk programowania (każda klasa w oddzielnym pliku, przestrzeganie konwencji nazywania elementów programu, nazwy w języku angielskim, nawiasy klamrowe w osobnych liniach, jawna deklaracja modyfikatorów dostępu, maksymalnie jedna wolna linia pomiędzy kolejnymi metodami itd.).
## 5. Użycie funkcjonalności
Dodajemy odpowiedni kod do metody `Main`. Tworzymy odpowiednie obiekty. Wypełniamy listy (najczęściej przy użyciu utworzonej do tego metody). Pobieramy odpowiednie dane od użytkownika. W przypadku aplikacji konsolowej, poza poznaną już przez nas metodą `Console.ReadLine();` pobierającą całą linijkę tekstu, przydatna może się okazać metoda `Console.ReadKey();`.

### Metoda `Console.ReadKey()`
Użyjemy jej w sytuacji, gdy chcemy zareagować na wciśnięcie tylko jednego klawisza. Np. gdy chcemy pobrać tylko jeden znak lub wykonać odpowiednią akcję po wciśnięciu wybranego klawisza funkcyjnego. Zwraca ona strukturę `ConsoleKeyInfo`. Zawiera ona m.in. informację, który klawisz został wciśnięty (właściwość `Key`), jaka jest jego reprezentacja w formie `char` (jaki znak został wpisany - właściwość `KeyChar`) i jaki był stan klawiszy modyfikujących (SHIFT, ALT i CTRL) w momencie wciskania tego klawisza - które były wciśnięte (właściwość `Modifiers`). Przykład użycia:

```csharp =
Console.WriteLine("----------Main Menu----------");
Console.WriteLine("1. ...");
Console.WriteLine("2. ...");
Console.WriteLine("3. ...");
Console.Write("Choose option number: ");
var option = Console.ReadKey();
Console.WriteLine();
switch(option.KeyChar)
{
    case '1':
        ...
        break;
    case '2':
        ...
        break;
    case '3':
        ...
        break;
    default:
        Console.WriteLine("Option you chose does not exist.");
}
```

## 6. Uruchomienie programu
Po skończeniu jakiegoś etapu uruchamiamy program, aby sprawdzić, czy działa zgodnie z naszymi oczekiwaniami i czy nie popełniliśmy żadnych błędów.
## 7. Debugowanie
Jeżeli kompilator poinformuje nas o jakichś błędach, lub gdy program zadziała w nieoczekiwany dla nas sposób, staramy się znaleźć tego przyczynę, a w razie potrzeby korzystamy z narzędzi do debugowania.
## 8. Czyszczenie komentarzy
Po zaimplementowaniu jakiejś funkcjonalności (lub jej etapu), możemy usunąć odpowiedni, dotyczący go, komentarz (napisany w punktach 2., 3.).
## 9. Refaktoryzacja
W dalszych etapach nauki dokonamy refaktoryzacji naszego kodu. Kiedy lepiej poznamy język C# i nauczymy się nowych mechanizmów zobaczymy, że niektóre fragmenty naszego programu można napisać lepiej, zwiększyć ich czytelność lub sprawność działania. Wówczas zaczniemy modyfikować nasz kod, zgodnie z naszą aktualną wiedzą.

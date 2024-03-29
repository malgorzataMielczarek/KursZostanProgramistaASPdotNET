# [LEKCJA 7 – Twój pierwszy program](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-1-plan-gry/lekcja-8-konwencje-pisania/)
W tej lekcji piszemy pierwszy program w języku C#. Zgodnie z programistyczną tradycją będzie to Hello World. Stworzymy cross-platformową konsolową aplikację desktopową w technologii ASP.NET Core. Należy zwrócić uwagę aby wybrać wersję aplikacji konsolowej wspierającej wszystkie platformy (Window, Linux, macOS), w odróżnieniu od starszej wersji tworzonej przy użyciu .NET Framework, wspierającej jedynie platformę Windows. Zarówno tworzonemu projektowi (_Project name_) jak i solucji (_Solution name_) nadajemy nazwę HelloWorld. W nowszej wersji kursu używa się frameworka .NET 6.0. Poniżej opisany program był tworzony wcześniej jeszcze w starszej wersji 3.1.

## 1. [Kod programu HelloWorld](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs)
```csharp = 
using System;

namespace HelloWorld
{
    public class Program
    {
        public static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
            Console.WriteLine("Nazywam się Małgorzata Mielczarek.");
        }
    }
}
```
![Kod programu HelloWorld](Ilustracje/Program.png)


Aplikacja tworzona w nowszych wersjach (począwszy od .NET 5 - C# 9) może używać tzw. instrukcji najwyższego poziomu (ang. _top-level statements_). Oznacza to, że nie musimy już jawnie ustawiać metody Main(). Powyższy zapis może więc wyglądać prościej:
```csharp = 
    Console.WriteLine("Hello World!");
    Console.WriteLine("Nazywam się Małgorzata Mielczarek.");
```

## 2. Ogólna budowa programu:
### :orange_circle: 1. [Sekcja „using”](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L1-L2)
Część programu w której po słowie kluczowym _using_ umieszcza się nazwę biblioteki z których korzysta się w programie i kończy linię średnikiem. Jeżeli korzystamy z kilku bibliotek umieszczamy kilka takich linijek
```csharp
using NazwaBiblioteki;
```
**Biblioteka** – paczka kodu zawierająca funkcje, stałe i klasy służące do określonego celu.
Później będziemy np. korzystać z takich bibliotek jak:
* _System.IO_ - służącej m.in. do obsługi plików, wejścia, wyjścia
* _System.Linq_ - do działania na kolekcjach
* _System.Security_ - do kryptografii, zabezpieczania aplikacji
* _System.Text_ - do działania na tekście, formatowania JSON-a.
### :large_blue_circle: 2. [Namespace](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L3-L13) – z ang. przestrzeń nazw
Miejsce skupiające podobne klasy (o podobnych funkcjonalnościach lub roli w programie). Służy głównie do uporządkowania nazw typów, klas, funkcji itd., aby zmniejszyć ryzyko  kolizji nazw. Na ogół jest ona związana z folderem w którym znajduje się dany plik. Pliki będące w tej samej przestrzeni nazw mogą bezpośrednio się z sobą komunikować. Żeby móc skorzystać z elementów należących do innego namespace-a trzeba natomiast dodać go do sekcji "using" (`using NazwaNamespace;`). [Przestrzeń nazw tworzy się](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L3) podając kolejno słowo kluczowe _namespace_ i nazwę, którą chcemy mu nadać. [Wnętrze przestrzeni nazw](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L4-L13) (klasy które zawiera) są ujęte w nawiasie klamrowym. Nawias klamrowy w języku C# oznacza zakres danego elementu, w tym wypadku przestrzeni nazw.
```csharp
namespace NazwaPrzestrzeniNazw
{
    //klasy w niej zawarte
}
```

### :green_circle: 3. [Klasa](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L5-L12)
Częściowa lub całkowita definicja dla obiektu. Opisuje domyślny początkowy stan obiektów, oraz ich zachowanie itp. Jest to zbiór atrybutów i metod opisujący jakiś obiekt (zbiór obiektów). [Klasę tworzy się](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L5) podając kolejno modyfikator dostępu określający poziom dostępu klasy, ewentualne dodatkowe modyfikatory (np. `static`), słowo kluczowe `class` i nazwę klasy. [Wnętrze klasy](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L6-L12), wszystkie atrybuty i metody ją tworzące, są ujęte wewnątrz nawiasu klamrowego (ciało klasy jest ograniczone nawiasem klamrowym).
```csharp
modyfikatorDostępu class NazwaKlasy
{
    //atrybuty i/lub jej metody
}
```
W języku C# występują 4 modyfikatory dostępu:
* `public` (z ang. publiczny),
* `protected` (z ang. chroniony),
* `internal` (z ang. wewnętrzny),
* `private` (z ang. prywatny).

Przy ich pomocy można określić 6 poziomów dostępu:
* `public` – określa, że klasa jest ogólnie dostępna, ze wszystkich części programu (dostęp nie jest ograniczony);
* `protected` – określa, że dostęp jest ograniczony do zawierającej klasy lub typów pochodzących od klasy zawierającej;
* `internal` – określa, że dostęp jest ograniczony do bieżącego zestawu.  
**Zestaw** (ang. _assembly_) – zbiór typów i źródeł działających razem i tworzących logiczną jednostkę funkcjonalną. Tworzy ona plik .exe lub  .dll;
* `protected internal` – określa, że dostęp jest ograniczony do bieżącego zestawu lub typów pochodzących od klasy zawierającej;
* `private` – określa, że dostęp jest ograniczony do typu zawierającego;
* `private protected` – określa, że dostęp jest ograniczony do zawierającej klasy lub typów pochodzących od klasy zawierającej w bieżącym zestawie.

### :white_circle: 4. [Metoda](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L7-L11)
Funkcja będąca częścią klasy. Zawiera logikę konkretnej czynności wykonywany przez obiekt danego typu. [Tworzy się ją](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L7) podając kolejno modyfikatory dostępu, następnie jeżeli chcemy aby metoda była statyczna wpisujemy słowo kluczowe `static`, po czym obowiązkowo musimy podać typ zmiennej zwracanej przez metodę, nazwę tworzonej metody i w nawiasie okrągłym parametry funkcji (dane przyjmowane przez metodę). Parametry określamy podając kolejno typ i nazwę każdego z nich. Jeżeli metoda przyjmuje więcej niż jeden parametr to oddzielamy je od siebie przecinkami. Jeżeli metoda nie wymaga żadnych danych, to wstawiamy jedynie pusty nawias. Wnętrze metody ([ciało metody](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L8-L11)) jest ujęte w nawiasach klamrowych.
```csharp
modyfikatoryDostępu opcjonalnieInneModyfikatory typDanychZwracanychPrzezMetodę NazwaMetody(/*opcjonalnie*/ typParametru1 nazwaParametru1, typParametru2 nazwaParametru2 ...)
{
    //logika metody
}
```
**Metoda statyczna** – metoda tworzona na etapie kompilacji programu. W programie istnieje tylko jedna instancja tej metody (nie ma oddzielnej kopii dla każdego obiektu klasy). W praktyce oznacza to m.in., że można ją wykonywać przed utworzeniem obiektu klasy do której ta metoda należy.<br/>
**Metoda `Main`** (z ang. główna) – metoda odpowiedzialna za kontrolę wykonywania pracy programu. Każda wykonywalna aplikacja (np. aplikacja konsolowa) musi ją zawierać. Tu program rozpoczyna i kończy swoją  pracę. Jest punktem wejściowym w pliku .exe programu. Aplikacja może mieć tylko jeden punkt wejściowy. Jeżeli więc w kodzie programu mamy kilka metod `Main` na etapie kompilacji musimy podać która z nich będzie tym punktem (metody te muszą jednak należeć do różnych klas). Najlepiej jednak, aby aplikacja posiadała tylko jedną metodę `Main`. Musi ona być statyczna. Deklaruje się ją wewnątrz klasy lub struktury. Metoda `Main` może, lecz nie musi zwracać dane w postaci liczb całkowitych (mieszczących się w pewnym zakresie). Jeżeli nie chcemy aby metoda cokolwiek zwracała, przypisujemy jej typ `void` (z ang. pusty). Natomiast gdy chcemy uzyskać dane (najczęściej dotyczące poprawności wykonania programu – kody błędów) na zakończenie działania programu, przypisujemy jej typ `int` (ang. _integer_ – liczba całkowita). Metoda `Main` może nie przyjmować żadnych parametrów. Wówczas po nazwie wstawiamy pusty nawias okrągły. Może również pobierać argumenty z wywołania programu w wierszu poleceń. Dane te są pobierane w postaci tablicy ciągu znaków alfanumerycznych. Wówczas wewnątrz nawiasu okrągłego wpisujemy `string[] args`.

### :red_circle: 5. [Polecenia](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L9-L10)
Czynności wykonywane kolejno przez program. Każde polecenie zakończone jest średnikiem. Takim poleceniem może [np.](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L9) być wywołanie funkcji bibliotecznej. Wówczas podajemy nazwę metody której chcemy użyć, wraz z całą ścieżką dostępu. Następnie znajduje się nawias okrągły, a po nim średnik kończący polecenie. Jeżeli wywołana przez nas metoda przyjmuje jakieś parametry, dane określonego typu (same dane bez typu) podajemy wewnątrz, znajdującego się po nazwie, nawiasu okrągłego. Jeżeli funkcja ma kilka parametrów to dane które chcemy przypisać do kolejnych parametrów oddzielamy od siebie przecinkami.
Przez ścieżkę dostępu rozumie się nazwy klas w których zawarta jest metoda oddzielonych od siebie kropkami. Klasy podaje się od najbardziej zewnętrznej do wewnętrznej, do której bezpośrednio należy wywoływana funkcja. Następnie wstawia się kropkę i nazwę żądanej metody.

## 3. Elementy użyte w programie

### :orange_circle: 1. [Biblioteka `System`](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L1) – biblioteka systemowa
Jedna z podstawowych i najczęściej używanych bibliotek C#. Zawiera podstawowe klasy, definiuje podstawowe typy danych itd.

### :large_blue_circle: 2. [Przestrzeń nazw `HelloWorld`](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L3-L13)
Utworzona przez nas przestrzeń nazw zawierającą cały kod naszego programu.

### :green_circle: 3. [Klasa `Program`](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L5-L12)
Jedyna utworzona przez nas klasa zawierająca całą logikę naszego programu. Należy do przestrzeni nazw `HelloWorld`. Jest to klasa publiczna, co oznacza, że dostęp do niej można uzyskać z dowolnego miejsca programu.

### :white_circle: 4. [Metoda `Main`](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L7-L11)
Stworzona przez nas publiczna, statyczna metoda klasy `Program`. Nasza metoda nie zwraca żadnych wartości (jest typu `void`). Przyjmuje natomiast dane w postaci tablicy ciągu znaków (`string[]`) i zapisuje do parametru `args`.

### :red_circle: 5. [Metoda `WriteLine`](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L9-L10)
Metoda klasy `Console` wyświetlająca w konsoli podaną linijkę tekstu (wypisywanie tekstu jest zakończone przejściem do nowej linii). Metoda przyjmuje jako argument ciąg znaków alfanumerycznych łącznie ze znakami białymi (spacje itp.) – typ `string`. Tekst który chcemy wyświetlić w konsoli umieszczamy wewnątrz nawiasu okrągłego wywołania metody, w cudzysłowie. Pierwsze z wywołań spowoduje pojawienie się w konsoli linijki tekstu – [_Hello World!_](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L9). Drugie natomiast linijki – [_Nazywam się Małgorzata Mielczarek._](https://github.com/malgorzataMielczarek/HelloWorld/blob/cbc46abed557bb000ed8696b578ac3c082309107/Program.cs#L10).


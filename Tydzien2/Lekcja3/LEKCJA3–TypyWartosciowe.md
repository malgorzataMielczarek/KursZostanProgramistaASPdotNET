# [LEKCJA 3 – Typy wartościowe](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-3-typy-wartosciowe/)

Typy wartościowe, są to typy których zmienne są przechowywane na stosie. Oznacza to, że ilość zajmowanej przez nie pamięci musi być góry wiadoma, w momencie kompilacji i zawsze jest taka sama, nie zależnie od przechowywanych w nich danych. Stos jest bowiem bardzo uporządkowaną strukturą pamięci podręcznej. Przeszukiwanie jej jest bardzo wydajne, jednak może ona bezpośrednio przechowywać jedynie typy wartościowe alokowane w sposób statyczny i zajmującą stałą ilość pamięci. Do typów wartościowych zaliczamy przede wszystkim
## 1. Typy proste
Zmienne tych typów tworzymy poprzez podanie nazwy typu i nazwy zmiennej, np.:
```csharp =
int customers;
short baskets;
double price;
char state;
bool isAvaliable;
```
Możemy teraz przypisać do nich wartości:
```csharp =
customers = 2000;
baskets = 100;
price = 25.99;
state = 'N';
isAvaliable = true;
```
Można też od razu przy deklaracji zmiennej zainicjalizować ją konkretną wartością, wpisaną wprost lub będącą wynikiem jakichś obliczeń, np.:
```csharp =
long headphones = 400000;
double value = price * headphones;
bool isFreeBasket = baskets > customers; 
```
### 1. Typy liczbowe
Wartością domyślną (`default`) dla wszystkich typów liczbowych jest zero (`0`). Wartość domyślna jest to wartość jaką przyjmuje zmienna danego typu przed inicjalizacją.
1. **Typy liczbowe całkowite**<br/>
Wartościowe typy proste reprezentujące liczby całkowite. Mogą być inicjalizowane literałami, czyli w tym przypadku wprost wpisanymi w kod liczbami całkowitymi. Literały liczb całkowitych mogą być:
    | System liczbowy | Prefiks | Przykład |
    | :---: | :--- | :--- |
    | dziesiętny |  | `var decimalLiteral = 42;` |
    | szesnastkowy | `0x` lub `0X` | `var hexLiteral = 0x2A;` |
    | binarny | `0b` lub `0B` | `var binaryLiteral = 0b_0010_1010;`\* |
    
    \*W przykładzie pokazano użycie `_` jako separatora cyfr. Można go używać ze wszystkimi rodzajami literałów liczbowych.<br/>
    
    W przykładach w tabeli użyto słowa kluczowego `var`, które oznacza, że typ zmiennej zostanie ustalony na podstawie inicjalizującej ją wartości. Jeśli wartość reprezentowana przez literał liczby całkowitej przekracza _UInt64.MaxValue_, występuje błąd kompilatora CS1021. Typy literałów są określane w następujący sposób:
    | Sufiks | Typ - pierwszy z kolejnych typów w którym wartość może być reprezentowana | Uwagi|
    | :---: | :---: | :--- |
    |  | `int`, `uint`, `long`, `ulong`| Literały są interpretowane jako wartości dodatnie. Na przykład literał `0xFF_FF_FF_FF` reprezentuje liczbę `4294967295` typu `uint`, choć ma taką samą reprezentację bitów, jak liczba `-1` typu `int`. Jeśli potrzebujesz wartości określonego typu, wykonaj rzutowanie literał do tego typu. Użyj operatora `unchecked`, jeśli wartości literału nie można przedstawić w typie docelowym. Na przykład `unchecked((int)0xFF_FF_FF_FF)` tworzy wartość `-1`. |
    | `U` lub `u` | `uint`, `ulong` |  |
    | `L` lub `l` | `long`, `ulong` | Małą literę `l` można użyć jako sufiksu. Generuje to jednak ostrzeżenie kompilatora, ponieważ literę l można mylić z cyfrą 1. Użyj `L` w celu zapewnienia przejrzystości. |
    | `UL`, `Ul`, `Lu`, `LU`, `ul`, `lU`, `uL` lub `lu` | `ulong` | Podobnie jak powyżej użycie małej litery l w sufiksie jest poprawne, lecz niezalecane |
    
    Wszystkie całkowite typy liczbowe obsługują operatory arytmetyczne, logiczne bitowe, porównania i równości. Można przekonwertować dowolny całkowity typ liczbowy na dowolny inny całkowity typ liczbowy. Jeśli typ docelowy może przechowywać wszystkie wartości typu źródłowego, konwersja jest niejawna. W przeciwnym razie należy użyć wyrażenia rzutowania w celu przeprowadzenia jawnej konwersji.
    
    * **int**<br/>
	Pochodzi od ang. _integer_, czyli liczba całkowita. Jest to tak na prawdę alias struktury Int32, czyli typu reprezentującego liczby całkowite w zakresie \<-2 147 483 648, 2 147 483 647\>, przechowywane na 32 bitach pamięci. Aby podejrzeć metadane tej struktury wystarczy na słowie kluczowym `int` lub `Int32` wcisnąć  klawisz **F12**. Możemy wówczas zobaczyć jakie metody zawiera dana struktura, jaka jest minimalna i maksymalna liczba jaką może przechowywać zmienna tego typu itd. Jeżeli spróbujemy zapisać w niej liczbę z poza tego zakresu (większą lub mniejszą) aplikacja wyrzuci nam błąd kompilacji lub wyjątek bezpieczeństwa. W zależności od sytuacji może on być różnego typu, np. wyjątki _OverflowException_, _ArgumentOutOfRangeException_ lub błędy kompilacji _Compiler Error CS0220 The operation overflows at compile time in checked mode_, czy _Compiler Error CS0266 Cannot implicitly convert type 'type1' to 'type2'_.
    * **short**<br/>
	Alias struktury Int16. Służy do przechowywania mniejszych liczb całkowitych, mieszczących się w zakresie \<-32 768, 32 767\>. Aby utworzyć zmienną tego typu używamy słów kluczowych `short`, `Int16` lub `System.Int16`. Zajmuje ona 16 bitów pamięci. Podobnie jak przy zmiennej typu _int_ próba zapisania w niej liczby z poza zakresu będzie skutkować wystąpieniem błędu. Metadane tego typu również możemy podejrzeć używając klawisza **F12**.
    * **long**<br/>
	Alias struktury Int64. Służy do przechowywania większych liczb całkowitych, mieszczących się w zakresie \<-9 223 372 036 854 775 808, 9 223 372 036 854 775 807\>. Aby utworzyć zmienną tego typu używamy słów kluczowych `long`, `Int64` lub `System.Int64`. Zajmuje ona 64 bitów pamięci. Podobnie jak w powyższych dwóch przypadkach próba zapisania w niej liczby z poza zakresu będzie skutkować wystąpieniem błędu. Metadane tego typu również możemy podejrzeć używając klawisza **F12**.
	
	Podsumowanie wszystkich typów liczbowych całkowitych znajduje się w tabelce poniżej:
	| Alias | Typ | Zakres | Rozmiar | Dodatkowe informacje |
	| :---: | :---: | :---: | :---: | :--- |
	| `sbyte` | `System.SByte` | \<-128, 127\> | 8 bitów (1 bajt) | Liczba całkowita znakowa. Pierwszy bit w zapisie jest bitem znaku (dodatnia/ujemna) |
	| `byte` | `System.Byte` | \<0, 255\> | 8 bitów (1 bajt) | Liczba całkowita bez znakowa |
	| `short` | `System.Int16` | \<-32 768, 32 767\> | 16 bitów (2 bajty) | Liczba całkowita znakowa |
	| `ushort` | `System.UInt16` | \<0, 65 535\> | 16 bitów (2 bajty) | Liczba całkowita bez znakowa |
	| `int` | `System.Int32` | \<-2 147 483 648, 2 147 483 647\> | 32 bity (4 bajty) | Liczba całkowita znakowa |
	| `uint` | `System.UInt32` | \<0, 4 294 967 295\> | 32 bity (4 bajty) | Liczba całkowita bez znakowa |
	| `long` | `System.Int64` | \<-9 223 372 036 854 775 808, 9 223 372 036 854 775 807\> | 64 bity (8 bajtów) | Liczba całkowita znakowa |
	| `ulong` | `System.UInt64` | \<0, 18 446 744 073 709 551 615\> | 64 bity (8 bajtów) | Liczba całkowita bez znakowa |
	| `nint` | `System.IntPtr` | Zależy od platformy (obliczana w czasie wykonywania programu) | 32 lub 64 bity (4 lub 8 bajtów) | Liczba całkowita znakowa o rozmiarze natywnym. Odpowiada typowi Int32, jeżeli jest uruchamiana w procesie 32-bitowym, a typowi Int64, gdy jest uruchamiana w procesie 64-bitowym |
	| `nuint` | `System.UIntPtr` | Zależy od platformy (obliczana w czasie wykonywania programu) | 32 lub 64 bity (4 lub 8 bajtów) | Liczba całkowita bez znakowa o rozmiarze natywnym. Odpowiada typowi UInt32, jeżeli jest uruchamiana w procesie 32-bitowym, a typowi UInt64, gdy jest uruchamiana w procesie 64-bitowym |
	
	\*Przedrostek _u-_ w nazwach typów pochodzi od ang. _unsigned_, czyli bez znaku, przedrostek _s-_ od ang. _signed_, czyli ze znakiem, a przedrostek _n-_ od ang. _native_, czyli natywny, w tym wypadku najlepszy dla danego procesora
  
2. **Typy liczbowe zmiennoprzecinkowe**<br/>
Wartościowe typy proste reprezentujące liczby rzeczywiste. Mogą być inicjowane literałami, czyli w tym przypadku wprost wpisanymi w kod liczbami rzeczywistymi. Typ literałów liczb rzeczywistych jest determinowany przez sufiks:
    | Sufiks | Typ | Przykład |
    | :---: | :---: | :--- |
	| brak, `d` lub `D` | `double` | <pre>double d = 3D;<br/>d = 4d;<br/>d = 3.934_001;</pre> |
	| `f` lub `F` | `float` | <pre>float f = 3_000.5F;<br/>f = 5.4f;</pre> |
	| `m` lub `M` | `decimal` | <pre>decimal myMoney = 3_000.5m;<br/>myMoney = 400.75M;</pre> |
    
    \*W przykładzie pokazano użycie `_` jako separatora cyfr. Można go używać ze wszystkimi rodzajami literałów liczbowych.<br/>

	Wszystkie typy liczbowe zmiennoprzecinkowe obsługują operatory arytmetyczne, porównania i równości. Ich wartością domyślną jest zero. Niejawna konwersja pomiędzy typami zmiennoprzecinkowymi jest możliwa tylko z typu _float_ na _double_. Można jednak dokonać jawnej konwersji między dowolnymi dwoma typami liczbowymi zmiennoprzecinkowymi. W wyrażeniach można również mieszać typy liczbowe całkowite i typy liczbowe zmiennoprzecinkowe. W tym wypadku typy całkowite są niejawnie konwertowane do typów zmiennoprzecinkowych. Jeżeli w jednym wyrażeniu mieszamy typy całkowite, _float_ i _double_ obowiązują następujące zasady:
	<ol>
		<li> typy liczbowe całkowite są konwertowane do jednego z typów zmiennoprzecinkowych (_float_ lub _double_)</li>
		<li> jeżeli jest to konieczne _float_ jest niejawnie konwertowany do _double_</li>
		<li> jeżeli w wyrażeniu występuje _double_, wyrażenie oblicza wartość _double_ lub _bool_ w porównaniach relacyjnych i równości</li>
		<li> jeżeli w wyrażeniu nie występuje _double_, wyrażenie oblicza wartość _float_ lub _bool_ w porównaniach relacyjnych i równości</li>
	</ol>
	
	Możemy również mieszać typy całkowite z typem _decimal_. Wówczas typy całkowite są niejawnie konwertowane do typu _decimal_, a całość wyrażenia obliczana jest do typu _decimal_ lub _bool_ w porównaniach relacyjnych i równości.<br/>
	Nie jest za to możliwe mieszanie typów _float_ i _double_ z typem _decimal_. W takim wypadku trzeba dokonać jawnej konwersji, np.:
	
	```csharp =
	double a = 1.0;
	decimal b = 2.1m;
	Console.WriteLine(a + (double)b);
	Console.WriteLine((decimal)a + b);
	```
	
    * **float**<br/>
	Pochodzi od ang. _floating-point_, czyli zmiennoprzecinkowy. Jest tak naprawdę aliasem struktury Single, czyli typu reprezentującego liczby zmiennoprzecinkowe w zakresie \<-3.40282347E+38, 3.40282347E+38\> (najmniejsza - `Single.MinValue` i największa - `Single.MaxValue` wartość możliwa do przedstawienia przy pomocy tego typu), z dokładnością 1.401298E-45 (najmniejsza wartość dodatnia możliwa do przedstawienia przy pomocy tego typu, która jest większa niż zero - `Single.Epsilon`), przechowywane na 32 bitach pamięci. Jest to tzw. typ zmiennoprzecinkowy pojedynczej precyzji (ang. _single precision_). Aby podejrzeć metadane tej struktury wystarczy jak poprzednio na słowie kluczowym `float` lub `Single` wcisnąć  klawisz **F12**. Możemy wówczas zobaczyć jakie metody zawiera dana struktura, jaka jest minimalna i maksymalna liczba jaką może przechowywać zmienna tego typu itd. Struktura `System.Single`, poza występującymi również w typach liczbowych całkowitych stałymi `MinValue` i `MaxValue` dostarczającymi minimalną i maksymalną skończoną wartość tego typu, definiuje również wspomnianą wcześniej stałą `Single.Epsilon`, oraz stałe reprezentujące wartość nienumeryczną (`Single.NaN` - ang. _not-a-number_) i wartości nieskończone (`Single.PositiveInfinity` - nieskończoność dodatnia i `Single.NegativeInfinity` - nieskończoność ujemna). Wartości przechowywane w zmiennych typu _float_ są pewnymi zaokrągleniami (w systemie dwójkowym). Niema na przykład wystąpienia liczby 0,1. Jest ona przedstawiana jedynie przy pomocy pewnego zaokrąglenia. Będzie to powodować błędy przybliżeń w obliczeniach. Jest to dobry typ jeżeli bardziej niż na dokładności obliczeń zależy nam na oszczędności pamięci.
    * **duble**<br/>
	Alias struktury Double, czyli typu reprezentującego liczby zmiennoprzecinkowe w zakresie \<-1.7976931348623157E+308, 1.7976931348623157E+308\> (najmniejsza - `Double.MinValue` i największa - `Double.MaxValue` wartość możliwa do przedstawienia przy pomocy tego typu), z dokładnością 4.94065645841247E-324 (najmniejsza wartość dodatnia możliwa do przedstawienia przy pomocy tego typu, która jest większa niż zero - `Double.Epsilon`), przechowywane na 64 bitach pamięci. Jest to tzw. typ podwójnej precyzji (ang. _double precision_). Aby podejrzeć metadane tej struktury wystarczy jak poprzednio na słowie kluczowym `double` lub `Double` wcisnąć  klawisz **F12**. Możemy wówczas zobaczyć jakie metody zawiera dana struktura, jaka jest minimalna i maksymalna liczba jaką może przechowywać zmienna tego typu itd. Struktura `System.Double` definiuje takie same jak typ _float_ stałe, o analogicznym znaczeniu: `Double.MinValue`, `Double.MaxValue`, `Double.Epsilon`, `Double.NaN`, `Double.PositiveInfinity` i `Double.NegativeInfinity`. Wartości przechowywane w zmiennych typu _double_ są pewnymi zaokrągleniami (w systemie dwójkowym). Niema na przykład wystąpienia liczby 0,1. Jest ona przedstawiana jedynie przy pomocy pewnego zaokrąglenia. Będzie to powodować błędy przybliżeń w obliczeniach. Jest to dobry typ jeżeli bardziej niż na dokładności obliczeń zależy nam na oszczędności pamięci i/lub szybkości obliczeń. Różnice w szybkości wykonywania obliczeń w stosunku do dokładniejszego typu _decimal_ będą jednak widoczne dopiero przy znacznej liczbie obliczeń (jedynie w najbardziej wymagających obliczeniowo aplikacjach).
    * **decimal**<br/>
	Alias struktury Decimal, czyli typu reprezentującego dziesiętne liczby zmiennoprzecinkowe w zakresie \<-79 228 162 514 264 337 593 543 950 335, 79 228 162 514 264 337 593 543 950 335\> (najmniejsza - `Decimal.MinValue` i największa - `Decimal.MaxValue` wartość możliwa do przedstawienia przy pomocy tego typu), z dokładnością 0.000000000000000000000000001 (najmniejsza wartość dodatnia możliwa do przedstawienia przy pomocy tego typu, która jest większa niż zero - `Decimal.NearPositiveZero`), przechowywane na 128 bitach pamięci. Aby podejrzeć metadane tej struktury wystarczy jak poprzednio na słowie kluczowym `dacimal` lub `Decimal` wcisnąć  klawisz **F12**. Możemy wówczas zobaczyć jakie metody zawiera dana struktura, jaka jest minimalna i maksymalna liczba jaką może przechowywać zmienna tego typu itd. Wartości przechowywane w zmiennych typu _decimal_ nie są wolne od zaokrągleń (w systemie dwójkowym). Dacimal raczej zmniejsza błąd dzięki zaokrągleniom. W odróżnieniu od zmiennych typu _float_ i _double_, liczba 0,1 może być dokładnie reprezentowana przez _decimal_. Jest to dobry typ jeżeli zależy nam na dokładności obliczeń. Odbywa się to jednak kosztem szybkości i znacznym kosztem zajętości pamięci (zmienna typu _decimal_ zajmuje 4 razy więcej pamięci niż zmienna typu _float_). Zmniejszenie szybkości wykonywania obliczeń w stosunku do mniej dokładnego typu _double_ będzie jednak widoczne dopiero przy znacznej liczbie obliczeń (jedynie w najbardziej wymagających obliczeniowo aplikacjach).
	
	Podsumowanie wszystkich typów liczbowych zmiennoprzecinkowych znajduje się w tabelce poniżej:
	| Alias | Typ | Przybliżony zakres | Dokładność | Rozmiar | Dodatkowe informacje |
	| :---: | :---: | :---: | :---: | :---: | :--- |
	| `float` | `System.Single` | ±1,5 x 10^−45 do ±3,4 x 10^38 | ~6-9 cyfry | 4 bajty | Najodpowiedniejszy typ gdy zależy nam na znacznej oszczędności pamięci |
	| `double` | `System.Double` | ±5,0 × 10^−324 do ±1,7 × 10^308 | ~15-17 cyfr | 8 bajtów | Kompromis między zajętością pamięci, a precyzją obliczeń. Najodpowiedniejszy typ do wyboru w najbardziej wymagających obliczeniowo aplikacjach, w celu przyspieszenia ich działania lub gdy dokładność obliczeń nie jest aż tak istotna.  |
	| `decimal` | `System.Decimal` | ±1,0 x 10^–28 do ±7,9228 x 10^28 | 28–29 cyfr | 16 bajtów | Najodpowiedniejszy, gdy wymagany stopień dokładności jest określany przez liczbę cyfr po prawej stronie przecinka (punktu dziesiętnego). Używany np. w aplikacjach finansowych do reprezentacji kursów walutowych, stóp procentowych itd. |
### 2. bool
Słowo kluczowe `bool` jest aliasem struktury `System.Boolean` (ang. _boolean_ - logiczny, od nazwiska angielskiego uczonego George'a Boole'a, współtwórcy logiki matematycznej). Reprezentuje ona wartość logiczną mogącą przyjmować tylko jedną z dwóch wartości: `true` - prawda lub `false` - fałsz. Wartością domyślną jest `false`. Na tym typie danych można przeprowadzać operacje logiczne, o których mowa będzie w kolejnych lekcjach tego tygodnia (Lekcja 7). Jest on również rezultatem porównań relacyjnych (\<, \>, \<=, \>=) i równości. Wyrażeń typu _bool_ będziemy często używać w wyrażeniach warunkowych wyrażenia `if`, pętli, czy operatora warunkowego `?:`, o których również w dalszych lekcjach tego tygodnia. Nie istnieje żadna standardowa konwersja między typem _bool_ a innymi typami wartościowymi. W odróżnieniu od innych języków takich jak C, czy C++, gdzie wartość zero liczby całkowitej lub zmiennoprzecinkowej oraz wskaźnik null są konwertowane do wartości _bool_ `false`, a wartości niezerowe liczb lub wskaźniki niebędące nullami do wartości `true`, w C# taka konwersja jest niemożliwa. Można ją osiągnąć przez jawne porównanie liczb do zera lub referencji do obiektu do wartości null. Tak jak zmienne innych typów wartościowych, zmienne typu _bool_ również można inicjalizować literałami, np.:

```csharp =
bool check = true;
System.Boolean isCheckFinished = false;
```
Jeżeli potrzebujesz natomiast logiki trzy-wartościowej można skorzystać z nullowalnego typu bool, czyli typu `bool?`. Poza wartościami `true` i `false` może on jeszcze przyjmować wartość `null`. Może się to przydać np. przy pracy z bazami danych lub gdy zadajemy pytanie na które odpowiedź może być tak/nie/nie wiem.
### 3. char
Słowo kluczowe `char` jest aliasem struktury `System.Char` (ang. _character_ - znak). Reprezentuje ona znak UNICODE UTF-16.
| Alias | Typ | Zakres | Rozmiar |
| :---: | :---: | :---: | :---: |
| `char` | `System.Char` | U+0000 do U+FFFF (0 do 65535) | 16 bitów |

Wartością domyślną typu _char_ jest wartość `\0`, czyli U+0000, czyli znak zerowy (nie mylić ze znakiem przedstawiającym cyfrę zero). Typ _char_ obsługuje operatory porównania, równości, inkrementacji i dekrementacji. Co więcej w przypadku operandów _char_ operatory arytmetyczne i bitowe wykonują operacje na odpowiednich kodach znaków i generują wynik typu _int_. Typ referencyjny _string_ o którym będzie mowa już w kolejnej lekcji reprezentuje tekst w postaci ciągu znaków _char_. Jak zmienne wszystkich typów wartościowych, zmienne tego typu mogą zostać zainicjalizowane literałami znaku. Poza tym można to zrobić używając kodów znaków na trzy sposoby:

* sekwencja ucieczki Unicode, czyli `\u` po czym następuje czteroznakowa reprezentacja szesnastkowa znaku (muszą być podane wszystkie 4 znaki, aby sekwencja była prawidłowa)
* sekwencja ucieczki szesnastkowej, czyli `\x` po czym następuje reprezentacja szesnastkowa kodu znaku (w przypadku tej sekwencji można pominąć wiodące zera)
* jawna konwersja wartości reprezentującej kod znaku na typ _char_
	
```csharp =
var chars = new[]
{
	'j',
	'\u006A',
	'\x006A',
	(char)106,
};
Console.WriteLine(string.Join(" ", chars));  // w konsoli wyświetli się: j j j j
```

Typ `char` jest niejawnie konwertowany na następujące typy całkowite: `ushort`, `uint`, `int`, `long` i `ulong`. Jest również niejawnie konwertowany na wbudowane typy liczbowe zmiennoprzecinkowe : `float`, `double` i `decimal`. Jawnie można go również konwertować na typy całkowite `sbyte`, `byte` i `short`. Nie ma natomiast niejawnych konwersji z innych typów na typ `char`. Jednak każdy typ liczbowy całkowity lub zmiennoprzecinkowy może być jawnie konwertowany na `char`.
## 2. Enumy - typy wyliczeniowe
Enumy tworzymy poprzez podanie modyfikatora dostępu, słowa kluczowego `enum` i nazwy naszego typu wyliczeniowego. Następnie między klamrami podajemy informacje jakie chcemy przetrzymywać w naszym typie wyliczeniowym, oddzielone od siebie przecinkami. Służy on do przechowywania danych typu słownikowego. Przykład nowego typu wyliczeniowego:
```csharp =
public enum ItemTypes
{
  Grocery,
  Clothing,
  Electronics
}
```
Więcej informacji o typach wyliczeniowych znajdzie się w kolejnych lekcjach tego tygodnia (Lekcja 12 - Enum). Wartością domyślną dla enumów są ich elementy zerowe.
## 3. Struktury
Typ bardzo podobny do klasy. Możemy w nim zadeklarować zmienne (tzw. pola), metody, właściwości. W odróżnieniu od klas jest to jednak typ wartościowy. Oznacza to, że będzie przechowywany na stosie, w stałym miejscu pamięci, a ilość zajmowanego przez niego miejsca w pamięci jest zawsze taka sama, nie zależnie ilu zmiennym struktury przypiszemy jakieś wartości. Strukturę tworzymy podając modyfikator dostępu, słowo kluczowe `struct` nazwę nowej struktury, a w klamrach ciało struktury (zmienne, metody itd.), np.:
```csharp =
public struct SomeStructure
{
  private int no;
  private string name;
  public SomeStructure(int number, string name)
  {
    this.no = number;
    this.name = name;
  }
  public string GetNameIfNo(int number)
  {
    if(this.no == number)
      return this.name;
    else
      return "";
  }
}
```

## Wartości domyślne typów w C#
Wartość domyślna jest to wartość, jaką przyjmuje niezainicjalizowana zmienna danego typu. Możemy ją również jawnie przypisać używając domyślnego operatora lub domyślnego literału.

### Domyślny operator (_default operator_)
Służy do uzyskania wartości domyślnej dla danego typu.

```csharp =
typ nazwaZmiennej = default(typ);

// np. dla typu int
int a = default(int);
```

### Domyślny literał (_default literal_)
Służy do (jawnej) inicjalizacji zmiennej wartością domyślną jej typu.
```csharp =
typ nazwaZmiennej = default;

//np. dla typu int
int a = default;
```

### Wartości domyślne dla poszczególnych typów
| Typ | Wartość domyślna |
| :--- | :---: |
| Jakikolwiek typ referencyjny | `null` |
| Wbudowane typy liczbowe całkowite (`int`, `long` itd.) | `0` (zero) |
| Wbudowane typy zmiennoprzecinkowe (`float`, `double`, `decimal`) | `0` (zero) |
| `bool` | `false` |
| `char` | `'\0'` (U+0000) |
| `enum` | wartość wyrażenia `(E)0`, gdzie `E` to identyfikator enuma (nazwa) |
| `struct` (struktura) | wartość uzyskana w wyniku przypisania wszystkim polom typów wartościowych odpowiadających im wartości domyślnych, a polom typów referencyjnych wartości `null` |
| Wszystkie typy wartościowe nullowalne (typy wartościowe mogące przyjąć wartość `null` np. `int?`, `bool?` itd.) | Instancja, dla której właściwość `HasValue` ma wartość `false`, a właściwość `Value` jest niezdefiniowana (`undefined`). Jest znana również jako wartość `null` typu wartościowego nullowalnego (dopuszczającego wartość `null`) |

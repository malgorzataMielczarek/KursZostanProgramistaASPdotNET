# TYDZIEŃ 2 – Podstawy języka C#
## Spis treści
### [LEKCJA 1 – Powitanie](#lekcja-1--powitanie-1)
### [LEKCJA 2 – Zmienne i stałe](#lekcja-2--zmienne-i-stałe-1)
### [LEKCJA 3 – Typy wartościowe](#lekcja-3--typy-wartościowe-1)
### [LEKCJA 4 – Typy referencyjne](#lekcja-4--typy-referencyjne-1)
### [LEKCJA 5 – Warunki](#lekcja-5--warunki-1)
### [LEKCJA 6 – Operatory](#lekcja-6--operatory-1)
### [LEKCJA 7 – Operatory Logiczne](#lekcja-7--operatory-logiczne-1)
### [LEKCJA 8 – Pętle](#lekcja-8--pętle-1)
### [LEKCJA 9 – Instrukcje skoku](#lekcja-9--instrukcje-skoku-1)
### [LEKCJA 10 – Tablice](#lekcja-10--tablice-1)
### [LEKCJA 11 – Listy](#lekcja-11--listy-1)
### [LEKCJA 12 – Enum](#lekcja-12--enum-1)
### [LEKCJA 13 – Klasy i obiekty](#lekcja-13--klasy-i-obiekty-1)
### [LEKCJA 14 – Metody](#lekcja-14--metody-1)
### [LEKCJA 15 – Parametry metod](#lekcja-15--parametry-metod-1)
### [LEKCJA 16 – Pola i właściwości](#lekcja-16--pola-i-właściwości-1)
### [LEKCJA 17 – Zakresy widoczności](#lekcja-17--zakresy-widoczności-1)
### [LEKCJA 18 – Piszemy aplikację](#lekcja-18--piszemy-aplikację-1)
### [LEKCJA 19 – Błędy początkujących](#lekcja-19--błędy-początkujących-1)
### [LEKCJA 20 – Praca domowa](#lekcja-20--praca-domowa-1)

## [LEKCJA 1 – Powitanie](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-1-powitanie/)
W tygodniu 2. poznamy między innymi:
* jakie typy danych występują w języku C#,
* instrukcje warunkowe,
* pętle,
* tablice,
* listy,
* klasy,
* metody,
* pola,
* właściwości
* itd.

Wybierzemy również projekt do wykonania. W pliku załączonym do lekcji podano przykładowe projekty.

## [LEKCJA 2 – Zmienne i stałe](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-2-zmienne-i-stale/)
Zaczynamy tworzenie naszej aplikacji konsolowej.

### Utworzenie nowego projektu
Tworzymy nowy projekt (**Create a new project**) aplikacji konsolowej .NET Core (**Console App (.NET Core)**). Aby zawęzić opcje wyszukiwania odpowiedniego typu aplikacji możemy ustawić język wyszukiwania (rozwijane menu z napisem **All languages**) wybierając opcję **C#**. Następnie możemy ustawić platformę, na której ma działać nasz program (rozwijane menu z napisem **All platforms**) jako **Windows**, oraz wybrać typ projektu (rozwijane menu z napisem **All project types**) – aplikacja konsolowa (**Console**). Przy wyborze typu projektu należy zwrócić uwagę, aby wybrać opcję z językiem **C#**, a nie np. _Visual Basic_ (**VB**). Następnie nadajemy nazwę naszemu projektowi. Przypominamy, że nazwa powinna być w formacie **PascalCase** (wyrazy pisane łącznie, pierwsza litera każdego wyrazu wielka, nazwa najlepiej w języku angielskim). Jeżeli chcemy możemy zmienić lokalizację projektu (**Location**), aby później łatwo było nam go znaleźć. Tworzymy nasz projekt. Możemy go od razu umieścić na naszym koncie na githubie, tworząc nowe repozytorium.

### Umieszczenie projektu na naszym koncie na githubie

#### Za pośrednictwem Visual Studio
Można to zrobić np. poprzez kliknięcie zakładki **Git** z menu głównego i wybranie opcji **Create Git Repository**. Następnie uzupełniamy dane w oknie **Create a Git Repository**, które nam się pojawi. Zmieniamy w nim typ repozytorium z prywatnego (odznaczamy opcję **Private repository**, jeżeli jest zaznaczona) na publiczny (dostępny dla wszystkich).

#### Za pośrednictwem strony internetowej
Alternatywnie możemy utworzyć nowe repozytorium na swoim koncie przez [stronę internetową](https://github.com/login), a następnie umieścić w nim nasz projekt.

1.	Zaloguj się na swoim koncie github.
2.	Kliknij przycisk **New** przy **Repositories**.
3.	Wpisz nazwę repozytorium (**Repository name**), najlepiej taką samą jak nazwa naszego projektu. Jeśli chcemy możemy dodać opis projektu (**Description**). Pozostawiamy wybraną opcję **Public** (tworzymy repozytorium publiczne – dostępne dla wszystkich).
4.	Można również dodać plik **README**, zaznaczając opcję **Add a README file**, w którym możemy opisać nasz projekt.
5.	Od razu możemy dodać też plik **.gitignore** zawierający pliki będące częścią naszego projektu, które nie chcemy jednak, aby zostały umieszczone w naszym repozytorium. Możemy wybrać gotowy szablon pliku (**.gitignore template**). Nie ma na razie szablonu dla platformy .NET Core. Możemy więc wybrać szablon **Visual Studio** lub utworzyć pusty plik (typ szablonu **None**). Utworzony plik można później zmodyfikować do naszych potrzeb.

#### Przy pomocy konsoli Windows (cmd – ang. _Command Line_ - Wiersz poleceń)
Można również wykonać te czynności za pośrednictwem konsoli Windows, poprzez użycie odpowiednich poleceń (tekst po polsku należy zastąpić właściwymi danymi):
 
```
cd ścieżka\do\folderu\z\projektem
git init
dotnet new gitignore
git add -A
git commit -m "Initialize project"
```

Jeżeli nasza konsola nie obsługuje poleceń `git`, to być może nie mamy go jeszcze zainstalowanego w naszym systemie. Należy go wówczas pobrać i zainstalować zgodnie ze wskazówkami ze [strony](https://github.com/git-guides/install-git). Dotneta zainstalowaliśmy razem z programem Visual Studio, ale jeżeli nasza konsola nie obsługuje polecenia `dotnet`, być może musimy dodać jego lokalizację do _Path_ w zmiennych środowiskowych:

1. Klikamy **ikonkę Windows** i wyszukujemy **Edytuj zmienne środowiskowe systemu**.
2. Po kliknięciu na wynik wyszukiwania pojawi nam się okno **Właściwości systemu**, zakładka **Zaawansowane**.
3. Klikamy **Zmienne środowiskowe...**.
4. W **Zmienne systemowe** wybieramy zmienną **Path** (**path** lub **PATH**) i klikamy **Edytuj...**.
5. Klikamy **Nowy**.
6. Wpisujemy ścieżkę dostępu do folderu zawierającego plik _dotnet.exe_, np. _C:\Program Files\dotnet_.
7. Na zakończanie klikamy **OK**.

Jeżeli nasza konsola nie obsługuje poleceń `git`, pomimo, że jest on zainstalowany w systemie to być może również trzeba dodać go do _Path_ w zmiennych środowiskowych, analogicznie jak _dotnet_.

Kiedy już wszystko zainstalujemy i wykonamy powyższe polecenia, to znaczy, że utworzyliśmy już lokalne repozytorium gitowe i umieściliśmy w nim nasz projekt.

Teraz musimy wejść na nasze konto github na [stronie internetowej](https://github.com/login) i utworzyć nowe puste repozytorium, najlepiej o tej samej nazwie co nasz projekt. Następnie możemy wrócić do naszej konsoli i wykonać kolejne polecenia:
 
```
git remote add origin url/utworzonego/przed/chwilą/ repozytorium/na/naszym/koncie/github
git push --set-upstream origin master
```

Po ich wykonaniu aktualna wersja naszego projektu została umieszczona w repozytorium na naszym koncie github. Być może będziemy musieli ustawić opcje w globalnej konfiguracji git, takie jak nazwa użytkownika (autora), czy e-mail:

```
git config --global user.name NazwaUżytkownika(naszPodpis)
git config --global user.email NaszAdresEmail
```

Powyższe polecenia ustawią nazwę użytkownika i email, które będą stosowane dla wszystkich repozytoriów utworzonych przez aktualnie zalogowanego użytkownika systemu. Można również ustawić te właściwości tylko dla danego repozytorium, wówczas zamiast _--global_, piszemy _--local_.

Alternatywnie możemy utworzyć nowe repozytorium i umieścić w nim nasz projekt przy pomocy przeznaczonej do tego aplikacji, takiej jak np. **GitHub Desktop** czy **Git GUI**.
### Kod projektu
#### Sekcja „using” – używanie aliasów namespace’ów
W górnej części pliku .cs, jak już wiemy, mamy sekcję „using”. Znajdują się w niej aliasy używanych przez nas bibliotek. Np. polecenia `Console.WriteLine` (służące do wypisania linijki tekstu w konsoli), czy `Console.ReadLine` (służące do pobierania z konsoli wpisanej przez użytkownika linijki tekstu) znajdują się w bibliotece `System`. Możemy z nich skorzystać zapisując całą ścieżkę dostępu (`System.Console.WriteLine`, `System.Console.ReadLine`) lub używając właśnie aliasu. Drugi sposób oznacza, że na górze pliku w którym korzystać będziemy z tej/tych funkcji zapisujemy `using System;`. Wówczas możemy używać skróconego zapisu nazw funkcji (`Console.WriteLine`, `Console.ReadLine`). Alias oznacza, że do naszej przestrzeni nazw (_namespace_) ładujemy wszystkie klasy, metody itd. znajdujące się pod podaną ścieżką dostępu. Dzięki temu możemy z nich korzystać tak, jakby umieszczone były w naszym namespace’ie. Metody tej warto użyć, jeżeli w danym pliku wielokrotnie używamy jakiejś klasy/metody lub wielu elementów z danej przestrzeni nazw, aby skrócić ich zapis. Jeżeli natomiast takie użycia są jednostkowe, lepiej korzystać z pełnych ścieżek dostępów.
##### Metoda Main
Jak już wiemy jest to główna metoda programu. Od niej zaczyna on swoją pracę i wykonuje kolejne znajdujące się w niej polecenia. Jeżeli więc na jej początku umieścimy kilka poleceń `Console.WriteLine("tekst")`, to właśnie umieszczony w nich tekst wyświetli się jako pierwszy w konsoli.

###### Polecenia `Console.WriteLine`, `Console.ReadLine`<br/>
Tych metod można użyć do stworzenia prostego menu:<br/>
```csharp =
Console.WriteLine("Witamy w aplikacji Zwierzak");
Console.WriteLine();
Console.WriteLine("Co chcesz zrobić ze swoim zwierzakiem?");
Console.WriteLine("\t1. Nakarm Zwierzaka.");
Console.WriteLine("\t2. Baw się ze Zwierzakiem.");
Console.WriteLine("\t3. Zabierz Zwierzaka na spacer.");
Console.WriteLine("\t4. Połóż Zwierzaka spać.");
Console.WriteLine();
Console.Write("Wybierz co chcesz zrobić wpisując numer czynności (1, 2, 3, 4): ");
string action = Console.ReadLine();
int actionNo;
Int32.TryParse(action, out actionNo);
```
<br/>Powyższy kod spowoduje wyświetlenie się w konsoli:

    Witamy w aplikacji Zwierzak

    Co chcesz zrobić ze swoim zwierzakiem?
    1. Nakarm Zwierzaka.
    2. Baw się ze Zwierzakiem.
    3. Zabierz Zwierzaka na spacer.
    4. Połóż Zwierzaka spać.

    Wybierz co chcesz zrobić wpisując numer czynności (1, 2, 3, 4): 

 
Metoda `Console.Write` spowoduje wyświetlenie w konsoli podanego jej tekstu, bez przejścia do nowej linii.<br/>
Po wyświetleniu tekstu program czeka na wpisanie przez użytkownika w konsoli dowolnego tekstu i wciśnięciu klawisza **Enter** (metoda `Console.ReadLine`). Wpisaną przez użytkownika wartość zapisujemy w zmiennej `action` (`string action = Console.ReadLine();`). Jest to zmienna typu `string`, czyli typu tekstowego (przechowująca tekst). Następnie tworzymy zmienną typu `int`, czyli mogącą przechowywać liczby całkowite z pewnego zakresu. Na koniec przy pomocy metody `Int32.TryParse(string number, out int no)` próbujemy dokonać konwersji z typu `string` na `int` (liczbę zapisaną w formie tekstu przekształcamy w wartość liczbową), a wynik tej operacji zapisujemy w zmiennej wyjściowej (`out`).
### Stałe
Stała jest to taki element aplikacji, który raz zdefiniowany nie może zostać zmieniony. Tworzymy ją podając kolejno słowo kluczowe `const`, typ stałej (np. `string` lub `int`) i jej nazwę. Następnie po znaku `=` możemy zadeklarować tą stałą konkretną wartością.

```csharp =
const string APPLICATION_NAME = "Zwierzak";
const int NUMBER_OF_MEALS_PER_DAY = 2;
```

Jest to jedyne miejsce w kodzie w którym stałej możemy przypisać wartość. Wartość ta jest niezmienna. Gdybyśmy w dalszej części programu próbowali stałej przypisać nową wartość, to otrzymamy błąd kompilacji, nawet jeśli ta nowa wartość będzie taka sama jak obecna. Przypominamy, że nazwy stałych tworzymy najczęściej używając konwencji nazewnictwa UPPER_CASE. Stałe często są w pewien sposób globalne dla całej klasy lub dla większej części projektu. W związku z tym można jej definicję umieścić poza metodą `Main` i w jej zapisie typ stałej poprzedzić modyfikatorem dostępu. Jeżeli stała będzie globalna, oznaczać to będzie, że zarezerwowane zostanie dla niej miejsce w pamięci, na cały okres działania programu. W przeciwnym razie przestanie ona istnieć w momencie gdy program wyjdzie poza zakres w którym została utworzona (najczęściej poza nawiasy klamrowe między którymi znajduje się jej definicja).
### Zmienne
Podobny do stałej element aplikacji, umożliwiający przechowywanie wartości w znanym miejscu pamięci aplikacji, jednak w odróżnieniu od stałej przechowywana tam wartość może się wielokrotnie zmieniać. Aby używać zmiennej musimy ją najpierw zadeklarować. W tym celu podajemy typ danych jakie przechowywać będzie nasza zmienna i jej nazwę (zapisaną najczęściej w konwencji camelCase). Na początku przed typem można jeszcze podać modyfikator dostępu. Utworzoną zmienną można teraz zainicjalizować konkretną wartością. Można to zrobić od razu z deklaracją, czyli analogicznie jak przy stałej, po nazwie zmiennej wstawić znak przypisania (`=`) i podać wartość jaką chcemy przypisać do zmiennej, zakańczając jak zawsze linię średnikiem. Można również po nazwie zmiennej wstawić średnik, a inicjalizacji dokonać w dalszej części programu. Kiedy do zdefiniowanej już zmiennej będziemy chcieli przypisać jakąś wartość (wszystko jedno czy po raz pierwszy, czy kolejny) wystarczy podać nazwę zmiennej, znak przypisania (`=`), wybraną wartość i średnik. Np.:

```csharp =
string petName;
int actionNo = 0;
…
petName = "Rex";
…
actionNo = 1;
…
actionNo = 2;
```

Zmienna przestaje istnieć w momencie gdy program wyjdzie poza zakres w którym została utworzona (najczęściej poza nawiasy klamrowe między którymi znajduje się jej definicja). Wyjątek stanowią zmienne globalne, które istnieją przez cały czas trwania programu.

## [LEKCJA 3 – Typy wartościowe](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-3-typy-wartosciowe/)

Typy wartościowe, są to typy których zmienne są przechowywane na stosie. Oznacza to, że ilość zajmowanej przez nie pamięci musi być góry wiadoma, w momencie kompilacji i zawsze jest taka sama, nie zależnie od przechowywanych w nich danych. Stos jest bowiem bardzo uporządkowaną strukturą pamięci podręcznej. Przeszukiwanie jej jest bardzo wydajne, jednak może ona bezpośrednio przechowywać jedynie typy wartościowe alokowane w sposób statyczny i zajmującą stałą ilość pamięci. Do typów wartościowych zaliczamy przede wszystkim
### 1. Typy proste
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
#### 1. Typy liczbowe
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
#### 2. bool
Słowo kluczowe `bool` jest aliasem struktury `System.Boolean` (ang. _boolean_ - logiczny, od nazwiska angielskiego uczonego George'a Boole'a, współtwórcy logiki matematycznej). Reprezentuje ona wartość logiczną mogącą przyjmować tylko jedną z dwóch wartości: `true` - prawda lub `false` - fałsz. Wartością domyślną jest `false`. Na tym typie danych można przeprowadzać operacje logiczne, o których mowa będzie w kolejnych lekcjach tego tygodnia (Lekcja 7). Jest on również rezultatem porównań relacyjnych (\<, \>, \<=, \>=) i równości. Wyrażeń typu _bool_ będziemy często używać w wyrażeniach warunkowych wyrażenia `if`, pętli, czy operatora warunkowego `?:`, o których również w dalszych lekcjach tego tygodnia. Nie istnieje żadna standardowa konwersja między typem _bool_ a innymi typami wartościowymi. W odróżnieniu od innych języków takich jak C, czy C++, gdzie wartość zero liczby całkowitej lub zmiennoprzecinkowej oraz wskaźnik null są konwertowane do wartości _bool_ `false`, a wartości niezerowe liczb lub wskaźniki niebędące nullami do wartości `true`, w C# taka konwersja jest niemożliwa. Można ją osiągnąć przez jawne porównanie liczb do zera lub referencji do obiektu do wartości null. Tak jak zmienne innych typów wartościowych, zmienne typu _bool_ również można inicjalizować literałami, np.:

```csharp =
bool check = true;
System.Boolean isCheckFinished = false;
```
Jeżeli potrzebujesz natomiast logiki trzy-wartościowej można skorzystać z nullowalnego typu bool, czyli typu `bool?`. Poza wartościami `true` i `false` może on jeszcze przyjmować wartość `null`. Może się to przydać np. przy pracy z bazami danych lub gdy zadajemy pytanie na które odpowiedź może być tak/nie/nie wiem.
#### 3. char
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
### 2. Enumy - typy wyliczeniowe
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
### 3. Struktury
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

### Wartości domyślne typów w C#
Wartość domyślna jest to wartość, jaką przyjmuje niezainicjalizowana zmienna danego typu. Możemy ją również jawnie przypisać używając domyślnego operatora lub domyślnego literału.

#### Domyślny operator (_default operator_)
Służy do uzyskania wartości domyślnej dla danego typu.

```csharp =
typ nazwaZmiennej = default(typ);

// np. dla typu int
int a = default(int);
```

#### Domyślny literał (_default literal_)
Służy do (jawnej) inicjalizacji zmiennej wartością domyślną jej typu.
```csharp =
typ nazwaZmiennej = default;

//np. dla typu int
int a = default;
```

#### Wartości domyślne dla poszczególnych typów
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

## [LEKCJA 4 – Typy referencyjne](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-4-typy-referencyjne/)

Zmienne w programie są przechowywane na dwóch typach pamięci: na stosie i na stercie.

### Stos
ang. _stack_, jak mówiliśmy w poprzedniej lekcji, jest to fragment bardzo uporządkowanej pamięci wirtualnej, przydzielonej aplikacji podczas uruchamiania. Jest strukturą typu LIFO (Last-In-First-Out - ostatni włożony, pierwszy wyjęty). Można go sobie wyobrazić jako górę pudełek, ułożonych równo jedno na drugim. Pudełka te są naszymi zmiennymi. Mogą się one od siebie różnić, mieć różną pojemność, ale każde pudełko ma z góry znane wymiary i można wsadzić do niego tylko tyle ile zmieści się do środka. Niezależnie jednak co do nich włożymy (jaka wartość jest aktualnie przypisana do zmiennej), jak bardzo je wypełnimy, pudełko zawsze będzie zajmowało taką samą przestrzeń w naszej wierzy (w pamięci, na stosie). Tworząc nową zmienną "dokładamy nowe pudełko na szczyt naszej wierzy". Jeżeli będziemy sięgnąć "do wnętrza jakiegoś pudełka" będziemy to robić sięgając "od góry po kolejne pudełka". Oznacza to, że najszybciej uzyskamy dostęp do zmiennej, którą ostatnio utworzyliśmy lub użyliśmy. Najczęściej są to właśnie te zmienne do których chcemy się dostać. Przeszukiwanie stosu jest więc bardzo efektywne i umożliwia szybki dostęp do zmiennych. Można jednak przechowywać na nim wyłącznie zmienne o z góry określonym, stałym rozmiarze, alokowane statycznie (kiedy budujemy wierzę, musimy mieć wcześniej przygotowane pudełka). Przechowuje się więc w niej stałe i zmienne typów wartościowych, które zawsze mają taki sam, z góry ustalony, rozmiar, nie zależnie od przechowywanych w nich aktualnie wartości. Poza nimi na stosie znajdują się również referencje do zmiennych referencyjnych, czyli jakby odnośniki do miejsca na stercie, w którym faktycznie przechowywana jest nasza zmienna referencyjna (a dokładniej, gdzie się zaczyna).

### Sterta
ang. _heap_, jest to fragment pamięci wirtualnej nieuporządkowanej aplikacji, o bardziej złożonej strukturze. Służy do przechowywania zmiennych typów referencyjnych, czyli obiektów (ang. _objects_). W odróżnieniu od stosu, który umożliwia tylko dodawanie/usuwanie zmiennych "z wierzchu", na stercie możemy uzyskać dostęp do dowolnego elementu w dowolnym czasie. Oznacza to jednak, że struktura i zarządzanie stosem są o wiele bardziej złożone, co łączy się również z mniejszą wydajnością. Sterta umożliwia dynamiczne alokowanie pamięci i służy do przechowywanie zmiennych typów referencyjnych. Oznacza to, że nie musimy z góry znać wielkości naszej zmiennej, a pamięć jest nam  przydzielana/odbierana na bieżąco wedle potrzeb. Za zarządzanie alokowaniem i zwalnianiem pamięci na stercie w C# odpowiada program zwany _garbage collector_ (GC).

### Zmienne typów referencyjnych
Zmienne typów referencyjnych, inaczej obiekty, deklarujemy analogicznie jak typy wartościowe. Załóżmy, że tworzymy obiekt będący instancją klasy _Item_ (zmienną typu _Item_):

```csharp =
Item item;
```

Powyższy kod spowodował utworzenie na stosie zmiennej typu _Item_. Zmienna ta nie może jednak przechowywać żadnej wartości, a jedynie adres pamięci sterty, gdzie przetrzymywany będzie nasz obiekt. Na razie jednak na stercie nie został jeszcze zarezerwowany żaden fragment pamięci, więc w zmiennej _item_ nie możemy przetrzymywać jeszcze żadnych danych. Aby zaalokować na stercie pamięć dla naszej zmiennej, musimy posłużyć się słowem kluczowym `new` i konstruktorem (specjalną metodą, tworzącą obiekt danego typu) danej klasy, czyli np.:

```csharp =
item = new Item();
```

Możemy oczywiście dwie powyższe czynności wykonać od razu, w jednej linii:

```csharp =
Item item = new Item();
```

Teraz nasza zmienna istnieje zarówno na stosie, jak i na stercie. Możemy więc już korzystać z metod danej klasy, wypełniać jej pola danymi itd. Jeżeli zmienne tworzymy wewnątrz jakiejś metody, po wyjściu z tej metody usunięte zostaną wszystkie utworzone na stosie, również zmienna typu referencyjnego. Obiekt znajdujący się na stercie jednak dalej tam pozostanie. Oby wyczyścić pamięć sterty z niepotrzebnych już obiektów możemy skorzystać z dostarczanego przez .NET, utworzonego w tym celu mechanizmu _garbage collector_ lub podobnie jak w innych językach programowania zrobić to osobiście, używając odpowiedniej metody.

### Typy referencyjne
Do typów referencyjnych zaliczamy przede wszystkim typy dziedziczące po klasie `System.Object`. Są to przede wszystkim elementy tworzone przez samego programistę, czyli klasy, interfejsy. Dodatkowo powszechnie używanym typem referencyjnym jest typ `string`, służący do przechowywania i obsługi tekstu.

#### Tworzenie własnego typu referencyjnego - klasy
Aby utworzyć klasę podajemy po kolei modyfikator dostępu, ewentualnie dodatkowe modyfikatorów, słowo kluczowe `class`, nazwę klasy i w nawiasach klamrowych wnętrze klasy. Poniżej pokazano prosty przykład stworzonej przez programistę klasy:

```csharp =
namespace Application
{
	public class Item
	{
		public int Id { get; set; }
		public string Name { get; set }
	}
}
```

Przypominamy, że do dobrych praktyk należy tworzenie każdej klasy w osobnym pliku.<br/>
Teraz już możemy tworzyć obiekty tej klasy. Np. w metodzie `Main` naszej aplikacji możemy wpisać kod:

```csharp =
Item item = new Item() { Id = 1, Name = "Apple" };
```

Jak widać, na podstawie powyższego fragmentu kodu, podczas tworzenia obiektu możemy od razu przypisać wartości jego właściwością. Wartości te możemy później zmieniać, np.:

```csharp =
item.Id = 5;
item.Name = "Banana";
```

#### Przypisywanie zmiennych typów referencyjnych
Jeżeli mamy dwie zmienne tego samego typu wartościowego i dokonamy przypisania jednej zmiennej do drugiej, to przypiszemy jedynie aktualnie przechowywaną tam wartość. Czyli np.:

```csharp =
int a = 5;
int b = 6;
Console.WriteLine(a);	//wyświetli się: 5
Console.WriteLine(b);	//wyświetli się: 6

b = a;
Console.WriteLine(a);	//wyświetli się: 5
Console.WriteLine(b);	//wyświetli się: 5

b = 4;
Console.WriteLine(a);	//wyświetli się: 5
Console.WriteLine(b);	//wyświetli się: 4

b = a;
a = 3;
Console.WriteLine(a);	//wyświetli się: 3
Console.WriteLine(b);	//wyświetli się: 5
```

Jeżeli natomiast analogiczne przypisanie wykonalibyśmy między dwoma zmiennymi tego samego typu referencyjnego, to tak na prawdę nie dokonamy przypisania wartości (nie będziemy mieć dwóch obiektów z takimi samymi wartościami właściwości), a jedynie referencji (adresu, pod którym obiekt istnieje na stercie). Oznacza to, że tak na prawdę istnieje tylko jeden obiekt do którego mamy dwie referencje (aliasy), istnieje pod dwoma nazwami. Czyli np.:

```csharp =
Item item = new Item() { Id = 1, Name = "Apple" };
Item item2 = new Item() { Id = 2; Name = "Orange" };
Console.WriteLine(item.Name);	//wyświetli się: Apple
Console.WriteLine(item2.Name);	//wyświetli się Orange

item2 = item;
Console.WriteLine(item.Name);	//wyświetli się: Apple
Console.WriteLine(item2.Name);	//wyświetli się Apple

item2.Name = "Banana";
Console.WriteLine(item.Name);	//wyświetli się: Banana
Console.WriteLine(item2.Name);	//wyświetli się Banana

item.Name = "Melon";
Console.WriteLine(item.Name);	//wyświetli się: Melon
Console.WriteLine(item2.Name);	//wyświetli się Melon
```

W powyższym kodzie mamy więc dwie referencje do obiektu utworzonego w pierwszej linijce, pod nazwami `item` i `item2`. Obiekt utworzony w drugiej linijce dalej istnieje na stercie, ale nie ma już do niego żaden referencji.

Jest możliwe, aby znak przypisania między zmiennymi typu referencyjnego, zamiast przypisania referencji, powodował utworzenie kopii obiektu (drugiego obiektu z przypisanymi takimi samymi wartościami), jednak wymaga to celowej implementacji w klasie.

#### String
Klasa `System.String` służąca do przechowywania sekwencji znaków UNICODE UTF-16, czyli sekwencję `char`ów. Zmienne tego typu można również tworzyć używając aliasu `string`, analogicznie do zmiennych typów wartościowych. Tekst który chcemy umieścić w obiekcie typu `string` umieszczamy w cudzysłowach. W _stringach_
występują również tzw. znaki specjalne, np.

| Znak specjalny | Opis |
| ---: | :--- |
| \r | powrót karetki, pozostałość po maszynie do pisania, oznacza powrót na początek linii |
| \n | nowa linia |
| \t | tabulacja |
| \b | backspace |
| \ | backslash jest tzw. znakiem ucieczki, oznacza to, że umożliwia on wstawianie w tekście znaków używanych w innym kontekście, jak np. cudzysłowów |

| Sekwencja wpisana w stringu | wyświetlany tekst | Opis|
| ---: | :---: | :--- |
| `\'` | ' | pojedynczy cudzysłów, apostrof |
| `\"` | " | cudzysłów |
| `\\` | \ | backslash |

Możemy również "wyłączyć" interpretowanie znaków specjalnych. W tym celu przed cudzysłowem rozpoczynającym nasz string wstawiamy znak `@`. Oczywiście wstawienie kolejnego znaku `"` będzie dalej oznaczać zakończenie naszego stringa, a znak `\` nie będzie już interpretowany jako znak ucieczki, tylko po prostu backslash. W tak tworzonym stringu nie da się więc wstawić znaku cudzysłowa. W wielu sytuacjach zapis taki może być jednak bardzo pomocny, np. przy zapisywaniu ścieżek do plików, aby zwiększyć czytelność. Np.:

```csharp
const string FILE = "C:\\Users\\adam\\Images\\smiley.bmp";
// można też zapisać np.
const string SMILEY_PATH = @"C:\Users\adam\Images\smiley.bmp";
```

Wstawiony przed rozpoczynającym string cudzysłowem znaku `$` daje znać kompilatorowi, że chcemy w stringu używać wartości innych zmiennych. Możemy wówczas wpisać wewnątrz stringu nazwę zmiennej otoczoną klamrami. Kompilator zastąpi nam tą sekwencję aktualną wartością zmiennej przekonwertowaną do typu `string`;

## [LEKCJA 5 – Warunki](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-5-warunki/)
Instrukcje warunkowe umożliwiają wykonanie określonych instrukcji lub zwrócenie określonej wartości w zależności od spełnienia jakiegoś warunku.
### if
Podstawową instrukcją warunkową jest instrukcja `if`. Ma ona postać:

```csharp =
if (warunek)
{
	//instrukcje wykonywane jeśli warunek jest spełniony (ma wartość bool true)
}
```

Działa ona w następujący sposób. Jeżeli warunek, będący wyrażeniem typu `bool`, ma wartość `true`, to zostają wykonane kolejno instrukcje znajdujące się pomiędzy klamrami. Jeśli natomiast warunek ma wartość `false` instrukcje znajdujące się pomiędzy klamrami są pomijane i program przechodzi bezpośrednio do kodu znajdującego się po instrukcji `if`.

Ta instrukcja warunkowa może mieć bardziej rozbudowaną formę tzw. `if else`. Wygląda wówczas następująco:

```csharp =
if (warunek)
{
	// pierwszy blok instrukcji - instrukcje wykonywane jeśli warunek jest spełniony (ma wartość bool true)
}
else
{
	//drugi blok instrukcji - instrukcje wykonywane jeśli warunek nie jest spełniony (ma wartość bool false)
}
```

W powyższym przypadku, jeżeli warunek ma wartość `true` wykonują się instrukcje z pierwszego bloku (znajdujące się między klamrami po słowie kluczowym `if`), natomiast blok drugi (instrukcje między klamrami po słowie kluczowym `else`) jest ignorowany. Natomiast jeśli warunek ma wartość `false`, pierwszy blok kodu jest ignorowany, a wykonuje się drugi blok.

Możemy również dokonywać wyborów wielowariantowych. Wówczas polecenie `if else` rozbudowujemy o sekcję `else if`, czyli:

```csharp =
if (warunek1)
{
	//pierwszy blok instrukcji - instrukcje wykonywane jeśli warunek1 jest spełniony (ma wartość bool true)
}
else if (warunek2)
{
	//drugi blok instrukcji - instrukcje wykonywane jeśli warunek1 ma wartość false, ale warunek2 jest spełniony (ma wartość bool true)
}
else
{
	//ostatni blok instrukcji - instrukcje wykonywane jeśli żaden z warunków nie jest spełniony (zarówno warunek1, jak i warunek2 mają wartość bool false)
}
```

Sekcji `else if` może być dowolnie dużo (możemy więc sprawdzać dowolną ilość warunków). Powyższa instrukcja działa analogicznie jak poprzednie, z zastrzeżeniem, że po napotkaniu pierwszego warunku o wartości `true`, kolejne warunki nie są nawet sprawdzane. Tak więc najpierw obliczana jest wartość warunku `warunek1`. Jeżeli wynosi ona `true` to wykonuje się pierwszy blok instrukcji, a reszta instrukcji warunkowej jest ignorowana. Natomiast gdy `warunek1` ma aktualnie wartość `false`, pominięty zostaje pierwszy blok instrukcji i przechodzimy do obliczania wartości `warunek2`. Jeżeli wynosi ona `true` wykonuje się drugi blok instrukcji, a pozostała część instrukcji warunkowej jest ignorowana. Gdy natomiast ma ona wartość `false`, drugi blok instrukcji jest ignorowany i jeżeli jest więcej sekcji `else if` to postępujemy z nimi analogicznie, a jak nie to wykonują się instrukcje z ostatniego bloku, po słowie kluczowym `else`. Instrukcja warunkowa wielowariantowa nie musi zawierać sekcji `else`. Wówczas, jeżeli żaden z warunków nie jest spełniony to żadna z instrukcji wewnątrz instrukcji warunkowej się nie wykona.

We wszystkich powyższych przypadkach, jeżeli któryś z bloków instrukcji składa się tylko z jednego polecenia to otaczające je nawiasy klamrowe można pominąć.

Możliwe jest również zagnieżdżanie instrukcji `if` dowolnych wariantów. To znaczy, że blok instrukcji może również zawierać instrukcję `if` (`if else`, `if else if else` itd.).

### Operator trójargumentowy
ang. _conditional operator_ lub _ternary conditional operator_ (operator warunkowy, operator trójargumentowy), czyli operator `?:` zwracający różne wartości w zależności od spełnienia określonego warunku. Ma on następującą budowę:

```csharp =
var zmienna = warunek ? wartosc1 : wartosc2;
```

Jeżeli wyrażenie warunkowe ma wartość `true` ( `warunek` jest równy `true`) `zmienna` przyjmie wartość `wartosc1`. Gdy natomiast ma wartość `false`, to `zmienna` będzie mieć wartość `wartosc2`. Operatory mogą być również w sobie zagnieżdżone. Tą samą operację można przeprowadzić używając instrukcji `if else`, jednak użycie operatora trójargumentowego skraca zapis. Np. operacja:

```csharp =
int index = item.Name == 'Banana' ? 1 : 0;
```

zapisana przy pomocy instrukcji `if else` będzie wyglądać następująco:

```csharp =
int index;
if (item.Name == "Banana")
{
	index = 1;
}
else
{
	index = 0;
}
```

### switch
Instrukcja warunkowa sprawdzająca czy jej argument jest równy konkretnej, z góry znanej wartości. Ma ona następującą budowę:

```csharp =
switch (argument)
{
	case wartosc1:
		//blok 1 - instrukcje wykonywane gdy argument == wartosc1
		break;
	case wartosc2:
		//blok 2 - instrukcje wykonywane gdy argument == wartosc2
		break;
	.
	.
	.
	default:
		//ostatni blok - instrukcje wykonywane gdy argument nie jest równy żadnej z wymienionych wyżej wartości
		break;
}
```

Powyższa instrukcja działa tak samo jak następująca instrukcja `if`:

```csharp =
if (argument == wartosc1)
{
	//blok 1
}
else if (argument == wartosc2)
{
	//blok 2
}
.
.
.
else
{
	//ostatni blok
}
```

Operator `==` jest operatorem porównania, czyli wyrażenie `a == b` ma wartość `true`, jeżeli zmienna `a` ma taką samą wartość jak zmienna `b`. W przeciwnym razie wyrażenie ma wartość `false`. Słowo kluczowe `break` oznacza przerwanie wykonywania danej instrukcji. Czyli po dojściu do instrukcji `break` program natychmiast przerywa wykonywanie instrukcji `switch` i przechodzi do wykonywania kolejnych poleceń po niej występujących. W odróżnieniu np. od instrukcji `if` bloki instrukcji nie są otoczone nawiasami klamrowymi. Dany blok zaczyna się za znakiem `:` i kończy poleceniem `break` (zamiast `break` możliwe jest również użycie instrukcji `return` lub `throw`). Taki zapis jest wynikiem zaszłości z języków poprzedzających język C#. W odróżnieniu od np. C++, w C# umieszczenie instrukcji `break` jest obligatoryjne. W instrukcji `switch` analogicznie do instrukcji `if` może być sprawdzana dowolna liczba przypadków (dowolna liczba bloków `case`). Można również pominąć blok `default` (czyli odpowiednik `else` z `if`a). Instrukcja `switch` ma ograniczone działanie w stosunku do instrukcji `if`. Tradycyjnie po pierwsze jej warunki są wyłącznie operacjami porównania dwóch wartości typów podstawowych (typy wartościowe, stringi), `argument` musi więc być zmienną/wyrażeniem typu podstawowego. Sytuacja ta zmieniła się nieco od wersji C# 7, od której składnia i możliwości instrukcji `switch` zostały znacznie rozbudowane i są jeszcze bardziej rozszerzane w kolejnych wersjach języka (dla zainteresowanych ciekawy artykuł o `switch` w C# 7 i C# 8 - [link](https://geek.justjoin.it/nowy-switch-w-c-8-0-jak-dziala-property-pattern/)). Drugim ograniczeniem jest, że wartości z którymi porównywany jest nasz `argument` muszą być stałymi (wartościami znanymi jeż na etapie kompilacji), w odróżnieniu od instrukcji `if`, gdzie wartości te obliczane są dopiero podczas wykonywania instrukcji (sprawdzania warunku). Przez te ograniczenia instrukcja `switch` jest dość rzadko stosowana. Może nam się jednak przydać np. do obsługi menu naszej aplikacji konsolowej, co jest dobrym przykładem jej zastosowania.

### switch expressions
Występujące od C# 8 wyrażenie o działaniu podobnym do instrukcji `switch`, które jednak podobnie jak operator trójargumentowy zwraca jakąś wartość (zależną od tego który warunek jest spełniony). Ma ono następującą budowę:

```csharp =
var zmienna = argument switch
{
	wzorzec1 => wartosc1,
	wzorzec2 => wartosc2,
	.
	.
	.
	_ => wartoscN
};
```

Ma ona następujące działanie:
1. sprawdzamy, czy `argument` jest zgodny ze wzorcem `wzorzec1`
	1. jeżeli tak, to `switch` zwraca wartość `wartosc1` i kończy swoje działanie
	2. jeżeli nie, to przechodzimy do punktu 2.
2. sprawdzamy, czy `argument` jest zgodny z kolejnym wzorcem
	1. jeżeli tak, to `switch` zwraca odpowiednią wartość i kończy swoje działanie
	2. jeżeli nie, to
		1. jeżeli są jeszcze jakieś wzorce do sprawdzenia, wracamy do punktu 2.
		2. jeżeli nie ma już żadnych wzorców przechodzimy do punktu 3.
3. ostatnim elementem wyrażenia `switch` może być `_`, choć nie jest to element obligatoryjny. Oznacza on dowolną wartość i jest równoważny sekcji `default` instrukcji `switch`. Jeżeli ten element istnieje i `argument` nie pasował do żadnego ze wzorców, wyrażenie zwraca wartość `wartoscN`.


Począwszy od C# 7 `argument` zarówno w instrukcji jak i wyrażeniu `switch` może być obiektem lub tulpem (zestawem wartości, zamiast jedną wartością). Począwszy od C# 9 możliwe jest również poza prostym porównaniem, porównanie relacyjne (<, <=, >, >=).

## [LEKCJA 6 – Operatory](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-6-operatory/)

Kolejność wykonywania działań z użyciem operatorów jest zgodna z zasadami matematyki. Jeżeli kilka operacji jest równoważnych (tak samo ważnych jeśli chodzi o kolejność wykonywania działań) to są one wykonywane od lewej do prawej. Jeżeli ostateczny wynik będziemy znać po wykonaniu tylko części operacji (będzie to czasem miało miejsce np. w przypadku operacji logicznych), to dalsze operacje nie są wykonywane.

### Arytmetyczne
Zdefiniowanie zmiennych do przykładów z poniższej tabeli:

```csharp =
int a = 5;
int b = 10;
```

| Operator | Znaczenie | Przykład |
| :---: | :--- | :--- |
| `+` | dodawanie | `int c = a + b; // wynik: 15` |
| `-` | odejmowanie | `int c = b - a; // wynik: 5` |
| `*` | mnożenie | `int c = a * b; // wynik: 50` |
| `/` | dzielenie | `int c = b / a; // wynik: 2` |
| `%` | modulo - reszta z dzielenia | `int c = b % a; // wynik: 0` |

Jeżeli wynik operacji chcemy przypisać do zmiennej znajdującej się po lewej stronie operatora, zapis operacji możemy skrócić stosując operatory (`+=`, `-=`, `*=`, `/=` i `%=`), np.:

```csharp =
int a = 0;
int b = 10;
a += 2; // to samo co a = a + 2;
// czyli teraz a == 2, b == 10
a -= b; // to samo co a = a - b;
//teraz a == -8, b == 10
a *= -1; // to samo co a = a * (-1);
// a == 8, b == 10
b %= a; // to samo co b = b % a;
// a == 8, b == 2
a /= b; // to samo co a = a / b;
// a == 4, b == 2
```

### Inkrementacji/dekrementacji
1. **Inkrementacja** - zwiększenie wartości o jeden. Jest to operacja jednoargumentowa, której operatorem są dwa znaki plus (`++`). Może występować w dwóch wersjach:
	1. postinkrementacja -  operator wstawiamy po argumencie. W tym wypadku wyrażenie ma wartość argumentu (najpierw następuje przypisanie, a potem inkrementacja). Np.:
	
	```csharp =
	int i = 0;
	i++; // to samo co i = i + 1; lub i += 1;
	// wyrażenie ma wartość i
	```
	
	2. preinkrementacja -  operator wstawiamy przed argumentem. W tym wypadku wyrażenie ma wartość argumentu powiększonego o jeden (najpierw następuje inkrementacja, a potem przypisanie). Np.:
	
	```csharp =
	int i = 0;
	++i; // to samo co i = i + 1; lub i += 1;
	// wyrażenie ma wartość i + 1
	```
	
	Przykład pokazujący różnicę między postinkrementacją i preinkrementacją:
	
	```csharp =
	int i = 0, post, pre;
	
	// Postinkrementacja
	post = i++;
	/*
	Inaczej można to zapisać:
	post = i;
	i = i + 1;
	czyli teraz post == 0, a i == 1
	*/
	
	i = 0;
	//Preinkrementacja
	pre = ++i;
	/*
	Inaczej można to zapisać:
	i = i + 1;
	pre = i;
	czyli teraz pre == 1, a i == 1
	*/
	```

2. **Dekrementacja** - zmniejszenie wartości o jeden. Jest to operacja jednoargumentowa, której operatorem są dwa znaki minus (`--`). Podobnie jak inkrementacja, może występować w dwóch wersjach, o analogicznym działaniu:
	1. postdekrementacja -  operator wstawiamy po argumencie. W tym wypadku wyrażenie ma wartość argumentu (najpierw następuje przypisanie, a potem dekrementacja). Np.:
	
	```csharp =
	int i = 0;
	i--; // to samo co i = i - 1; lub i -= 1;
	// wyrażenie ma wartość i
	```
	
	2. predekrementacja -  operator wstawiamy przed argumentem. W tym wypadku wyrażenie ma wartość argumentu pomniejszonego o jeden (najpierw następuje dekrementacja, a potem przypisanie). Np.:
	
	```csharp =
	int i = 0;
	--i; // to samo co i = i - 1; lub i -= 1;
	// wyrażenie ma wartość i - 1
	```
	
	Przykład pokazujący różnicę między postdekrementacją i predekrementacją:
	
	```csharp =
	int i = 0, post, pre;
	
	// Postdekrementacja
	post = i--;
	/*
	Inaczej można to zapisać:
	post = i;
	i = i - 1;
	czyli teraz post == 0, a i == -1
	*/
	
	i = 0;
	//Predekrementacja
	pre = --i;
	/*
	Inaczej można to zapisać:
	i = i - 1;
	pre = i;
	czyli teraz pre == -1, a i == -1
	*/
	```
	
Podsumowanie:

| Operator | Znaczenie | Przykład |
| :---: | :--- | :--- |
| `++` | inkrementacja | `i++; // postinkrementacja`<br/>`++i; // preinkrementacja` |
| `--` | dekrementacja | `i--; // postdekrementacja`<br/>`--i; // predekrementacja` |

### Przypisania
Operator pozwalający nadać zmiennej wartość. Jest to po prostu znak równa się (`=`) i był już przez nas wielokrotnie stosowany, np.:

```csharp =
int a, b;
a = 5; // nadanie zmiennej a wartości 5
b = a; // nadanie zmiennej b takiej samej wartości jaką ma zmienna a, czyli 5
```

| Operator | Znaczenie | Przykład |
| :---: | :--- | :--- |
| `=` | przypisanie wartości do zmiennej | `int i = 0;` |

### Relacji
Operatory porównujące dwie wartości i zwracające wartość `bool` `true` jeśli zależność jest prawdziwa lub `false`, jeśli nie. Zdefiniowanie zmiennych do przykładów z poniższej tabeli:

```csharp =
int a = 5;
int b = 5;
```

| Operator | Znaczenie | Przykład |
| :---: | :--- | :--- |
| `==` | czy równe | `bool c = (a == b); // wynik: true` |
| `!=` | czy różne | `bool c = (a != b); //wynik: false` |
| `>` | większe niż | `bool c = (a > b); // wynik: false` |
| `<` | mniejsze niż | `bool c = (a < b); // wynik: false` |
| `>=` | większe równe | `bool c = (b >= a); // wynik: true` |
| `<=` | mniejsze równe | `bool c = (b <= a); // wynik: true` |

Przykład użycia:

```csharp =
int a = 5;
int b = 0;
int result;

if (b != 0)
{
	result = a / b;
	Console.WriteLine($"{a} // {b} = {result}");
}
else
{
	Console.WriteLine("Nie można dzielić przez zero.");
}
```	

### Logiczne warunkowe
Zostaną omówione w kolejnej lekcji (lekcja 7).

### Konkatenacji
czyli łączenia łańcuchów tekstowych. Operatorem konkatenacji jest znak plus (`+`). Np.:

```csharp =
string person = "Ala";
string action = "ma kota.";
string sentence = person + " " + action;
Console.WriteLine(sentence); //zostanie wypisane: Ala ma kota.
sentence = person + " miała kota";
sentence += ", ale zdechł.";
Console.WriteLine(sentence); //zostanie wypisane: Ala miała kota, ale zdechł.
```

Podobnie jak w przypadku operatora dodawania, można go łączyć z operatorem przypisania (`+=`), np.:

```csharp =
string a = "Ala";
a += " ma kota."; // to samo co a = a + " ma kota."
// wynik: Ala ma kota.
```

| Operator | Znaczenie | Przykład |
| :---: | :--- | :--- |
| `+` | dla argumentów typu `string` oznacza konkatenację, połączenie dwóch stringów w jeden | `string a = "Ala" + " miała kota." // wynik: Ala miała kota.` |

### Łączenia wartości null
ang. _null coalescing operator_. Operatorem łączenia wartości null jest podwójny znak zapytania (`??`). Jest to operator dwuargumentowy.

Budowa wyrażenia: `x ?? y`, gdzie `x` jest wyrażeniem dowolnego typu, który może przyjmować wartość `null`, a `y` typu, który może zostać przypisany do `x` (nie musi móc przyjmować wartości `null`).

Działanie:</br>
1. Obliczenie wartości wyrażenia `x`
2. Jeżeli `x` wynosi `null` to patrz 3., przeciwnie patrz 4.
3. Obliczanie wyrażenia `y`. `x ?? y` ma wartość wyrażenia `y`.
4. `x ?? y` ma wartość wyrażenia `x`.

Np.:
```csharp =
string? nullableString = null;
string notNullString = nullableString ?? "Tu jest NULL";
Console.WriteLine(notNullString); //zostanie wypisane: Tu jest NULL

nullableString = "Już niema NULL";
notNullString = nullableString ?? "Tu jest NULL";
Console.WriteLine(notNullString); //zostanie wypisane: Już niema NULL
```

Począwszy od C# 8.0 operator łączenia wartości null można również stosować w połączeniu z operatorem przypisania (`??=`). Wówczas wartość wyrażenia łączenia wartości null jest przypisywana do lewego argumentu tego wyrażenia. W tym wypadku lewy argument musi być zmienną, właściwością lub indeksatorem. Np.:

```csharp =
int? a = null;

a ??= 5; //to samo co a = a ?? 5;, czyli a jest teraz równe 5
a ??= 7; //to samo co a = a ?? 7;, czyli a jest nadal równe 5
```

Operatory `??` i `??=` są prawostronnie asocjacyjne. Oznacza to, że operatory te są grupowane od prawej do lewej. Czyli wyrażenie `a ?? b ?? c`, jest przetwarzane jako `a ?? (b ?? c)`, a wyrażenie `d ??= e ??= f`, jako `d ??= (e ??= f)`. Ogólnie wyrażenie postaci `E1 ?? E2 ?? ... ?? EN` zwraca wartość pierwszego argumentu różnego niż `null` lub `null`, jeżeli wszystkie argumenty mają wartość `null`. Analogicznie w wyrażeniu `E1 ??= E2 ??= ... ??= EN` `E1` będzie miał wartość pierwszego argumentu różnego od `null` lub `null`, jeżeli wszystkie argumentu mają wartość `null`.

## [LEKCJA 7 – Operatory Logiczne](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-7-operatory-logiczne/)
W lekcji tej poznamy operatory logiczne warunkowe. Wyróżniamy operatory logiczne jednoargumentowe i dwuargumentowe. Wszystkie argumenty są typu `bool`. Wynik operacji logicznych również jest tego typu. Operatory logiczne są często stosowane w wyrażeniach warunkowych instrukcji `if`, pętli itp.

### Operatory logiczne jednoargumentowe
Jest tylko jedna jednoargumentowa operacja logiczna - NOT, czyli zaprzeczenie. Zmienia on wartość logiczną wyrażenia na przeciwną (z `true` na `false`, a z `false` na `true`). Operatorem NOT jest wykrzyknik (`!`) stawiany przed argumentem (wyrażeniem/zmienną itd.). Umożliwia on np. wykonanie jakichś czynności, gdy warunek nie jest spełniony.

| Operator | Nazwa | Przykład | Uwagi |
| :---: | :---: | :--- | :--- |
| `!` | NOT (nie, zaprzeczenie, negacja) | `bool a = !true; // wynik: false` | jest to operator jednoargumentowy, stojący po lewej stronie argumentu, spośród operacji logicznych negacja jest wykonywana jako pierwsza (ma najwyższy priorytet, nie licząc nawiasów) |

| Wartość argumentu | Negacja | Wynik |
| :---: | :---: | :--- |
| `true` | `!true` | `false` |
| `false` | `!false` | `true` |

### Operatory logiczne dwuargumentowe
Operacje logiczne dwuargumentowe mają postać `argument1 operator argument2`. Zarówno oba argumenty, jak i całość operacji są typu `bool`. Jeżeli argumenty są wyrażeniami, a operator jest operatorem warunkowym (`&&` lub `||`), to gdy wynik operacji jest znany po obliczeniu pierwszego argumentu, to drugi argument nie jest już w ogóle liczony. Możemy wymienić następujące operatory logiczne dwuargumentowe:

| Operator | Nazwa | Przykład | Uwagi |
| :---: | :---: | :--- | :--- |
| `&` | logiczne AND (i, koniunkcja) | `bool a = false & true; // wynik: false` | Zawsze obliczane są oba argumenty operacji, niezależnie od wyniku. Można ten operator łączyć z operatorem przypisania (`&=`), wówczas wynik operacji jest przypisany do zmiennej będącej jej lewym argumentem. |
| `&&` | warunkowy operator logiczny AND | `bool a = false && true; // wynik: false` | Jeżeli lewy argument operacji jest równy `false`, to prawy argument nie jest liczony (czyli jeżeli byłaby to np. funkcja zwracająca wartość logiczną, to w ogóle by się ona nie wykonała), a całość operacji daje wynik `false`. Operatora nie można łączyć z operatorem przypisania. |
| `\|` | logiczne OR (lub, alternatywa) | `bool a = false \| true; // wynik: true` | Zawsze obliczane są oba argumenty operacji, niezależnie od wyniku. Można ten operator łączyć z operatorem przypisania (`\|=`), wówczas wynik operacji jest przypisany do zmiennej będącej jej lewym argumentem. |
| `\|\|` | warunkowy operator logiczny OR | `bool a = false \|\| true; // wynik: true` | Jeżeli lewy argument operacji jest równy `true`, to prawy argument nie jest liczony (czyli jeżeli byłaby to np. funkcja zwracająca wartość logiczną, to w ogóle by się ona nie wykonała), a całość operacji daje wynik `false`. Operatora nie można łączyć z operatorem przypisania. |
| `^` | logiczne XOR (albo, alternatywa rozłączna, wykluczająca) | `bool a = false ^ true; // wynik: true` | Zawsze obliczane są oba argumenty operacji, niezależnie od wyniku. Można ten operator łączyć z operatorem przypisania (`^=`), wówczas wynik operacji jest przypisany do zmiennej będącej jej lewym argumentem. |

Operacje z użyciem powyższy operatorów przyjmują następujące wartości dla różnych wartości argumentów:

| argument 1 (`x`) | argument 2 (`y`) | AND (`x & y` lub `x && y`) | OR (`x \| y` lub `x \|\| y`) | XOR (`x ^ y`) |
| :---: | :---: | :---: | :---: | :---: |
| `true` | `true` | `true` | `true` | `false` |
| `true` | `false` | `false` | `true` | `true` |
| `false` | `true` | `false` | `true` | `true` |
| `false` | `false` | `false` | `false` | `false` |

Operatory `&` i `|` mogą również obsługiwać logikę trójwartościową, czyli przyjmować argumenty i zwracać wartość typu `bool?` (mającego jedną z trzech wartości: `true`, `false` lub `null`). Wówczas przyjmują one następujące wartości:

| argument 1 (`x`) | argument 2 (`y`) | AND (`x & y`) | OR (`x \| y`) |
| :---: | :---: | :---: | :---: |
| `true` | `true` | `true` | `true` |
| `true` | `fałsz` | `fałsz` | `true` |
| `true` | `null` | `null` | `true` |
| `fałsz` | `true` | `fałsz` | `true` |
| `fałsz` | `fałsz` | `fałsz` | `fałsz` |
| `fałsz` | `null` | `fałsz` | `null` |
| `null` | `true` | `null` | `true` |
| `null` | `fałsz` | `fałsz` | `null` |
| `null` | `null` | `null` | `null` |

### Łączenie operacji logicznych
Operacje logiczne, podobnie jak arytmetyczne można ze sobą łączyć w jedno wyrażenie logiczne. Podobnie jak w przypadku operatorów arytmetycznych obowiązuje tu również matematyczna kolejność wykonywania działań (zgodnie z logiką matematyczną). Kolejność ta jest następująca:
1. Operator negacji logicznej (`!`)
2. Operator logiczny AND (`&`)
3. Logiczny wyłączny operator OR - XOR (`^`)
4. Operator logiczny OR (`|`)
5. Warunkowy operator logiczny AND (`&&`)
6. Warunkowy operator logiczny OR (`||`)

## [LEKCJA 8 – Pętle](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-8-petle/)
Służą do iteracyjnego powtórzenia jakiejś czynności. Oznacza to, że dany blok kodu wykona się określoną ilość razy, zależną od warunków początkowych jakie przyjmiemy i warunku przerwania. W języku C# wyróżniamy cztery rodzaje pętli:

### while
Najbardziej podstawowa pętla. Składa sią ze słowa kluczowego `while`, ujętego w nawias okrągły warunku typu `bool` i zawartego w klamrach bloku kodu:

```csharp =
while(warunek)
{
	//blok kodu
	//zrób coś tak długo jak warunek jest spełniony
	//najczęściej zmień parametry użyte w warunku
}
```

Pętla działa w następujący sposób:
1. Obliczenie warunku, sprawdzenie jego prawdziwości
2. Jeżeli warunek jest prawdziwy (ma wartość równą `true`) to patrz punkt 3., jeżeli nie (ma wartość równą `false`) to patrz punkt 5.
3. Wykonanie bloku kodu. Na jego końcu znajduje się najczęściej instrukcja zmieniająca parametry warunku.
4. Obliczenie na nowo warunku (najczęściej ze zmienionymi parametrami) i powrót do punktu 2.
5. Gdy warunek równy `false` blok kodu nie jest więcej wykonywany, a pętla kończy swoją pracę (przechodzimy do kodu po pętli).

Przykład:

```csharp =
// Program wypisujący w konsoli pięć znaków plus (+++++)
int i = 0;
while(i < 5)
{
	Console.Write("+");
	i++;
}
Console.WriteLine();
```

Pętlę `while` stosujemy najczęściej jeżeli nie wiemy dokładnie przez ile wykonań pętli warunek będzie spełniony.

### do while
Chyba najrzadziej stosowany rodzaj pętli, charakteryzujący się tym, że zawsze jest wykonywana przynajmniej raz (niezależnie od warunku). Składa sią ze słowa kluczowego `do`, zawartego w klamrach bloku kodu, słowa kluczowego `while`, ujętego w nawias okrągły warunku typu `bool` i postawionego na końcu instrukcji średnika:

```csharp =
do
{
	//blok kodu
	//zrób coś następnie sprawdź warunek i jeśli jest spełniony, to zrób to ponownie
	//najczęściej zmień parametry użyte w warunku
}
while(warunek);
```

Pętla działa w następujący sposób:
1. Wykonanie bloku kodu. Najczęściej zawiera on instrukcję, która będzie zmieniać parametry warunku.
2. Obliczenie warunku, sprawdzenie jego prawdziwości
3. Jeżeli warunek jest prawdziwy (ma wartość równą `true`) to patrz punkt 1., jeżeli nie (ma wartość równą `false`) to patrz punkt 4.
4. Gdy warunek równy `false` blok kodu nie jest więcej wykonywany, a pętla kończy swoją pracę (przechodzimy do kodu po pętli).

Przykład z pierwszego rodzaju pętli możemy również wykonać przy pomocy pętli `do while`:

```csharp =
// Program wypisujący w konsoli pięć znaków plus (+++++)
int i = 0;
do
{
	Console.Write("+");
	i++;
}
while(i < 5);
Console.WriteLine();
```

Prosty przykład pokazujący różnice między pętlą `while` i `do while`:

```csharp =
int i = 0;
while(i > 0)
{
	Console.Write(i);
	i++;
}
Console.WriteLine();
//zostanie wypisane:
//nic sią nie wypisze, bo pętla nie wykona się ani razu

int j = 0;
do
{
	Console.Write(j);
	j++;
}
while(j > 0);
Console.WriteLine();
//zostanie wypisane: 01234567891011121314151617181920212223...
//pętla nieskończona, będzie się wykonywać nieskończoną ilość razy
//UWAŻAĆ NA TAKIE PĘTLE!!!
```

### for
Jeden z najczęściej stosowanych rodzajów pętli. Składa się ze słowa kluczowego `for`, ujętego w nawias okrągły wyrażenia inicjalizacyjnego (inicjalizacja zmiennej użytej jako iterator pętli), warunku (wrażenie typu `bool`) i kroku pętli (instrukcje wykonywane po wykonaniu bloku kodu między klamrami) oddzielonych od siebie średnikami i ujętego w klamry bloku kodu:

```csharp =
for(inicjalizacja; warunek; krok)
{
	//blok kodu
	//zrób coś tak długo jak warunek jest spełniony
}
```

Pętla działa w następujący sposób:
1. Wykonanie instrukcji inicjalizacji.
2. Obliczenie warunku, sprawdzenie jego prawdziwości.
3. Jeżeli warunek jest prawdziwy (ma wartość równą `true`) to patrz punkt 4., jeżeli nie (ma wartość równą `false`) to patrz punkt 7.
4. Wykonanie bloku kodu.
5. Wykonanie instrukcji kroku.
6. Obliczenie na nowo warunku i powrót do punktu 3.
7. Gdy warunek równy `false` blok kodu nie jest więcej wykonywany, a pętla kończy swoją pracę (przechodzimy do kodu po pętli).

Przykład z poprzednich rodzajów pętli możemy również wykonać przy pomocy pętli `for` i jest to najlepszy wybór dla tego przypadku:

```csharp =
// Program wypisujący w konsoli pięć znaków plus (+++++)
for(int i = 0; i < 5; i++;)
{
	Console.Write("+");
}
Console.WriteLine();
```

Pętlę `for` stosujemy najczęściej jeżeli z góry wiemy ile razy pętla ma się wykonać. Pisząc instrukcję `for` możemy pominąć dowolny element z nawiasu okrągłego (inicjalizacje, warunek i/lub krok) pozostawiając jedynie średniki. Jeżeli nie podamy warunku, stworzymy pętlę nieskończoną (pętla `for( ; ; ){//kod}` będzie się wykonywać w nieskończoność).

### foreach
Pętla przeznaczona do iterowania po elementach kolekcji. Składa się ze słowa kluczowego `foreach`, ujętych w nawias okrągły definicji zmiennej przechowującej element kolekcji, słowa kluczowego `in` i kolekcji po której chcemy iterować. Na końcu umieszczony jest ujęty w klamry blok kodu:

```csharp =
foreach(var element in kolekcja)
{
	//blok kodu
	//zrób coś na kolejnych elementach kolekcji, dopóki kolekcja się nie skończy
}
```

Ten typ pętli zostanie dokładniej omówiony w lekcji 10, gdy poznamy tablice.

## [LEKCJA 9 – Instrukcje skoku](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-9-instrukcje-skoku/)
Instrukcje składające się z określonego słowa kluczowego i średnika. Najogólniej mówiąc służą do przenoszenia do innej części kodu. Najczęściej stosuje się je w połączeniu z instrukcją warunkową `if`. Wyróżniamy następujące instrukcje skoku:

### break
Stosowana głównie w pętlach, gdy istnieje potrzeba przerwania działania pętli, nawet jeśli warunek zakończenia pętli nie został jeszcze spełniony (warunek dalej jest równy `true`). Kiedy program napotyka na instrukcję `break` wykonywana właśnie pętla (lub inna instrukcja np. `switch`) zostaje natychmiast przerwana (przeskakujemy do kodu wykonywanego po danej instrukcji). Weźmy np., użytą w poprzedniej lekcji do pokazania różnicy między pętlą `while` i `do while`, pętlę nieskończoną `do while` wypisującą podaną liczbę i kolejne liczby, jeżeli były one dodatnie. Użyjmy instrukcji `break`, aby wypisywać tą pętlą nie więcej niż 10 cyfr:

```csharp =
Console.Write("Podaj liczbę: ");
int j = Int32.Parse(Console.ReadLine());
int i = 0;
do
{
	Console.Write(j);
	if( i == 9) break;
	i++;
	j++;
}
while(j > 0);
Console.WriteLine();
//np. dla j = 0
//zostanie wypisane: 0123456789
//a dla j = 5
//zostanie wypisane: 567891011121314
//a dla j = -1
//zostanie wypisane: -1
```

Teraz powyższa pętla nie jest już pętlą nieskończoną.

### continue
Stosowana w pętlach, gdy istnieje potrzeba pominięcia części kodu w bloku kodu pętli. Po napotkaniu tej instrukcji program nie wykonuje następujących po niej instrukcji bloku kodu, a przeskakuje prosto do sprawdzenia warunku wykonywania pętli. Np.:

```csharp =
for(int i = 0; i < 10; i++)
{
	Console.Write("+");
	
	if (i%2 == 1) continue;
	
	Console.WriteLine(";");
	Console.Write(i/2);
}
//program wypisze w konsoli:
//	+;
//	0++;
//	1++;
//	2++;
//	3++;
//	4+
```

Oczywiście powyższy program niema większego sensu i można go zaprogramować z użyciem samej instrukcji warunkowej (bez instrukcji `continue`), ale chodziło nam tu tylko o pokazanie działania instrukcji `continue`.

### goto
Służy do przejścia w zupełne inne miejsce w kodzie. Składa się ze słowa kluczowego `goto` i nazwy etykiety oraz etykiety umieszczonej w kodzie w miejscu do którego chcemy się przenieść. Program po natrafieniu na instrukcję `goto` przeskakuje do miejsca w kodzie gdzie znajduje się etykieta o podanej nazwie i nie wraca już do miejsca w kodzie gdzie znajduje się instrukcja `goto`:

```csharp =
...
goto Etykieta;
.
.
.
Etykieta:
//kod wykonywany po napotkaniu instrukcji goto lub gdy program dotarł do niego w skutek normalnego wykonywania kolejnych instrukcji
```

Instrukcja `goto` jest pozostałością po językach niższego poziomu i nie jest stosowana we współczesnym programowaniu. Jeżeli jest to tylko możliwe (a praktycznie zawsze jest) unikaj więc jej stosowania, gdyż takie skakanie po kodzie zmniejsza jego spójność i czytelność.

### return
Stosowana głównie w metodach. Zostanie omówiona później w tym tygodniu w lekcji dotyczącej metod (Lekcja 14).

### throw
Stosowana do wyrzucania wyjątków. Zostanie omówiona później w kursie, przy okazji omawiania obsługi wyjątków.

## [LEKCJA 10 – Tablice](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-10-tablice/)
Obiekty służące do przetrzymywania z góry określonej liczby danych tego samego typu. Klasą bazową dla tablic jest zawsze klasa `System.Array` (nie zależnie od tego jakiego typu dane przechowuje tablica). 

### Tworzenie tablic
Tablicę możemy sobie wyobrazić jako mebel na książki. Budując mebel (tablicę) z góry wiemy ile książek (danych) danego typu chcemy w nim umieścić - jakie wymiary ma mieć tablica. Wiemy też, czy ma to być półka (tablica jednowymiarowa), czy może regał (tablica dwuwymiarowa), albo mebel jeszcze innego typu (o innej liczbie wymiarów), czyli z góry wiemy ile wymiarów ma nieć nasza tablica. Oczywiście jeśli jest to regał to wiemy nie ile książek łącznie ma się na nim zmieścić, ale raczej ile ma mieć półek i ile książek zmieści się na każdej półce, a jeżeli jest to cała biblioteka z przesuwnymi regałami, to ile takich regałów ma się w niej znajdować. Inaczej mówiąc budując tablicę podajemy wielkość każdego jej wymiaru. 
#### 1. Deklaracja
Deklaracja zmiennej typu tablicowego składa się z typu danych jakie będziemy w niej przechowywać, nawiasu kwadratowego z wymiarami tablicy oddzielonymi przecinkami i nazwy zmiennej. Spowoduje to utworzenie zmiennej na stosie, ale bez rezerwacji miejsca na stercie (czyli nie możemy w niej jeszcze niczego przechowywać - na razie zmówiliśmy mebel, wiemy już czy będzie to półka, szafa, czy jeszcze co innego, ale na razie nikt jej nie buduje, więc nie musimy jeszcze znać jej dokładnych wymiarów). Na tym etapie nie musimy jeszcze znać konkretnych wartości jakie będzie mieć każdy z wymiarów, musimy jednak zaznaczyć ile tych wymiarów będzie. To znaczy w nawiasie kwadratowym nie wpisujemy liczb, ale musimy pozostawić przecinki.
#### 2. Inicjalizacja
Inicjalizację pętli wykonuje się z użyciem słowa kluczowego `new`, po którym następuje typ przechowywanych w niej danych i nawias kwadratowy z podanymi wielkościami wymiarów, oddzielonymi od siebie przecinkami (podajemy dokładne wymiary i mebel zostaje zbudowany). Tym razem musimy określić z ilu konkretnie elementów będzie się składał każdy z wymiarów (podać nieujemne wartości typu `int`). Tablic została już utworzona (zarezerwowano pamięć na stercie - mebel jest już zbudowany), ale na razie jest pusta. Pusta, to znaczy jeżeli dany typ danych przechowywany w tablicy może przyjmować wartość `null`, to właśnie taką wartość ma każdy z elementów tablicy. Gdy natomiast dany typ nie może być równy `null`, to każdy z elementów tablicy przyjmuje wartość domyślną dla danego typu (np. dla typu `int` jest to wartość `0`, a dla typu `string` tzw. pusty string - `String.Empty`, czyli inaczej `""`). Teraz powinniśmy więc wypełniać tablicę żądanymi wartościami.
#### 3. Wypełnienie tablicy danymi
Możemy to zrobić na kilka sposobów:
1. razem z inicjalizacją, czyli po nawiasie kwadratowym w klamrach podać wartości wszystkich kolejnych elementów oddzielone od siebie przecinkami. Jeżeli tablica jest wielowymiarowa to będziemy zagnieżdżać w sobie nawiasy klamrowe. Np. dla tablicy dwuwymiarowej piszemy nawias klamrowy w jego wnętrzu wstawiamy nawiasy klamrowe oddzielone przecinkami - tyle nawiasów klamrowych ile mamy rzędów (wielkość pierwszego wymiaru), a w każdym z tych nawiasów wypisujemy tyle przedzielonych przecinkami wartości ile rzędów ma nasza tablica (wielkość drugiego wymiaru). Czyli wracając do naszego przykładu z regałem - zewnętrzne klamry będą naszym regałem. Wewnątrz regału mamy półki (nawiasy klamrowe) które są porozdzielane (przecinki). Na każdej półce znajdują się książki (wartości jakie chcemy wpisać do tablicy), które również są pooddzielane przecinkami (nie chcemy przecież aby książki na półce skleiły się w jedną całość). Przy tym sposobie możemy pominąć podawanie wielkości tablicy w wyrażeniu inicjalizacyjnym, gdyż zostanie ona sprecyzowana przez ilość wartości które przypisujemy.
2. po inicjalizacji, w dowolnym miejscu programu. Wpisujemy wartość do konkretnej komórki tabeli. W tym celu podajemy nazwę tabeli, ujęty w nawias kwadratowy indeks, znak przypisania (`=`) i wartość jaką chcemy przypisać. Indeks mówi nam w którym miejscu tabeli mamy wpisać daną wartość (na której półce, w którym miejscu mam wsadzić tą książkę). Jeżeli tablica jest wielowymiarowa, to indeksy poszczególnych wymiarów rozdzielamy przecinkami. Indeksację w tablicach zaczynamy od zera tzn. pierwszy element w tablicy jednowymiarowej będzie miał indeks `[0]`, w dwuwymiarowej `[0, 0]`, t trójwymiarowej `[0, 0, 0]` itd. Możemy oczywiście ręcznie podawać indeksy wszystkich kolejnych elementów tablicy, ale przy wypełnianiu najczęściej robi się to przy pomocy pętli `for`, gdzie za indeks służy zmienna będąca iteratorem pętli. Jeżeli tablica jest wielowymiarowa, to stosuje się zagnieżdżone pętle 'for'. Przypisanie wartości do konkretnej komórki tabeli stosuje się też później w programie do zmian wartości konkretnej komórki.

Pokażmy więc przykład tablicy jedno-, dwu- i trójwymiarowej:

```csharp =
string[] animals;
animals = new string[10];
animals[0] = "cat";
animals[1] = "dog";
animals[2] = "dolphin";
animals[3] = "monkey";
animals[4] = "duck";
animals[5] = "bat";
animals[6] = "hamster";
animals[7] = "rat";
animals[8] = "wolf";
animals[9] = "tiger";

//number of animals in shelter1 - first column, and in shelter2 - second column
int[,] qtty = new int[10, 2] { { 2, 5 }, { 3, 8 }, { 4, 10 }, { 6, 2 }, { 9, 15 }, { 1, 0 }, { 5, 7 }, { 8, 0 }, { 0, 1 }, { 0, 0 } };

//animal index in animals, animal qtty from qtty for shelter1 and shelter2
int[,,] animalQtty = new int[2, 10, 2];
for(int i = 0; i < 2; i++)
{
	//shelter i
	for(int j = 0; j < 10; j++)
	{
		animalQtty[i, j, 0] = j;
		animalQtty[i, j, 1] = qtty[j, i];
	}
}

for(int i = 0; i < 2; i++)
{
	Console.WriteLine("--------------------------------------------------------------");
	Console.WriteLine("Shelter" + (i+1));
	for(int j = 0; j < 10; j++)
	{
		Console.Write(animals[j] + ":\t");
		Console.WriteLine(qtty[j, i]);
	}

}
Console.WriteLine("--------------------------------------------------------------");

/*output:
--------------------------------------------------------------
Shelter1
cat:    2
dog:    3
dolphin:        4
monkey: 6
duck:   9
bat:    1
hamster:        5
rat:    8
wolf:   0
tiger:  0
--------------------------------------------------------------
Shelter2
cat:    5
dog:    8
dolphin:        10
monkey: 2
duck:   15
bat:    0
hamster:        7
rat:    0
wolf:   1
tiger:  0
--------------------------------------------------------------
*/
int[] qtty1 = new int[10], qtty2 = new int[10], indexes1 = new int[10], indexes2 = new int[10];
for(int i = 0; i < 10; i++)
{
	qtty1[i] = qtty[i, 0];
	qtty2[i] = qtty[i, 1];
	indexes1[i] = i;
	indexes2[i] = i;
}
Array.Sort(qtty1, indexes1);
Array.Sort(qtty2, indexes2);

int[,,] animalsQttySorted = new int[2, 10, 2];
for( int i = 0; i < 10; i++)
{
	for (int j = 0; j < 2; j++)
		animalsQttySorted[0, i, j] = animalQtty[0, indexes1[i], j];
}
for (int i = 0; i < 10; i++)
{
	for (int j = 0; j < 2; j++)
		animalsQttySorted[1, i, j] = animalQtty[1, indexes2[i], j];
}
Console.WriteLine("\n\nAfter sorting");
for (int i = 0; i < 2; i++)
{
	Console.WriteLine("--------------------------------------------------------------");
	Console.WriteLine("Shelter" + (i+1));
	for (int j = 0; j < 10; j++)
	{
		Console.Write(animals[animalsQttySorted[i, j, 0]] + ":\t");
		Console.WriteLine(animalsQttySorted[i, j, 1]);
	}
}
Console.WriteLine("--------------------------------------------------------------");
/*output:


After sorting
--------------------------------------------------------------
Shelter1
wolf:   0
tiger:  0
bat:    1
cat:    2
dog:    3
dolphin:        4
hamster:        5
monkey: 6
rat:    8
duck:   9
--------------------------------------------------------------
Shelter2
bat:    0
rat:    0
tiger:  0
wolf:   1
monkey: 2
cat:    5
hamster:        7
dog:    8
dolphin:        10
duck:   15
--------------------------------------------------------------
*/

```

### Właściwości i metody klasy `System.Array`
Jak już wspomnieliśmy klasą bazową dla wszystkich tablic jest klasa `System.Array`. Możemy więc korzystać ze zdefiniowanych w niej właściwości (ang. _properties_) i metod (ang. _methods_). Jedną z podstawowych  właściwości jest `Length`. Przechowuje ona łączną liczbę wszystkich elementów tablicy. Jej wartość możemy uzyskać podając nazwę tablicy, kropkę i nazwę właściwości (`nazwaTablic.Length`). Możemy z niej korzystać np. iterując po wszystkich elementach tablicy, czy chcąc otrzymać ostatni element tablicy. Jednak **UWAGA**, `Length` zwraca liczbę elementów, czyli to co wpisaliśmy w nawiasie klamrowym przy inicjalizacji (a w przypadku tablicy wielowymiarowej wszystkie wartości przemnożone przez siebie). Natomiast indeksowanie w tablicach rozpoczyna się od zera. Oznacza to, że ostatni element tablicy będzie mieć indeks o jeden mniejszy niż długość (wielkość) tablicy (`tablica[tablica.Length - 1]`). Często stosowanymi metodami są np.:
1. `IndexOf` - metoda zwracająca numer indeksu pierwszego napotkanego elementu tablicy jednowymiarowej o podanej wartości (dokładnie tej wartości włącznie np. z wielkością liter). Stosuje się ją w następujący sposób: `Array.IndexOf(nazwaTablicy, szukanaWartosc)`. Czyli np. chcąc w tablicy `animals` z naszego przykładu sprawdzić pod jakim indeksem jest przechowywana małpa (`"monkey"`) napiszemy:

```csharp =
int monkeyIndex = Array.IndexOf(animals, "monkey");
```

2. `Sort` - metoda sortująca elementy w tablicy jednowymiarowej. Sortowanie odbywa się dla typów numerycznych w kolejności od najmniejszego do największego, dla typów znakowych w kolejności alfabetycznej, a dla innych obiektów według specjalnie zdefiniowanych zasad (implementacja interfejsu `IComparer`). W podstawowej wersji Użycie metody sort wygląda następująco: `Array.Sort(nazwaTablicy)`. Np. gdybyśmy nazwy zwierząt z tablicy `animals` (z naszego przykładu) uporządkować w kolejności alfabetycznej napisalibyśmy:

```csharp = 
//skopiujmy najpierw tablicę
string [] animalsSorted = new string[animals.Length];
animals.CopyTo(animalsSorted, 0); //lub Array.Copy(animals, animalsSorted, animals.Length);
//posortujmy nazwy zwierząt w tablicy animalsSorted
Array.Sort(animalsSorted);
```

3. `Clear` - metoda służąca do czyszczenia ("zerowania") tablicy. Przez czyszczenie rozumie się przypisanie takich wartości jak w przypadku inicjalizacji pustej tablicy (czyli dla typu `int` wartości `0`, dla typu `string` `String.Empty`, dla typów mogących przyjmować wartość `null` wartość `null` itd.). Można stosować ją na dwa sposoby. Aby wyczyścić całą tablicę napiszemy: `Array.Clear(nazwaTablicy);`. Gdy natomiast chcemy wyczyścić tylko część tablicy, możemy to zrobić podając dodatkowo jako parametr indeks elementu od którego chcemy zacząć czyszczenie i liczbę elementów które chcemy wyczyścić (`Array.Clear(nazwaTablicy, indeksElementuOdKtoregoZaczynamyCzyszczenie, liczbaElementowDoWyczyszczenia);`).

Metody `IndexOf` i `Sort` można stosować tylko w przypadku tablic jednowymiarowych. Mogą one przyjmować też inne argumenty (inna liczba i/lub typ). Np. w przykładowym kodzie użyliśmy metody `Sort` przyjmującej dwa argumenty typu `Array` (tablice). Pierwszy przechowuje klucze (identyfikatory), a drugi wartości. Pierwsza tablica zostanie posortowana normalnie, a druga według tej samej kolejności co pierwsza (jeżeli pierwszy element tablicy kluczy stanie się teraz jej trzecim elementem, to pierwszy element tablicy wartości również stanie się jej trzecim elementem). Więcej informacji dotyczących tych metod można znaleźć w dokumentacji ([`IndexOf`](https://learn.microsoft.com/en-us/dotnet/api/system.array.indexof?view=net-7.0), [`Sort`](https://learn.microsoft.com/en-us/dotnet/api/system.array.sort?view=net-7.0), [`Clear`](https://learn.microsoft.com/en-us/dotnet/api/system.array.clear?view=net-7.0)). Można tam też znaleźć więcej właściwości i metod klasy [`System.Array`](https://learn.microsoft.com/en-us/dotnet/api/system.array?view=net-7.0), np. użytą w przykładzie `Sort` metodę do kopiowania tablic [`CopyTo`](https://learn.microsoft.com/en-us/dotnet/api/system.array.copyto?view=net-7.0) lub [`Copy`](https://learn.microsoft.com/en-us/dotnet/api/system.array.copy?view=net-7.0).

### Operatory indeksu i zakresu
Począwszy od C# 8.0 wprowadzono nowe operatory do obsługi kolekcji.

#### Operator `^` - `System.Index` od końca
Prefix `^` użyty wraz z numerem indeksu, oznacza numer indeksu liczony od końca kolekcji. `^indeks`, oznacza więc to samo co `dlugoscKolekcji - indeks`.

Na przykład:
```csharp =
var array = new int[] { 1, 2, 3, 4, 5 };
var thirdItem = array[2];    // array[2]
var lastItem = array[^1];    // array[new Index(1, fromEnd: true)], czyli array[array.Length - 1], czyli array[4]

//wartosci array:			 1	 2	 3	 4	 5
//numer indeksu:			 0	 1	 2	 3	 4
//numer indeksu od konca:	^5	^4	^3	^2	^1
```

Zachowanie tego operatora jest zdefiniowane tylko dla wartości większych lub równych zero. Należy pamiętać, że indeksy `^0` i `^n`, gdzie `n` jest większe niż długość kolekcji, będę wskazywać na wartości spoza zakresu (za ostatnim lub przed pierwszym elementem kolekcji) - wyjątek `IndexOutOfRangeException`.

#### Operator zakresu `x..y`
Stosuje się go jako uproszczony zapis, który wywołuje odpowiednią metodę struktury `System.Range`. `x` w powyższym zapisie oznacza indeks początkowy, a `y` indeks końcowy zakresu. Przy czym obiekt o indeksie `x`, będzie znajdował się w tworzonym fragmencie kolekcji, natomiast obiekt o indeksie `y` nie. Zarówno argument `x` jak i `y` może być w tym zapisie pominięty. Pominięcie `x` oznacza, że wyznaczamy zakres od początku kolekcji, natomiast pominięcie `y`, że do końca.

Pokażmy to na przykładach:
```csharp =
var array = new int[] { 1, 2, 3, 4, 5 };
var slice1 = array[2..3];    // array[new Range(2, 3)], czyli { 3 }
var slice2 = array[..3];     // array[Range.EndAt(3)] lub inaczej array[0..3], czyli array[new Range(0, 3)], czyli { 1, 2, 3}
var slice3 = array[2..];     // array[Range.StartAt(2)] lub inaczej array[2..array.Length], czyli array[new Range(2, array.Length)], czyli { 3, 4, 5 }
var slice4 = array[..];      // array[Range.All], czyli cała tablica array, { 1, 2, 3, 4, 5 }
```

Możemy również łączyć oba operatory. Np.:
```csharp =
var array = new int[] { 1, 2, 3, 4, 5 };
var slice1 = array[2..^3];    // array[new Range(2, new Index(3, fromEnd: true))], czyli { } pusta kolekcja
var slice2 = array[..^3];     // array[Range.EndAt(new Index(3, fromEnd: true))], czyli { 1, 2 }
var slice3 = array[^2..];     // array[Range.StartAt(new Index(2, fromEnd: true))], czyli { 4, 5 }
var slice4 = array[0..^0];    // array[Range.All], czyli cała tablica array, { 1, 2, 3, 4, 5 }
```

Jeżeli indeks końcowy jest mniejszy niż początkowy, to zostanie wyrzucony wyjątek `IndexOutOfRangeException`.

### Operatory warunkowe null `?.` i `?[]`
W C# 6.0 wprowadzono operatory warunkowe null (ang. _null-conditional operators_) odnoszące się do operacji dostępu do składowej (`?.`) i elementu (`?[]`) obiektu. 

Budowa wyrażeń: `a?.x`, `a?[x]`

Działanie:
* Jeżeli `a` ma wartość `null`, to wyrażenia `a?.x` i `a?[x]` mają wartość `null`
* Jeżeli `a` ma wartość różną od `null`, to wyrażenie `a?.x` ma wartość wyrażenia `a.x`, natomiast wyrażenie `a?[x]` wyrażenia `a[x]`.

Np.:

```csharp =
string[] animals;
string pet = animals?[1]; // pet jest równe null

string[] pets = new string[3];
animals?.CopyTo(pets, 0); // wyrażenie ma wartość null i nic nie zostanie skopiowane

animals = new string[] {'dog', 'cat', 'parrot'};
pet = animals?[1]; // pet jest równe 'cat'
animals?.CopyTo(pets, 0); // pets jest równe {'dog', 'cat', 'parrot'}
```

Operatory te przydają się, gdy nie mamy pewności, czy używany obiekt została zainicjalizowany. Często będziemy go stosować w połączeniu z operatorem `??`. Np.:
```csharp =
int GetSumOfFirstTwoOrDefault(int[] numbers)
{
    if ((numbers?.Length ?? 0) < 2)
    {
        return 0;
    }
    return numbers[0] + numbers[1];
}

Console.WriteLine(GetSumOfFirstTwoOrDefault(null));  // output: 0
Console.WriteLine(GetSumOfFirstTwoOrDefault(new int[0]));  // output: 0
Console.WriteLine(GetSumOfFirstTwoOrDefault(new[] { 3, 4, 5 }));  // output: 7
```

## [LEKCJA 11 – Listy](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-11-listy/)
Klasa `System.Collections.Generic.List` należy do tzw. typów generycznych. Oznacza to, że lista jest zawsze obiektem typu `List`, ale może przetrzymywać obiekty innego typu, który definiujemy podczas inicjalizacji. Podobnie jak tablice, listy służą do przechowywania wielu obiektów tego samego typu. Różnice między tymi dwoma typami powodują jednak, że stosuje się je w różnych sytuacjach.

### Różnice między listami a tablicami
#### 1. Tworzenie obiektów danego typu
Listy deklarujemy podając słowo kluczowe `List`, następnie w nawiasach ostrych (`<>`) podajemy typ jaki przetrzymywać będzie dana lista, a na koniec podajemy nazwę naszego obiektu. Wyrażenie inicjalizacyjne, jak w przypadku każdego obiektu, rozpoczyna się od słowa kluczowego `new`, następnie podajemy nazwę klasy do której należy nasz obiekt (`List`), w nawiasach ostrych typ jaki będzie przechowywać ta lista i na koniec nawias okrągły (wywołanie konstruktora klasy - więcej o konstruktorach w lekcji 2 kolejnego tygodnia). Nasza lista jest już utworzona. W przypadku list, podczas inicjalizacji nie podajemy jej rozmiaru. Jak tworzy się tablice omówiliśmy w poprzedniej lekcji. Dla przypomnienia pokażmy jeszcze jak tworzy się tablice i porównajmy to z tworzeniem listy, na przykładzie obiektów obu typów przechowujących `int`y:

```csharp =
//TABLICA
int[] tablica = new int[10]; //musimy z góry określić ile elementów będzie przechowywać nasza tablica i nie powinniśmy później zmieniać jej rozmiarów

//LISTA
List<int> lista = new List<int>();
```

Oczywiście jak zawsze inicjalizacji listy nie musimy przeprowadzać razem z deklaracją. Równie dobrze możemy to zrobić w dalszej części kodu. Jeśli jest to możliwe warto to jednak zrobić od razu, inaczej trzeba uważać, aby nie próbować używać obiektu, który nie został jeszcze zainicjalizowany.
#### 2. Sposób alokowania pamięci
**Tablice** są uporządkowanymi (ciągłymi) strukturami alokowanymi statycznie w momencie inicjalizacji. Jest rezerwowany obszar pamięci mogący pomieścić całą tablicę. Dla tego musimy z góry znać jej rozmiar i podać go przy inicjalizacji. Nie powinniśmy też tego rozmiaru później zmieniać. Dzięki temu dodawanie, usuwanie i odczytywanie wartości pod określonym indeksem jest w tablicach bardzo szybkie. Musimy jednak z góry wiedzieć ile wartości będziemy przechowywać, a rozmiar naszej tablicy będzie taki sam niezależnie ile z pośród tych wartości znamy już w tym momencie.

**Listy** mają z kolei bardziej rozrzuconą (nieciągłą) strukturę i są alokowane w sposób dynamiczny. Oznacza to, że w momencie inicjalizacji nie musimy wiedzieć ile obiektów będziemy chcieli w nich przechowywać. Możemy też w dowolnym momencie zmieniać rozmiar naszej listy (dodawać nowe obiekty, usuwać istniejące). Przy czym usuwanie obiektów w tablicy oznacza usunięcie ich z pamięci (zwolnienie pamięci, w odróżnieniu od tablicy, w której oznaczało to przypisanie obiektowi wartości domyślnej). Dzięki sposobowi alokacji list w pamięci, możemy już po inicjalizacji zarezerwować dla nich więcej pamięci lub zwolnić dowolną część z tej już przez nią zajmowanej. Daje to programiście większą swobodę działania, ale wiąże się ze zmniejszeniem szybkości dodawania, usuwania i odczytywania wartości danego obiektu. Jeżeli więc wiemy ile obiektów będziemy chcieli przechowywać i wartość ta jest niezmienna w czasie trwania programu, to warto stosować tablice, ze względu na szybkość ich działania. Ponieważ jednak listy należą do typów generycznych, więc mamy dostęp do większej liczby mechanizmów służących m.in. do sortowania, czy wyszukiwania elementów w liście.

| Cecha | Tablica | Lista |
| :--- | :---: | :---: |
| Tworzenie | Oryginalna struktura danych niższego poziomu, oparta o ideę indeksacji | Bazuje na tablicy |
| Sposób alokowania pamięci | Statyczny | Dynamiczny |
| Zajętość pamięci | Wydajna | Znacznie większa - każdy element (węzeł) ma swoją strukturę pamięci |
| Struktury | Uporządkowana - alokacja pamięci w sposób ciągły, gdzie najniższy adres wskazuje na pierwszy element, a najwyższy na ostatni | Nieuporządkowana - alokowana pamięć nie jest ciągła, a rozrzucona |
| Rozmiar | Stały, ustalony podczas inicjalizacji | Zmienny |
| Zmiana rozmiaru | Nie należy zmieniać (jest to bardzo kosztowne) | Można dowolnie zmieniać (zwiększać, zmniejszać) - listy są dynamiczne z natury |
| Indeksacja | Struktura oparta na indeksacji, gdzie najmniejszy adres jest pierwszy, a największy ostatni | Struktura nie oparta na indeksacji, ale na idei węzłów (ang. _nodes_) (każdy węzeł jest alokowany niezależnie i poza przechowywaniem danych wskazuje miejsce w pamięci gdzie przechowywany jest kolejny węzeł) |
| Szybkość dostępu do przechowywanych obiektów (dodawanie, usuwanie, odczytywanie wartości) | Duża, niezależna od pozycji obiektu (numeru indeksu - który to jest obiekt w tablicy) | Mała, operacja czasochłonna, zależna od pozycji obiektu (dotarcie do każdego kolejnego obiektu zajmie więcej czasu) |
| Typ dostępu do przechowywanych obiektów | Bezpośredni i sekwencyjny (można bezpośrednio uzyskać dostęp do obiektu o wybranym indeksie, adres jest obliczany poprzez dodanie do adresu pierwszego elementu odpowiedniej liczby wskazanej przez typ obiektu i numer indeksu, albo dostać się do każdego obiektu po kolei) | Tylko sekwencyjny (dostęp do obiektu można uzyskać tylko przechodząc przez wszystkie poprzedzające go obiekty) |
| Typ generyczny | Nie | Tak |
| Efektywność sortowania, przeszukiwania - liczba dostępnych metod | Mała | Duża |
| Zastosowanie | Częsty dostęp do przechowywanych obiektów, znany stały rozmiar | Częste dodawanie i usuwanie obiektów, zmienność rozmiaru |
| Wymiary | Może być wielowymiarowa (wsparcie dla obsługi wielowymiarowości) | Jednowymiarowa (można stworzyć pseudo wielowymiarową listę, tworząc listę list - "dwa wymiary", listę list list - "trzy wymiary" itd.|

### Operacje na listach

#### Dodawanie elementów

##### 1. Metoda `Add`
Metoda klasy `System.Collections.Generic.List` pozwalająca dodać jeden nowy obiekt na końcu listy (za jej ostatnim dotychczasowym elementem - pierwszy element jeśli lista była pusta). Np.:

```csharp =
List<int> list = new List<int>();
for (int i = 0; i < 5; i++)
	list.Add(i);
foreach (int i in list)
	Console.WriteLine(i);
/*Zostanie wypisane:
	0
	1
	2
	3
	4
*/
```

##### 2. Metoda `AddRange`
Metoda klasy `System.Collections.Generic.List` pozwalająca dodać istniejącą listę na końcu naszej listy. Np. do utworzonej przed chwilą listy dodajmy kolejne wartości:

```csharp =
list.AddRange(new List<int>() { 10, 11, 12, 13 });
for(int i = 0; i < list.Count; i++)
    Console.WriteLine(list[i]);
/*Zostanie wypisane:
	0
	1
	2
	3
	4
	10
	11
	12
	13
*/
```

##### 3. Metoda `Insert`
Metoda klasy `System.Collections.Generic.List` pozwalająca dodać jeden nowy obiekt w wybranym miejscu listy. Jako parametry tej metody podajemy numer indeksu pod którym ma się znaleźć nowy element, oraz dodawany obiekt. Numer indeksu jest liczbą całkowitą nie mniejszą od zera i nie większą od liczby elementów w liście (wskazanej przez właściwość `Count`). Np. do tworzonej przez nas listy dodajmy liczbę `5` po liczbie `4`:

```csharp =
int index = list.IndexOf(4);
list.Insert(index + 1, 5);
for(int i = 0; i < list.Count; i++)
    Console.WriteLine(list[i]);
/*Zostanie wypisane:
	0
	1
	2
	3
	4
	5
	10
	11
	12
	13
*/
```

##### 4. Metoda `InsertRange`
Metoda klasy `System.Collections.Generic.List` pozwalająca dodać listę obiektów w wybranym miejscu naszej listy. Jako parametry tej metody podajemy numer indeksu pod którym ma się zaczynać dodawana lista, oraz dodawana list. Numer indeksu jest liczbą całkowitą nie mniejszą od zera i nie większą od liczby elementów w liście (wskazanej przez właściwość `Count`). Np. do tworzonej przez nas listy dodajmy listę `new List<int>() { 6, 7, 8, 9 }`, zaraz za dodaną przed chwilą cyfrą `5`:

```csharp =
list.InsertRange(index + 2, new List<int>() { 6, 7, 8, 9 });
foreach (int i in list)
	Console.WriteLine(i);
/*Zostanie wypisane:
	0
	1
	2
	3
	4
	5
	6
	7
	8
	9
	10
	11
	12
	13
*/
```

#### Uzyskiwanie dostępu do elementów
Jak pokazano w powyższych przykładach dostęp do konkretnego elementu listy można uzyskać tak samo jak w przypadku tablic podając nazwę listy i otoczony nawiasami kwadratowymi numer indeksu. Można w ten sposób zarówno uzyskać aktualnie przechowywany w tym miejscu obiekt, zmodyfikować go lub przypisać nowy obiekt. Można również wyszukać w liście obiekt(y) spełniający(e) określone warunki. Do tego służą różne metody _find_ klasy `List`. Więcej na temat tych i innych metod i właściwości można znaleźć w [dokumentacji klasy `List`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1?view=net-7.0).

#### Sortowanie elementów
Do uporządkowania elementów służy metoda `Sort` klasy `List`. Jej wywołanie wygląda następująco: `nazwaTablicy.Sort();`. Działa ona analogicznie jak omówiona w poprzedniej lekcji tablicowa metoda o takiej samej nazwie.

#### Usuwanie elementów
Jak już wspomniano dynamiczna natura list pozwala na łatwe zarówno dodawanie jak i usuwanie jej elementów. Do tego celu klasa `System.Collections.Generic.List` implementuje kilka metod:
##### 1. Metoda `Remove`
Służy do usunięcia z listy konkretnego obiektu, podanego jako argument (pierwszego wystąpienia tego obiektu na liście, czyli jeśli podany obiekt występuje w liści kilka razy, tylko pierwsze wystąpienie zostanie usunięte, a reszta pozostanie bez zmian). Np. gdybyśmy z naszej przykładowej listy chcieli usunąć `10`, napisalibyśmy:

```csharp =
list.Remove(10);
foreach (int i in list)
	Console.WriteLine(i);
/*Zostanie wypisane:
	0
	1
	2
	3
	4
	5
	6
	7
	8
	9
	11
	12
	13
*/
```

##### 2. Metoda `RemoveAll`
Służy do usunięcia z listy wszystkich obiektów spełniających podany warunek. Metoda `RemoveAll` przyjmuje jeden argument typu `Predicate<T>`, gdzie `T` oznacza typ obiektów przechowywanych w liście. Jest to tzw. delegata, która definiuje warunki usunięcia elementów. Więcej o metodach przyjmujących argumenty tego typu dowiemy się w kolejnych lekcjach.

##### 3. Metoda `RemoveRange`
Jak w przypadku dodawania obiektów do listy, usunąć również możemy od razu wiele elementów. Służy do tego metoda `RemoveRange`. Przyjmuje ona dwa argumenty: numer indeksu pierwszego elementu listy, który chcemy usunąć i liczbę elementów jakie chcemy usunąć. Załóżmy więc, że z naszej przykładowej listy chcemy teraz usunąć trzy ostatnie elementy. Napiszemy wówczas:

```csharp =
int startIndex = list.Count - 3;
list.RemoveRange(startIndex, 3);
foreach (int i in list)
	Console.WriteLine(i);
/*Zostanie wypisane:
	0
	1
	2
	3
	4
	5
	6
	7
	8
	9
*/
```

##### 4. Metoda `RemoveAt`
Możemy również usunąć obiekt o określonym indeksie. Do tego służy właśnie metoda `RemoveAt`, przyjmująca jako argument numer indeksu elementu, który chcemy usunąć (liczba całkowita z zakresu od `0` do `nazwaTablicy.Count - 1`). Załóżmy więc, że chcemy teraz usunąć ostatni element z naszej przykładowej listy. Napiszemy więc:

```csharp =
list.RemoveAt(list.Count - 1);
foreach (int i in list)
	Console.WriteLine(i);
/*Zostanie wypisane:
	0
	1
	2
	3
	4
	5
	6
	7
	8
*/
```

#### Wykonanie operacji na każdym elemencie listy
Jeżeli chcemy wykonać jakąś operację na wszystkich elementach listy możemy to zrobić na kilka sposobów. Dla przykładu, pokażemy jak wypisać w konsoli wszystkie elementy listy z powyższych przykładów, każdym z tych sposobów.
##### 1. Z użyciem iteratora
Jak już wiemy możemy uzyskać dostęp do konkretnego elementu listy używając "notacji tablicowej", czyli podając w nawiasie kwadratowym numer indeksu. Wiemy też, że możemy użyć pętli, aby uzyskać wartości kolejnych indeksów. Najczęściej będzie to stosowana już w powyższych przykładach pętla `for`:

```csharp =
for(int i = 0; i < list.Count; i++)
{
	Console.WriteLine(list[i]);
}
```

Choć oczywiście można również użyć pętli `while`:

```csharp =
int i = 0;
while(i < list.Count)
{
	Console.WriteLine(list[i++]);
}
```

##### 2. Z użyciem pętli `foreach`
Jak już wiemy pętla ta służy do uzyskiwania dostępu do każdego kolejnego elementu kolekcji. Zastosowaliśmy ją już w kilku przykładach powyżej, ale dla przypomnienia:
```csharp =
foreach(var item in list)
{
	Console.WriteLine(item);
}
```

Słowo kluczowe `var` oznacza zmienną dowolnego typu. Zmienna taka przyjmuje typ obiektu (literału), który został do niej przypisany podczas inicjalizacji. W naszym przypadku będzie to `int`, ale gdyby lista przechowywała obiekty innego typu, to byłby to ten inny typ. W powyższym przykładzie można też oczywiście podać typ elementu jawnie, czyli w naszym przypadku, zamiast słowa kluczowego `var`, napisać `int`.

##### 3. Z użyciem metody `ForEach`
Klasa `System.Collections.Generic.List` definiuje również specjalnie przeznaczoną do tego celu metodę `ForEach`. Przyjmuje ona jeden argument typu `Action<T>`, gdzie `T` jest typem obiektów naszej listy. Argument ten jest delegatą (metodą) wykonywaną na każdym elemencie listy. Można jej użyć na dwa sposoby:

1. Podając jako argument zdefiniowaną oddzielnie metodę:

```csharp =
list.ForEach(Print);

void Print(int i)
{
    Console.WriteLine(i);
}
```

2. Używając metody anonimowej:

```csharp =
list.ForEach(delegate(int item)
{
    Console.WriteLine(item);
});
```

Więcej o argumentach tego typu dowiemy się w dalszej części kursu.

## [LEKCJA 12 – Enum](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-12-enum/)
Enum należy do wyliczeniowych typów wartościowych. Wspominaliśmy już o nim przy okazji omawiania typów wartościowych. Służy do przechowywania określonych danych słownikowych, których ilość się nie zmienia. Przypomnijmy sobie jak tworzy sią typ `enum`:

```csharp =
public enum Nazwa
{
	Slowo1,
	Slowo2,
	Slowo3
	...
}
```

Enum jest typem statycznym. Nie możemy dodawać nowych wartości do zdefiniowanego `enum`a. Każdej wartości słownikowej w tym typie odpowiada określona liczba typu `int`. Tradycyjnie numerowanie rozpoczyna się od zera (pierwszy element ma wartość `0`, a każdy kolejny o jeden większą niż poprzedni), jednak w enumie możemy odpowiadające wartości `int` zdefiniować samodzielnie. Można to zrobić przypisując żądaną wartość do wybranego elementu (wówczas każdy kolejny element będzie mieć wartość o jeden większą niż poprzedni, a każdy poprzedzający go element wartość o jeden mniejszą niż ten następujący po nim). Można również przypisać wartości wszystkim elementom, wówczas nie muszą być to kolejne liczby, ale każdy element musi mieć inną (unikatową) wartość. Dobrym przykładem zastosowania `enum`a jest stosowany kiedyś powszechnie `enum` przechowujący dni tygodnia:

```csharp =
public enum Weekday
{
	Monday = 1,
	Tuesday,
	Wednesday,
	Thursday,
	Friday,
	Saturday,
	Sunday
}
```

Obecnie nie jest już on tak powszechnie definiowany przez programistów, gdyż nowsze wersje C# mają już podobne wyliczenie predefiniowane ([`System.DayOfWeek`](https://learn.microsoft.com/pl-pl/dotnet/api/system.dayofweek?view=net-7.0), niedziela ma tu wartość `0`, poniedziałek `1` itd.). Można również określić dzień tygodnia na podstawie podanej daty, używając w tym celu właściwości `DayOfWeek` struktury `System.DateTime`, reprezentującej moment w czasie wyrażony jako data i godzina (więcej informacji na temat tej struktury można znaleźć w [dokumentacji](https://learn.microsoft.com/en-us/dotnet/api/system.datetime?view=net-7.0)). Enum dni tygodnia jest jednak dobrym przykładem zastosowania wyliczenia, gdyż mamy tu ściśle określone elementy, co do których mamy pewność, że nie zmienią się w czasie. Zostańmy więc przy tym przykładzie i stwórzmy zmienną tego typu:

```csharp =
Weekday day = Weekday.Friday;
Console.WriteLine("Jest {0} dzień tygodnia, czyli: {1}", (int)day, day);
if (day == Weekday.Friday)
	Console.WriteLine("Piątek, piąteczek, piątunio :-D");
```

Jak widać w powyższym przykładzie możemy dokonać rzutowania zmiennej `enum` na typ `int`, aby otrzymać odpowiadającą wartość `int` lub użyć samej nazwy zmiennej, aby otrzymać wartość słownikową. Podajmy jeszcze dla przykładu wyliczenie ze zdefiniowanymi wszystkimi wartościami `int`:

```csharp =
public enum PeriodsOfTime
{
	Minute = 1,
	Quarter = 15,
	Half = 30,
	Hour = 60,
	Day = 1440
}
```

Typy wyliczeniowe są stosunkowo szybkie w użyciu, jednak mają ograniczone zastosowanie. Należy pamiętać, że nie można ich dynamicznie zmieniać (dopisywać kolejnych wartości), więc ich zastosowanie należy dobrze przemyśleć. Późniejsze zmiany warunków lub wymagań w naszej aplikacji, które będą wiązały się z typami enum będą również oznaczały zmiany w innych częściach kodu programu.

## [LEKCJA 13 – Klasy i obiekty](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-13-klasy-i-obiekty/)
Klasy są podstawą języka C#, ponieważ jest to język obiektowy. Z założenia wszystkie jego elementy są więc obiektami.

**Klasy** - definicje dla obiektów jakiegoś typu (tej klasy).

**Obiekty** - instancje danej klasy.

### Klasa
Każda klasa ma swoje cechy (właściwości - ang. _properties_) i zachowania (metody - ang. _methods_). Klasę tworzymy podając modyfikator dostępu, ewentualne dodatkowe modyfikatory (np. `static`, jeśli ma być to klasa statyczna), słowo kluczowe `class`, nazwę i w klamrach jej właściwości i metody. Klasa będzie nam więc opisywać jakie cechy posiada każdy obiekt danego typu (wartość tej cechy będzie indywidualna dla każdego obiektu, ale fakt jej posiadania pozostaje niezmienny) oraz jakie czynności może wykonywać (lub czasem jakie czynności można na nim wykonywać). Stwórzmy więc przykładową klasę `Dog`, w której postaramy się opisać psa. Każdy pies ma określone cechy, takie jak np. imię, rasa, wiek, płeć, waga, wysokość, długość, umaszczenie, długość sierści itd. Każdy pies wykonuje też pewne, takie same czynności, np.: je, śpi, wychodzi na spacer, bawi się. Spróbujmy więc to zapisać:

```csharp =
public class Dog
{
	// Properties
	public string Name { get; set; }
	public string Breed { get; set; }
	public int Age { get; set; } // in years
	public string Gender { get; set; } // Male, Female
	public decimal Weight { get; set; } // in kg
	public decimal Height { get; set; } // height at the withers in cm
	public decimal Length { get; set; } // in cm
	public string Color { get; set; }
	public CoatLength Fur { get; set; }

	// Methods
	public void Eat(string foodName, decimal quantity) // quantity in grams
	{
		Console.WriteLine($"{this.Name} is eating {quantity} grams of {foodName}.");
		this.Weight += 0.9M * quantity / 1000;
	}
	public void Sleep(int hours)
	{
		Console.WriteLine("{0} is sleeping. It will take {1} {2} hours to wake up.", this.Name, (this.Gender == "Male" ? "him" : "her"), hours);
		this.Weight -= hours * 0.001M;
	}
	public void Walk(int length) // in minutes
	{
		Console.WriteLine("{0} is happy, because {1} went on {2} minutes walk.", this.Name, (this.Gender == "Male" ? "he" : "she"), length);
		this.Weight -= (length * 0.001M + this.Weight * 0.005M);
	}
	public void Play(string playName, int length) //length in minutes
	{
		Console.WriteLine("{0} is happy, because {1} is playing {2} for the next {3} minutes.", this.Name, (this.Gender == "Male" ? "he" : "she"), playName, length);
		this.Weight -= length * 0.002M;
	}
}
public enum CoatLength
{
	Shorthaired,
	Longhaired,
	Hairless,
	Combination
}
```

Stwórzmy teraz psa (konkretny egzemplarz).

### Obiekt
Jak już wiemy klasa, jest to opis całej grupy. Obiekt będzie zaś konkretnym wystąpieniem (egzemplarzem, instancją) tej grupy (klasy). Stwórzmy więc naszego psa. Nasz pies to czarny, pięcioletni kundel, o imieniu Torpeda. Torpeda jest niewielką, energiczną suczką ważącą 5kg i mierzącą 25 cm wysokości w kłębie i 33 cm długości. Jej sierść ma mieszaną długość, gdzieniegdzie jest dłuższa, a w innych miejscach krótsza. Spróbujmy to zapisać:

```csharp =
Dog dog = new Dog();
dog.Color = "black";
dog.Age = 5;
dog.Breed = "mongrel";
dog.Name = "Torpeda";
dog.Gender = "Female";
dog.Weight = 5;
dog.Height = 25;
dog.Length = 33;
dog.Fur = CoatLength.Combination;
```

Pozwólmy też Torpedzie wykonać kilka czynności. Nakarmmy ją, wyprowadźmy na spacer, pobawmy się z nią chwilę i pozwólmy jej odpocząć:

```csharp =
dog.Eat("dry food", 150);
dog.Walk(30);
dog.Play("tug of war", 10);
dog.Sleep(5);

Console.WriteLine("Dog weight after activities: " + dog.Weight);
```

Kiedy będziemy operować na danych z bazy danych, będziemy je na ogół tłumaczyć na obiekty klasy. Później będziemy mogli nimi manipulować przy użyciu metod danej klasy i zmiany właściwości obiektów.

## [LEKCJA 14 – Metody](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-14-metody/)
Jak wspomnieliśmy w poprzedniej lekcji, klasy składają się z właściwości i metod.

Współcześnie często tworzymy osobne klasy z samymi właściwościami, gdyż są one najczęściej odzwierciedleniem bazy danych, więc ma to sens, aby nie zawierały żadnych metod.

### Klasy serwisowe
Do manipulacji danymi przechowywanymi w tych klasach tworzy się najczęściej tzw. klasy serwisowe. Nadaje się im najczęściej nazwy takie jak nazwa klasy podstawowej z dodanym na końcu słowem `Service`. Klasy serwisowe posiadają najczęściej obiekt lub listę obiektów klasy podstawowej oraz metody do manipulacji nimi. 

### Metody
Wróćmy jednak do metod. Służą one do wykonywania jakichś operacji na właściwościach klasy i/lub danych podanych im jako parametry (dane które przekazujemy do metody w momencie jej wywołania). Metody definiujemy w następujący sposób: podajemy modyfikator dostępu (`public`, `private` itp.), następnie, jeśli chcemy aby była to metoda statyczna, piszemy słowo kluczowe `static`, typ zwracanych danych (lub `void`, jeśli nie chcemy aby nasza metoda cokolwiek zwracała), w końcu podajemy nazwę metody, w nawiasach okrągłych parametry (typ i nazwa, kolejne parametry oddzielone przecinkami lub puste nawiasy, jeżeli nie chcemy przekazywać do metody żadnych danych) a w klamrach ciało metody (instrukcje wykonywane gdy wywołamy metodę).
#### Instrukcja skoku `return`
Jak wspomnieliśmy metoda, kiedy skończy się wykonywać, może zwracać jakąś daną. Służy do tego słowo kluczowe `return`. Oznacza ono natychmiastowe zakończenie wykonywania danego elementu (w tym wypadku metody). Po nim podajemy daną którą chcemy zwrócić (otrzymać jako wynik działania metody) i średnik. Jeżeli chcemy zakończyć wykonywanie metody, ale nie chcemy zwracać żadnych danych (metoda typu `void`), to piszemy tylko `return;`. Jeżeli zadeklarowaliśmy, że nasza metoda będzie zwracać dane jakiegoś typu, to zawsze (nie zależnie od wariantu jej zakończenia) musi zwracać jakąś daną tego typu.

Stwórzmy więc dwie przykładowe klasy, jedną składającą się z samych właściwości i klasę serwisową do jej obsługi:

```csharp =
public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Surname { get; set; }
    public int Age { get; set; }
}
```

```csharp =
public class PersonService
{
    public List<Person> People { get; set; }

    public PersonService()
    {
        People = new List<Person>();
    }

    public int AddNewPerson(string name, string surname, int age)
    {
        int id;
        if(People.Count == 0) id = 1;
        else id = People.Max(person => person.Id) + 1;
        People.Add(new Person() {Id = id, Name = name, Surname = surname, Age = age });
        return id;
    }

    public List<Person> GetAllPeople()
    {
        return People;
    }
    public void DisplayAllPeople()
    {
        Console.WriteLine("----------------------------------");
        foreach (var person in People)
        {
            Console.WriteLine("Id: " + person.Id);
            Console.WriteLine("Imię: " + person.Name);
            Console.WriteLine("Nazwisko: " + person.Surname);
            Console.WriteLine("Wiek: " + person.Age);
            Console.WriteLine("----------------------------------");
        }
    }
}
```

Pamiętajmy, że definicja (i implementacja) każdej klasy powinna się znaleźć w osobnym pliku (jest to dobra praktyka ułatwiająca nawigowanie po kodzie).
#### Metody typu `void`
Wróćmy jeszcze na chwilę do metod typu `void`. Są to metody, które nic nie zwracają. Wykonują one operacje wyłącznie wewnątrz klasy, na składnikach obiektu, których rezultat działania właściwie nas nie interesuje. Rezultatu działanie takich metod nie będziemy w dalszych etapach używać.
#### Wywoływanie metod
Po utworzeniu obiektu danej klasy, możemy swobodnie korzystać w kodzie programu ze wszystkich jego metod publicznych (z modyfikatorem dostępu `public`). Korzystamy z nich tak jak robiliśmy to ze wszystkimi metodami do tej pory. Podajemy ścieżką dostępu, nazwę metody i jej parametry (argumenty), oddzielone przecinkami, ujęte w nawias okrągły. Ścieżką dostępu będzie w tym wypadku utworzony przez nas obiekt (czyli po prostu nazwa utworzonego przez nas obiektu danej klasy). Użyjmy więc dla przykładu metod klasy `PersonService`. Po utworzeniu obiektu tej klasy, najpierw dopiszemy kilka osób do listy, następnie wyświetlimy wpisane dane, a na koniec policzymy średnią wieku osób z listy.
 
```csharp
PersonService people = new PersonService();

people.AddNewPerson("Jan", "Kowalski", 33);
people.AddNewPerson("Piotr", "Nowak", 88);
people.AddNewPerson("Agnieszka", "Luźna", 45);
people.AddNewPerson("Jolanta", "Piątek", 20);
people.AddNewPerson("Max", "Pierwszy", 15);

people.DisplayAllPeople();

List<Person> peopleList = people.GetAllPeople();
int sumAge = 0;
foreach (Person person in peopleList)
{
    sumAge += person.Age;
}
decimal average = (decimal)sumAge / (decimal)peopleList.Count;
Console.WriteLine("Średnia wieku: " + average);
```

#### Metody i klasy statyczne
Jak już wspominaliśmy tworzone przez nas metody mogą być statyczne (dopisujemy słowo kluczowe `static` pomiędzy modyfikator dostępu a zwracany typ). Oznacza to, że istnieje jeden egzemplarz takiej metody (niezależnie ile obiektów danej klasy utworzymy) i można jej używać, nawet gdy żaden obiekt tej klasy nie istnieje. Taka metoda jest wywoływana nie na obiekcie, a na klasie (ścieżką dostępu będzie nazwa klasy, a nie tak jak w pozostałych przypadkach nazwa obiektu). W związku z tym nie można w takiej metodzie modyfikować żadnych właściwości klasy, gdyż nie ma ona do nich dostępu (chyba, że są to właściwości statyczne). Metoda statyczna jest niezależna od obiektów danej klasy. Właśnie z tych względów metod statycznych prawie nigdy nie stosuje się w klasach serwisowych. Najczęściej używa się ich do zapisania wielokrotnie wykonywanych obliczeń, walidacji itp. i umieszcza w specjalnej pomocniczej klasie statycznej. Na przykład możemy używać ich do przeliczeń jednostek. Załóżmy więc, że chcielibyśmy wiek osób zapisywać w miesiącach, a nie w latach. Wówczas moglibyśmy stworzyć metodę statyczną:

```csharp =
public static int YearsToMonths(int years)
{
    return years * 12;
}
```

Oczywiście jest to przeliczenie wieku w miesiącach zaokrąglonego do pełnych lat i takie jego zapisywanie niema zbyt wielkiego sensu. Moglibyśmy sobie jednak wyobrazić podobną metodę przeliczającą aktualny wiek na podstawie daty urodzenia. Moglibyśmy oczywiście umieścić tą metodę w klasie `PersonService` lub `Person` (wówczas jej wywołanie wyglądałoby np. `PersonService.YearsToMonths(45);` lub `Person.YearsToMonths(58);`). Najprawdopodobniej jednak umieścilibyśmy ją w osobnej klasie statycznej przechowującej różne przeliczenia tego typu, gdyż mogą one dotyczyć nie tylko wieku osób, ale też np. częstotliwości raportowania itd.

**Klasy statyczne** są to klasy których wszystkie składniki (zarówno właściwości jak i metody) muszą być statyczne. Klasy statyczne tworzymy dodając słowo kluczowe `static` pomiędzy modyfikator dostępu (np. `public`) a słowo kluczowe `class`. Nie można tworzyć obiektów klas statycznych.

## [LEKCJA 15 – Parametry metod](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-15-parametry-metod/)
Jak wiemy z poprzednich lekcji metody mogą przyjmować dowolną liczbę argumentów. Jeżeli jednak nasza funkcja musi przyjąć ich wiele (powyżej 7), to dobrą praktyką jest, aby utworzyć nową klasę przechowującą wartości tych argumentów i przekazywać do metody obiekt tej klasy. Argumenty podajemy w nawiasie okrągłym za nazwą funkcji i oddzielamy od siebie przecinkami. Istnieją one tylko w obrębie metody.

### Parametry w definicji metody
W definicji funkcji każdy parametr składa się z typu i nazwy (deklarujemy zmienne, których będziemy używać w funkcji).

```csharp =
modyfikatorDostepu typ nazwaMetody (typ1 nazwaParametru1, typ2 nazwaParametru2, ..., typN nazwaParametruN)
{
    typ ret;
    //operacje z wykozystaniem parametrow
    //nadanie zmiennej ret wartosci
    return ret;
}
```

### Parametry w wywołaniu metody
W wywołaniu metody są to wartości (literały, zmienne, obiekty), które chcemy przekazać do metody (inicjalizujemy zmienne zadeklarowane w definicji metody, przypisując im konkretne wartości). Możemy to zrobić wypisując same wartości. Należy jednak uważać, oby kolejność wypisywanych wartości była zgodna z kolejnością parametrów w definicji metody (pierwsza wartość zostanie przypisana do pierwszego parametru, druga do drugiego itd.). C# daje nam również możliwość przekazywania argumentów w dowolnej kolejności. Wówczas musimy jednak dla każdego parametru podać jego nazwę (użytą w definicji metody), dwukropek i wartość jaką chcemy temu parametrowi przypisać. W obu przypadkach kolejne parametry oddzielamy od siebie przecinkami. Musimy też zawsze podać wartości dla wszystkich obowiązkowych parametrów metody (parametry opcjonalne opcjonalne opisane są niżej).

```csharp =
NazwaKlasy objekt = new NazwaKlasy();
typ1 parametr1;
typ2 parametr2;
...
typN paremetrN;

//nadanie wartosci parametrom

//wywolanie metody
typ rezultat = objekt.nazwaMetody(parametr1, parametr2, ..., parametrN);
//lub
typ rezultat2 = objekt.nazwaMetody(nazwaParametruN: parametrN, nazwaParametru1: parametr1, nazwaParametru2: parametr2, ...);
```

### Typy parametrów
Możemy zadeklarować parametry dowolnego typu. Mogą to być zarówno typy wartościowe, jak i klasy. Należy jednak pamiętać, że typ przekazywanej wartości musi być zgodny z zadeklarowanym typem parametru (lub musi być możliwość niejawnej konwersji do tego typu).

### Parametry opcjonalne
1. Definicja<br/>
Są to parametry, którym w definicji metody przypisujemy wartość domyślną. Robimy to dodając w liście parametrów za nazwą argumentu znak `=` i wartość którą chcemy przypisać. Wartość ta musi być stałą (wartością znaną w momencie kompilacji). Parametry domyślne muszą być podane na końcu listy parametrów (za parametrem opcjonalnym nie może znaleźć się żaden parametr obowiązkowy).

```csharp =
public void Metoda(typ1 nazwaParametru1, typ2 nazwaParametry2 = wartoscTypu2, ..., typN nazwaParametruN = wartoscTypuN)
{
    //ciało metody
}
```

2. Wywołanie<br/>
W wywołaniu metody z parametrami opcjonalnymi nie musimy przypisywać im żadnej wartości. To znaczy, że jeżeli nie podamy wartości dla tych parametrów, to przyjmą one wartość domyślną z definicji, natomiast jeżeli je podamy, to przyjmą one podane przez nas wartości. Możemy też podać wartości tylko wybranym parametrom domyślnym. Jeżeli w wywołaniu metody używamy sposobu z wypisaniem kolejno samych wartości parametrów, to musimy podać wszystkie wartości z ewentualnym pominięciem parametrów domyślnych na końcu. To znaczy, że jeżeli chcielibyśmy przekazać wartość do trzeciego z kolei parametru domyślnego, to musimy również podać wartości pierwszego i drugiego parametru domyślnego. Jeżeli natomiast użyjemy sposobu z użyciem nazw parametrów, to musimy przypisać wartości wszystkim parametrom obowiązkowym i możemy przypisać wartości tylko tym argumentom obowiązkowym, którym chcemy, bez względu na ich kolejność. Można też połączyć oba sposoby, np. wypisując po kolei wartości wszystkich parametrów obowiązkowych, a następnie podać wartości parametrów z nazwami dla wybranych parametrów opcjonalnych.

```csharp =
NazwaKlasy objekt = new NazwaKlasy();
typ1 parametr1;
typ2 parametr2;
...
typN paremetrN;

//nadanie wartosci parametrom

//przykłady wywołania metody z parametrami opcjonalnymi
objekt.Metoda(parametr1);
objekt.Metoda(parametr1, parametr2, ..., parametrN);
objekt.Metoda(parametr1, parametr2);
objekt.Metoda(nazwaParametruN: parametrN, nazwaParametru1: parametr1, nazwaParametru2: parametr2);
objekt.Metoda(parametr1, parametr2, nazwaParametruN: parametrN);
```

### Słowa kluczowe `out` i `ref`
Tradycyjnie metoda może zwrócić tylko jedną wartość. Gdybyśmy jednak potrzebowali uzyskać z niej kilka wartości możemy użyć tzw. argumentów wyjściowych. Jak już wspomnieliśmy normalnie argumenty istnieją tylko wewnątrz metody (są usuwana po zakończeniu jej wykonywania). Używając jednak słów kluczowych `out` lub `ref` możemy stworzyć argumenty, które będą istnieć nawet po zakończeniu wykonywania metody. Robimy to dodając przed nazwą parametru (w definicji metody) i przypisywaną mu wartością (w wywołaniu metody, niezależnie którym sposobem) wybrane słowo kluczowe. Sposób ten jest jednak rzadko stosowany. Kiedy musimy zwrócić kilka wartości najczęściej tworzy się nową klasą, która będzie je przechowywać i zwraca się obiekt tej klasy. Czasami jednak spotkamy się z użyciem parametrów wyjściowych. Mogliśmy to już np. zobaczyć w funkcjach do parsowania `string`ów na inne typy. Np:

```csharp =
int age;
Console.Write("Wiek: ");
string ageString = Console.ReadLine();
Int32.TryParse(ageString, out age);
```

Zobaczmy jednak co użycie tych słów kluczowych właściwie oznacza.
#### Przekazywanie argumentów przez wartość
Normalnie parametry metod są przekazywane przez wartość. To znaczy, że przekazujemy samą liczbę (typy liczbowe), znak (`char`), referencję do obiektu na stercie (typy referencyjne) itd., a nie zmienną. Jeżeli więc w metodzie przypiszemy parametrowi inną wartość (lub zmodyfikujemy dotychczasową), to zmiany te nie będą widoczne poza metodą. Oczywiście jeżeli używamy typu referencyjnego i nie zmieniając przechowywanej w argumencie referencji dokonujemy zmian we wskazywanym przez nią obiekcie na stosie, to zmiany te będą widoczne również gdy odwołamy się do tego obiektu przez zmienną spoza metody. Jeżeli jednak w metodzie przypiszemy argumentowi nową referencję (inny obiekt), to nie zmieni to obiektu na który wskazuje zmienna, którą użyliśmy w wywołaniu. 
#### Przekazywanie argumentów przez referencję
Użycie słów kluczowych `ref` lub `out` oznacza, że ten argument przekazujemy przez referencję, a nie przez wartość. Czyli nasz argument nie jest nową zmienną, a jedynie aliasem do zmiennej przekazanej w wywołaniu (w wywołaniu musimy użyć zmiennej, czegoś co ma zarezerwowany konkretny adres w pamięci, a nie np. literału). Oznacza to, że argument jest tak na prawdę tą samą zmienną, co ta przekazana w wywołaniu (wskazuje na ten sam obszar w pamięci), występuje ona tylko pod nową nazwą. Wszelkie zmiany jakie dokonamy w takim argumencie będą więc też widoczne w oryginalnej zmiennej (użytej w wywołaniu) po zakończeniu działania metody.
#### Porównanie argumentów `out` i `ref`

| Cecha | `ref` | `out` |
| :--- | :---: | :---: |
| Inicjalizacja zmiennej, przed przekazaniem jako argument metody | musi być zainicjalizowana | nie musi być zainicjalizowana |
| Inicjalizacja lub przypisanie wartości do parametru wewnątrz metody | nie wymagana | wymagana przed zakończeniem metody |
| Zastosowanie | gdy metoda powinna zmodyfikować wartość przekazanej zmiennej | gdy metoda ma zwrócić więcej niż jedną wartość |
| Inicjalizacja wartości argumentu wewnątrz metody przed jej użyciem (wewnątrz metody) | opcjonalna | wymagana |
| Kierunek przekazywania danych | dane mogą być przekazane zarówno do metody, jak i z metody (dwukierunkowe) | dane są przekazywane tylko z metody (jednokierunkowe) |

1. Oba rodzaje argumentów są traktowane inaczej podczas wykonywania, ale tak samo podczas kompilacji. Oznacza to, że nie będziemy mogli przeciążyć metod, gdzie jedyną różnicą będzie to, że jedna będzie przyjmować argument jako `ref`, a druga jako `out` (o przeciążaniu metod będzie w przyszłym tygodniu).
2. Pamiętajmy również, że właściwości metod nie są zmiennymi. Nie możemy więc ich przekazać jako parametrów `out` lub `ref`.
3. Argumenty `out` i `ref` nie mogą być również argumentami domyślnymi.

## [LEKCJA 16 – Pola i właściwości](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-16-pola-i-wlasciwosci/)
### Pola
Mówiliśmy już, że klasa składa się z właściwości i metod. Nic nie stoi jednak na przeszkodzie, aby w klasie stworzyć też zwykłą zmienną. Robimy to podając modyfikator dostępu, typ i nazwę zmiennej. Na końcu oczywiście stawiamy średnik. Zmienną będącą składnikiem klasy nazywamy polem. Np.:

```csharp =
public class Cube
{
    public int EdgeLength;
    private int volume;
    private int lateralSurface;
    public Cube (int a = 0) 
    { 
        EdgeLength = a;
        volume = a * a * a;
        lateralSurface = a * a * 6;
    }
    public int Volume()
    {
        volume = EdgeLength * EdgeLength * EdgeLength;
        return volume;
    }
    public int LateralSurface()
    {
        lateralSurface = EdgeLength * EdgeLength * 6;
        return lateralSurface;
    }
}
```

Dostęp do pól publicznych możemy uzyskać w ten sam sposób, jak do wszystkich innych publicznych składników klasy:

```csharp =
Cube cube = new Cube();
Console.WriteLine("Domyślna długość krawędzi: " + cube.EdgeLength);
cube.EdgeLength = 10;
Console.WriteLine("Nowa długość krawędzi: " + cube.EdgeLength);
Console.WriteLine("Objętość kostki: " + cube.Volume());
Console.WriteLine("Pole powierzchni bocznej kostki" + cube.LateralSurface());
```

### Właściwości
Właściwości są to tak na prawdę struktury składające się z pola prywatnego i dwóch publicznych metod (`get` i `set`). Czyli np. właściwość:

```csharp =
public class Person
{
    public int Id { get; set; }
}
```

można by tak na prawdę zapisać jako:

```csharp =
public class Person
{
    private int id;
    public int getId()
    {
        return id;
    }
    public void setId(int newId)
    {
        id = newId;
    }
}
```

C# zajmuje się jednak całą tą mechaniką za nas. Może jednak zdarzyć się, że będziemy chcieli jawnie zaimplementować `get`tery i `set`tery. Np. chcemy mieć pewność, że nasze id będzie mieścić się w przedziale od 1 do 10 oraz wyświetlić odpowiednią informację w konsoli zarówno przy jego pobieraniu jak i zwracaniu. Możemy to zrobić np. tak:

```csharp =
public class Person
{
    private int _id;
    public int Id 
    { 
        get
        {
            Console.WriteLine($"Pobrano wartość Id równą {_id}.");
            return _id;
        }
        set
        {
            _id = value % 10 + 1;
            Console.WriteLine($"Ustawiono Id na {_id}.");
        } 
    }
}
```

Jak widać nazwę prywatnego pola ustawia się na ogół na taką samą jak nazwa właściwości, tylko pisaną z małej litery i poprzedzoną podkreślnikiem (_). W `set`terze natomiast użyliśmy słowa kluczowego `value`, które oznacza wartość jaką do niego przekazaliśmy. Czyli jeżeli stworzyliśmy sobie obiekt `Person person = new Person();` i zapisaliśmy niżej `person.Id = 5;`, to `value` będzie równe właśnie to `5`.

#### Właściwości read-only
Może się też tak zdarzyć, że chcemy, aby można było odczytać wartość naszej właściwości, jednak nie chcemy dawać możliwości jej jawnej zmiany. Np. nasze Id będzie ustawiane automatycznie w momencie tworzenia obiektu (zaprogramujemy to w kodzie klasy) i nie chcemy, aby ktokolwiek mógł nam tam mieszać. Oby to zrobić wystarczy w definicji właściwości pominąć sekcję `set`. Czyli w podstawowej formie będzie to wyglądało:

```csharp =
public class Person
{
    public int Id { get; }
}
```

natomiast gdy musimy skorzystać z jawnej deklaracji pola prywatnego:

```csharp =
public class Person
{
    private int _id;
    public int Id 
    { 
        get
        {
            return _id;
        }
    }

    //możemy to też zapisać w jednej linii: public int Id { get { return _id; } }
}
```

W nowszych wersjach języka C# (od C# 7.0) możemy skrócić ten zapis i napisać:

```csharp =
public class Person
{
    private int _id;
    public int Id => _id;
}
```

Taki skrócony zapis można również stosować przy właściwościach read-write (przy jawnej deklaracji zarówno `get`tera jak i `set`tera):

```csharp =
public class Person
{
    private int _id;
    public int Id 
    { 
        get => _id;
        set => _id = value % 10 + 1;
    }
}
```

Można też oczywiście mieszać oba zapisy (np. skrócony zapis dla `get`tera i normalny dla `set`tera).

## [LEKCJA 17 – Zakresy widoczności](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-17-zakresy-widocznosci/)
Poszczególne elementy programu (klasy, metody, właściwości, zmienne) mają różną widoczność w zależności od miejsca w programie. Definiujemy ją przy pomocy tzw. modyfikatorów dostępu.
### Modyfikatory dostępu
Są to słowa kluczowe które umieszczaliśmy na początku każdej deklaracji. Podawanie ich jawnie za każdym razem, nawet w sytuacjach gdy nie są wymagane, zwiększa czytelność i przeszukiwanie kodu. Należy więc to do dobrych praktyk programowania. Wyróżniamy cztery podstawowe modyfikatory dostępu:
#### `public`
Oznaczamy tak elementy do których chcemy mieć dostęp w każdym elemencie programu. Możemy sią do nich odwoływać zarówno w innych klasach, jak i nawet w innych projektach w danej solucji (wtedy musimy dodać sekcję using z nazwą projektu z elementem który chcemy użyć, lub odpowiedniej ścieżki dostępu).
#### `private`
Oznaczamy tak elementy do których chcemy mieć dostęp tylko w obrębie elementu bezpośrednio go zawierającego (klasy, metody).To znaczy, że próba odwołania się do takich w innej klasie, zakończy się fiaskiem.
#### `protected`
Do tak oznaczonych elementów klasy, mamy dostęp tylko wewnątrz tej klasy, oraz w klasach po niej dziedziczących (o dziedziczeniu powiemy więcej w przyszłym tygodniu).
#### `internal`
Oznacza elementy do których mamy dostęp tylko wewnątrz danego projektu. To znaczy nie możemy się do nich odwołać z innych projektów jakie dodamy do naszej solucji. Jeżeli w definicji klasy nie ma podanego modyfikatora dostępu, to są one domyślnie `internal`.

## [LEKCJA 18 – Piszemy aplikację](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-18-piszemy-aplikacje/)
W tej lekcji możemy zobaczyć początek implementacji przykładowej aplikacji. Kod programu możemy znaleźć [tutaj](https://github.com/szkoladotneta/warehouse/tree/tydz2).

### 1. Utworzenie projektu
Tworzenie programu rozpoczynamy oczywiście od utworzenia projektu w którym implementować będziemy naszą aplikację
### 2. Wypisanie funkcjonalności
Następnie, jeszcze zanim zaczniemy pisać jakiś kod dobrze jest wypisać sobie w komentarzach podstawowe funkcjonalności, które będziemy chcieli utworzyć w pierwszej kolejności. Można to zrobić np. wyobrażając sobie co po kolei powinno się dziać po uruchomieniu naszego programu.
### 3. Rozpisanie funkcjonalności krok po kroku
Poniżej w komentarzach dobrze jest wypisać poszczególne etapy z jakich będzie się składać dana funkcjonalność (w punktach).
### 4. Implementacja kolejnych funkcjonalności
Teraz możemy przejść do implementacji kolejnych funkcjonalności. Będziemy to robić najczęściej tworząc nowe klasy. Zazwyczaj będą to dwie klasy (podstawowa i serwisowa). Staramy się unikać powtarzania kodu. Dobrze, aby nasz kod był możliwie elastyczny. Wówczas być może będzie możliwe jego ponowne użycie. Dobrze również, aby ewentualne późniejsze zmiany (rozszerzenia funkcjonalności itp.) wymagały modyfikacji kodu w jak najmniejszej liczbie miejsc (a idealnie tylko w jednym). Jeżeli ktoś potrzebuje, może sobie najpierw rozpisać jakie klasy i metody będą mu potrzebne do implementacji poszczególnych etapów. Pamiętajmy również przy wymyślaniu nazw oraz implementacji, aby nasz kod był jak najbardziej czytelny i zrozumiały zarówno dla nas jak i dla innych. Starajmy się przestrzegać dobrych praktyk programowania (każda klasa w oddzielnym pliku, przestrzeganie konwencji nazywania elementów programu, nazwy w języku angielskim, nawiasy klamrowe w osobnych liniach, jawna deklaracja modyfikatorów dostępu, maksymalnie jedna wolna linia pomiędzy kolejnymi metodami itd.).
### 5. Użycie funkcjonalności
Dodajemy odpowiedni kod do metody `Main`. Tworzymy odpowiednie obiekty. Wypełniamy listy (najczęściej przy użyciu utworzonej do tego metody). Pobieramy odpowiednie dane od użytkownika. W przypadku aplikacji konsolowej, poza poznaną już przez nas metodą `Console.ReadLine();` pobierającą całą linijkę tekstu, przydatna może się okazać metoda `Console.ReadKey();`.

#### Metoda `Console.ReadKey()`
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

### 6. Uruchomienie programu
Po skończeniu jakiegoś etapu uruchamiamy program, aby sprawdzić, czy działa zgodnie z naszymi oczekiwaniami i czy nie popełniliśmy żadnych błędów.
### 7. Debugowanie
Jeżeli kompilator poinformuje nas o jakichś błędach, lub gdy program zadziała w nieoczekiwany dla nas sposób, staramy się znaleźć tego przyczynę, a w razie potrzeby korzystamy z narzędzi do debugowania.
### 8. Czyszczenie komentarzy
Po zaimplementowaniu jakiejś funkcjonalności (lub jej etapu), możemy usunąć odpowiedni, dotyczący go, komentarz (napisany w punktach 2., 3.).
### 9. Refaktoryzacja
W dalszych etapach nauki dokonamy refaktoryzacji naszego kodu. Kiedy lepiej poznamy język C# i nauczymy się nowych mechanizmów zobaczymy, że niektóre fragmenty naszego programu można napisać lepiej, zwiększyć ich czytelność lub sprawność działania. Wówczas zaczniemy modyfikować nasz kod, zgodnie z naszą aktualną wiedzą.

## [LEKCJA 19 – Błędy początkujących](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-19-bledy-poczatkujacych/)
### Tworzenie metod
Nie twórz klas ze zbyt dużą liczbą argumentów. Jeżeli nie możesz zapamiętać kolejności w jakiej występują, to znaczy, że jest ich prawdopodobnie za dużo. Wtedy warto utworzyć dodatkową klasę pomocniczą, która zbierze wszystkie argumenty które chcieliśmy przekazać. Wówczas do naszej metody będziemy przekazywać tylko jeden argument typu tej klasy pomocniczej.
### Typy danych
Jeżeli masz problem jakiego typu powinna być dana zmienna lub właściwość, to zastanów się, co oznaczałaby ona w świecie realnym. Jeżeli jest to ilość czegoś, to najczęściej będzie to liczba całkowita, czyli użyjemy typu `int`. Gdy chcemy natomiast przechowywać cenę, to wiemy, że jest to wartość z dwoma miejscami dziesiętnymi. Użyjemy więc typu `decimal`, gdyż zależy nam tu na dużej dokładności obliczeń na liczbach dziesiętnych.
### Instrukcje warunkowe
Stosuj je często. Zawsze, gdy pobierasz dane od użytkownika, sprawdź, czy podał on takie dane jakich oczekiwaliśmy, zanim użyjesz ich dalej w programie. Jeżeli np. oczekiwaliśmy, że użytkownik wpisze cyfrę, a on np. wciśnie Enter, to próba rzutowania na typ `int` może zakończyć się błędem programu i zakończeniem jego pracy.

W instrukcjach warunkowych uważaj na wzajemnie się wykluczające warunki.
### Pętle nieskończone
Uważaj na pętle nieskończone. Stosuj je świadomie i pamiętaj, że są kosztowne dla programu.

## [LEKCJA 20 – Praca domowa](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-20-praca-domowa/)
Zaczynamy pisać naszą aplikację konsolową. Tworzymy projekt i implementujemy pierwsze funkcjonalności. Celem zadania jest stworzenie działającego programu z dostępną określoną liczbą funkcjonalności. Pamiętajmy o przestrzeganiu wszystkich poznanych dobrych praktyk programowania, ale nie przejmujmy się, że nie będą one jeszcze zgodne ze wszystkimi standardami. Tym zajmiemy się później w dalszej części nauki. Pisząc program wykorzystajmy całą naszą dotychczasową wiedzę. Zawrzyjmy w nich wszystkie elementy, które poznaliśmy w tym tygodniu. Stwórzmy tablice, listy, wykorzystajmy większość typów wartościowych i referencyjnych. Zaimplementujmy pierwsze klasy i metody, oraz połączmy je w funkcjonalności, które można zaprezentować innym. Poza stworzeniem samego projektu opiszmy go również. powiedz jakie funkcjonalności są w nim w tej chwili dostępne dla użytkownika i jak używać naszego programu.


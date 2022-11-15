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
1. **Typy liczbowe całkowite**
    * int<br/>
	Pochodzi od ang. _integer_, czyli liczba całkowita. Jest to tak na prawdę alias struktury Int32, czyli typu reprezentującego liczby całkowite w zakresie \<-2 147 483 648, 2 147 483 647\>, przechowywane na 32 bitach pamięci. Aby podejrzeć metadane tej struktury wystarczy na słowie kluczowym `int` lub `Int32` wcisnąć  klawisz **F12**. Możemy wówczas zobaczyć jakie metody zawiera dana struktura, jaka jest minimalna i maksymalna liczba jaką może przechowywać zmienna tego typu itd. Jeżeli spróbujemy zapisać w niej liczbę z poza tego zakresu (większą lub mniejszą) aplikacja wyrzuci nam błąd kompilacji lub wyjątek bezpieczeństwa. W zależności od sytuacji może on być różnego typu, np. wyjątki _OverflowException_, _ArgumentOutOfRangeException_ lub błędy kompilacji _Compiler Error CS0220 The operation overflows at compile time in checked mode_, czy _Compiler Error CS0266 Cannot implicitly convert type 'type1' to 'type2'_.
    * short<br/>
	Alias struktury Int16. Służy do przechowywania mniejszych liczb całkowitych, mieszczących się w zakresie \<-32 768, 32 767\>. Aby utworzyć zmienną tego typu używamy słów kluczowych `short`, `Int16` lub `System.Int16`. Zajmuje ona 16 bitów pamięci. Podobnie jak przy zmiennej typu _int_ próba zapisania w niej liczby z poza zakresu będzie skutkować wystąpieniem błędu. Metadane tego typu również możemy podejrzeć używając klawisza **F12**.
    * long<br/>
	Alias struktury Int64. Służy do przechowywania większych liczb całkowitych, mieszczących się w zakresie \<-9 223 372 036 854 775 808, 9 223 372 036 854 775 807\>. Aby utworzyć zmienną tego typu używamy słów kluczowych `long`, `Int64` lub `System.Int64`. Zajmuje ona 64 bitów pamięci. Podobnie jak w powyższych dwóch przypadkach próba zapisania w niej liczby z poza zakresu będzie skutkować wystąpieniem błędu. Metadane tego typu również możemy podejrzeć używając klawisza **F12**.
	
	Podsumowanie wszystkich typów liczbowych całkowitych znajduje się w tabelce poniżej:
	| Alias | Typ | Zakres | Rozmiar | Dodatkowe informacje |
	| :---: | :---: | :---: | :---: | :--- |
	| sbyte	| System.SByte | \<-128, 127\> | 8 bitów (1 bajt) | Liczba całkowita znakowa. Pierwszy bit w zapisie jest bitem znaku (dodatnia/ujemna) |
	| byte | System.Byte | \<0, 255\> | 8 bitów (1 bajt) | Liczba całkowita bez znakowa |
	| short	| System.Int16 | \<-32 768, 32 767\> | 16 bitów (2 bajty) | Liczba całkowita znakowa |
	| ushort | System.UInt16 | \<0, 65 535\> | 16 bitów (2 bajty) | Liczba całkowita bez znakowa |
	| int | System.Int32 | \<-2 147 483 648, 2 147 483 647\> | 32 bity | Liczba całkowita znakowa |
	| uint | System.UInt32 | \<0, 4 294 967 295\> | 32 bity | Liczba całkowita bez znakowa |
	| long | System.Int64 | \<-9 223 372 036 854 775 808, 9 223 372 036 854 775 807\> | 64 bity | Liczba całkowita znakowa |
	| ulong | System.UInt64 | \<0, 18 446 744 073 709 551 615\> | 64 bity | Liczba całkowita bez znakowa |
	| nint | System.IntPtr | Zależy od platformy (obliczana w czasie wykonywania programu) | 32 lub 64 bity | Liczba całkowita znakowa o rozmiarze natywnym. Odpowiada typowi Int32, jeżeli jest uruchamiana w procesie 32-bitowym, a typowi Int64, gdy jest uruchamiana w procesie 64-bitowym |
	| nuint	| System.UIntPtr | Zależy od platformy (obliczana w czasie wykonywania programu) | 32 lub 64 bity | Liczba całkowita bez znakowa o rozmiarze natywnym. Odpowiada typowi UInt32, jeżeli jest uruchamiana w procesie 32-bitowym, a typowi UInt64, gdy jest uruchamiana w procesie 64-bitowym |
  \*Przedrostek _u-_ w nazwach typów pochodzi od ang. _unsigned_, czyli bez znaku
  
2. **Typy liczbowe zmiennoprzecinkowe**
    * float
    * duble
    * decimal
### 2. char
### 3. bool
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
Więcej informacji o typach wyliczeniowych znajdzie się w kolejnych lekcjach tego tygodnia (Lekcja 12 - Enum).
## 3. Struktury
Typ bardzo podobny do klasy. Możemy w nim zadeklarować zmienne, metody, pola, właściwości. W odróżnieniu od klas jest to jednak typ wartościowy. Oznacza to, że będzie przechowywany na stosie, w stałym miejscu pamięci, a ilość zajmowanego przez niego miejsca w pamięci jest zawsze taka sama, nie zależnie ilu zmiennym struktury przypiszemy jakieś wartości. Strukturę tworzymy podając modyfikator dostępu, słowo kluczowe `struct` nazwę nowej struktury, a w klamrach ciało struktury (zmienne, metody itd.), np.:
```csharp =
public struct SomeStructure
{
  private int no;
  private string name;
  public SomeStructure(int number, string name)
  {
    this.no = number;
    tis.name = name;
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

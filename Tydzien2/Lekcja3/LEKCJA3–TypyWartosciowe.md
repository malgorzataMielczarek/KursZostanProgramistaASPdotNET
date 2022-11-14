#[LEKCJA 3 – Typy wartościowe](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-3-typy-wartosciowe/)

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
#### 1. int
Pochodzi od ang. _integer_, czyli liczba całkowita. Jest to tak na prawdę alias struktury Int32, czyli typu reprezentującego liczby całkowite w zakresie \<-2147483648, 2147483647\>, przechowywane na 32 bitach pamięci. Aby podejrzeć metadane tej struktury wystarczy na słowie kluczowym `int` lub `Int32` wcisnąć  klawisz **F12**. Możemy wówczas zobaczyć jakie metody zawiera dana struktura, jaka jest minimalna i maksymalna liczba jaką może przechowywać zmienna tego typu itd. Jeżeli spróbujemy zapisać w niej liczbę z poza tego zakresu (większą lub mniejszą) aplikacja wyrzuci nam błąd kompilacji lub wyjątek bezpieczeństwa. W zależności od sytuacji może on być różnego typu, np. wyjątki _OverflowException_, _ArgumentOutOfRangeException_ lub błędy kompilacji _Compiler Error CS0220 The operation overflows at compile time in checked mode_, czy _Compiler Error CS0266 Cannot implicitly convert type 'type1' to 'type2'_.
#### 1. short
#### 2. long
#### 3. float
#### 4. duble
#### 5. decimal
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

# [LEKCJA 8 – Pętle](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-8-petle/)
Służą do iteracyjnego powtórzenia jakiejś czynności. Oznacza to, że dany blok kodu wykona się określoną ilość razy, zależną od warunków początkowych jakie przyjmiemy i warunku przerwania. W języku C# wyróżniamy cztery rodzaje pętli:

## while
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

## do while
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
while(i < 5)
Console.WriteLine();
```

Prosty przykład pokazujący różnice między pętlą `while` i `do while`:

```csharp =
// Program wypisujący w konsoli pięć znaków plus (+++++)
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
while(j > 0)
Console.WriteLine();
//zostanie wypisane: 01234567891011121314151617181920212223...
//pętla nieskończona, będzie się wykonywać nieskończoną ilość razy
//UWAŻAĆ NA TAKIE PĘTLE!!!
```

## for
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

Pętlę `for` stosujemy najczęściej jeżeli z góry wiemy ile razy pętla ma się wykonać.

## foreach
Pętla przeznaczona do iterowania po elementach kolekcji. Składa się ze słowa kluczowego `foreach`, ujętych w nawias okrągły definicji zmiennej przechowującej element kolekcji, słowa kluczowego `in` i kolekcji po której chcemy iterować. Na końcu umieszczony jest ujęty w klamry blok kodu:

```csharp =
foreach(var element in kolekcja)
{
	//blok kodu
	//zrób coś na kolejnych elementach kolekcji, dopóki kolekcja się nie skończy
}
```

Ten typ pętli zostanie dokładniej omówiony w lekcji 10, gdy poznamy tablice.

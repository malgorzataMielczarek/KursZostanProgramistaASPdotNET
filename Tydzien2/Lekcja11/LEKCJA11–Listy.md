# [LEKCJA 11 – Listy](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-11-listy/)
Klasa `System.Collections.Generic.List` należy do tzw. typów generycznych. Oznacza to, że lista jest zawsze obiektem typu `List`, ale może przetrzymywać obiekty innego typu, który definiujemy podczas inicjalizacji. Podobnie jak tablice, listy służą do przechowywania wielu obiektów tego samego typu. Różnice między tymi dwoma typami powodują jednak, że stosuje się je w różnych sytuacjach.

## Różnice między listami a tablicami
### 1. Tworzenie obiektów danego typu
Listy deklarujemy podając słowo kluczowe `List`, następnie w nawiasach ostrych (`<>`) podajemy typ jaki przetrzymywać będzie dana lista, a na koniec podajemy nazwę naszego obiektu. Wyrażenie inicjalizacyjne, jak w przypadku każdego obiektu, rozpoczyna się od słowa kluczowego `new`, następnie podajemy nazwę klasy do której należy nasz obiekt (`List`), w nawiasach ostrych typ jaki będzie przechowywać ta lista i na koniec nawias okrągły (wywołanie konstruktora klasy - więcej o konstruktorach w lekcji 2 kolejnego tygodnia). Nasza lista jest już utworzona. W przypadku list, podczas inicjalizacji nie podajemy jej rozmiaru. Jak tworzy się tablice omówiliśmy w poprzedniej lekcji. Dla przypomnienia pokażmy jeszcze jak tworzy się tablice i porównajmy to z tworzeniem listy, na przykładzie obiektów obu typów przechowujących `int`y:

```csharp =
//TABLICA
int[] tablica = new int[10]; //musimy z góry określić ile elementów będzie przechowywać nasza tablica i nie powinniśmy później zmieniać jej rozmiarów

//LISTA
List<int> lista = new List<int>();
```

Oczywiście jak zawsze inicjalizacji listy nie musimy przeprowadzać razem z deklaracją. Równie dobrze możemy to zrobić w dalszej części kodu. Jeśli jest to możliwe warto to jednak zrobić od razu, inaczej trzeba uważać, aby nie próbować używać obiektu, który nie został jeszcze zainicjalizowany.
### 2. Sposób alokowania pamięci
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

## Operacje na listach

### Dodawanie elementów

#### 1. Metoda `Add`
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

#### 2. Metoda `AddRange`
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

#### 3. Metoda `Insert`
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

#### 4. Metoda `InsertRange`
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

### Uzyskiwanie dostępu do elementów
Jak pokazano w powyższych przykładach dostęp do konkretnego elementu listy można uzyskać tak samo jak w przypadku tablic podając nazwę listy i otoczony nawiasami kwadratowymi numer indeksu. Można w ten sposób zarówno uzyskać aktualnie przechowywany w tym miejscu obiekt, zmodyfikować go lub przypisać nowy obiekt. Można również wyszukać w liście obiekt(y) spełniający(e) określone warunki. Do tego służą różne metody _find_ klasy `List`. Więcej na temat tych i innych metod i właściwości można znaleźć w [dokumentacji klasy `List`](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1?view=net-7.0).

### Sortowanie elementów
Do uporządkowania elementów służy metoda `Sort` klasy `List`. Jej wywołanie wygląda następująco: `nazwaTablicy.Sort();`. Działa ona analogicznie jak omówiona w poprzedniej lekcji tablicowa metoda o takiej samej nazwie.

### Usuwanie elementów
Jak już wspomniano dynamiczna natura list pozwala na łatwe zarówno dodawanie jak i usuwanie jej elementów. Do tego celu klasa `System.Collections.Generic.List` implementuje kilka metod:
#### 1. Metoda `Remove`
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

#### 2. Metoda `RemoveAll`
Służy do usunięcia z listy wszystkich obiektów spełniających podany warunek. Metoda `RemoveAll` przyjmuje jeden argument typu `Predicate<T>`, gdzie `T` oznacza typ obiektów przechowywanych w liście. Jest to tzw. delegata, która definiuje warunki usunięcia elementów. Więcej o metodach przyjmujących argumenty tego typu dowiemy się w kolejnych lekcjach.

#### 3. Metoda `RemoveRange`
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

#### 4. Metoda `RemoveAt`
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

### Wykonanie operacji na każdym elemencie listy
Jeżeli chcemy wykonać jakąś operację na wszystkich elementach listy możemy to zrobić na kilka sposobów. Dla przykładu, pokażemy jak wypisać w konsoli wszystkie elementy listy z powyższych przykładów, każdym z tych sposobów.
#### 1. Z użyciem iteratora
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

#### 2. Z użyciem pętli `foreach`
Jak już wiemy pętla ta służy do uzyskiwania dostępu do każdego kolejnego elementu kolekcji. Zastosowaliśmy ją już w kilku przykładach powyżej, ale dla przypomnienia:
```csharp =
foreach(var item in list)
{
	Console.WriteLine(item);
}
```

Słowo kluczowe `var` oznacza zmienną dowolnego typu. Zmienna taka przyjmuje typ obiektu (literału), który został do niej przypisany podczas inicjalizacji. W naszym przypadku będzie to `int`, ale gdyby lista przechowywała obiekty innego typu, to byłby to ten inny typ. W powyższym przykładzie można też oczywiście podać typ elementu jawnie, czyli w naszym przypadku, zamiast słowa kluczowego `var`, napisać `int`.

#### 3. Z użyciem metody `ForEach`
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

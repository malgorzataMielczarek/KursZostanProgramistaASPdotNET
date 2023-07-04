# [LEKCJA 2 – Kolekcje w .NET](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-5-praca-z-danymi/lekcja-2-kolekcje-w-net/)
W naszych aplikacjach będziemy często korzystać z kolekcji. Są to swego rodzaju tablice, które przechowujemy na stercie. Wcześniej poznaliśmy już podstawowy typ kolekcji, czyli listę (`List<T>`). Jest ona najczęściej stosowanym typem kolekcji, jednak ma swoje ograniczenia. Może się więc zdarzyć, że do konkretnego celu bardziej odpowiedni będzie dla nas jeden z pozostałych typów. Warto więc pokrótce poznać, kilka podstawowych typów.

W C# wyróżniamy dwa rodzaje typów obsługujących kolekcje (zbiór) danych.
## Kolekcje niegeneryczne
Odkąd do standardu języka C# wprowadzono kolekcje generyczne, kolekcje niegeneryczne są rzadko stosowane. Przez to, że mogą one przechowywać dane różnego typu są one mniej wydajne.
### `ArrayList`
Typ podobny do standardowej tablicy. W odróżnieniu od niej jest jednak przechowywany na stercie, a nie na stosie.

```csharp =
ArrayList arrayList = new ArrayList();
```

### `SortedList`
Typ podobny do `ArrayList`, z tą różnicą, że składa się z pary klucz (_key_) - wartość (_value_). Przy czym klucz musi być unikalną liczbą typu `int`, inną dla każdego elementu (nie mogą się powtarzać w obrębie jednej kolekcji). Podczas dodawania nowych elementów do listy, jest ona automatycznie sortowana według klucza, w kolejności rosnącej.

```csharp =
SortedList sortedList = new SortedList();
```

### `Stack`
Czyli stos. Struktura podobna w budowie do stosu w pamięci. Jest to struktura typu LIFO (_Last In First Out_), czyli ostatnio dodany do kolekcji element jest z niej pobierany i usuwany jako pierwszy.

```csharp =
Stack stack = new Stack();
```

Poza standardowymi metodami dodawania i usuwania jej elementów mamy tu dodatkowo trzy funkcje:
#### `Push`
Służy do dodania nowego elementu w odpowiednie miejsce kolekcji.
#### `Pop`
Służy do pobrania i usunięcia ostatnio dodanego (przy pomocy metody `Push`) elementu kolekcji.
#### `Peek`
Służy do pobrania ostatnio dodanego (przy pomocy metody `Push`) elementu kolekcji, jednak bez jego usuwania.

### `Queue`
Czyli kolejka. Struktura typu FIFO (_First In First Out_), czyli pierwszy dodany do kolekcji element jest z niej usuwany jako pierwszy. Możemy sobie ją wyobrazić jako standardową kolejkę (np. w sklepie). Klient, który ustawił się w kolejce jako pierwszy, zostanie obsłużony w pierwszej kolejności.

```csharp =
Queue queue = new Queue();
```

Poza standardowymi metodami dodawania i usuwania elementów kolekcji, mamy tu dodatkowo trzy funkcje:
#### `Enqueue`
Służy do dodawania nowych elementów na końcu kolejki.
#### `Dequeue`
Służy do pobierania i usuwania pierwszego (dodanego jako pierwszy) elementu kolejki.
#### `Peek`
Służy do pobierania pierwszego (dodanego jako pierwszy) elementu kolejki, jednak bez jego usuwania.

### `Hashtable`
Czyli tablica haszująca. Typ podobny do typu `SortedList`, również składający się z pary klucz - wartość. Tu jednak klucz jest typu `string`, a nie `int`, ale oczywiście wartości kluczy tu również nie mogą się powtarzać. W odróżnieniu od `SortedList` jej wartości nie są w żaden sposób sortowane.

```csharp =
Hashtable hashtable = new Hashtable();
```

## Kolekcje generyczne
Znacznie częściej stosowane i szeroko rozpowszechnione typy kolekcji. Generyczne, czyli, dla przypomnienia, takie, w których podajemy jakiego typu dane będziemy w nich przechowywać. Oznacz to, że do kolekcji generycznej będziemy mogli dodawać wyłącznie elementy typu, podanego na początku podczas deklaracji obiektu. Dzięki temu kolekcje generyczne są zdecydowanie szybsze, bardziej wydajne, a co za tym idzie preferowane przez programistów. Jednym z typów kolekcji generycznych jest poznana przez nas wcześniej generyczna lista (`List<T>`).
### `List<T>`
Tradycyjna lista przechowująca elementy podanego typu `T`. Jest to najczęściej stosowany typ kolekcji, dający dużą swobodę dodawania i usuwania elementów.
#### Przykład
```csharp =
List<int> list = new List<int>();
//dodawanie pojedynczych elementow
list.Add(1);
list.Add(2);
list.Add(3);
// 1, 2, 3

//usuwanie pojedynczych elementow
list.Remove(3); // usuniecie elementu 3
// 1, 2

//usuwanie wszystkich elementow spełniających podany warunek
list.RemoveAll(item => item < 3);
// 

for(int i = 0; i < 10; i++)
{
    list.Add(i);
}
// 0, 1, 2, 3, 4, 5, 6, 7, 8, 9

//usuwanie kilku elementow
list.RemoveRange(3, 5); // 5 elementow, rozpoczynajac od indeksu 3
// 0, 1, 2, 8, 9

//usuwanie elementu o okreslonym indeksie
list.RemoveAt(3); // usuniecie elementu o indeksie 3
// 0, 1, 2, 9

//dodawanie na koniec kolekcji elementow z innej kolekcji
list.AddRange(list);
// 0, 1, 2, 9, 0, 1, 2, 9
```

### `Dictionary<TKey, TValue>`
Czyli słownik. Kolekcja, podobnie jak kolekcje `SortedList`, czy `Hashtable`, przechowuje pary klucz - wartość. Zarówno klucz, jak i wartość może być dowolnego, podanego przez nas na początku, typu. Częstymi przykładami są słowniki przechowujące pary `<int, string>`, `<string, string>`, ale również zdefiniowane przez nas klasy np. `<int, Item>`. Elementy słownika nie są automatycznie sortowane. Poza metodami analogicznymi jak w liście, posiada on dodatkowo metody sprawdzające, czy słownik zawiera element o podanym kluczu (`slownik.ContainsKey(TKey key)`) lub wartości (`slownik.ContainsValue(TValue value)`), zwracające typ `bool` (`true`, jeśli zawiera, `false`, jeśli nie). Ciekawą metodą jest metoda `slownik.TryGetValue(TKey key, out TValue value)`, zwracająca wartość elementu o podanym kluczu, jeżeli został on znaleziony na liście. Oczywiście wartości kluczy w kolekcji nie mogą się powtarzać.
#### Przykład
```csharp =
Dictionary<int, Item> dictionary = new Dictionary<int, Item>();
dictionary.Add(1, new Item(1, "Apple", 2));
dictionary.Add(2, new Item(2, "Strawberry", 2));

int i = 1;
while(dictionary.ContainsKey(i)) i++;
Item orange = new Item(i, "Orange", 2);
dictionary.Add(i, orange);

if(!dictionary.ContainsValue(orange)) i--;

Item item;
dictionary.TryGetValue(i, out item);
// item powinien rownac sie orange
```

`Dictionary` najczęściej używane są do przechowywania różnego rodzaju wartości słownikowych, czyli np: kategorii, typów, państw, kodów pocztowych itp.

### `SortedList<TKey, TValue>`
Czyli posortowana lista. Jest ona podobna do tradycyjnej listy, ale również zawiera elementy słownika. Podobnie do tego drugiego składa się ona z par klucz - wartość. Analogicznie, zarówno klucz jak i wartość, mogą być dowolnego, zadeklarowanego na początku typu. Podobnie jak w poprzednich przypadkach, klucz musi być unikatowy (jego wartość nie może powtarzać się w obrębie kolekcji). `SortedList<TKey, TValue>` posiada takie same metody jak słownik. W odróżnieniu od niego, podobnie jak niegeneryczna `SortedList`, podczas dodawania nowych elementów do listy, są one automatycznie sortowane, zgodnie z rosnącą wartością klucza. Co za tym idzie, dodawanie nowych elementów do posortowanej listy jest wolniejsze niż w przypadku słownika. Natomiast późniejsze wyszukiwanie elementów jest znacznie szybsze.
#### Przykład
```csharp =
SortedList<int, Item> sortedList = new SortedList<int, Item>();
sortedList.Add(2, new Item(2, "Strawberry", 2));
sortedList.Add(1, new Item(1, "Apple", 2));

int i = 0;
while(sortedList.ContainsKey(i)) i++;
Item orange = new Item(i, "Orange", 2);
sortedList.Add(i, orange);

if(!sortedList.ContainsValue(orange)) i = 2;

Item item;
sortedList.TryGetValue(i, out item);
// item powinien rownac sie orange
```

### `Queue<T>`
Czyli kolejka. Jest to uporządkowana kolekcja, realizująca zasadę FIFO (_First In First Out_). Działa analogicznie jak kolejka niegeneryczna (`Queque`), z tym, że może przechowywać wyłącznie elementy określonego przez nas na początku (podczas tworzenia obiektu typu `Queue<T>`) typu (`T`).
#### Przykład
```csharp =
Queue<Item> queue = new Queue<Item>();
Item apple = new Item(1, "Apple", 2), strawberry = new Item(2, "Strawberry", 2);
queue.Enqueue(apple);
//queue: apple
queue.Enqueue(strawberry);
//queue: apple, strawberry

Item item =  queue.Peek();
//item: apple
//queue: apple, strawberry

Item item2 = queue.Dequeue();
//item2: apple
//queue: strawberry
```

### `Stack<T>`
Czyli stos. Jest to uporządkowana kolekcja, realizująca zasadę LIFO (_Last In First Out_). Działa analogicznie jak stos niegeneryczny (`Stack`), z tym, że może przechowywać wyłącznie elementy określonego przez nas na początku (podczas tworzenia obiektu typu `Stack<T>`) typu (`T`).
#### Przykład
```csharp =
Stack<Item> stack = new Stack<Item>();
Item apple = new Item(1, "Apple", 2), strawberry = new Item(2, "Strawberry", 2);
stack.Push(apple);
//stack: apple
stack.Push(strawberry);
//stack: apple, strawberry

Item item =  stack.Peek();
//item: strawberry
//stack: apple, strawberry

Item item2 = stack.Pop();
//item2: strawberry
//stack: apple
```

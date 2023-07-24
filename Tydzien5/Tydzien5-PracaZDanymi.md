# TYDZIEŃ 5 – Praca z danymi
## Spis treści
### [LEKCJA 1 – Powitanie](#lekcja-1--powitanie-1)
### [LEKCJA 2 – Kolekcje w .NET](#lekcja-2--kolekcje-w-net-1)
### [LEKCJA 3 – IQueryable i IEnumerable](#lekcja-3--iqueryable-i-ienumerable-1)
### [LEKCJA 4 – LINQ podstawy](#lekcja-4--linq-podstawy-1)
### [LEKCJA 5 – Manipulacje plikami](#lekcja-5--manipulacje-plikami-1)
### [LEKCJA 6 – Praca z XML](#lekcja-6--praca-z-xml-1)
### [LEKCJA 7 – Praca z JSON](#lekcja-7--praca-z-json-1)
### [LEKCJA 8 – Błędy początkujących](#lekcja-8--błędy-początkujących-1)
### [LEKCJA 9 – Praca domowa](#lekcja-9--praca-domowa-1)

## [LEKCJA 1 – Powitanie](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-5-praca-z-danymi/lekcja-1-powitanie/)
W tygodniu 5 nauczymy się pracować z danymi zewnętrznymi. Poza danymi wprowadzonymi przez użytkownika przez terminal, mogą to być dane dostarczane przez pliki w różnych formatach. Poznamy m.in. czym jest:
* JSON
* XML
* w jaki sposób pracować z plikami
* LINQ - do pracy z kolekcjami

W tym tygodniu zakończymy pracę nad naszą aplikacją konsolową.

## [LEKCJA 2 – Kolekcje w .NET](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-5-praca-z-danymi/lekcja-2-kolekcje-w-net/)
W naszych aplikacjach będziemy często korzystać z kolekcji. Są to swego rodzaju tablice, które przechowujemy na stercie. Wcześniej poznaliśmy już podstawowy typ kolekcji, czyli listę (`List<T>`). Jest ona najczęściej stosowanym typem kolekcji, jednak ma swoje ograniczenia. Może się więc zdarzyć, że do konkretnego celu bardziej odpowiedni będzie dla nas jeden z pozostałych typów. Warto więc pokrótce poznać, kilka podstawowych typów.

W C# wyróżniamy dwa rodzaje typów obsługujących kolekcje (zbiór) danych.
### Kolekcje niegeneryczne
Odkąd do standardu języka C# wprowadzono kolekcje generyczne, kolekcje niegeneryczne są rzadko stosowane. Przez to, że mogą one przechowywać dane różnego typu są one mniej wydajne.
#### `ArrayList`
Typ podobny do standardowej tablicy. W odróżnieniu od niej jest jednak przechowywany na stercie, a nie na stosie.

```csharp =
ArrayList arrayList = new ArrayList();
```

#### `SortedList`
Typ podobny do `ArrayList`, z tą różnicą, że składa się z pary klucz (_key_) - wartość (_value_). Przy czym klucz musi być unikalną liczbą typu `int`, inną dla każdego elementu (nie mogą się powtarzać w obrębie jednej kolekcji). Podczas dodawania nowych elementów do listy, jest ona automatycznie sortowana według klucza, w kolejności rosnącej.

```csharp =
SortedList sortedList = new SortedList();
```

#### `Stack`
Czyli stos. Struktura podobna w budowie do stosu w pamięci. Jest to struktura typu LIFO (_Last In First Out_), czyli ostatnio dodany do kolekcji element jest z niej pobierany i usuwany jako pierwszy.

```csharp =
Stack stack = new Stack();
```

Poza standardowymi metodami dodawania i usuwania jej elementów mamy tu dodatkowo trzy funkcje:
##### `Push`
Służy do dodania nowego elementu w odpowiednie miejsce kolekcji.
##### `Pop`
Służy do pobrania i usunięcia ostatnio dodanego (przy pomocy metody `Push`) elementu kolekcji.
##### `Peek`
Służy do pobrania ostatnio dodanego (przy pomocy metody `Push`) elementu kolekcji, jednak bez jego usuwania.

#### `Queue`
Czyli kolejka. Struktura typu FIFO (_First In First Out_), czyli pierwszy dodany do kolekcji element jest z niej usuwany jako pierwszy. Możemy sobie ją wyobrazić jako standardową kolejkę (np. w sklepie). Klient, który ustawił się w kolejce jako pierwszy, zostanie obsłużony w pierwszej kolejności.

```csharp =
Queue queue = new Queue();
```

Poza standardowymi metodami dodawania i usuwania elementów kolekcji, mamy tu dodatkowo trzy funkcje:
##### `Enqueue`
Służy do dodawania nowych elementów na końcu kolejki.
##### `Dequeue`
Służy do pobierania i usuwania pierwszego (dodanego jako pierwszy) elementu kolejki.
##### `Peek`
Służy do pobierania pierwszego (dodanego jako pierwszy) elementu kolejki, jednak bez jego usuwania.

#### `Hashtable`
Czyli tablica haszująca. Typ podobny do typu `SortedList`, również składający się z pary klucz - wartość. Tu jednak klucz jest typu `string`, a nie `int`, ale oczywiście wartości kluczy tu również nie mogą się powtarzać. W odróżnieniu od `SortedList` jej wartości nie są w żaden sposób sortowane.

```csharp =
Hashtable hashtable = new Hashtable();
```

### Kolekcje generyczne
Znacznie częściej stosowane i szeroko rozpowszechnione typy kolekcji. Generyczne, czyli, dla przypomnienia, takie, w których podajemy jakiego typu dane będziemy w nich przechowywać. Oznacz to, że do kolekcji generycznej będziemy mogli dodawać wyłącznie elementy typu, podanego na początku podczas deklaracji obiektu. Dzięki temu kolekcje generyczne są zdecydowanie szybsze, bardziej wydajne, a co za tym idzie preferowane przez programistów. Jednym z typów kolekcji generycznych jest poznana przez nas wcześniej generyczna lista (`List<T>`).
#### `List<T>`
Tradycyjna lista przechowująca elementy podanego typu `T`. Jest to najczęściej stosowany typ kolekcji, dający dużą swobodę dodawania i usuwania elementów.
##### Przykład
```csharp =
List<int> list = new List<int>();
// dodawanie pojedynczych elementow
list.Add(1);
list.Add(2);
list.Add(3);
// 1, 2, 3

// usuwanie pojedynczych elementow
list.Remove(3); // usuniecie elementu 3
// 1, 2

// usuwanie wszystkich elementow spełniajacych podany warunek
list.RemoveAll(item => item < 3);
// 

for(int i = 0; i < 10; i++)
{
    list.Add(i);
}
// 0, 1, 2, 3, 4, 5, 6, 7, 8, 9

// usuwanie kilku elementow
list.RemoveRange(3, 5); // 5 elementow, rozpoczynajac od indeksu 3
// 0, 1, 2, 8, 9

// usuwanie elementu o okreslonym indeksie
list.RemoveAt(3); // usuniecie elementu o indeksie 3
// 0, 1, 2, 9

// dodawanie na koniec kolekcji elementow z innej kolekcji
list.AddRange(list);
// 0, 1, 2, 9, 0, 1, 2, 9
```

#### `Dictionary<TKey, TValue>`
Czyli słownik. Kolekcja, podobnie jak kolekcje `SortedList`, czy `Hashtable`, przechowuje pary klucz - wartość. Zarówno klucz, jak i wartość może być dowolnego, podanego przez nas na początku, typu. Częstymi przykładami są słowniki przechowujące pary `<int, string>`, `<string, string>`, ale również zdefiniowane przez nas klasy np. `<int, Item>`. Elementy słownika nie są automatycznie sortowane. Poza metodami analogicznymi jak w liście, posiada on dodatkowo metody sprawdzające, czy słownik zawiera element o podanym kluczu (`slownik.ContainsKey(TKey key)`) lub wartości (`slownik.ContainsValue(TValue value)`), zwracające typ `bool` (`true`, jeśli zawiera, `false`, jeśli nie). Ciekawą metodą jest metoda `slownik.TryGetValue(TKey key, out TValue value)`, zwracająca wartość elementu o podanym kluczu, jeżeli został on znaleziony na liście. Oczywiście wartości kluczy w kolekcji nie mogą się powtarzać.
##### Przykład
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

#### `SortedList<TKey, TValue>`
Czyli posortowana lista. Jest ona podobna do tradycyjnej listy, ale również zawiera elementy słownika. Podobnie do tego drugiego składa się ona z par klucz - wartość. Analogicznie, zarówno klucz jak i wartość, mogą być dowolnego, zadeklarowanego na początku typu. Podobnie jak w poprzednich przypadkach, klucz musi być unikatowy (jego wartość nie może powtarzać się w obrębie kolekcji). `SortedList<TKey, TValue>` posiada takie same metody jak słownik. W odróżnieniu od niego, podobnie jak niegeneryczna `SortedList`, podczas dodawania nowych elementów do listy, są one automatycznie sortowane, zgodnie z rosnącą wartością klucza. Co za tym idzie, dodawanie nowych elementów do posortowanej listy jest wolniejsze niż w przypadku słownika. Natomiast późniejsze wyszukiwanie elementów jest znacznie szybsze.
##### Przykład
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

#### `Queue<T>`
Czyli kolejka. Jest to uporządkowana kolekcja, realizująca zasadę FIFO (_First In First Out_). Działa analogicznie jak kolejka niegeneryczna (`Queque`), z tym, że może przechowywać wyłącznie elementy określonego przez nas na początku (podczas tworzenia obiektu typu `Queue<T>`) typu (`T`).
##### Przykład
```csharp =
Queue<Item> queue = new Queue<Item>();
Item apple = new Item(1, "Apple", 2), strawberry = new Item(2, "Strawberry", 2);
queue.Enqueue(apple);
// queue: apple
queue.Enqueue(strawberry);
// queue: apple, strawberry

Item item =  queue.Peek();
// item: apple
// queue: apple, strawberry

Item item2 = queue.Dequeue();
// item2: apple
// queue: strawberry
```

#### `Stack<T>`
Czyli stos. Jest to uporządkowana kolekcja, realizująca zasadę LIFO (_Last In First Out_). Działa analogicznie jak stos niegeneryczny (`Stack`), z tym, że może przechowywać wyłącznie elementy określonego przez nas na początku (podczas tworzenia obiektu typu `Stack<T>`) typu (`T`).
##### Przykład
```csharp =
Stack<Item> stack = new Stack<Item>();
Item apple = new Item(1, "Apple", 2), strawberry = new Item(2, "Strawberry", 2);
stack.Push(apple);
// stack: apple
stack.Push(strawberry);
// stack: apple, strawberry

Item item =  stack.Peek();
// item: strawberry
// stack: apple, strawberry

Item item2 = stack.Pop();
// item2: strawberry
// stack: apple
```

## [LEKCJA 3 – IQueryable i IEnumerable](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-5-praca-z-danymi/lekcja-3-iqueryable-i-ienumerable/)
Są to główne interfejsy kolekcji używane do pracy z LINQ (więcej o LINQ w kolejnej lekcji). Są one podstawą do pracy z danymi. Najczęściej stosuje się je w stosunku do danych pobieranych z bazy danych. Oba interfejsy są do siebie bardzo podobne, a główna różnica między nimi, leży właśnie we współpracy z LINQ.

### Przykład
Napiszmy więc prosty przykład, który pomoże nam pokazać podstawową różnicę pomiędzy obydwoma interfejsami. Stworzymy w tym celu nową klasę `ItemService`. Jako naszą kolekcję wykorzystamy listę obiektów klasy `Item`. Ponieważ nie utworzyliśmy jeszcze żadnej bazy danych w nowo utworzonej klasie stworzymy pomocniczą metodę `Seed`, której zadaniem będzie stworzenie i zwrócenie przykładowych danych.

```csharp =
using System;

namespace Warehouse
{
    public class Item
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public int Quantity { get; set; }
        public ItemType Type { get; set; }
        public DateTime CreateDate { get; set; }
        public string CreatedBy { get; set; }
    }
}
```

```csharp =
using System;
using System.Collections.Generic;
using System.Text;
namespace Warehouse
{
    public enum ItemType
    {
        Clothing,
        Electronics,
        Grocery
    }
}
```

```csharp =
using System;
using System.Collections.Generic;
using System.Text;

namespace Warehouse
{
    public class ItemService
    {
        public IEnumerable<Item> Seed()
        {
            List<Item> items = new List<Item>();
            for (int i = 0; i < 500; i++)
            {
                Item item = new Item()
                {
                    Id = i,
                    Name = i.ToString(),
                    Quantity = i * 100,
                    Type = ItemType.Grocery,
                    CreateDate = DateTime.Now,
                    CreatedBy = "Me"
                };
                items.Add(item);
            }
            return items;
        }

        public IQueryable<Item> GetAllItemsQueryable()
        {
            IQueryable<Item> items = Seed().AsQueryable();
            return items;
        }

        public IEnumerable<Item> GetAllItemsEnumerable()
        {
            IQueryable<Item> items = Seed();
            return items;
        }

        public IQueryable<Item> GetNFirstItemsQueryable(int n)
        {
            IQueryable<Item> items = Seed().AsQueryable().Where(p => p.Id < n);
            return items;
        }

        public IEnumerable<Item> GetNFirstItemsEnumerable(int n)
        {
            IQueryable<Item> items = Seed().Where(p => p.Id < n);
            return items;
        }
    }
}
```

### Porównanie interfejsów `IQueryable` i `IEnumerable`
Zapytanie SQLowe które zostałoby stworzone w przypadku metod `GetAllItemsQueryable` i `GetAllItemsEnumerable` wyglądałoby dokładnie tak samo. W uproszczeniu:
```sql =
SELECT * FROM items;
```
co oznacza, pobierz wszystkie dane z tabeli `items`.
Różnica pojawi się w przypadku metod `GetNFirstItemsQueryable` i `GetNFirstItemsEnumerable`. 

W przypadku kolekcji `IQueryable` metoda `Where` spowoduje modyfikację zapytania SQLowego. Załóżmy, że `n = 100` Wówczas zapytanie SQLowe wyglądałoby teraz w uproszczeniu:
```sql =
SELECT * FROM items WHERE Id < 100;
```
co oznacza, pobierz z tabeli `items` wszystkie dane z rekordów, w których `Id` jest mniejsze niż `100`. Skutkiem takiego zapytania, byłoby pobranie do pamięci aplikacji tylko interesujących nas, przefiltrowanych danych. Po stronie aplikacji, nie musimy więc już nic robić.

W przypadku kolekcji `IEnumerable` zapytanie SQLowe pozostanie natomiast bez zmian (`SELECT * FROM items;`). Wszystkie dane z tabeli `items` zostaną pobrane do pamięci aplikacji. Następnie aplikacja dokonuje selekcji w obrębie pobranej kolekcji i zwraca interesujące nas dane. Oznacza to, że niepotrzebnie zajmujemy pamięć aplikacji i "dokładamy jej roboty".

Z powodu tych różnic kolekcje `IQueryable` używamy zawsze, gdy kolekcje pobieramy z bazy danych. Kolekcje `IEnumerable` są natomiast stosowane, gdy dane mamy już w pamięci programu (np. zostały stworzone przez inny serwis aplikacji) lub tak czy inaczej musimy pobrać do pamięci programu wszystkie dane (np. dla tego, że pobieramy je z pliku, a nie z bazy danych).

| Cecha | `System.Linq.IQueryable<T>` | `System.Collections.Generic.IEnumerable<T>` |
| ---: | :---: | :---: |
| Implementuje | `IEnumerable<T>`, `IEnumerable`, `IQueryable` | tylko jedną metodę `IEnumerator<T> GetEnumerator()`, która pozwala iterować po kolekcji |
| Pobieranie i filtrowanie kolekcji przy pomocy LINQ | pobiera przefiltrowaną kolekcję <br/> (filtrowanie po stronie bazy danych) | pobiera całą kolekcję, a następnie ją filtruje <br/> (filtrowanie po stronie aplikacji) |
| Zastosowanie | pobieranie przefiltrowanych danych z bazy danych | manipulacja danymi znajdującymi się już w pamięci programu lub pobieranie danych w całości (np. z pliku - filtracja możliwa dopiero w aplikacji, po pobraniu wszystkich danych) |

## [LEKCJA 4 – LINQ podstawy](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-5-praca-z-danymi/lekcja-4-linq-podstawy/)
LINQ (ang. _**L**anguage **IN**tegrated **Q**uery_) jest zbiorem metod rozszerzających służących do pracy z danymi (obiektami implementującymi interfejs `IQueryable`). Jest to właściwie podstawa współczesnego programowania. Jest używane głównie do zapisywania i odczytywania danych z bazy danych. Przy czym baza danych, nie musi być koniecznie bazą SQLową. Może być to dowolny obiekt zawierający dane (np. plik, dokument XML lub dowolne inne źródło danych) do którego odczytu (zapisu) mamy odpowiednią bibliotekę implementującą interfejs `IQueryable`. Dzięki temu nie interesuje nas skąd pochodzą dane. Niezależnie od źródła, operacje na danych (dodawanie, usuwanie itd.), wykonane przy pomocy LINQ, będą wyglądać tak samo.


Do pokazania podstawowych metod LINQ, użyjemy kolekcji tworzonej w poniższym przykładzie.
```csharp =
using System;

namespace Warehouse.Domain.Entity
{
    public class Item
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public int Quantity { get; set; }
        public int TypeId { get; set; }
        public DateTime CreateDate { get; set; }
        public string CreatedBy { get; set; }

        public Item(int id, string name, int typeId)
        {
            Id = id;
            Name = name;
            Quantity = 0;
            TypeId = typeId;
            CreateDate = DateTime.Now;
            CreatedBy = "Me";
        }
    }
}
```
```csharp =
using System.Collections.Generic;
using Warehouse.Domain.Entity;
using System.Linq;

namespace Warehouse.App.Concrete
{
    public class ListService
    {
        public void Method()
        {
            List<Item> list = Seed();
        }

        private List<Item> Seed()
        {
            List<Item> list = new List<Item>();
            list.Add(new Item(2, "Jeans", 3) { Quantity = 200 });
            list.Add(new Item(1, "TShirt", 3) { Quantity = 200 });
            list.Add(new Item(1, "Apple", 2) { Quantity = 500 });
            list.Add(new Item(3, "Strawberry", 2) { Quantity = 1500 });
            list.Add(new Item(2, "Pineapple", 2) { Quantity = 50 });

            return list;
        }
    }
}
```

Każdą z metod LINQ można zapisać na dwa, równoważne sposoby. Pierwszy, choć nadal jest to język C#, ma przypominać wyglądem składnię SQL (ang. _query syntax_). Drugi natomiast, jest zapisem łańcuchowym, skróconym, i bardziej przypomina składnię języka C# (ang. _chain (method) syntax_). Wybór metody zapisu nie ma wpływu na działanie programu i zależy wyłącznie od indywidualnych preferencji programisty.

### Wyszukiwanie

#### Zapis "SQL"
```csharp =
public void Method()
{
    List<Item> list = Seed();
    var items = from i in list
                where i.Quantity >= 500
                select i;
}
```

Powyższa instrukcja LINQ oznacza:
1. Z elementu `i` (lokalna nazwa zmiennej przechowującej obecnie przetwarzany element kolekcji) kolekcji `list`
2. gdzie `i.Quantity >= 500`
3. zwróć cały ten element.

Czyli podsumowując: Zwróć wszystkie elementy kolekcji `list`, których `Quantity` wynosi przynajmniej `500`.

To samo moglibyśmy zapisać przy pomocy pętli `foreach` i warunku `if`. Wyglądałoby to mniej więcej tak:

```csharp =
public void Method()
{
    List<Item> list = Seed();

    List<Item> filteredList = new List<Item>();
    foreach(Item i in list)
    {
        if(i.Quantity >= 500)
            filteredList.Add(i);
    }
    var items = filteredList.AsEnumerable();
}
```

Jak widać LINQ pozwala nam uprościć zapis i usprawnić działanie programu. Jeżeli chcemy, możemy ten zapis skrócić jeszcze bardziej, używając zapisu łańcuchowego (ang. _chain syntax_).

#### Zapis łańcuchowy (metodowy)
Zapis łańcuchowy korzysta z odpowiednich metod rozszerzających. W tym wypadku będzie to metoda `Where`.
```csharp =
public void Method()
{
    List<Item> list = Seed();
    var items = list.Where(i => i.Quantity >= 500);
}
```

### Uzyskiwanie przefiltrowanej kolekcji w postaci listy
W przypadku obu zapisów, otrzymamy w wyniku kolekcję typu `IEnumerable<Item>`. Gdybyśmy chcieli, żeby `items` było listą (typu `List<Item>`), to musielibyśmy na końcu dokonać konwersji, przy pomocy metody `ToList`. Czyli aby uzyskać taki sam efekt, jak w przypadku pętli:

```csharp =
public void Method()
{
    List<Item> list = Seed();

    List<Item> items = new List<Item>();
    foreach(Item i in list)
    {
        if(i.Quantity >= 500)
            items.Add(i);
    }
}
```

musimy zapisać
#### Zapis "SQL"

```csharp =
public void Method()
{
    List<Item> list = Seed();
    var items = (from i in list
                where i.Quantity >= 500
                select i).ToList();
}
```

lub
#### Zapis łańcuchowy (metodowy)
```csharp =
public void Method()
{
    List<Item> list = Seed();
    var items = list.Where(i => i.Quantity >= 500).ToList();
}
```

### Sortowanie
Elementy kolekcji możemy również posortować, według wybranej właściwości (jednej lub kilku), w kolejności malejącej lub rosnącej.
#### Zapis "SQL"

```csharp =
public void Method()
{
    List<Item> list = Seed();
    
    // sortowanie wdlug rosnacego Id
    var itemsIdAsc = from i in list
                orderby i.Id ascending
                select i;
    
    // sortowanie wdlug malejacego Id
    var itemsIdAsc = from i in list
                orderby i.Id descending
                select i;
    
    // sortowanie wedlug rosnacego TypeId i rosnacego Id
    var itemsIdAsc = from i in list
                orderby i.TypeId ascending
                orderby i.Id ascending
                select i;
}
```

#### Zapis łańcuchowy (metodowy)
```csharp =
public void Method()
{
    List<Item> list = Seed();
    
    // sortowanie wdlug rosnacego Id
    var itemsIdAsc = list.OrderBy(i => i.Id);
    
    // sortowanie wdlug malejacego Id
    var itemsIdDesc = list.OrderByDescending(i => i.Id);
    
    // sortowanie wedlug rosnacego TypeId i rosnacego Id
    var itemsTypeIdAscIdAsc = list.OrderBy(i => i.TypeId).ThenBy(i => i.Id);
}
```

### Zwracanie wybranych informacji z elementów kolekcji
Do tej pory, za każdym razem zwracaliśmy całe elementy kolekcji. Możemy jednak wyszukać tyko wybrane informacje o interesujących nas elementach kolekcji. Możemy zwracać zarówno wartości pojedynczych właściwości (np. `Id`, otrzymując kolekcję `IEnumerable<int>`), jak i kilku właściwości. W tym drugim przypadku możemy skorzystać np. z dynamicznie tworzonych typów anonimowych (tzw. typów `'a`). Są to typy, które nie mają określonej nazwy, mają za to ściśle określoną listę nazwanych elementów.
#### Zapis "SQL"
```csharp =
public void Method()
{
    List<Item> list = Seed();

    // wyszukiwanie Id elementow, o Quantity przynajmniej 500
    var items = (from i in list
                where i.Quantity >= 500
                select i.Id).ToList();  // items jest typu List<int>

    // wyszukiwanie Id i Name elementow, o Quantity przynajmniej 500
    var items = (from i in list
                where i.Quantity >= 500
                select new { IdOfElement = i.Id, NameOfElement = i.Name }).ToList();  
                // items jest typu List<'a>, gdzie 'a jest typem anonimowym skladajacym sie z dwoch elementow: int IdOfElement i string NameOfElement
}
```

#### Zapis łańcuchowy (metodowy)
```csharp =
public void Method()
{
    List<Item> list = Seed();

    // wyszukiwanie Id elementow, o Quantity przynajmniej 500
    var items = list.Where(i => i.Quantity >= 500).Select(i => i.Id).ToList();  // items jest typu List<int>
    
    // wyszukiwanie Id i Name elementow, o Quantity przynajmniej 500
    var items = list.Where(i => i.Quantity >= 500).Select(i => new { IdOfElement = i.Id, NameOfElement = i.Name }).ToList();  
        // items jest typu List<'a>, gdzie 'a jest typem anonimowym skladajacym sie z dwoch elementow: int IdOfElement i string NameOfElement
}
```

### Wyszukiwanie jednego elementu
Do tej pory za każdym razem zwracaliśmy kolekcję elementów. Może nas jednak interesować otrzymanie tylko jednego elementu. W tym celu stworzono metodę `FirstOrDefault` zwracającą pierwszy element spełniający dany warunek, bądź wartość domyślną, jeżeli nie znaleziono żadnego elementu, który by go spełniał. Wartością domyślną dla typów referencyjnych jest `null`, a np. dla typu `int` jest to `0`.
#### Zapis łańcuchowy (metodowy)
```csharp =
public void Method()
{
    List<Item> list = Seed();

    // wyszukiwanie elementu, o Id rownym 1 (pierwszego takiego elementu w kolekcji, czyli new Item(1, "TShirt", 3) { Quantity = 200 })
    var selectedItem = list.FirstOrDefault(i => i.Id == 1); // selectedItem jest typu Item
}
```

#### Zapis "SQL"
Powyższa metoda niema swojego odpowiednika w składni _query_. Jeżeli chcemy możemy więc jedynie połączyć obie formy zapisu.
```csharp =
public void Method()
{
    List<Item> list = Seed();

    // wyszukiwanie elementu, o Id rownym 1 (pierwszego takiego elementu w kolekcji, czyli new Item(1, "TShirt", 3) { Quantity = 200 })
    var selectedItem = (from i in list select i).FirstOrDefault(i => i.Id == 1);
    // lub
    selectedItem = (from i in list where i.Id == 1 select i).FirstOrDefault();
    // W obu przypadkach selectedItem jest typu Item
}
```

## [LEKCJA 5 – Manipulacje plikami](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-5-praca-z-danymi/lekcja-5-manipulacje-plikami/)
Aplikacje często korzystają z plików zewnętrznych. Pobieramy z nich dodatkowe informacje potrzebne do działania programu albo generujemy pliki zewnętrzne np. raporty, które będziemy chcieli potem rozesłać. Dlatego ważne jest, aby nauczyć się poprawnej pracy z plikami. Poznajmy więc podstawy odczytu i zapisu informacji do plików.
### Przykładowy plik
Zrobimy to na podstawie pliku CSV (_Comma Separated Value_), czyli formatu pliku w którym przechowywane wartości oddzielone są od siebie przecinkami, a w jednej linii umieszcza się informacje dotyczące tylko jednego elementu. W naszym pliku będziemy przechowywać informacje potrzebne do stworzenia kilku obiektów typu `Item`.
#### Klasa `Item`
Dla przypomnienia klasa Item wyglądała następująco:
```csharp =
using System;

namespace Warehouse.Domain.Entity
{
    public class Item
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public int Quantity { get; set; }
        public int TypeId { get; set; }
        public DateTime CreateDate { get; set; }
        public string CreatedBy { get; set; }

        public Item(int id, string name, int typeId)
        {
            Id = id;
            Name = name;
            Quantity = 0;
            TypeId = typeId;
            CreateDate = DateTime.Now;
            CreatedBy = "Me";
        }
    }
}
```
#### Plik CSV
Nasz przykładowy plik nazwiemy _items.csv_ i umieścimy w folderze _Temp_ na dysku _C_. Pełna ścieżka do pliku będzie więc wyglądać następująco: _C:\Temp\items.csv_. Przykładowa treść pliku:
```
1,Apple,2
2,Strawberry,2
3,Pineapple,2
1,TShirt,3
2,Jeans,3
1,Laptop,1
2,Screen,1
```
### Strumień pliku
#### Instrukcja _using_
Instrukcja, która pomaga w zarządzaniu pamięcią naszego programu. Zapewnia ona poprawne użycie obiektów `IDisposable`. W obrębie instrukcji `using` taki obiekt jest tylko do odczytu. `IDisposable` jest interfejsem udostępniającym mechanizmy zwalniania niezarządzanych zasobów, czyli takich których usuwanie nie odbywa się automatycznie. Przykładem klasy implementującej ten interfejs jest klasa `Stream` (strumień), czyli abstrakcyjna klasa zajmująca się obsługą strumieni danych. Jedną z jej klas pochodnych jest natomiast klasa `FileStream`, której zaraz użyjemy.

Tradycyjnie instrukcja `using` składa się ze słowa kluczowego `using` po którym w nawiasie okrągłym umieszczamy obiekt `IDisposable`, którego będziemy używać. Na końcu znajduje się umieszczony w klamrach blok instrukcji wykonywanych na tym obiekcie. Po zakończeniu ich wykonywania odpowiednia informacja trafia do _garbage collector_ i obiekt ten zostaje usunięty z pamięci programu.

Począwszy od C# 8.0 możliwy jest uproszczony zapis instrukcji `using`, bez nawiasów. Składa się on wówczas jedynie ze słowa kluczowego `using` i obiektu `IDisposable`. Na końcu instrukcji stawiamy tu oczywiście średnik. W tym wypadku blok instrukcji związanych z tą instrukcją `using` kończy się wraz z końcem bloku zawierającego instrukcję `using` (czyli najczęściej metody). Tam też właśnie zostaje automatycznie wywołana metoda `Dispose` obiektu `IDisposable`, odpowiedzialna za zwolnienie zajmowanej przez niego pamięci.

#### `FileStream`
Jak już wspominaliśmy przykładową klasą implementującą interfejs `IDisposable` jest klasa `FileStream`. Zajmuje się ona obsługą strumienia danych związanego z plikami zewnętrznymi. Kiedy nasz program uzyskuje dostęp do jakiegoś pliku znajdującego się na dysku, taki dostęp jest równocześnie zablokowany dla wszystkich innych. To znaczy, że jeżeli otworzymy w naszej aplikacji jakiś plik, to nie będziemy mogli tego pliku otworzyć i modyfikować w żadnym innym programie. Ponadto mogą to być bardzo duże pliki, a co za tym idzie, kiedy wczytamy je do programu, zajmą one dużo jego pamięci. Ze względu na te dwa powody, bardzo ważne jest aby kontrolować jak używamy plików w programie (kiedy dokładnie powinniśmy je otworzyć, a kiedy zamknąć i zwolnić pamięć). Klasa `FileStream`, jak każda klasa dziedzicząca po klasie `Stream` zawiera dane w postaci sekwencji bajtów. Odczytując dane z plików będziemy więc je uzyskiwać właśnie w takiej formie. Aby były one użyteczne w naszym programie będziemy musieli je najpierw zdekodować, a następnie odpowiednio obrobić.
#### Przykład
Zobaczmy więc jak wygląda instrukcja `using` i działa strumień plików na przykładzie prostego odczytu danych z naszego pliku _items.csv_.

##### Stary zapis instrukcji using
```csharp =
public void Method()
{
    using(FileStream fs = File.Open(@"C:\Temp\items.csv", FileMode.Open, FileAccess.Read))
    {
        byte[] buf = new byte[1024];
        int c;
        while ((c = fs.Read(buf, 0, buf.Length)) > 0)
        {
            string text = Encoding.UTF8.GetString(buf, 0, c);

            // Instrukcje do obrobki danych
        }

        // Tu zostanie wywolana metoda fs.Dispose();
    }

    // Ewentualne inne instrukcje
}
```
##### Skrócony zapis instrukcji using
```csharp =
public void Method()
{
    using FileStream fs = File.Open(@"C:\Temp\items.csv", FileMode.Open, FileAccess.Read);

    byte[] buf = new byte[1024];

    int c;
    while ((c = fs.Read(buf, 0, buf.Length)) > 0)
    {
        string text = Encoding.UTF8.GetString(buf, 0, c);

        // Instrukcje do obrobki danych
    }

    // Ewentualne inne instrukcje

    // Tu zostanie wywolana metoda fs.Dispose();
}
```

##### Wykonane czynności
1. Utworzenie strumienia pliku</br>
Pracę z plikiem rozpoczynamy od utworzenia związanego z nim strumienia danych.
2. Deklaracja bufora do odczytu danych</br>
Następnie deklarujemy bufor, w którym będziemy zapisywać porcje danych pobrane ze strumienia. Ponieważ pliki mogą być bardzo duże, wiec nie chcemy pobierać ich w całości do pamięci programu. Lepiej zrobić to mniejszymi porcjami. Ponieważ nasz plik jest krótki, wiec zadeklarowany bufor pomieści cały plik naraz, jednak nie zawsze będziemy mieć taką sytuację.
3. Deklaracja licznika odczytanych bajtów</br>
Deklarujemy również zmienną `c`, w której będziemy zapisywać ile danych faktycznie pobraliśmy ze strumienia. Jest to ważne, ponieważ, jeżeli w strumieniu pozostało mniej danych niż próbujemy pobrać, to ta wartość pomoże nam ustalić do którego miejsca bufora znajdują się nowe dane. Pozwoli nam ona również ustalić kiedy skończy się plik, gdyż ilość pobranych bajtów wyniesie wówczas `0`.
4. Pobieranie danych</br>
Kiedy mamy już otwarty strumień i zadeklarowane odpowiednie zmienne, możemy przejść do pobierania danych. Odczytane ze strumienia dane zapisujemy do bufora (od początku do końca bufora), dopóki nie dojdziemy do końca pliku (niema już czego pobierać, `c == 0`).
5. Dekodowanie danych
Każdą porcję pobranych bajtów, musimy zdekodować na bardziej użyteczny dla nas format. Używając odpowiedniego kodowania (najczęściej UTF-8) przekształcamy strumień bajtów na `string`.
6. Obróbka danych</br>
W końcu możemy przejść do obróbki pobranej przez nas porcji danych. Prawdopodobnie wykorzystamy jakieś wyrażenie regularne do wyszukania końca linii i podzielenia otrzymanego `string`a na elementy. Następnie zapewne użyjemy kolejnych wyrażeń regularnych do wyszukania przecinków w każdym elemencie i uzyskania pojedynczych wartości. Na koniec będziemy musieli sparsować dane z typu `string` na typ odpowiedniej właściwości (pola) i utworzyć nasze obiekty `Item`.
##### Użyte klasy i metody
* **`File`** - klasa związana z obsługą plików (otwieranie, zmykanie itd.).<br/>
* **`File.Open(string path, FileMode mode, FileAccess access, FileShare share)`** - metoda klasy `File` przyjmująca dwa (`path`, `mode`), trzy (`path`, `mode`, `access`) lub cztery (`path`, `mode`, `access`, `share`) argumenty, otwierająca plik, blokująca dostęp do niego dla innych programów i zwracająca obiekt typu `FileStream`.
    * `path` - `string` przedstawiający ścieżkę do pliku, który chcemy otworzyć.
    * `mode` - tryb otwieranego pliku, tzn. czy chcemy stworzyć nowy plik, otworzyć już istniejący, nadpisać plik lub otworzyć plik ustawiając kursor na jego końcu.
    * `access` - jaki dostęp do pliku chcemy uzyskać, tzn., czy chcemy z niego tylko odczytywać informacje, tylko zapisywać, a może jedno z drugim.
    * `share` - jakiego dostępu do plików chcemy udzielić wątkom pochodnym (odczyt, zapis, oba, usuwanie, żaden, dziedziczony).

* **`fs.Read(byte[] array, int offset, int count)`** - metoda klasy `FileStream` odczytująca `count` bajtów danych ze strumienia i zastępująca nimi dane z tablicy `array` począwszy od indeksu `offset`. Metoda zwraca liczbę rzeczywiście odczytanych bajtów.
* **`Encoding.UTF8.GetString(byte[] bytes, int index, int count)`** - metoda klasy `System.Text.Encoding`, zaimplementowana przez klasę `System.Text.UTF8Encoding`, dekodująca sekwencję bajtów do typu `string`. Korzystając z właściwości `Encoding.UTF8` uzyskujemy dostęp do obiektu klasy `UTF8Encoding`, implementującej metodę `GetString`, wykorzystując do dekodowania kodowanie UTF-8. Metoda zwraca `string` zawierający rezultat dekodowania podanej sekwencji bajtów. Występuje w kilku wariantach (może przyjmować różne argumenty). W zastosowanym, podajemy kolejno:
    * `bytes` - tablicę bajtów, zawierającą sekwencję bajtów, która ma zostać zdekodowana,
    * `index` - numer indeksu pierwszego bajtu do zdekodowania,
    * `count` - liczbę bajtów do zdekodowania.

#### `FileMode`
Enum określający jak system operacyjny powinien otworzyć plik.
```csharp =
namespace System.IO
{
    [Serializable]
    [ComVisible(true)]
    public enum FileMode
    {
        CreateNew = 1,
        Create,
        Open,
        OpenOrCreate,
        Truncate,
        Append
    }
}
```
##### 1. `CreateNew`
Oznacza, że system operacyjny powinien stworzyć nowy plik. Jeżeli plik już istnieje, to zostaje wyrzucony wyjątek `System.IO.IOException`. Użycie tej opcji wymaga uprawnienia do zapisu (`System.Security.Permissions.FileIOPermissionAccess.Write`).
##### 2. `Create`
Oznacza, że system operacyjny powinien stworzyć nowy plik. Jeżeli natomiast plik już istnieje, to zostanie on nadpisany. Użycie tej opcji również wymaga uprawnienia do zapisu (`System.Security.Permissions.FileIOPermissionAccess.Write`). Wybór tej opcji jest równoznaczny ze stwierdzeniem: jeżeli plik jeszcze nie istnieje to wybierz opcję `CreateNew`, w przeciwnym razie wybierz opcję `Truncate`. Jeżeli plik już istnieje, ale jest to plik ukryty zostaje wyrzucony wyjątek `System.UnauthorizedAccessException`, o nieautoryzowanym dostępie.
##### 3. `Open`
Oznacza, że system operacyjny powinien otworzyć istniejący plik. Jeżeli plik nie istnieje, zostaje wyrzucony wyjątek `System.IO.FileNotFoundException`. To jaki dostęp mamy do otwartego pliku (odczyt, zapis, oba) określa enum `System.IO.FileAccess`.
##### 4. `OpenOrCreate`
Oznacza, że jeżeli plik istnieje, system operacyjny powinien go otworzyć, w przeciwnym razie powinien stworzyć nowy plik. Jeżeli otwieramy plik do odczytu (`FileAccess.Read`), to jest wymagane uprawnienie do odczytu (`System.Security.Permissions.FileIOPermissionAccess.Read`). Jeżeli otwieramy plik do zapisu (`FileAccess.Write`), to wymagane jest uprawnienie do zapisu (`System.Security.Permissions.FileIOPermissionAccess.Write`). Gdy natomiast otwieramy plik do odczytu i zapisu (`FileAccess.ReadWrite`), to oba uprawnienia są wymagane (`System.Security.Permissions.FileIOPermissionAccess.Read` i `System.Security.Permissions.FileIOPermissionAccess.Write`).
##### 5. `Truncate`
Oznacza, że system operacyjny powinien otworzyć istniejący plik, a następnie wyczyścić całą jego zawartość, tak, aby jego rozmiar wynosił zero bajtów. Użycie tej opcji wymaga uprawnienia do zapisu (`System.Security.Permissions.FileIOPermissionAccess.Write`). Próba odczytu z pliku otwartego z opcją `Truncate` wywołuje wyjątek `System.ArgumentException`.
##### 6. `Append`
Oznacza, że system operacyjny powinien otworzyć plik, jeśli istnieje, i wyszukać jego koniec, lub stworzyć nowy plik. Wybór tej opcji wymaga pozwolenia do dołączania (`System.Security.Permissions.FileIOPermissionAccess.Append`). Może być ona stosowana wyłącznie w połączeniu z modyfikatorem dostępu do zapisu (`FileAccess.Write`). Po wybraniu tej opcji, próba wyszukania pozycji przed końcem pliku skutkuje wyjątkiem `System.IO.IOException`, natomiast próba odczytu z pliku wyjątkiem `System.NotSupportedException`.

#### `FileAccess`
Enum określający tryb dostępu do pliku.
```csharp
namespace System.IO
{
    [Serializable]
    [Flags]
    [ComVisible(true)]
    public enum FileAccess
    {
        Read = 0x1,
        Write = 0x2,
        ReadWrite = 0x3
    }
}
```

##### 1. `Read`
Oznacza dostęp do pliku w trybie odczytu. Dane mogą być więc odczytywane z pliku. Można zastosować go w połączeniu z trybem `Write`, aby umożliwić zarówno odczyt jaki zapis informacji do pliku.
##### 2. `Write`
Oznacza dostęp do pliku w trybie zapisu. Dane mogą być zapisywane do pliku. Można zastosować go w połączeniu z trybem `Read`, aby umożliwić zarówno zapis, jak i odczyt informacji z pliku.
##### 3. `ReadWrite`
Oznacza dostęp do pliku w trybie odczytu i zapisu. Dane mogą być zarówno zapisywane jak i odczytywane z pliku.
#### `FileShare`
Enum zawierający stałe określające jaki dostęp mogą mieć inne obiekty `System.IO.FileStream` do tego samego pliku.
```csharp =
namespace System.IO
{
    [Serializable]
    [Flags]
    [ComVisible(true)]
    public enum FileShare
    {
        None = 0x0,
        Read = 0x1,
        Write = 0x2,
        ReadWrite = 0x3,
        Delete = 0x4,
        Inheritable = 0x10
    }
}
```

##### 0. `None`
Odmawia dzielenia się dostępem do tego pliku. Jakiekolwiek próby otwarcia tego pliku przez ten lub inny proces nie powiodą się, dopóki plik nie zostanie zamknięty.
##### 1. `Read`
Pozwala na dalsze otwieranie pliku do odczytu. Jeśli ten znacznik nie jest określony, to wszelkie próby otwarcia pliku do odczytu przez ten lub inny proces nie powiodą się, dopóki plik nie zostanie zamknięty. Jednakże, nawet jeżeli ta flaga jest określona, to do otwarcia pliku wciąż mogą być potrzebne dodatkowe pozwolenia.
##### 2. `Write`
Pozwala na dalsze otwieranie pliku do zapisu. Jeśli ten znacznik nie jest określony, to wszelkie próby otwarcia pliku do zapisu przez ten lub inny proces nie powiodą się, dopóki plik nie zostanie zamknięty. Jednakże, nawet jeżeli ta flaga jest określona, to do otwarcia pliku wciąż mogą być potrzebne dodatkowe pozwolenia.
##### 3. `ReadWrite`
Pozwala na dalsze otwieranie pliku do odczytu lub zapisu. Jeśli ten znacznik nie jest określony, to wszelkie próby otwarcia pliku do odczytu lub zapisu przez ten lub inny proces nie powiodą się, dopóki plik nie zostanie zamknięty. Jednakże, nawet jeżeli ta flaga jest określona, to do otwarcia pliku wciąż mogą być potrzebne dodatkowe pozwolenia.
##### 4. `Delete`
Pozwala na późniejsze usunięcie pliku.
##### 16. `Inheritable`
Sprawia, że deskryptor pliku (ang. _file handle_) jest dziedziczony przez procesy potomne. Nie jest to bezpośrednio obsługiwane przez Win32.

### Odczyt danych z pliku
Pierwszą metodę do odczytu poznaliśmy w przykładzie wyżej. Omówmy ją sobie dokładniej.
#### Metoda `Read` klasy `FileStream`
```csharp
public override int Read (byte[] buffer, int offset, int count);
```
Metoda odczytuje blok bajtów ze strumienia i zapisuje dane w danym buforze.
##### Parametry:
1. `buffer` - gdy metoda wraca, parametr zawiera określoną tablicę bajtów z wartościami pomiędzy `offset` i `(offset + count - 1)` zastąpionymi przez bajty odczytane ze strumienia. Jeżeli potok zawierał mniej bajtów niż `count`, zamienione zostanie jedynie tyle wartości, ile zostało odczytanych (czyli między `offset` i `(offset + count - 1)` mogą znajdować się również stare wartości).
2. `offset` - przesunięcie bajtów w tablicy, w którym zostaną umieszczone odczytane bajty (numer indeksu od którego wartości zaczną być podmieniane).
3. `count` - maksymalna liczba bajtów do odczytu (ile bajtów chcemy odczytać). Zostanie odczytane `count` bajtów lub tyle ile zostało, jeżeli zostało mniej niż `count`.
##### Zwraca:
Metoda zwraca całkowitą liczbę bajtów wczytanych do bufora. Może to być mniej niż chcieliśmy, jeżeli taka liczba bajtów nie jest aktualnie dostępna, lub zero, jeżeli dotarliśmy do końca pliku.
##### Wyjątki:
1. `System.ArgumentNullException` - jeśli `buffer` jest `null`.
2. `System.ArgumentOutOfRangeException` - jeśli `offset` lub `count` są ujemne.
3. `System.NotSupportedException` - jeśli strumień danych nie wspiera odczytu danych.
4. `System.IO.IOException` - jeśli wystąpił błąd wejścia/wyjścia.
5. `System.ArgumentException` - jeśli `offset` i `count` opisują nieprawidłowy zakres w tablicy.
6. `System.ObjectDisposedException` - jeśli metoda była wywołana po zamknięciu strumienia (pliku).

#### Klasa `StreamReader`
Aby ułatwić sobie nieco zapis możemy skorzystać z klasy `System.IO.StreamReader`, implementującej klasę `System.IO.TextReader`, która odczytuje znaki za strumienia bajtów, używając odpowiedniego kodowania. Domyślnie jest to kodowanie UTF-8, o ile nie podano inaczej. Klasa implementuje interfejs `IDisposable`, będziemy więc tworzyć jej obiekty w połączeniu z instrukcją `using`. Do utworzenia obiektu tej klasy mamy do dyspozycji np. konstruktor przyjmujący jako argument strumień danych, ale także taki, przyjmujący ścieżkę do pliku:
```csharp =
using FileStream fs = File.Open(@"C:\Temp\items.csv", FileMode.Open, FileAccess.Read);
using StreamReader sr = new StreamReader(fs);
```
lub krócej
```csharp =
using StreamReader sr = new StreamReader(@"C:\Temp\items.csv");
```
Można go również utworzyć wywołując metodę `OpenText` na obiekcie klasy `FileInfo`:
```csharp =
FileInfo file = new FileInfo(@"C:\Temp\items.csv");
using StreamReader sr = file.OpenText();
```
lub jej statycznego odpowiednika klasy `File`
```csharp =
using StreamReader sr = File.OpenText(@"C:\Temp\items.csv");
```
Klasa posiada różne metody do odczytu tekstu ze strumienia bajtów.
##### `Read`
Podobnie jak klasa `FileStream`, posiada ona metodę `Read`. Może ona również występować w bardzo zbliżonej formie:
```csharp =
public override int Read (char[] buffer, int index, int count);
```
Zachowuje się ona analogicznie do przedstawionej wyżej metody, z tą różnicą, że dane zwracane do bufora są już zdekodowane i są typu `char`, a nie `byte`.

Powyższe przeciążenie występuje również pod nazwą `ReadBlock`:
```csharp =
public override int ReadBlock (char[] buffer, int index, int count);
```

Metoda `Read` ma również inne przeciążenia, np. bezargumentowe:
```csharp =
public override int Read ();
```
W tym wypadku metoda zwraca następny znak ze strumienia danych, przedstawiony jako `int` lub `-1`, jeżeli w strumieniu niema więcej znaków.
##### `Peek`
Inną ciekawą metodą jest metoda `Peek`:
```csharp =
public override int Peek ();
```
Zwraca ona wartości analogiczne do bezargumentowej metody `Read`. W odróżnieniu od niej nie przesuwa ona pozycji strumienia, czyli wywołując wielokrotnie metodę `Peek` lub wywołując metodę `Peek`, a następnie `Read` otrzymamy w rezultacie tą samą wartość.
##### `ReadLine`
Kolejną metodą jest `ReadLine`, która umożliwia pobieranie z pliku całych linii tekstu. Dzięki przedstawionym powyżej metodą klasy `StreamReader` zaoszczędziliśmy sobie pisanie implementacji dekodowania ciągu bajtów. Możemy jednak pójść jeszcze krok dalej. W późniejszej obróbce uzyskanych danych będziemy zapewne chcieli umieścić każdą linię tekstu z naszego pliku w osobnym `string`u, aby uzyskać informację dotyczącą jednego produktu. Funkcja `ReadLine` zrobi to już za nas.
```csharp =
public override string? ReadLine ();
```
Metoda zwraca następną linię ze strumienia wejściowego (pliku) lub `null`, gdy doszliśmy do końca pliku (lub innego używanego strumienia danych).
###### Przykład:
```csharp =
public void Method()
{
    using StreamReader sr = new StreamReader(@"C:\Temp\items.csv");

    string? itemInfo;
    while ((itemInfo = sr.ReadLine()) != null)
    {
        // stworzenie nowego obiektu Item przy uzyciu danych zawartych w itemInfo
        // dodanie obiektu do kolekcji
    }
}
```

#### Metody klasy `File`
Kolejne metody do odczytu danych z pliku dostarcza nam bezpośrednio klasa `System.IO.File`. W odróżnieniu od poprzednich nie wymagają ona jawnego użycia strumienia. Podobnie jak metoda powyżej, mają one wewnętrznie zaszyte dekodowanie i, w przypadku dwóch pierwszych metod, podział pobranych danych na linie. Wszystkie wymienione niżej metody występują w dwóch wariantach. Jeżeli używamy kodowania UTF-8, to możemy użyć wersji, przyjmującej tylko jeden argument - ścieżkę do pliku. Inaczej musimy również podać użyte w nim kodowanie.
##### Parametry:
1. `string path` - ścieżka do pliku, który ma zostać odczytany.
2. `[Encoding encoding = System.Text.Encoding.UTF8]` - kodowanie, które zostało zastosowane w odczytywanym pliku.
##### Wyjątki:
1. `ArgumentException` - w .NET Framework i .NET Core w wersjach starszych niż 2.1: jeżeli `path` ma długość `0`, składa się wyłącznie z białych znaków lub zawieraj przynajmniej jeden nieprawidłowy znak (jeden ze znaków zdefiniowanych przez metodę `GetInvalidPathChars()`).
2. `ArgumentNullException` - jeśli `path` jest `null`.
3. `DirectoryNotFoundException` - jeśli podana ścieżka jest nieprawidłowa (np. prowadzi do niezmapowanego dysku).
4. `FileNotFoundException` - jeśli plik wskazany przez ścieżkę `path` nie został znaleziony.
5. `IOException` - jeśli wystąpił błąd I/O (wejścia/wyjścia), podczas otwierania pliku.
6. `PathTooLongException` - jeśli `path` jest dłuższe niż zdefiniowana przez system maksymalna długość.
7. `SecurityException` - jeśli użytkownik nie posiada wymaganych uprawnień.
8. `UnauthorizedAccessException` - jeśli wskazany plik jest _read-only_ lub ta operacja nie jest wspierana przez używaną platformę, lub `path` wskazuje na folder, a nie plik, lub użytkownik nie ma wymaganych uprawnień.

##### `ReadLines` 
```csharp =
public static System.Collections.Generic.IEnumerable<string> ReadLines (string path);
```
```csharp =
public static System.Collections.Generic.IEnumerable<string> ReadLines (string path, System.Text.Encoding encoding);
```
###### Zwraca:
Metoda zwraca `IEnumerable<String>`, w związku z tym można ją stosować w połączeniu z LINQ. Zastosowana samodzielnie zawiera kolekcję wszystkich linii znajdujących się w podanym pliku. Zastosowana wewnątrz zapytania LINQ, zwraca tylko te linie, które są wynikiem zapytania. Przy czym, można rozpocząć wyliczanie, przed zwróceniem całej kolekcji. Od razu można więc przefiltrować zawartość pliku, tak, aby wyciągnąć z niego tylko interesujące nas linie tekstu.
###### Przykład:
Załóżmy, że z naszego pliku chcemy pobrać wyłącznie informacje dotyczące produktów spożywczych. Wówczas moglibyśmy napisać:
```csharp =
public void Method()
{
    var lines = File.ReadLines(@"C:\Temp\items.csv").Where(line => line.EndsWith(",2"));
    // Dalsza obrobka danych
}
```
###### Zastosowanie:
1. Wykonywanie zapytań LINQ (tzw. typu _LINQ to Objects_ (LINQ do obiektów), czyli wykonywanych bezpośrednio na kolekcjach `IEnumerable` lub `IEnumerable<T>`) do otrzymania z pliku przefiltrowanego zestawu linii.
2. Do zapisu otrzymanej kolekcji linii do pliku przy pomocy metody `File.WriteAllLines(String, IEnumerable<String>) ` lub dodania jej na końcu pliku, przy użyciu metody `File.AppendAllLines(String, IEnumerable<String>)`.
3. Do stworzenia natychmiast wypełnionej instancji kolekcji, która posiada konstruktor, przyjmujący jako argument kolekcję `IEnumerable<string>`, np. `IList<string>` lub `Queue<string>`.

##### `ReadAllLines`
```csharp =
public static string[] ReadAllLines (string path);
```
```csharp =
public static string[] ReadAllLines (string path, System.Text.Encoding encoding);
```
###### Zwraca:
Metoda zwraca tablicę `string`ów, zawierającą wszystkie linie znajdujące się w tekście. Wszelkie operacje na pobranych danych, ich filtrowanie itp. można wykonywać dopiero po pobraniu z pliku wszystkich plików, na gotowej tablicy.
###### Wyjątki:
Poza wyjątkami opisanymi wyżej, metoda może jeszcze zwrócić wyjątek `NotSupportedException`, jeżeli `path` jest w nieprawidłowym formacie.
###### Zastosowanie:
Kiedy potrzebujemy pobrać do pamięci naszej aplikacji całą zawartość pliku tekstowego i podzielić ją na pojedyncze linie.
##### `ReadAllText`
```csharp =
public static string ReadAllText (string path);
```
```csharp =
public static string ReadAllText (string path, System.Text.Encoding encoding);
```
###### Zwraca:
W odróżnieniu do dwóch poprzednich metod, metoda `ReadAllText`, nie dzieli treści pliku na linie. Cała zawartość strumienia, po zdekodowaniu, zostaje zapisana do pojedynczego `string`a.
###### Wyjątki:
Podobnie jak metoda `ReadAllLines`, poza wyliczonymi wyżej wyjątkami, może zwrócić wyjątek `NotSupportedException`, jeżeli `path` jest w nieprawidłowym formacie.
###### Zastosowanie:
Kiedy chcemy pobrać całą zawartość pliku, ale nie chcemy dzielić go na pojedyncze linie. Np. kiedy chcemy tylko wyświetlić zawartość pliku.
##### Porównanie metod
| Cecha | `ReadLines` | `ReadAllLines` | `ReadAllText` |
| :--- | :---: | :---: | :---: |
| Parametry | analogiczne | analogiczne | analogiczne |
| Zwracany typ | typ wyliczeniowy (`IEnumerable<string>`) | tablica (`string[]`) | `string` |
| Zwracane linie pliku | wszystkie lub wybrane | wszystkie | cała zawartość pliku, bez podziału na linie |
| Wyjątki | analogiczne | analogiczne + `NotSupportedException` | analogiczne + `NotSupportedException` |
| Użycie w zapytaniach LINQ | tak, nawet podczas pobierania | po pobraniu całości, jak każdą tablica | po pobraniu całości, jak każdy `string` |
| Możliwość filtrowanie | podczas odczytu | dopiero po odczycie całości | dopiero po odczycie całości |
| Domyślne kodowanie | UTF-8 | próbuje automatycznie wykryć kodowanie pliku, może wykryć kodowanie UTF-8 i UTF-32 (big-endian i little-endian) | próbuje automatycznie wykryć kodowanie pliku, może wykryć kodowanie UTF-8, UTF-16 (big-endian i little-endian) i UTF-32 (big-endian i little-endian), jeżeli plik rozpoczyna się odpowiednimi znacznikami kolejności bajtów (**BOM** - _**B**yte **O**rder **M**ark_) |

##### `ReadAllBytes`
Jeżeli nie chcemy dekodować treści pliku, bo np. zawiera on obrazek, a nie tekst, to klasa `File` posiada metodę również do tego:
```csharp =
public static byte[] ReadAllBytes (string path);
```
Otwiera ona plik binarny o podanej ścieżce. Następnie odczytuje cała zawartość pliku i zapisuje ją w tablicy bajtów. Na koniec, zamyka plik i zwraca utworzoną tablicę.

### Zapis danych do pliku
Do zapisu danych do pliku istnieję analogiczne metody jak w przypadku odczytu.
#### Metoda `Write` klasy `FileStream`
Analogicznie do metody `Read`, mamy w klasie `System.IO.FileStream` metodę `Write`.
```csharp =
public override void Write (byte[] buffer, int offset, int count);
```
Przyjmuje ona analogiczne argumenty, choć w tym wypadku `buffer`, w podanym przedziale, zawiera oczywiście dane do zapisu i nie zostaje zmieniony przez metodę. W odróżnieniu od metody `Read`, metoda `Write` nie zwraca żadnej wartości.
##### Przykład:
```csharp =
public void Method()
{
    using FileStream fs = File.Open(@"C:\Temp\items.csv", FileMode.Append, FileAccess.Write);

    byte[] buf = Encoding.UTF8.GetBytes("4,Watermelon,2");

    fs.Write(buf, 0, buf.Length);
}
```
1. Podobnie jak przy odczycie, najpierw musimy utworzyć strumień danych. W tym wypadku otwieramy go jednak z dostępem do zapisu (`Write`).
2. Następnie również tworzymy bufor bajtów. Tym razem będzie on jednak zawierał informacje, które chcemy zapisać. Aby zakodować `string`, jako tablicę bajtów, ponownie sięgamy do klasy `System.Text.Encoding`, a właściwie do dziedziczącej po niej klasy `System.Text.UTF8Encoding`. Implementuje ona metodę `GetBytes`, która koduje podany `string` na bajty, przy pomocy kodowania UTF-8 i zwraca uzyskane wartości jako tablicę.
3. Tym razem nie musimy więc tworzyć pętli, tylko możemy od razu zapisać wszystkie informacje.

| :warning:**UWAGA!** |
| :---: |
| Należy pamiętać, że metoda `Write` zapisuje dane do strumienia rozpoczynając od aktualnej pozycji strumienia. Ponieważ otwarcie strumienia pliku metodą `File.Open` w trybie `FileMode.Open` otwiera plik i ustawia pozycję strumienia na jego początku, oznacza to, że dane znajdujące się na początku pliku zostaną nadpisane przez nowe dane, a dalsza część pliku pozostanie bez zmian. Kiedy chcemy dopisać dane na końcu pliku, tak, aby obecna zawartość pliku pozostała nienaruszona, musimy zastosować modyfikator `Append`, jak to zrobiliśmy w powyższym przykładzie. Możemy również użyć opcji `Truncate`, gdybyśmy chcieli usunąć zawartość pliku i wstawić do niego wyłącznie nowe dane. |

Po zakończeniu zapisu metodą `Write`, pozycja strumienia jest ustawiona w miejscu zakończenia zapisu.

#### Klasa `StreamWriter`
Podobnie jak do odczytu danych ze strumienia danych mieliśmy do dyspozycji klasę `System.IO.StreamReader`, do zapisu istnieje klasa `System.IO.StreamWriter` implementująca klasę `System.IO.TextWriter`. Służy ona do zapisu znaków do strumienia bajtów, używając odpowiedniego kodowania. Domyślnie jest to ponownie kodowanie UTF-8, o ile nie podano inaczej. Ta klasa również implementuje interfejs `IDisposable`, będziemy więc tworzyć jej obiekty w połączeniu z instrukcją `using`. Do utworzenia obiektu tej klasy mamy do dyspozycji np. konstruktor przyjmujący jako argument strumień danych, ale także taki, przyjmujący ścieżkę do pliku:
```csharp =
using FileStream fs = File.Open(@"C:\Temp\items.csv", FileMode.Open, FileAccess.Read);
using StreamWriter sw = new StreamWriter(fs);
```
lub
```csharp =
using StreamWriter sw = new StreamWriter(@"C:\Temp\items.csv");
```
| :warning:**UWAGA!** |
| :---: |
| Jeżeli używamy konstruktora przyjmującego ścieżkę do pliku i taki plik już istnieje, to zostanie on nadpisany. W przeciwnym razie, plik zostanie utworzony. |

Obiekt klasy `StreamWriter` można również utworzyć wywołując metodę `CreateText` na obiekcie klasy `FileInfo`:
```csharp =
FileInfo file = new FileInfo(@"C:\Temp\items.csv");
using StreamWriter sw = file.CreateText();
```
lub jej statycznej wersji z klasy `File`:
```csharp =
using StreamWriter sw = File.CreateText(@"C:\Temp\items.csv");
```
| :warning:**UWAGA!** |
| :---: |
| Wywołanie tej metody powoduje utworzenie nowego pliku (nadpisanie pliku, jeżeli już istnieje). Plik o podanej ścieżce nie musi istnieć, ale musi istnieć katalog w którym się on znajduje. |

Jeżeli chcemy dopisać tekst do istniejącego pliku możemy natomiast skorzystać z metody `AppendText`:
```csharp =
FileInfo file = new FileInfo(@"C:\Temp\items.csv");
using StreamWriter sw = file.AppendText();
```
lub w wersji statycznej:
```csharp =
using StreamWriter sw = File.AppendText(@"C:\Temp\items.csv");
```
| :warning:**UWAGA!** |
| :---: |
| Wywołanie tej metody powoduje otwarcie istniejącego pliku i ustawienie się na jego końcu lub, gdy jeszcze nie istnieje, utworzenie nowego pliku. Plik o podanej nazwie nie musi istnieć, ale musi istnieć katalog, w którym ten plik ma się znaleźć. |

Klasa posiada różne metody do zapisu tekstu do strumienia bajtów.
##### `Write`
Podobnie jak klasa `FileStream`, posiada ona metodę `Write`. Może ona również występować w bardzo zbliżonej formie:
```csharp =
public override void Write (char[] buffer, int index, int count);
```
Zachowuje się ona analogicznie do przedstawionej wyżej metody, z tą różnicą, że dane w buforze są typu `char`, a nie `byte`.

Do metody `Write` możemy również dostarczyć dane, które chcemy zapisać w strumieniu, w postaci `string`a, sformatowanego `string`a, tablicy znaków `char[]` (jeśli podajemy tylko tablicę znaków, bez `index` i `count`, to do strumienia zostaną dopisane znaki z całej tablicy), pojedynczego znaku `char`, czy zakresu znaków `ReadOnlySpan<char>`.
##### `WriteLine`
Podobnie jak w klasie `StreamRead` mieliśmy metodę `ReadLine`, w klasie `StreamWriter` mamy metodę `WriteLine`. Przyjmuje ona jako argument `string` (sformatowany `string` lub `ReadOnlySpan<char>`), który chcemy dopisać do strumienia danych. Jak można się domyślić metoda, używając odpowiedniego kodowania, koduje podany ciąg znaków oraz znak nowej linii i zapisuje otrzymane bajty w strumieniu danych. Metoda nic nie zwraca.
###### Przykład:
```csharp =
public void Method()
{
    FileInfo file = new FileInfo(@"C:\Temp\items.csv");
    using StreamWriter sw = file.AppendText();

    sw.WriteLine("5,Melon,2");
}
```

#### Metody klasy `File`
Jak już wspominaliśmy przy okazji omówienia metody `ReadLines`, klasa `System.IO.File` posiada do zapisu danych do pliku m.in. metody `WriteAllLines` i `AppendAllLines`.
##### `WriteAllLines`
Jak można się domyślić metoda tworzy nowy plik (jeżeli plik już istnieje, to zostaje nadpisany), zapisuje do niego podaną kolekcję `string`ów, dodając po każdym z nich znak nowej linii i zamyka plik. Może ona przyjmować różne argumenty:
```csharp =
public static void WriteAllLines (string path, System.Collections.Generic.IEnumerable<string> contents);
```
```csharp =
public static void WriteAllLines (string path, string[] contents);
```
```csharp =
public static void WriteAllLines (string path, System.Collections.Generic.IEnumerable<string> contents, System.Text.Encoding encoding);
```
```csharp =
public static void WriteAllLines (string path, string[] contents, System.Text.Encoding encoding);
```
###### Parametry:
1. `path` - ścieżka do pliku, który ma zostać utworzony i do którego chcemy zapisać podane dane.
2. `contents` - linie tekstu, które chcemy zapisać w pliku.
3. `[encoding = System.Text.Encoding.UTF8]` - kodowanie, zgodnie z którym mają zostać zakodowane zapisywane w pliku informacje.

Jeżeli kodowanie nie zostało sprecyzowane, to domyślnie jest stosowane kodowanie UTF-8, bez znacznika kolejności bajtów (**BOM** - **B**yte **O**rder **M**ark). Jeżeli jest niezbędne umieszczenie identyfikatora kodowania UTF-8 (takiego jak BOM) na początku pliku, to trzeba jawnie podać kodowanie (użyć jednego z przeciążeń z parametrem `encoding`). Musimy to również zrobić, jeżeli chcemy użyć jakiegokolwiek innego kodowania.
###### Wyjątki:
1. `ArgumentException` - w .NET Framework i .NET Core w wersjach starszych niż 2.1: jeżeli `path` ma długość `0`, składa się wyłącznie z białych znaków lub zawieraj przynajmniej jeden nieprawidłowy znak (jeden ze znaków zdefiniowanych przez metodę `GetInvalidPathChars()`).
2. `ArgumentNullException` - jeśli `path` lub `contents` jest `null`.
3. `DirectoryNotFoundException` - jeśli podana ścieżka jest nieprawidłowa (np. prowadzi do niezmapowanego dysku).
4. `IOException` - jeśli wystąpił błąd I/O (wejścia/wyjścia), podczas otwierania pliku.
5. `PathTooLongException` - jeśli `path` jest dłuższe niż zdefiniowana przez system maksymalna długość.
6. `NotSupportedException` - jeśli `path` ma nieprawidłowy format.
7. `SecurityException` - jeśli użytkownik nie posiada wymaganych uprawnień.
8. `UnauthorizedAccessException` - jeśli wskazany plik jest _read-only_ lub ścieżka wskazuje na ukryty plik lub ta operacja nie jest wspierana przez używaną platformę, lub `path` wskazuje na folder, a nie plik, lub użytkownik nie ma wymaganych uprawnień.
##### `AppendAllLines`
Metoda otwiera plik (jeśli plik nie istnieje, to go tworzy), dopisuje podane informacje na jego końcu i zamyka go. Metoda ma tylko dwa przeładowania:
```csharp =
public static void AppendAllLines (string path, System.Collections.Generic.IEnumerable<string> contents);
```
```csharp =
public static void AppendAllLines (string path, System.Collections.Generic.IEnumerable<string> contents, System.Text.Encoding encoding);
```
###### Parametry:
1. `path` - ścieżka do pliku, do którego chcemy dopisać podane linie. Jeżeli plik o podanej ścieżce nie istnieje, zostanie on utworzony. Metoda może utworzyć nowy plik, ale nie nowe foldery. Tak więc ścieżka może zawierać wyłącznie istniejące foldery.
2. `contents` - linie tekstu, które chcemy dopisać do pliku.
3. `[encoding = System.Text.Encoding.UTF8]` - kodowanie, zgodnie z którym mają zostać zakodowane zapisywane w pliku informacje.
###### Wyjątki:
1. `ArgumentException` - jeżeli `path` ma długość `0`, składa się wyłącznie z białych znaków lub zawieraj przynajmniej jeden nieprawidłowy znak (jeden ze znaków zdefiniowanych przez metodę `GetInvalidPathChars()`).
2. `ArgumentNullException` - jeśli `path` lub `contents` jest `null`.
3. `DirectoryNotFoundException` - jeśli podana ścieżka jest nieprawidłowa (np. podany folder nie istnieje lub jest na niezmapowanym dysku).
4. `FileNotFoundException` - plik wskazany przez `path` nie został znaleziony.
5. `IOException` - jeśli wystąpił błąd I/O (wejścia/wyjścia), podczas otwierania pliku.
6. `PathTooLongException` - jeśli `path` jest dłuższe niż zdefiniowana przez system maksymalna długość.
7. `NotSupportedException` - jeśli `path` ma nieprawidłowy format.
8. `SecurityException` - jeśli użytkownik nie posiada uprawnienia do zapisu do pliku.
9. `UnauthorizedAccessException` - jeśli wskazany plik jest _read-only_ lub ta operacja nie jest wspierana przez używaną platformę, lub `path` wskazuje na folder, a nie plik.
###### Zastosowanie:
Można użyć tej metody, aby stworzyć plik zawierający:
1. wyniki zapytania LINQ to Objects na wierszach pliku, uzyskane przy użyciu metody `ReadLines`.
2. zawartość kolekcji implementującej interfejs `IEnumerable<T>` dla `string`ów.
##### `WriteAllText`
Metoda tworzy plik o podanej ścieżce, zapisuje w nim podaną treść i zamyka plik. Jeżeli wskazany plik już istnieje, to zostaje on nadpisany. Metoda występuje w dwóch przeładowaniach:
```csharp =
public static void WriteAllText (string path, string? contents);
```
```csharp =
public static void WriteAllText (string path, string? contents, System.Text.Encoding encoding);
```
###### Parametry:
1. `path` - ścieżka do pliku, który ma zostać utworzony i do którego chcemy zapisać podaną treść.
2. `contents` - tekst, który chcemy zapisać w pliku.
3. `[encoding = System.Text.Encoding.UTF8]` - kodowanie, zgodnie z którym ma zostać zakodowany zapisywany w pliku tekst.

Jeżeli kodowanie nie zostało sprecyzowane, to domyślnie jest stosowane kodowanie UTF-8, bez znacznika kolejności bajtów (**BOM** - **B**yte **O**rder **M**ark). Jeżeli jest niezbędne umieszczenie identyfikatora kodowania UTF-8 (takiego jak BOM) na początku pliku, to trzeba jawnie podać kodowanie (użyć jednego z przeciążeń z parametrem `encoding`). Musimy to również zrobić, jeżeli chcemy użyć jakiegokolwiek innego kodowania.
###### Wyjątki:
1. `ArgumentException` - w .NET Framework i .NET Core w wersjach starszych niż 2.1: jeżeli `path` ma długość `0`, składa się wyłącznie z białych znaków lub zawieraj przynajmniej jeden nieprawidłowy znak (jeden ze znaków zdefiniowanych przez metodę `GetInvalidPathChars()`).
2. `ArgumentNullException` - jeśli `path` jest `null`.
3. `DirectoryNotFoundException` - jeśli podana ścieżka jest nieprawidłowa (np. prowadzi do niezmapowanego dysku).
4. `IOException` - jeśli wystąpił błąd I/O (wejścia/wyjścia), podczas otwierania pliku.
5. `PathTooLongException` - jeśli podana ścieżka, nazwa pliku lub oba są dłuższe niż zdefiniowana przez system maksymalna długość.
6. `NotSupportedException` - jeśli `path` ma nieprawidłowy format.
7. `SecurityException` - jeśli użytkownik nie posiada wymaganych uprawnień.
8. `UnauthorizedAccessException` - jeśli wskazany plik jest _read-only_ lub ścieżka wskazuje na ukryty plik lub ta operacja nie jest wspierana przez używaną platformę, lub `path` wskazuje na folder, a nie plik, lub użytkownik nie ma wymaganych uprawnień.
##### `AppendAllText`
Metoda otwiera plik, dopisuje na jego końcu podany tekst i zamyka plik. Jeżeli plik o podanej ścieżce nie istnieje metoda najpierw go tworzy. Ma ona, podobnie jak metoda `WriteAllText`, dwa przeciążenia:
```csharp =
public static void AppendAllText (string path, string? contents);
```
```csharp =
public static void AppendAllText (string path, string? contents, System.Text.Encoding encoding);
```
###### Parametry:
1. `path` - ścieżka do pliku, do którego chcemy dopisać podane linie. Jeżeli plik o podanej ścieżce nie istnieje, zostanie on utworzony. Metoda może utworzyć nowy plik, ale nie nowe foldery. Tak więc ścieżka może zawierać wyłącznie istniejące foldery.
2. `contents` - tekst, który chcemy dopisać do pliku.
3. `[encoding = System.Text.Encoding.UTF8]` - kodowanie, zgodnie z którym ma zostać zakodowany zapisywany w pliku tekst.
###### Wyjątki:
1. `ArgumentException` - w .NET Framework i .NET Core w wersjach starszych niż 2.1: jeżeli `path` ma długość `0`, składa się wyłącznie z białych znaków lub zawieraj przynajmniej jeden nieprawidłowy znak (jeden ze znaków zdefiniowanych przez metodę `GetInvalidPathChars()`).
2. `ArgumentNullException` - jeśli `path` jest `null`.
3. `DirectoryNotFoundException` - jeśli podana ścieżka jest nieprawidłowa (np. folder nie istnieje lub znajduje się na niezmapowanym dysku).
4. `IOException` - jeśli wystąpił błąd I/O (wejścia/wyjścia), podczas otwierania pliku.
5. `PathTooLongException` - jeśli podana ścieżka, nazwa pliku lub oba są dłuższe niż zdefiniowana przez system maksymalna długość.
6. `NotSupportedException` - jeśli `path` ma nieprawidłowy format.
7. `SecurityException` - jeśli użytkownik nie posiada wymaganych uprawnień.
8. `UnauthorizedAccessException` - jeśli wskazany plik jest _read-only_ lub ta operacja nie jest wspierana przez używaną platformę, lub `path` wskazuje na folder, a nie plik, lub użytkownik nie ma wymaganych uprawnień.
##### `WriteAllBytes`
Podobnie jak w przypadku odczytywania, klasa `File` posiada również metodę do zapisywanie informacji przedstawionej w postaci bajtów. W odróżnieniu jednak od wszystkich innych sposobów zapisu informacji mamy tu dostępną tylko jedną metodę:
```csharp =
public static void WriteAllBytes (string path, byte[] bytes);
```
Metoda `WriteAllBytes` tworzy nowy plik, zapisuje w nim podane bajty informacji i zamyka go. Jeżeli wskazany plik już istnieje, to zostaje on nadpisany.

Klasa `File` nie udostępnia metody umożliwiającej dopisywanie informacji, przedstawionej w postaci tablicy bajtów, na końcu istniejącego pliku.

## [LEKCJA 6 – Praca z XML](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-5-praca-z-danymi/lekcja-6-praca-z-xml/)
Pliki XML (ang. _E**x**tensible **M**arkup **L**anguage_, rozszerzalny język znaczników) są jednym z najczęściej używanych formatów do przechowywania danych tekstowych. Można w nim zapisywać dane dowolnego typu. Format ten jest uważany za bezpieczny. Jest on powiązany z W3C (World Wide Web Consortium), czyli organizacją zajmującą się ustanawianiem standardów pisania i przesyłu stron WWW. Każdy plik XML powinien spełniać pewne standardy zdefiniowane przez tą organizację. Bardzo często w plikach podajemy więc użyty XSD (_XML Schema Definition_ - schemat XML). Dzięki temu łatwo jest sprawdzić poprawność danego pliku, również przy pomocy walidatorów on-line. Dodatkowo XML to jedyny format wspierany przez natywne elementy platformy .NET. Jest więc on chętnie używany przez programistów webowych, czy to do zapisywania danych, gdy nie mamy dostępu do bazy danych, czy do przekazywania danych pomiędzy różnymi serwisami. Często stosuje się go np. do komunikacji z serwisami rządowymi, czy innymi firmami, do przesyłania danych przez API (_**A**pplication **P**rogramming **I**nterface_ - Interfejs Programowania Aplikacji).

### 1. Dodanie odpowiedniego _usinga_
Klasy do obsługi plików XML znajdują się w przestrzeni nazw `System.Xml.Serialization`. Aby z nich korzystać, musimy więc do sekcji _using_ dołączyć:
```csharp =
using System.Xml.Serialization;
```
### 2. Opatrzenie wszystkich właściwości atrybutami
Aby umożliwić konwersję naszych obiektów do/z plików XML, musimy zaznaczyć w jaki sposób chcemy dokonać serializacji danych. W klasach związanych z danymi (klasach projektu _NazwaAplikacji.Domain_, w których zdefiniowaliśmy nasze _Entities_), które chcemy zapisywać do pliku XML (i/lub odczytywać z niego), każdą publiczną właściwość/pole musimy opatrzyć atrybutem. W nawiasie kwadratowym umieszczamy typ atrybutu i w nawiasie okrągłym w cudzysłowie nazwę, jaką będzie miał dany atrybut. Do wyboru mamy m.in. typy:
#### `XmlAttribute`
Wskazuje, że dana publiczna właściwość (pole, parametr, zwracana wartość) jest główna dla danego obiektu i zostanie zserializowana jako atrybut XML. Czyli zostanie umieszczona w znaczniku otwierającym danego obiektu, a nie pomiędzy własnymi znacznikami. Np.:
```csharp =
[XmlAttribute("Id")]
public int Id { get; set; }
```
Co w wygenerowanym pliku XML będzie np. wyglądać
```xml =
<Item Id="1"></Item>
```
#### `XmlElement`
Wskazuje, że dana publiczna właściwość (pole, parametr, zwracana wartość) zostanie zserializowana jako normalny element XML, czyli umieszczona pomiędzy własnymi znacznikami. Np.:
```csharp =
[XmlElement("Name")]
public string Name { get; set; }
```
Co w wygenerowanym pliku XML będzie np. wyglądać
```xml =
<Name>TShirt</Name>
```
#### `XmlIgnore`
Wskazuje, że dana publiczna właściwość (pole) nie zostanie zserializowana. Używamy jej, gdy nie chcemy zapisywać znajdujących się tam danych w pliku. Np.:
```csharp =
[XmlIgnore]
public int CreatedById { get; set; }
```
### 3. Dodanie bezargumentowego konstruktora
Jeżeli klasa którą chcemy serializować nie posiada bezargumentowego konstruktora, to musimy go dodać. Np.:
```csharp =
public Item() {}
```
### 4. Użycie serializatora
Do serializacji (zamiany danych z obiektów na format XML) i deserializacji (zmiana danych z formatu XML na obiekt) służy klasa `XmlSerializer`. Tworząc instancję tej klasy, podajemy od razu obiekty jakiego typu będziemy za jej pomocą (de)serializować. Np.:
```csharp =
XmlSerializer xmlSerializer = new XmlSerializer(typeof(List<Item>));
```
Gdy zapisujemy w pliku dane o całej kolekcji obiektów, to zostają one umieszczone w jednym elemencie nadrzędnym (_root_). Jeżeli chcemy, to możemy dostosować właściwości tego elementu, np. nadać mu własną nazwę (inaczej znacznik będzie miał domyślną nazwę). Jeżeli serializowany obiekt może mieć wartość `null`, to powinniśmy również to zaznaczyć. Możemy to zrobić tworząc obiekt typu `XmlRootAttribute` i podając go również w konstruktorze serializatora. Np.:
```csharp =
XmlRootAttribute root = new XmlRootAttribute();
root.ElementName = "Items"; // wszystkie zserializowane elementy znajda sie pomiedzy znacznikami <Items></Items>
root.IsNullable = true; // jesli serializowany obiekt jest null, to znacznik bedzie mial atrybut xsi:nil="true"

XmlSerializer xmlSerializer = new XmlSerializer(typeof(List<Item>), root); // serializer z dostosowanym rootem
```
Jeżeli chcemy odczytać dane z pliku w którym zastosowaliśmy własny element nadrzędny, to również musimy to uwzględnić tworząc nasz serializator (musimy podać odpowiedni obiekt `XmlRootAttribute` w konstruktorze). Jeśli tego nie zrobimy wystąpi błąd o znalezieniu nierozpoznanego znacznika.
#### Zapis danych
Gdy opatrzyliśmy już odpowiednie elementy klas pożądanymi atrybutami, mamy przygotowaną kolekcję obiektów (lub jeden obiekt) do zapisu i stworzyliśmy serializator, możemy przejść do zapisu danych do pliku. Posłuży nam do tego metoda `Serialize` naszego serializatora. Serializuje ona podany obiekt do dokumentu XML. Przyjmuje ona przynajmniej dwa parametry. Jednym z nich jest oczywiście obiekt (kolekcja) którą chcemy zapisać. Musimy też przekazać jej w jakiejś formie dokument XML, w którym dane chcemy umieścić. Do wyboru mamy kilka możliwości. Możemy przekazać obiekt typu `XmlWriter`, `TextWriter`, `XmlSerializationWriter` lub po prostu `Stream` z dostępem `Write`. Dla przykładu stwórzmy obiekt `StreamWriter` (implementujący klasę `TextWriter`) dla pliku _C:\Temp\items.xml_ i przekażmy go jako parametr metody:
```csharp =
using StreamWriter sw = new StreamWriter(@"C:\Temp\items.xml");
xmlSerializer.Serialize(sw, list);
```
#### Odczyt danych
Jeżeli chcemy pobrać dane z istniejącego pliku XML i przekształcić je na obiekty, klasa `XmlSerializer` posiada do tego celu metodę `Deserialize`. Musimy do niej, w jakiejś formie, przekazać plik XML, z którego chcemy odczytać dane. Może być to obiekt klasy `XmlReader`, `TextReader`, `XmlSerializationReader` lub po prostu `Stream` z dostępem `Read`. Metoda zwróci nam obiekt typu `object?`. Aby więc uzyskać obiekt pożądanego typu musimy dokonać jawnej konwersji. Np.:
```csharp =
using StringReader stringReader = new StringReader(xml);
var xmlItems = (List<Item>)xmlSerializer.Deserialize(stringReader);
```
### Przykład
#### Stworzenie szablonu
```csharp =
using System.Xml.Serialization;

namespace NazwaAplikacji.Domain.Entity
{
    public class Item
    {
        // glowny atrybut
        [XmlAttribute("Id")]
        public int Id { get; set; }
        
        // zwykłe wlasciwosci
        [XmlElement("Name")]
        public string Name { get; set; }
        [XmlElement("Type")]
        public int TypeId { get; set; }
        [XmlElement("Quantity")]
        public int Quantity { get; set; }

        // ignorowane wlasciwosci
        [XmlIgnore]
        public int CreatedById { get; set; }
        [XmlIgnore]
        public DateTime CreatedDateTime { get; set; }
        [XmlIgnore]
        public int? ModifiedById { get; set; }
        [XmlIgnore]
        public DateTime? ModifiedDateTime { get; set; }

        protected bool isLowInWarehouse;

        // XmlSerializer wymaga bezargumentowego konstruktora
        public Item() {}

        public Item(int id, string name, int typeId)
        {
            Name = name;
            TypeId = typeId;
            Id = id;
        }
    }
}
```
#### Zapis danych do pliku
```csharp =
using NazwaAplikacji.Domain.Entity;
using System.Xml.Serialization;

namespace NazwaAplikacji.App.Concrete
{
    public class ListService
    {
        public void Create()
        {
            List<Item> list = new List<Item>();
            list.Add(new Item(1, "TShirt", 3) { Quantity = 200 });
            list.Add(new Item(2, "Jeans", 3) { Quantity = 200 });
            list.Add(new Item(1, "Apple", 2) { Quantity = 500 });
            list.Add(new Item(3, "Strawberry", 2) { Quantity = 1500 });
            list.Add(new Item(2, "Pineapple", 2) { Quantity = 50 });

            XmlRootAttribute root = new XmlRootAttribute();
            root.ElementName = "Items";
            root.IsNullable = true;
            XmlSerializer xmlSerializer = new XmlSerializer(typeof(List<Item>), root);

            using StreamWriter sw = new StreamWriter(@"C:\Temp\items.xml");
            xmlSerializer.Serialize(sw, list);
        }
    }
}
```
#### Wygenerowany plik XML (_C:\Temp\items.xml_)
```xml =
<?xml version="1.0" encoding="utf-8"?>
<Items xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <Item Id="1">
    <Name>TShirt</Name>
    <Type>3</Type>
    <Quantity>200</Quantity>
  </Item>
  <Item Id="2">
    <Name>Jeans</Name>
    <Type>3</Type>
    <Quantity>200</Quantity>
  </Item>
  <Item Id="1">
    <Name>Apple</Name>
    <Type>2</Type>
    <Quantity>500</Quantity>
  </Item>
  <Item Id="3">
    <Name>Strawberry</Name>
    <Type>2</Type>
    <Quantity>1500</Quantity>
  </Item>
  <Item Id="2">
    <Name>Pineapple</Name>
    <Type>2</Type>
    <Quantity>50</Quantity>
  </Item>
</Items>
```
Jak widać znacznik `Items` ma jeszcze dodatkowe atrybuty. Są w nich wskazane schematy według których plik został utworzony (schematy W3C).

#### Odczyt danych z pliku
```csharp =
using NazwaAplikacji.Domain.Entity;
using System.Xml.Serialization;

namespace NazwaAplikacji.App.Concrete
{
    public class ListService
    {
        public List<Item> Read()
        {
            XmlSerializer xmlSerializer = new XmlSerializer(typeof(List<Item>), new XmlRootAttribute("Items"));

            string xml = File.ReadAllText(@"C:\Temp\items.xml");
            
            using StringReader stringReader = new StringReader(xml);
            var xmlItems = (List<Item>)xmlSerializer.Deserialize(stringReader);

            return xmlItems;
        }
    }
}
```

## [LEKCJA 7 – Praca z JSON](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-5-praca-z-danymi/lekcja-7-praca-z-json/)
JSON (_**J**ava**S**cript **O**bject **N**otation_ - notacja obiektów JavaScript) jest jednym z najbardziej popularnych formatów danych występujących w programowaniu internetowym. Jak sama nazwa wskazuje, jest on powszechnie stosowany we frameworkach JavaScriptowych. Ponieważ jest on lekki, jeśli chodzi o zajmowaną pamięć, prosty i przejrzysty w zapisie i odczycie, został więc również chętnie przyjęty przez programistów .NET. Obiekt w formacie JSON zapisuje się w nawiasach klamrowych, w konwencji klucz-wartość. Tzn. W cudzysłowie umieszczamy nazwę właściwości i po dwukropku podajemy jej wartość. Właściwości oddzielamy od siebie przecinkami. Kolekcje obiektów umieszczamy w nawiasach kwadratowych. Poszczególne obiekty w kolekcji również oddzielamy przecinkami.
### Przykład
Weźmy np. dane z poprzedniej lekcji. Zapisane w formacie JSON wyglądałyby one mniej więcej:
```json =
[
  {"Id":1,"Name":"TShirt","Type":3,"Quantity":200},
  {"Id":2,"Name":"Jeans","Type":3,"Quantity":200},
  {"Id":1,"Name":"Apple","Type":2,"Quantity":500},
  {"Id":3,"Name":"Strawberry","Type":2,"Quantity":1500},
  {"Id":2,"Name":"Pineapple","Type":2,"Quantity":50}
]
```
Dane w formacie JSON możemy zapisać w plikach .txt.
### Biblioteka Newtonsoft.Json
W odróżnieniu do formatu XML, JSON nie jest natywnie wspierany przez platformę .NET. Zamiast pisać własną implementację serializacji i deserializacji, możemy jednak skorzystać z jednej z wielu gotowych bibliotek. Najpopularniejszą z nich jest `Newtonsoft.Json`. Aby użyć jej w naszym projekcie, musimy ją najpierw do niego dodać, np. przy pomocy _NuGet Package Manager_, podobnie jak to robiliśmy już wcześniej. Szczegółową dokumentację biblioteki można znaleźć na [stronie twórców](https://www.newtonsoft.com/json/help/html/Introduction.htm). Poniżej omówimy tylko najbardziej podstawowe funkcje.
#### Serializacja
Aby przekształcić nasze dane do formatu JSON możemy np. skorzystać z metody `SerializeObject`, statycznej klasy `JsonConvert`.
##### Wersja podstawowa
W najprostszej wersji, używając ustawień domyślnych, przekazujemy do metody tylko obiekt, którego dane chcemy zapisać w formacie JSON. W efekcie działania metody, otrzymamy `string` z zserializowanym obiektem. Np.
```csharp =
List<Item> list = new List<Item>();
list.Add(new Item(1, "TShirt", 3) { Quantity = 200 });
list.Add(new Item(2, "Jeans", 3) { Quantity = 200 });
list.Add(new Item(1, "Apple", 2) { Quantity = 500 });
list.Add(new Item(3, "Strawberry", 2) { Quantity = 1500 });
list.Add(new Item(2, "Pineapple", 2) { Quantity = 50 });

string output = JsonConvert.SerializeObject(list);
```
Bez żadnych dodatkowych działań, nie otrzymamy jednak takiego efektu jak w przykładzie powyżej. Metoda użyje jako wartości kluczy nazw właściwości oraz przypisze nam również domyślne wartości dla niezainicjalizowanych właściwości:
```json =
[{"Id":1,"Name":"TShirt","TypeId":3,"Quantity":200,"CreatedById":0,"CreatedDateTime":"0001-01-01T00:00:00","ModifiedById":null,"ModifiedDateTime":null},{"Id":2,"Name":"Jeans","TypeId":3,"Quantity":200,"CreatedById":0,"CreatedDateTime":"0001-01-01T00:00:00","ModifiedById":null,"ModifiedDateTime":null},{"Id":1,"Name":"Apple","TypeId":2,"Quantity":500,"CreatedById":0,"CreatedDateTime":"0001-01-01T00:00:00","ModifiedById":null,"ModifiedDateTime":null},{"Id":3,"Name":"Strawberry","TypeId":2,"Quantity":1500,"CreatedById":0,"CreatedDateTime":"0001-01-01T00:00:00","ModifiedById":null,"ModifiedDateTime":null},{"Id":2,"Name":"Pineapple","TypeId":2,"Quantity":50,"CreatedById":0,"CreatedDateTime":"0001-01-01T00:00:00","ModifiedById":null,"ModifiedDateTime":null}]
```
##### Zmiana formatowania `string`a wynikowego
Inne przeładowania funkcji umożliwiają np. sformatowanie pliku do bardziej czytelnej formy. Polecenie:
```csharp =
string output = JsonConvert.SerializeObject(list, Formatting.Indented);
```
Zmieni formatowanie `string`u wynikowego na:
```json =
[
  {
    "Id": 1,
    "Name": "TShirt",
    "TypeId": 3,
    "Quantity": 200,
    "CreatedById": 0,
    "CreatedDateTime": "0001-01-01T00:00:00",
    "ModifiedById": null,
    "ModifiedDateTime": null
  },
  {
    "Id": 2,
    "Name": "Jeans",
    "TypeId": 3,
    "Quantity": 200,
    "CreatedById": 0,
    "CreatedDateTime": "0001-01-01T00:00:00",
    "ModifiedById": null,
    "ModifiedDateTime": null
  },
  {
    "Id": 1,
    "Name": "Apple",
    "TypeId": 2,
    "Quantity": 500,
    "CreatedById": 0,
    "CreatedDateTime": "0001-01-01T00:00:00",
    "ModifiedById": null,
    "ModifiedDateTime": null
  },
  {
    "Id": 3,
    "Name": "Strawberry",
    "TypeId": 2,
    "Quantity": 1500,
    "CreatedById": 0,
    "CreatedDateTime": "0001-01-01T00:00:00",
    "ModifiedById": null,
    "ModifiedDateTime": null
  },
  {
    "Id": 2,
    "Name": "Pineapple",
    "TypeId": 2,
    "Quantity": 50,
    "CreatedById": 0,
    "CreatedDateTime": "0001-01-01T00:00:00",
    "ModifiedById": null,
    "ModifiedDateTime": null
  }
]
```
Domyślnie wybrana jest opcja `Formatting.None`, tworząca jednowierszowy wynik.
##### Zmiana sposobu serializacji
Jeżeli chcemy zmienić sposób serializowania np. jakiegoś typu danych, używamy przeładowania przyjmującego obiekt klasy `JsonSerializerSettings` lub kolekcję `JsonConverter[]`, zawierającą obiekty, które zostaną użyte do wykonania konwersji.
##### Użycie atrybutów - wybór właściwości do serializacja
Podobnie jak to było w przypadku biblioteki do serializowania obiektów do XML, biblioteka `Newtonsoft.Json` umożliwia nam również przypisanie atrybutów do poszczególnych właściwości. Jeśli więc chcielibyśmy otrzymać wynik zbliżony do pokazanego w przykładzie na początku tej lekcji, musimy zmodyfikować kod naszej klasy `Item` dodając atrybuty:
```csharp =
namespace NazwaAplikacji.Domain.Entity
{
    public class Item
    {
        // glowny atrybut - musi posiadac wartosc
        [JsonRequired]
        public int Id { get; set; }

        // zwykle wlasciwosci
        [JsonProperty]
        public string Name { get; set; }

        [JsonProperty("Type")]  // uzywamy w JSON skroconej nazwy
        public int TypeId { get; set; }

        [JsonProperty]
        public int Quantity { get; set; }

        // ignorowane wlasciwosci
        [JsonIgnore]
        public int CreatedById { get; set; }

        [JsonIgnore]
        public DateTime CreatedDateTime { get; set; }

        [JsonIgnore]
        public int? ModifiedById { get; set; }

        [JsonIgnore]
        public DateTime? ModifiedDateTime { get; set; }

        protected bool isLowInWarehouse;

        public Item() { }

        public Item(int id, string name, int typeId)
        {
            Name = name;
            TypeId = typeId;
            Id = id;
        }

        [JsonConstructor]
        public Item(string name)
        {
            Name = name;
            CreatedById = 1;
            CreatedDateTime = DateTime.Now;
        }
    }
}
```
Wówczas wywołując metodę:
```csharp =
string output = JsonConvert.SerializeObject(list, Formatting.Indented);
```
Otrzymamy `string`:
```json =
[
  {
    "Id": 1,
    "Name": "TShirt",
    "Type": 3,
    "Quantity": 200
  },
  {
    "Id": 2,
    "Name": "Jeans",
    "Type": 3,
    "Quantity": 200
  },
  {
    "Id": 1,
    "Name": "Apple",
    "Type": 2,
    "Quantity": 500
  },
  {
    "Id": 3,
    "Name": "Strawberry",
    "Type": 2,
    "Quantity": 1500
  },
  {
    "Id": 2,
    "Name": "Pineapple",
    "Type": 2,
    "Quantity": 50
  }
]
```
#### Deserializacja
Do deserializacji `string`u zawierającego dane w formacie JSON z powrotem do obiektu naszego typu możemy użyć metody generycznej `DeserializeObject<T>`, statycznej klasy `JsonConvert`. Jeżeli do serializacji użyliśmy własnych konwerterów lub jakichś innych własnych ustawień serializatora, musimy je podać również jako argumenty tej metody (Jeżeli użyliśmy przeładowania `SerializeObject` z obiektem typu `JsonSerializerSettings` lub `JsonConverter[]`, musimy odpowiedni obiekt przekazać również do tej funkcji). W przeciwnym razie wystarczy podać do metody `string` który chcemy zdeserializować. Wynikiem działania metody jest obiekt podanego typu `T`.

##### Atrybut `JsonConstructor`
Jeżeli przed jednym z konstruktorów klasy do której będziemy deserializować umieścimy atrybut `JsonConstructor`, to właśnie on zostanie użyty do odtworzenia obiektów. Inaczej zostanie użyty konstruktor domyślny: bezargumentowy, a jeżeli nie istnieje to pojedynczy publiczny argumentowy (gdy jest kilka, to ten, którego argumenty najbardziej pasują do posiadanych danych), a gdy również nie istnieje, to niepubliczny. Tylko jeden konstruktor w danej klasie może być opatrzony atrybutem `JsonConstructor`.

##### Przykład
Załóżmy, że chcemy zdeserializować `output` otrzymany w którymś z przykładów powyżej. Napiszemy wówczas:
```csharp =
var list = JsonConvert.DeserializeObject<List<Item>>(output);
```
Otrzymany obiekt `list`, będzie typu `List<Item>?`. Będzie on zawierał listę obiektów `Item`, o wartościach parametrów zgodnych z przechowywanymi danymi. Jeżeli użyjemy zmodyfikowanej klasy `Item` ( z przypisanymi atrybutami JSON), to wówczas wartość `CreatedById` wszystkich utworzonych obiektów wyniesie `1`, a `CreatedDateTime`, czas tworzenia danego obiektu, zgodnie z konstruktorem oznaczonym do użycia. Stanie się tak niezależnie `output` który przykładu użyjemy (nawet jeśli w danych w JSON mieliśmy przypisane inne wartości dla tych parametrów).

#### Zapis do pliku
Jeżeli dane zapisane w formacie JSON chcemy zapisać do pliku, a nie używać jedynie wewnętrznie w programie (np. do wymiany danych pomiędzy warstwami aplikacji), możemy do tego użyć metody `Serialize` klasy `JsonSerializer`. Przyjmuje ona strumień do zapisu w postaci obiektu typu `TextWriter` lub `JsonWriter`, obiekt do serializacji i opcjonalnie typ tego obiektu.
* **`JsonWriter`** - Klasa abstrakcyjna biblioteki `Newtonsoft.Json`. Reprezentuje moduł zapisujący dedykowany do danych JSON. Implementuje on interfejs `IDisposable`, więc obiekty tego typu będziemy stosować w połączeniu z instrukcją `using`.
* **`JsonTextWriter`** - Klasa biblioteki `Newtonsoft.Json`, implementująca klasę `JsonWriter`. Posiada ona jednoargumentowy konstruktor przyjmujący obiekt typu `TextWriter`.

Aby dokonać serializacji obiektu do pliku musimy:
1. Utworzyć obiekt typu `TextWriter`, zapisujący dane w naszym pliku docelowym. Ponieważ klasa `TextWriter` jest abstrakcyjna, możemy więc np. utworzyć obiekt implementującej ją klasy `StreamWriter`. Jak już wspominaliśmy dane w formacie JSON zapisujemy w zwykłym pliku tekstowym (pliku z rozszerzeniem .txt).
```csharp =
using StreamWriter sw = File.CreateText(@"C:\Temp\items.txt");
```
2. Moglibyśmy teraz przekazać utworzony w poprzednim punkcie obiekt do serializatora. Skorzystajmy jednak z writera dedykowanego do zapisu danych JSON. Zmieńmy również formatowanie tworzonego JSONa, na bardziej czytelne.
```csharp =
using JsonWriter writer = new JsonTextWriter(sw) { Formatting = Formatting.Indented };
```
3. Mając gotowy writer dla naszego pliku, możemy utworzyć serializer i dokonać serializacji naszego obiektu, zapisując wynik w pliku _C:\Temp\items.txt_.
```csharp =
JsonSerializer serializer = new JsonSerializer();
serializer.Serialize(writer, list); // list to nasz serializowany obiekt, ktory wczesniej stworzylismy
```

I gotowe. Oczywiście możemy również utworzyć obiekt `string` z zapisanymi danymi w formacie JSON, jak to robiliśmy na początku lekcji i potem zapisać jego treść do pliku jednym ze sposobów poznanych w lekcji 5. tego tygodnia.

#### Odczyt z pliku
Analogicznie do zapisu możemy odczytywać dane JSON z pliku i deserializować je do obiektu odpowiedniego typu. Tym razem będziemy oczywiście, zamiast writerów, używać readerów. Metoda do deserializacji, podobnie jak ta użyta wcześniej, występuje również w wersji generycznej. Żeby odczytać zapisane przed chwilą dane możemy napisać np.:
```csharp =
using StreamReader sr = File.OpenText(@"C:\Temp\items.txt");
using JsonReader reader = new JsonTextReader(sr);
JsonSerializer serializer = new JsonSerializer();
var list2 = serializer.Deserialize<List<Item>>(reader);
```
Oczywiście możemy również odczytać tekst z pliku tak jak to robiliśmy w lekcji 5. tego tygodnia i później dokonać deserializacji tak jak to robiliśmy wcześniej w tej lekcji.

## [LEKCJA 8 – Błędy początkujących](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-5-praca-z-danymi/lekcja-8-bledy-poczatkujacych/)
1. **Zamykaj pliki!**</br>
Podczas pracy z plikami, należy pamiętać o ich zamykaniu. Puki nie zamkniemy pliku, inne programy nie będą mogły go używać. Wówczas nie będziemy mogli go otworzyć, przenieść, zmodyfikować itd., póki nie zamkniemy naszej aplikacji. Również w naszym programie nie będziemy mogli w takiej sytuacji ponownie skorzystać z tego pliku. Niepoprawne zamknięcie pliku może również spowodować utratę zapisanych tam danych.
2. **Ćwicz LINQ**</br>
Warto dobrze przećwiczyć użycie zapytań LINQ, bo często będziemy ich używać.

## [LEKCJA 9 – Praca domowa](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-5-praca-z-danymi/lekcja-9-praca-domowa/)
W tym tygodniu, do naszej aplikacji dodajmy pracę z plikami. Przenieśmy część danych z kodu do pliku i wczytajmy je z niego. Zapiszmy też jakieś dane do pliku, np. wynik działania naszej aplikacji, wynik rozgrywki, raport itp.


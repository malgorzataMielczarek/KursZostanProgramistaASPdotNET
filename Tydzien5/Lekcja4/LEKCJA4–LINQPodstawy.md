# [LEKCJA 4 – LINQ podstawy](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-5-praca-z-danymi/lekcja-4-linq-podstawy/)
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

## Wyszukiwanie

### Zapis "SQL"
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

### Zapis łańcuchowy (metodowy)
Zapis łańcuchowy korzysta z odpowiednich metod rozszerzających. W tym wypadku będzie to metoda `Where`.
```csharp =
public void Method()
{
    List<Item> list = Seed();
    var items = list.Where(i => i.Quantity >= 500);
}
```

## Uzyskiwanie przefiltrowanej kolekcji w postaci listy
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
### Zapis "SQL"

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
### Zapis łańcuchowy (metodowy)
```csharp =
public void Method()
{
    List<Item> list = Seed();
    var items = list.Where(i => i.Quantity >= 500).ToList();
}
```

## Sortowanie
Elementy kolekcji możemy również posortować, według wybranej właściwości (jednej lub kilku), w kolejności malejącej lub rosnącej.
### Zapis "SQL"

```csharp =
public void Method()
{
    List<Item> list = Seed();
    //sortowanie wdlug rosnacego Id
    var itemsIdAsc = from i in list
                orderby i.Id ascending
                select i;
    //sortowanie wdlug malejącego Id
    var itemsIdAsc = from i in list
                orderby i.Id descending
                select i;
    //sortowanie według rosnącego TypeId i rosnącego Id
    var itemsIdAsc = from i in list
                orderby i.TypeId ascending
                orderby i.Id ascending
                select i;
}
```

### Zapis łańcuchowy (metodowy)
```csharp =
public void Method()
{
    List<Item> list = Seed();
    //sortowanie wdlug rosnacego Id
    var itemsIdAsc = list.OrderBy(i => i.Id);
    //sortowanie wdlug malejącego Id
    var itemsIdDesc = list.OrderByDescending(i => i.Id);
    //sortowanie według rosnącego TypeId i rosnącego Id
    var itemsTypeIdAscIdAsc = list.OrderBy(i => i.TypeId).ThenBy(i => i.Id);
}
```

## Zwracanie wybranych informacji z elementów kolekcji
Do tej pory, za każdym razem zwracaliśmy całe elementy kolekcji. Możemy jednak wyszukać tyko wybrane informacje o interesujących nas elementach kolekcji. Możemy zwracać zarówno wartości pojedynczych właściwości (np. `Id`, otrzymując kolekcję `IEnumerable<int>`), jak i kilku właściwości. W tym drugim przypadku możemy skorzystać np. z dynamicznie tworzonych typów anonimowych (tzw. typów `'a`). Są to typy, które nie mają określonej nazwy, mają za to ściśle określoną listę nazwanych elementów.
### Zapis "SQL"
```csharp =
public void Method()
{
    List<Item> list = Seed();
    //wyszukiwanie Id elementów, o Quantity przynajmniej 500
    var items = (from i in list
                where i.Quantity >= 500
                select i.Id).ToList();  //items jest typu List<int>

    //wyszukiwanie Id i Name elementów, o Quantity przynajmniej 500
    var items = (from i in list
                where i.Quantity >= 500
                select new { IdOfElement = i.Id, NameOfElement = i.Name }).ToList();  
                //items jest typu List<'a>,
                    //gdzie 'a jest typem anonimowym składajacym się z dwoch elementow: int IdOfElement i string NameOfElement
}
```

### Zapis łańcuchowy (metodowy)
```csharp =
public void Method()
{
    List<Item> list = Seed();
    //wyszukiwanie Id elementów, o Quantity przynajmniej 500
    var items = list.Where(i => i.Quantity >= 500).Select(i => i.Id).ToList();  //items jest typu List<int>
    
    //wyszukiwanie Id i Name elementów, o Quantity przynajmniej 500
    var items = list.Where(i => i.Quantity >= 500).Select(i => new { IdOfElement = i.Id, NameOfElement = i.Name }).ToList();  
        //items jest typu List<'a>,
            //gdzie 'a jest typem anonimowym składajacym się z dwoch elementow: int IdOfElement i  string NameOfElement
}
```

## Wyszukiwanie jednego elementu
Do tej pory za każdym razem zwracaliśmy kolekcję elementów. Może nas jednak interesować otrzymanie tylko jednego elementu. W tym celu stworzono metodę `FirstOrDefault` zwracającą pierwszy element spełniający dany warunek, bądź wartość domyślną, jeżeli nie znaleziono żadnego elementu, który by go spełniał. Wartością domyślną dla typów referencyjnych jest `null`, a np. dla typu `int` jest to `0`.
### Zapis łańcuchowy (metodowy)
```csharp =
public void Method()
{
    List<Item> list = Seed();
    //wyszukiwanie elementu, o Id rownym 1 (pierwszego takiego elementu w kolekcji, czyli new Item(1, "TShirt", 3){Quantity = 200})
    var selectedItem = list.FirstOrDefault(i => i.Id == 1); //selectedItem jest typu Item
}
```

### Zapis "SQL"
Powyższa metoda niema swojego odpowiednika w składni _query_. Jeżeli chcemy możemy więc jedynie połączyć obie formy zapisu.
```csharp =
public void Method()
{
    List<Item> list = Seed();
    //wyszukiwanie elementu, o Id rownym 1 (pierwszego takiego elementu w kolekcji, czyli new Item(1, "TShirt", 3){Quantity = 200})
    var selectedItem = (from i in list select i).FirstOrDefault(i => i.Id == 1);
    //lub
    selectedItem = (from i in list where i.Id == 1 select i).FirstOrDefault();
        //selectedItem jest typu Item
}
```

# [LEKCJA 9 – Typy generyczne](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-9-typy-generyczne/)
**Typy generyczne** - są to typy mogące obsługiwać różne typy danych. Stosuje się je, gdy na różnych typach danych chcemy wykonywać takie same operacje. Przykładem typu generycznego jest poznana już przez nas lista (`List<T>`) służąca do tworzenia i obsługi kolekcji obiektów danego typu (`T` - zamiast T wstawiamy dowolny typ danych), określonego w definicji. Wszystkie predefiniowane typy generyczne należą do namespace'u `System.Collection.Generic`. Typy generyczne często stosuje się w aplikacjach webowych do obsługi operacji CRUD-owych na klasach.

## Operacje CRUD
Czyli operacje Create (tworzenia) - Read (odczytywania) - Update (aktualizacji) - Delete (usuwania). Są to najczęściej operacje związane z obsługą bazy danych. Właściwie na każdej tabeli z bazy danych wykonuje się takie operacje. Aby nie implementować tych metod dla każdej klasy osobno, często stosuje się właśnie klasy generyczne, które będą wykonywały te operacje podczas pracy programu. Będziemy musieli jedynie podać typ (klasę) dla jakiego te operacje mają być wykonane.

## Definicja typu generycznego
Typ generyczny definiujemy prawie tak samo jak każdą inną klasę, dodając jedynie, za nazwą klasy, nawias ostry z literką `T` w środku.

```csharp =
ModyfikatorDostepu class NazwaKlasyGenerycznej<T>
{
    //cialo klasy
}
```

### Przykład
Stwórzmy przykładowy typ generyczny dla operacji CRUD.

```csharp =
public class GenericService<T>
{
    public List<T> Items { get; set: }

    public GenericService()
    {
        Items = new List<T>();
    }

    public List<T> GetAll()
    {
        return Items;
    }

    public void Add(T obj)
    {
        Items.Add(obj);
    }

    public void Remove(T obj)
    {
        Items.Remove(obj);
    }
}
```

## Korzystanie z typów generycznych
Z typów generycznych korzystamy prawie tak samo jak z każdego innego typu, dodając jedynie po nazwie typu generycznego w nawiasach ostrych nazwę typu który chcemy, aby nasz typ generyczny obsłużył.

```csharp =
NazwaKlasyGenerycznej<typ> nazwaZmiennej = new NazwaKlasyGenerycznej<typ>();
```

### Przykład
Załóżmy np. że mamy w programie zdefiniowany jakiś typ `Item`.

```csharp =
GenericService<Item> genericItemService = new GenericService<Item>();
Item item = new Item();
genericItemService.Add(item);
var list = genericItemService.GetAll();
Console.WriteLine(list.Count);  //  1
genericItemService.Remove(item);
list = genericItemService.GetAll();
Console.WriteLine(list.Count);  //  0
```

Moglibyśmy w ten sam sposób skorzystać z naszej klasy generycznej dla zupełnie innej klasy.

## Przykłady typów generycznych C#
Język C# ma już zdefiniowane wiele typów generycznych, z których możemy korzystać. Są to np. typy:
* `List<T>` - listy,
* `Dictionary<TKey, TValue>` - słowniki,
* `SortedList<TKey, TValue>` - uporządkowane listy,
* `SortedDictionary<TKey, TValue>` - uporządkowane słowniki,
* `LinkedList<T>` - podwójne listy połączone

Więcej informacji o tych i innych typach generycznych można znaleźć w [dokumentacji](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic?view=net-7.0).

## Ograniczanie klas generycznych
Definiując typ generyczny możemy ograniczyć obsługi jakich typów będzie on dotyczył. Możemy np. stworzyć klasę generyczną tylko dla typów referencyjnych, albo tylko dla typów implementujących jakiś interfejs itd. Służy do tego słowo kluczowe `where`. W definicji klasy generycznej, za nawiasem ostrym z literką T (`<T>`) piszemy `where T : warunek`.
### Przykłady
Typ generyczny obsługujący tylko klasy (gdy spróbujemy użyć go np. dla typu `int`, otrzymamy błąd kompilacji)

```csharp =
public class GenericService<T> where T : class
{
    //cialo klasy
}
```

albo tylko klasy implementujące interfejs `IItem` (gdy spróbujemy użyć go dla typu, który nie implementuje tego interfejsu otrzymamy błąd kompilacji)

```csharp =
public class GenericService<T> where T : IItem
{
    //cialo klasy
}
```

itd.

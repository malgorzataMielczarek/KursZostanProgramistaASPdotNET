# [LEKCJA 3 – IQueryable i IEnumerable](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-5-praca-z-danymi/lekcja-3-iqueryable-i-ienumerable/)
Są to główne interfejsy kolekcji używane do pracy z LINQ (więcej o LINQ w kolejnej lekcji). Są one podstawą do pracy z danymi. Najczęściej stosuje się je w stosunku do danych pobieranych z bazą danych. Oba interfejsy są do siebie bardzo podobne, a główna różnica między nimi, leży właśnie we współpracy z LINQ.

## Przykład
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

## Porównanie interfejsów `IQueryable` i `IEnumerable`
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

| Cecha | `IQueryable` | `IEnumerable` |
| ---: | :---: | :---: |
| Implementuje | `IEnumerable` | tylko jedną metodę `IEnumerator<T> GetEnumerator()`, która pozwala iterować po kolekcji |
| Pobieranie i filtrowanie kolekcji przy pomocy LINQ | pobiera przefiltrowaną kolekcję <br/> (filtrowanie po stronie bazy danych) | pobiera całą kolekcję, a następnie ją filtruje <br/> (filtrowanie po stronie aplikacji) |
| Zastosowanie | pobieranie przefiltrowanych danych z bazy danych | manipulacja danymi znajdującymi się już w pamięci programu lub pobieranie danych w całości (np. z pliku - filtracja możliwa dopiero w aplikacji, po pobraniu wszystkich danych) |

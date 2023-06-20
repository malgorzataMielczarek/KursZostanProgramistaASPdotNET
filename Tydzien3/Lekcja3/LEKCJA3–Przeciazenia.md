# [LEKCJA 3 – Przeciążenia](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-3-przeciazenia/)
**Przeciążanie metod** jest to stworzenie drugiej (kolejnej) metody o takiej samej nazwie, ale o innej liczbie i/lub typie przekazywanych parametrów. Standardowo metoda i jej przeciążenia zwracają ten sam typ (chociaż możliwe jest zwracanie różnych typów).<br/>
Standardowo przeciążenia różnią się więc między sobą tylko argumentami i ciałem metody.

## Przykład zastosowania
Załóżmy np., że mamy klasę `Item`:

```csharp =
public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Type { get; set; }
}
```

Mamy również klasę serwisową `ItemService`, która będzie przechowywać listę obiektów `Item`. W tej klasie musimy więc mieć metodę do dodawania nowych obiektów do listy. Raz jednak chcielibyśmy dodać "pusty" obiekt, nie znając jeszcze jego nazwy ani typu (to uzupełnimy gdzieś dalej w programie). Innym razem chcielibyśmy dodać do listy cały znany nam już obiekt. Jeszcze innym razem wiemy jaką nazwę i typ ma mieć nowy obiekt i chcemy, aby jego id było o jeden większe niż ostatniego itema na liście. Do tego przydadzą nam się właśnie metody przeciążone. Spróbujmy to więc zapisać:

```csharp =
public class ItemService
{
    public List<Item> Items { get; set; }
    public ItemService()
    {
        Items = new List<Item>();
    }
    public int AddItem()
    {
        int id = Items.Last().Id + 1;
        Item item = new Item() { Id = id };
        Items.Add(item);
        return item.Id;
    }
    public int AddItem(Item item)
    {
        Items.Add(item);
        return item.Id;
    }
    public int AddItem(string name, string type)
    {
        int id = Items.Last().Id + 1;
        Item item = new Item() { Id = id, Name = name, Type = type };
        Items.Add(item);
        return id;
    }
}
```

Jak widać stworzyliśmy trzy przeciążone metody `AddItem`. Każda z nich przyjmuje inne argumenty. Dzięki temu nasza klasa jest bardziej elastyczna i uwzględnia więcej przypadków zastosowania. Wszystkie zwracają jednak ten sam typ (w każdym przypadku zwracamy Id nowo dodanego produktu). Taka konsekwentność zwiększa czytelność naszego kodu i sprawia, że wiemy czego się spodziewać, niezależnie którą z metod przeciążonych użyjemy.

Inne przykłady metod przeciążonych można znaleźć w poprzedniej lekcji, gdzie m.in. przeciążaliśmy konstruktory.

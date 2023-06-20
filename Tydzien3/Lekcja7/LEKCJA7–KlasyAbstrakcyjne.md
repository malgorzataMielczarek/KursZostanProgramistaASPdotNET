# [LEKCJA 7 – Klasy abstrakcyjne](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-7-klasy-abstrakcyjne/)
**Klasa abstrakcyjna** jest to klasa bazowa, swoisty szablon dla innych klas (które będą po niej dziedziczyć). Nie można natomiast utworzyć obiektu tej klasy (klasy abstrakcyjnej). Tworzy się ją dodając w deklaracji klasy słowo kluczowe `abstract`, przed słowem kluczowym `class`. Mimo, że nie możemy utworzyć obiektu klasy abstrakcyjnej, to możemy dokonać rzutowania na ten typ. Przypomnijmy sobie nasze klasy z lekcji dotyczącej polimorfizmu (tydzień 3, lekcja 5).

```csharp =
public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }
    
    public virtual void ShowItem()
    {
        Console.WriteLine($"Item: {Id}. {Name}");
    }
}
```
```csharp =
public class GroceryItem : Item
{
    public DateTime ExpiryDate { get; set; }

    public override void ShowItem()
    {
        Console.WriteLine($"Grocery: {Id}. {Name} {ExpiryDate.ToString("d")}");
    }
}
```
```csharp =
public class ClothingItem : Item
{
    public string Size { get; set; }

    public override void ShowItem()
    {
        Console.WriteLine($"Clothe: {Id}. {Name} {Size}");
    }
}
```
```csharp =
public class ElectronicItem : Item
{
    public string Category { get; set; }
    public string EnergyClass { get; set; }
    public new void ShowItem()
    {
        Console.WriteLine($"{Id}. {Name}\t Category: {Category}\t Energy class: {EnergyClass}");
    }
}
```

Pamiętajmy, że każdą klasę powinniśmy umieścić w osobnym pliku.

Przypomnijmy sobie również przykład ich użycia.

```csharp =
List<Item> items = new List<Item>();
items.Add(new Item() { Id = 1, Name = "Bowl"});
items.Add(new GroceryItem() { Id = 2, Name = "Apple", ExpiryDate = DateTime.Now.AddDays(7) });
items.Add(new ClothingItem() { Id = 3, Name = "T-shirt", Size = "M" });
List<ElectronicItem> electronics = new List<ElectronicItem>();
electronics.Add(new ElectronicItem() { Id = 4, Name = "Smart TV", Category = "TV", EnergyClass = "A" });
electronics.Add(new ElectronicItem() { Id = 5, Name = "Headphones", Category = "Audio", EnergyClass = "A++" });
electronics.Add(new ElectronicItem() { Id = 6, Name = "Speaker", Category = "Audio", EnergyClass = "A+" });

items.AddRange(electronics);

foreach(var item in items)
{
    item.ShowItem();
}

Console.WriteLine("\nElectronics:");
foreach(var el in electronics)
{
    el.ShowItem();
}

//  Output:
//  Item: 1. Bowl
//  Grocery: 2. Apple 2022-12-22
//  Clothe: 3. T-shirt M
//  Item: 4. Smart TV
//  Item: 5. Headphones
//  Item: 6. Speaker
//  
//  Electronics:
//  4. Smart TV      Category: TV    Energy class: A
//  5. Headphones    Category: Audio         Energy class: A++
//  6. Speaker       Category: Audio         Energy class: A+
```

## Klasa abstrakcyjna
Jeżeli teraz nie chcielibyśmy aby w naszym programie można było utworzyć nieskategoryzowany item (obiekt klasy `Item`), to możemy przerobić tą klasę, na klasę abstrakcyjną:

```csharp =
public abstract class Item
{
    public int Id { get; set; }
    public string Name { get; set; }
    
    public virtual void ShowItem()
    {
        Console.WriteLine($"Item: {Id}. {Name}");
    }
}
```

Teraz nie możemy już utworzyć obiektu tej klasy, więc linijka `items.Add(new Item() { Id = 1, Name = "Bowl"});` będzie nieprawidłowa (wystąpi błąd kompilacji). Nadal możemy jednak wykonywać rzutowania na klasę `Item`, czyli możemy również nadal utworzyć listę wszystkich produktów (listę typu `Item` zawierającą obiekty innych typów, klas dziedziczących po klasie `Item`). Możemy więc usunąć błędną linijkę kodu i ponownie uruchomić program, otrzymując taki sam wynik (z wyłączeniem linijki tekstu generowanej przez usunięty kod).

Klasa abstrakcyjna jest właściwie podobna do interfejsu (o którym w następnej lekcji - tydzień 3., lekcja 8.), jednak jest to nadal klasa. Pozostaje więc w mocy zasada, że klasa może dziedziczyć bezpośrednio (nie kaskadowo) tylko po jednej klasie.

## Metoda abstrakcyjna
Wewnątrz klasy abstrakcyjnej mogą występować metody abstrakcyjne. Są to jedynie szablony wskazujące, że wszystkie klasy dziedziczące po tej klasie muszą takie metody implementować. Metodę abstrakcyjną, w klasie bazowej, tworzy się dodając słowo kluczowe `abstract` przed typem, zwracanym przez metodę. W klasie bazowej występuje jedynie deklaracja metody, bez implementacji (bez ciała metody). W klasach pochodnych (dziedziczących po klasie abstrakcyjnej) musimy zaimplementować tą metodę, używając poznanego już słowa kluczowego `override`. Dodajmy do klasy `Item` metodę abstrakcyjną `ShowCategory()`, która wyświetli nam kategorię do jakiej należy dany produkt (obiekt), oraz zwróci nam tą wartość w formie stringa. Przy okazji zmian wprowadzanych w kolejnych klasach zmodyfikujmy również nieco metody `ShowItem`, usuwając z nich nazwy kategorii.

```csharp =
public abstract class Item
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual void ShowItem()
    {
        Console.WriteLine($"{Id}. {Name}");
    }
    public abstract string ShowCategory();
}
```

Zaimplementujmy metodę abstrakcyjną w klasach pochodnych (i zmodyfikujmy metodę `ShowItem`).

```csharp =
public class GroceryItem : Item
{
    public DateTime ExpiryDate { get; set; }

    public override void ShowItem()
    {
        Console.WriteLine($"{Id}. {Name} {ExpiryDate.ToString("d")}");
    }
    public override string ShowCategory()
    {
        string category = "Grocery";
        Console.Write(category);
        return category;
    }
}
```
```csharp =
public class ClothingItem : Item
{
    public string Size { get; set; }

    public override void ShowItem()
    {
        Console.WriteLine($"{Id}. {Name} {Size}");
    }
    public override string ShowCategory()
    {
        string category = "Clothing";
        Console.Write(category);
        return category;
    }
}
```
```csharp =
public class ElectronicItem : Item
{
    public string Category { get; set; }
    public string EnergyClass { get; set; }
    public new void ShowItem()
    {
        Console.WriteLine($"{Id}. {Name}\t Category: {Category}\t Energy class: {EnergyClass}");
    }
    public override string ShowCategory()
    {
        string category = "Electronic";
        Console.Write(category);
        return category;
    }
}
```

Sprawdźmy jak działają zaimplementowane metody. W przykładzie użycia klas zmieńmy pierwszy element na liście produktów (ten który był typu `Item`). Utwórzmy zamiast niego obiekt `GroceryItem`. Stwórzmy słownik (`Dictionary`), w którym zapiszemy wszystkie kategorie produktów znajdujących się na naszej liście, wraz z ilością produktów danej kategorii. W pętli, w której wypisywaliśmy wszystkie elementy listy produktów dopiszmy wywołanie naszej metody abstrakcyjnej i jeżeli kategoria produktu znajduje się już w słowniku to zwiększmy ilość produktów o jeden, a jeżeli nie, to dopiszmy kategorię do słownika z ilością `1`. Na koniec wypiszmy kategorie wszystkich produktów, wraz z ilościami (wypiszmy nasz słownik).

```csharp =
List<Item> items = new List<Item>();
items.Add(new GroceryItem() { Id = 1, Name = "Orange", ExpiryDate = DateTime.Now.AddDays(5) });
items.Add(new GroceryItem() { Id = 2, Name = "Apple", ExpiryDate = DateTime.Now.AddDays(7) });
items.Add(new ClothingItem() { Id = 3, Name = "T-shirt", Size = "M" });

List<ElectronicItem> electronics = new List<ElectronicItem>();
electronics.Add(new ElectronicItem() { Id = 4, Name = "Smart TV", Category = "TV", EnergyClass = "A" });
electronics.Add(new ElectronicItem() { Id = 5, Name = "Headphones", Category = "Audio", EnergyClass = "A++" });
electronics.Add(new ElectronicItem() { Id = 6, Name = "Speaker", Category = "Audio", EnergyClass = "A+" });

items.AddRange(electronics);
Dictionary<string, int> categories = new Dictionary<string, int>();
foreach(var item in items)
{
    string category = item.ShowCategory();
    if(categories.ContainsKey(category))
        categories[category]++;
    else
        categories.Add(category, 1);
    Console.Write("\t");
    item.ShowItem();

}

Console.WriteLine("\nElectronics:");
foreach(var el in electronics)
{
    el.ShowItem();
}

Console.WriteLine();
foreach(var category in categories)
{
    Console.WriteLine($"{category.Key}:\t{category.Value}");
}

//  Output:
//  Grocery 1. Orange 2022-12-26
//  Grocery 2. Apple 2022-12-28
//  Clothing        3. T-shirt M
//  Electronic      4. Smart TV
//  Electronic      5. Headphones
//  Electronic      6. Speaker
//  
//  Electronics:
//  4. Smart TV      Category: TV    Energy class: A
//  5. Headphones    Category: Audio         Energy class: A++
//  6. Speaker       Category: Audio         Energy class: A+
//  
//  Grocery:        2
//  Clothing:       1
//  Electronic:     3
```

## Klasy "zapieczętowane"
Są to ostateczne (finalne) klasy, takie, po których nie można dziedziczyć. Tworzy się je dodając w definicji słowo kluczowe `sealed` (przed słowem kluczowym `class`).

```csharp =
ModyfikatorDostepu sealed class NazwaKlasy
{
    //cialo klasy
}
```

Stosuje się je raczej rzadko, gdy chcemy uniemożliwić tworzenie klas pochodnych od danej klasy. Próba utworzenia klasy dziedziczącej po klasie sealed będzie skutkować błędem kompilacji. Ponieważ nie można po niej dziedziczyć, nie może ona posiadać metod abstrakcyjnych, ani wirtualnych. Nie możemy jej już zmieniać (nadpisywać), ani rozszerzać.

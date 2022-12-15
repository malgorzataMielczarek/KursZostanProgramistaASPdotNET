# [LEKCJA 5 – Polimorfizm](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-5-dziedziczenie/)
**Polimorfizm** jest to drugi paradygmat (filar) programowania obiektowego. Z języka greckiego _polys_ - wiele, _morfe_ - kształt, czyli wielopostaciowość (coś o wielu kształtach), oznacza, że dany obiekt może występować w różnych kształtach. W programowaniu oznacza to, że wszystkie elementy mogą mieć jeden interfejs i wiele funkcji do użycia. Polimorfizm jest ściśle związany z poznanym w poprzedniej lekcji dziedziczeniem. Mówiliśmy np., że po klasie bazowej Zwierzę mogą dziedziczyć m.in. klasy Ptak, Ssak, Gad. Czyli obiekt klasy Zwierzę może mieć różne postaci. Może być ptakiem (obiektem klasy Ptak), albo ssakiem (obiektem klasy Ssak), albo gadem (obiektem klasy Gad) itd. Może też być bliżej nieokreślonym Zwierzęciem (czyli po prostu obiektem klasy Zwierzę). Wyróżniamy dwa rodzaje polimorfizmu: statyczny i dynamiczny.

## Polimorfizm statyczny
Ten rodzaj polimorfizmu poznaliśmy już trochę w poprzednich lekcjach. Jest to po prostu przeciążanie konstruktorów i innych metod danej klasy, lub klasy bazowej. Polimorfizm statyczny oznacza, że coś może występować w wielu postaciach (np. metoda może być przeładowana i przyjmować różne zestawy argumentów) i już w momencie kompilacji wiemy, w której postaci wystąpi w danym fragmencie kodu (np. wywołując metodę jawnie wybieramy przeciążenie, poprzez przekazanie odpowiednich argumentów).

## Polimorfizm dynamiczny
Oznacza, że postać w której będzie występować dany element znamy dopiero w momencie wykonywania. Załóżmy np., że mamy klasę `Item`, która będzie reprezentować jakiś produkt w sklepie:

```csharp =
public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }
    
    public void ShowItem()
    {
        Console.WriteLine($"Item: {Id}. {Name}");
    }
}
```

W sklepie mogą występować różne produkty. Mamy więc np. klasy `GroceryItem`, `ClothingItem`, które będą dziedziczyć po klasie `Item`. 

```csharp =
public class GroceryItem : Item
{
    public DateTime ExpiryDate { get; set; }

    public void ShowItem()
    {
        Console.WriteLine($"Grocery: {Id}. {Name} {ExpiryDate.ToString("d")}");
    }
}
```

```csharp =
public class ClothingItem : Item
{
    public string Size { get; set; }

    public void ShowItem()
    {
        Console.WriteLine($"Clothe: {Id}. {Name} {Size}");
    }
}
```

Chcielibyśmy teraz utworzyć listę wszystkich produktów. Znajdą się na niej zarówno produkty typu `Item`, jak i produkty spożywcze (obiekty klasy `GroceryItem`) oraz ubrania (obiekty klasy `ClothingItem`).

```csharp =
List<Item> items = new List<Item>();
items.Add(new Item() { Id = 1, Name = "Bowl"});
items.Add(new GroceryItem() { Id = 2, Name = "Apple", ExpiryDate = DateTime.Now.AddDays(7) });
items.Add(new ClothingItem() { Id = 3, Name = "T-shirt", Size = "M" });
```

Następnie chcemy wyświetlić całą listę, używając metody `ShowItem`.

```csharp =
foreach(var item in items)
{
    item.ShowItem();
}
```

Jeżeli zrobimy to w obecnej formie otrzymamy:

```csharp =
//  Output:
//  Item: 1. Bowl
//  Item: 2. Apple
//  Item: 3. T-shirt
```

Stanie się tak, gdyż przypisując obiekty do listy dokonaliśmy ich niejawnego rzutowania do `Item`, więc wszystkie skorzystają z metody `ShowItem` z tej właśnie klasy. Chcielibyśmy jednak, aby ubrania wywoływały metodę `ShowItem` z klasy `ClothingItem`, produkty spożywcze z klasy `GroceryItem` itd. Do tego posłuży nam właśnie polimorfizm dynamiczny (dopiero w momencie wywoływania metody `ShowItem` będziemy wiedzieć jakiego typu jest obiekt `item`, na którym ją wywołujemy). Aby go wykorzystać musimy w definicji metod naszych klas dodać odpowiednie słowa kluczowe.

### Słowo kluczowe `virtual`
Najpierw w definicji metody w klasie bazowej dodamy słowo kluczowe `virtual`.

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

Wstawienie tego słowa w definicji metody oznacza, że pozwalam, aby ta metoda została nadpisana przez odpowiadającą jej metodę z klasy po niej dziedziczącej. Czyli jeżeli w klasie która dziedziczy po tej klasie bazowej znajduje się metoda nadpisująca tą metodę, to nawet po zrzutowaniu obiektu klasy pochodnej na obiekt klasy bazowej wywołana zostanie metoda z klasy pochodnej. Metoda z klasy pochodnej ma "pierwszeństwo". Czyli pozwoli nam to osiągnąć to co chcemy. Dzięki temu będziemy mogli wywoływać metodę `ShowItem` z odpowiedniej klasy, przechodząc pętlą po liście obiektów `Item`. Musimy jednak jeszcze zaznaczyć, że chcemy aby nasze metody w klasach pochodnych nadpisywały metodę z klasy bazowej.

### Słowo kluczowe `override`
Dodając do definicji metody słowo kluczowe `override` zaznaczamy, że chcemy, aby ta metoda nadpisywała metodę wirtualną (ze słowem kluczowym `virtual`) z klasy bazowej. Nadpiszmy więc metodę `ShowItem` w naszych klasach pochodnych.

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

Jeżeli po zastosowaniu tych zmian ponownie przeszlibyśmy pętlą po elementach naszej listy, to otrzymalibyśmy teraz:

```csharp =
foreach(var item in items)
{
    item.ShowItem();
}
//  Output:
//  Item: 1. Bowl
//  Grocery: 2. Apple 2022-12-22
//  Clothe: 3. T-shirt M
```

### Słowo kluczowe `new` w definicji metody
Może się jednak zdarzyć, że w którejś z klas pochodnych nie chcemy, aby utworzona przez nas metoda nadpisywała metodę wirtualną z klasy bazowej. Chcemy, aby były one od siebie niezależne. Np. chcielibyśmy, aby podczas wypisywania listy wszystkich przedmiotów `List<Item>` wywoływana była metoda z klasy `Item` podająca mniej szczegółów, natomiast podczas listowania produktów tego typu użyta była metoda z klasy pochodnej, opisująca nasz produkt bardziej szczegółowo. Do tego posłuży nam nowe zastosowanie znanego nam już słowa kluczowego `new`. Utwórzmy więc nową klasę `EletronicItem`, dziedziczącą po naszej klasie `Item`.

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

Utwórzmy teraz listę `List<ElectronicItem>` kilku produktów tego typu. Następnie dodajmy tę listę do naszej listy wszystkich produktów (`items`). Na koniec dla każdej z list wywołajmy w pętli metodę `ShowItem`, dla każdego elementu.

```csharp =
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

Jak widać otrzymaliśmy dokładnie to, czego się spodziewaliśmy.

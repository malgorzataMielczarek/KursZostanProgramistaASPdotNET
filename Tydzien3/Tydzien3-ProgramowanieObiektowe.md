# TYDZIEŃ 3 – Programowanie obiektowe
## Spis treści
### [LEKCJA 1 – Powitanie](#lekcja-1--powitanie-1)
### [LEKCJA 2 – Konstruktory](#lekcja-2--konstruktory-1)
### [LEKCJA 3 – Przeciążenia](#lekcja-3--przeciążenia-1)
### [LEKCJA 4 – Dziedziczenie](#lekcja-4--dziedziczenie-1)
### [LEKCJA 5 – Polimorfizm](#lekcja-5--polimorfizm-1)
### [LEKCJA 6 – Hermetyzacja](#lekcja-6--hermetyzacja-1)
### [LEKCJA 7 – Klasy abstrakcyjne](#lekcja-7--klasy-abstrakcyjne-1)
### [LEKCJA 8 – Interfejsy](#lekcja-8--interfejsy-1)
### [LEKCJA 9 – Typy generyczne](#lekcja-9--typy-generyczne-1)
### [LEKCJA 10 – Refaktoryzacja](#lekcja-10--refaktoryzacja-1)
### [LEKCJA 11 – Błędy początkujących](#lekcja-11--błędy-początkujących-1)
### [LEKCJA 12 – Praca domowa](#lekcja-12--praca-domowa-1)

## [LEKCJA 1 – Powitanie](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-1-powitanie/)
Przechodzimy do poznawania programowania obiektowego. W tym tygodniu poznamy co to jest dziedziczenie, hermetyzacja, polimorfizm. Poznamy również takie struktury jak interfejsy, czy klasy abstrakcyjne. Pod koniec tego tygodnia dokonamy refaktoryzacji naszej aplikacji, z uwzględnieniem wszystkich zasad jakie poznamy w tym tygodniu.

## [LEKCJA 2 – Konstruktory](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-2-konstruktory/)
**Konstruktor** jest to specjalny rodzaj metody, wywoływanej podczas inicjalizacji obiektu danej klasy.

### Tworzenie konstruktora
Konstruktor tworzymy prawie tak samo jak każdą inną metodę. Jedynymi zastrzeżeniami jest fakt, że nie podajemy w nim zwracanego typu, a jego nazwa musi być identyczna z nazwą klasy. Czyli np.:

```csharp =
public class MyClass
{
    public MyClass()
    {
        //ciało konstruktora
    }
}
```

### Wywoływanie konstruktora
Standardowo konstruktor jest wywoływany w momencie tworzenia nowego obiektu danej klasy, czyli gdy piszemy np.:

```csharp =
MyClass newObject = new MyClass();
```

### Konstruktor domyślny
Jeżeli w naszej klasie nie zadeklarujemy konstruktora to zostaje wywoływany tzw. domyślny bezparametrowy konstruktor. Jego zadaniem jest zainicjalizowanie wartościami domyślnymi wszystkich pól i właściwości klasy. Czyli np. do pól i właściwości typu `int` podstawiane jest `0`, do typów referencyjnych `null` itd.

### Konstruktor z parametrami
Tworzony przez nas konstruktor może mieć również parametry. Możemy wówczas przypisać do wszystkich lub wybranych właściwości i pól naszej klasy konkretne wartości w momencie tworzenia obiektów. Np.:

```csharp =
public class User
{
    public int Id { get; set; }
    public string Login { get; set; }
    private string password;

    public User(string login, string pwd)
    {
        Login = login;
        password = pwd;
    }
}
```

Wywołuje się go tworząc obiekt w następujący sposób:

```csharp =
User newUser = new User("admin", "dfghj");
```

Konstruktory w takiej formie tworzy się, gdy klasa zawiera jakieś właściwości/pola których wypełnienie jest niezbędne do poprawnego działania danej klasy. Dzięki temu mamy pewność, że żaden obiekt tej klasy nie zostanie utworzony bez nadania wartości tym właściwościom/polom.

Utworzenie konstruktora z parametrem spowoduje, że nie powstanie konstruktor domyślny. Próba stworzenia obiektu tej klasy bez podania odpowiednich parametrów spowoduje błąd kompilacji. Oczywiście dalej możemy stworzyć konstruktor bezparametrowy. Musimy go jednak zadeklarować w sposób jawny.

### Konstruktor z listą inicjalizacyjną
Gdybyśmy chcieli zapewnić nadanie wartości określonym parametrom, ale jednocześnie dać możliwość tworzenia obiektów bez podawania parametrów możemy wówczas skorzystać z konstruktora z listą inicjalizacyjną. Możemy np. Do powyższej klasy `User` dopisać drugi konstruktor:

```csharp
public User() : this("guest", "1234"){}
```

Wówczas możemy stworzyć obiekt tej klasy nie podając żadnych parametrów:

```csharp =
User guest = new User();
```

W ten sposób, wywołamy konstruktor bezparametrowy, który z kolei wywoła konstruktor z parametrami, przypisując argumentom odpowiednie wartości podane w liście inicjalizacyjnej (`: this("guest", "1234")`). Czyli pod parametr `login` podstawi wartość `"guest"`, a pod parametr `pwd` wartość `"1234"`.

Stosowanie konstruktorów z listą inicjalizacyjną jest dobrą metodą, jeżeli mamy możliwość nadania naszym wymaganym do wypełnienia właściwością wartości domyślnych.

### Tworzenie wielu konstruktorów
Możemy stworzyć dowolną ilość konstruktorów, jednak muszą się one różnić ilością i/lub typem danych przekazywanych przez parametry. Jest to tzw. przeciążanie metod, o którym więcej nauczymy się w kolejnej lekcji (lekcja 3).

### Kiedy tworzyć własne konstruktory
W wielu przypadkach nie tworzy się konstruktorów, gdyż konstruktor domyślny jest dla nas wystarczający. W klasach z samymi właściwościami, powiązanych w jakiś sposób z bazą danych, tabelami w bazie danych, czy prezentujących jakiś rodzaj modeli na ogół nie mamy jawnie zdefiniowanych konstruktorów. W takich klasach na ogół niema potrzeby nadawania właściwością konkretnych wartości podczas tworzenia obiektów tej klasy. Tworzenie konstruktorów z przekazywanymi wszystkimi parametrami nie skróci natomiast (ani nie zwiększy czytelności) zapisu nadawania wartości wszystkim właściwością publicznym po inicjalizacji. Czyli np. dla klasy:

```csharp =
public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Type { get; set; }
}
```

zapisu:

```csharp =
Item item = new Item() { Id = 1, Name = "bear", Type = "toy" };
```

**Konstruktory tworzymy więc najczęściej jeżeli mamy tylko część właściwości jako dane wymagane.**

## [LEKCJA 3 – Przeciążenia](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-3-przeciazenia/)
**Przeciążanie metod** jest to stworzenie drugiej (kolejnej) metody o takiej samej nazwie, ale o innej liczbie i/lub typie przekazywanych parametrów. Standardowo metoda i jej przeciążenia zwracają ten sam typ (chociaż możliwe jest zwracanie różnych typów).<br/>
Standardowo przeciążenia różnią się więc między sobą tylko argumentami i ciałem metody.

### Przykład zastosowania
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

## [LEKCJA 4 – Dziedziczenie](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-4-polimorfizm/)
**Dziedziczenie** jest to jeden z trzech paradygmatów (filarów) programowania obiektowego. Pozwala ona na przejście od ogółu do szczegółu. Tzn. mamy jakąś ogólną klasę (np. Ssak) i klasy szczegółowe (np. Pies, Kot, Małpa, Człowiek itp.), które będą w sobie zawierać wszystkie cechy klasy ogólnej (pola, właściwości, metody). Dodatkowo każda z tych bardziej szczegółowych klas będzie posiadała własne, charakterystyczne tylko dla siebie cechy (pola, właściwości, metody). Oczywiście dziedziczenie możemy realizować wielopoziomowo. Możemy np. mieć klasę Zwierzę, po której będą dziedziczyć klasy Ssak, Ptak, Gad, Płaz itd. Później np. po klasie Ssak będą dziedziczyć klasy Naczelny, Gryzoń itd., a po klasie Gryzoń np. Wiewiórka, Szczur itd. Wówczas klasa Szczur będzie posiadać wszystkie składniki klas Zwierzę, Ssak, Gryzoń i Szczur. Dziedziczenie może odbywać się jednak tylko po jednej klasie (dana bardziej szczegółowa klasa może mieć tylko jedną klasę bazową). Czyli np. nasza klasa Szczur, ponieważ dziedziczy już po klasie Gryzoń, nie mogłaby jeszcze dodatkowo dziedziczyć po klasie ZwierzątkoDomowe. Klasa może natomiast implementować kilka interfejsów, ale o tym w dalszych lekcjach tego tygodnia. Pokażmy więc jak wygląda dziedziczenie na przykładzie.

### Przykład
Załóżmy, że mamy np. klasę `Rectangle` (prostokąt). Każdy prostokąt ma boki (właściwości `Width` i `Height`). Konstruktor tej klasy będzie więc posiadał dwa argumenty (`width` i `height`). Napiszemy również metodę liczącą jego pole powierzchni (`CountArea()`) i dwie metody do narysowania prostokąta w konsoli przy pomocy znaku `'*'` (wypełnionego - `Draw()` i samych krawędzi - `DrawEdges()`). Będzie to nasza klasa bazowa.

```csharp =
 public class Rectangle
{
    public int Width { get; set; }
    public int Height { get; set; }
    public Rectangle(int width, int height)
    {
        Width = width;
        Height = height;
    }
    public int CountArea()
    {
        return Width * Height;
    }
    public void Draw()
    {
        if (Width < 1 || Height < 1) return;
        Console.WriteLine();
        for(int h = 0; h < Height; h++)
        {
            for (int w = 0; w < Width; w++)
            {
                Console.Write("* ");
            }
            Console.WriteLine();
        }
    }
    public void DrawEdges()
    {
        if (Width < 1 || Height < 1) return;
        Console.WriteLine();
        for (int h = 0; h < Height; h++)
        {
            if (Width >= 2)
                Console.Write("* ");
            for (int w = 0; w < Width - 2; w++)
            {
                if (h == 0 || h == Height - 1)
                    Console.Write("* ");
                else
                    Console.Write("  ");
            }
            Console.WriteLine("*");
        }
    }
}
```

Teraz stwórzmy drugą klasę `Square` (kwadrat). Jak wiemy każdy kwadrat jest prostokątem, więc klasa ta będzie dziedziczyła po klasie `Rectangle`.

```csharp =
public class Square : Rectangle
{
    public int Edge { get => Width; set { Width = value; Height = value; } }
    public Square(int edge) : base(edge, edge) { }
    public void Draw()
    {
        Console.WriteLine($"\nThis is square of area {CountArea()}");
        base.Draw();
    }
    public void DrawEdges()
    {
        Console.WriteLine($"\nThis is square of area {CountArea()}");
        base.DrawEdges();
    }
}
```

Dziedziczenie zaznaczamy pisząc po nazwie klasy dwukropek i nazwę klasy po której chcemy dziedziczyć. Jak już wspomnieliśmy możemy dziedziczyć tylko po jednej klasie. 

#### Konstruktor
Ponieważ klasa bazowa (`Rectangle`) niema zdefiniowanego konstruktora domyślnego (bezargumentowego), a jedynie konstruktor przyjmujący dwa argumenty, więc w klasie `Square` również musimy zdefiniować konstruktor. Nie musi on przyjmować takich samych argumentów jak konstruktor klasy `Rectangle`, musi jednak go wywoływać. Aby to zrobić po definicji konstruktora klasy `Square` piszemy dwukropek, słowo kluczowe `base` i w nawiasie odpowiednie argumenty, które wyślemy do konstruktora klasy bazowej. Nie musimy więc jeszcze raz przepisywać kodu, który zawarliśmy w konstruktorze klasy bazowej. Jeżeli w klasie `Square` mielibyśmy jakieś dodatkowe pola/właściwości, które chcielibyśmy zainicjalizować, albo jakieś inne czynności jakie chcielibyśmy wykonać podczas tworzenia obiektu tego typu, to możemy dopisać odpowiedni kod w ciele konstruktora. Podczas tworzenia obiektu najpierw zostanie wywołany konstruktor klasy bazowej, a następnie konstruktor klasy pochodnej (najpierw konstruktor `Rectangle`, a potem `Square`).

#### Przykrywanie metod
W klasie pochodnej poza dodatkowymi metodami niewystępującymi w klasie bazowej możemy również utworzyć metody o takich samych nazwach jak te występujące w klasie bazowej. Takie metody przykryją te występujące w klasie bazowej (dla obiektów klasy pochodnej będą wywoływane zamiast nich). W implementacji klasy pochodnej możemy jednak odwołać się również do oryginalnych implementacji z klasy bazowej. W tym celu ponownie używamy słowa kluczowego `base` i po kropce podajemy nazwę metody klasy bazowej, którą chcemy wywołać i w nawiasie odpowiednie argumenty.

#### Przykład użycia obiektów
Zobaczmy teraz przykładowy kod wykorzystujący powyższe klasy, aby zobaczyć jak one działają: 

```csharp =
Rectangle rectangle = new Rectangle(4, 5);
Console.WriteLine($"Rectangle of height {rectangle.Height}, width {rectangle.Width} and area {rectangle.CountArea()}:");
rectangle.Draw();
rectangle.DrawEdges();
Square square = new Square(5);
Console.WriteLine($"Square of edge length {square.Edge}, height {square.Height}, width {square.Width} and area {square.CountArea()}:");
square.Draw();
square.DrawEdges();

//  Output:

//  Rectangle of height 5, width 4 and area 20:
//  
//  * * * *
//  * * * *
//  * * * *
//  * * * *
//  * * * *
//  
//  * * * *
//  *     *
//  *     *
//  *     *
//  * * * *
//  Square of edge length 5, height 5, width 5 and area 25:
//  
//  This is square of area 25
//  
//  * * * * *
//  * * * * *
//  * * * * *
//  * * * * *
//  * * * * *
//  
//  This is square of area 25
//  
//  * * * * *
//  *       *
//  *       *
//  *       *
//  * * * * *
```

### Popularna klasa bazowa w aplikacjach webowych
W aplikacjach webowych będziemy tworzyć dużo klas będących bezpośrednim odzwierciedleniem tabel w bazie danych (jedna tabela, jedna klasa). Większość współczesnych baz danych posiada w każdej tabeli informacje o tym, kto (id użytkownika) i kiedy utworzył i zmodyfikował dany rekord (pozycję/rząd w tabeli). Ponieważ takie informacje będę w każdej (lub prawie każdej) tabeli więc, aby nie powtarzać wielokrotnie tego samego kodu (co niepotrzebnie wydłuża kod i zwiększa ryzyko błędu), często tworzy się klasę bazową, która będzie je przechowywać.

```csharp =
public class AuditableModel
{
    public int CreatedById { get; set; }    // kto utworzył rekord
    public DateTime CreatedDateTime { get; set; }   // kiedy rekord został utworzony
    public int? ModifiedById { get; set; }  // jezeli rekord zostal zmodyfikowany, to kto go zmodyfikowal (jezeli nie zostal zmodyfikowany to null, dlatego typ int?, czyli nullowalny int)
    public DateTime? ModifiedDateTime { get; set; }  // jezeli rekord zostal zmodyfikowany, to kiedy (jezeli nie zostal zmodyfikowany to null, dlatego typ DateTime?, czyli nullowalna struktura DateTime)
}
```

Wówczas wszystkie klasy będące odzwierciedleniem tabel bazy danych (oczywiście tych zawierające takie dane) będą dziedziczyć po klasie `AudiatableModel`.

## [LEKCJA 5 – Polimorfizm](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-5-dziedziczenie/)
**Polimorfizm** jest to drugi paradygmat (filar) programowania obiektowego. Z języka greckiego _polys_ - wiele, _morfe_ - kształt, czyli wielopostaciowość (coś o wielu kształtach), oznacza, że dany obiekt może występować w różnych kształtach. W programowaniu oznacza to, że wszystkie elementy mogą mieć jeden interfejs i wiele funkcji do użycia. Polimorfizm jest ściśle związany z poznanym w poprzedniej lekcji dziedziczeniem. Mówiliśmy np., że po klasie bazowej Zwierzę mogą dziedziczyć m.in. klasy Ptak, Ssak, Gad. Czyli obiekt klasy Zwierzę może mieć różne postaci. Może być ptakiem (obiektem klasy Ptak), albo ssakiem (obiektem klasy Ssak), albo gadem (obiektem klasy Gad) itd. Może też być bliżej nieokreślonym Zwierzęciem (czyli po prostu obiektem klasy Zwierzę). Wyróżniamy dwa rodzaje polimorfizmu: statyczny i dynamiczny.

### Polimorfizm statyczny
Ten rodzaj polimorfizmu poznaliśmy już trochę w poprzednich lekcjach. Jest to po prostu przeciążanie konstruktorów i innych metod danej klasy, lub klasy bazowej. Polimorfizm statyczny oznacza, że coś może występować w wielu postaciach (np. metoda może być przeładowana i przyjmować różne zestawy argumentów) i już w momencie kompilacji wiemy, w której postaci wystąpi w danym fragmencie kodu (np. wywołując metodę jawnie wybieramy przeciążenie, poprzez przekazanie odpowiednich argumentów).

### Polimorfizm dynamiczny
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

#### Słowo kluczowe `virtual`
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

#### Słowo kluczowe `override`
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

#### Słowo kluczowe `new` w definicji metody
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

## [LEKCJA 6 – Hermetyzacja](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-6-hermetyzacja/)
**Hermetyzacja**, inaczej **enkapsulacja**, jest trzecim i ostatnim paradygmatem (filarem) programowania obiektowego. Oznacza ona ograniczenie dostępu użytkownika (np. innej klasy) do elementów programu. Tworząc nową klasę musimy świadomie udzielać dostępu tylko do tych jej elementów, do których inne klasy powinny mieć dostęp, i tylko w takim zakresie jaki jest konieczny. To znaczy elementy związane z wewnętrznymi procesami zachodzącymi w klasie ukrywamy całkowicie, a inne możemy chcieć udostępnić tylko do odczytu, albo tylko za pośrednictwem określonych metod. Jest to możliwe przy pomocy modyfikatorów dostępu (`public`, `internal`, `protected`, `private`) oraz innych modyfikatorów (`readonly`, `const` itd.).

Wcześniej omówiliśmy już modyfikatory dostępu (tydzień 2., lekcja 17.) oraz stałe (tydzień 2., lekcja 2.). Powiedzmy więc jeszcze pokrótce co oznacza słowo kluczowe `readonly`.

### Słowo kluczowe `readonly`
Słowem kluczowym `readonly` (z ang. tylko do odczytu), jak sama nazwa wskazuje, oznacza się element których odczyt chcemy umożliwić, ale chcemy je zabezpieczyć przed zmianą ich wartości. Tego słowa kluczowego możemy użyć w czterech sytuacjach:
#### 1. W deklaracji pola klasy

```csharp =
modyfikatorDostepu readonly typ nazwaPola;
```

Oznacza to, że nie można modyfikować wartości przypisanej temu polu. Jedynymi miejscami w których można to zrobić jest deklaracja pola oraz konstruktor klasy. Później możemy już tylko odczytywać jego wartość.

##### Różnice między polem `readonly` a `const`
Stałej (zmienna/pole z modyfikatorem `const`) możemy przypisać wartość wyłącznie w momencie deklaracji. Ponadto wartość ta musi być znana już na etapie kompilacji. Nigdzie indziej w programie nie możemy już jej zmienić.

Polu `readonly` możemy przypisać wartość w momencie deklaracji i/lub w konstruktorze klasy, w momencie tworzenia obiektu. Wartość ta może więc się zmienić (jedną przypiszemy w deklaracji, a inną w konstruktorze). Co więcej, możemy przypisać jej różną wartość, np. w zależności od użytego konstruktora lub jego parametrów. Możemy więc tę wartość poznać dopiero w momencie wykonywania programu. Po utworzeniu obiektu danej klasy, nie możemy już jednak zmieniać wartości jego pola `readonly`. Możemy jedynie odczytywać jego wartość.

#### 2. W deklaracji struktury

```csharp =
modytikatorDostepu readonly struct NazwaStruktury { //cialo struktury }
```

Oznaczenie struktury jako `readonly` oznacza, że wszystkie jej elementy (poza konstruktorem) są również `readonly`.

#### 3. W deklaracji elementów struktury, klasy, np. metod, właściwości

```csharp =
modyfikatorDostepu struct NazwaStruktury
{
    ...
    modytikatorDostepu readonly typ NazwaMetody(/*parametry*/)
    {
        //cialo metody
    }
    modytikatorDostepu readonly typ NazwaWlasciwosci { get; set; }
    ...
}
```

Oznaczenie właściwości jako `readonly` oznacza, że wszystkie jej elementy są również `readonly`. Czyli tak na prawdę będziemy mieć wyłącznie metodę `get`.

Oznaczenie metody jako `readonly` oznacza, że metoda ta, nie będzie modyfikować struktury w której się znajduje. Tzn. może wykonywać różne obliczenia, zwracać jakąś wartość itd., jednak nie zmieni wartość żadnego pola struktury. Wewnątrz ciała metody `readonly` można wywołać jednak metodę bez takiego modyfikatora. Aby zapewnić niezmienność struktury, tworzona jest wówczas kopia struktury i na tej kopii wykonywana jest metoda bez modyfikatora `readonly` (kompilator niema więc pewności, czy nie zmieni ona struktury).

Elementy `readonly` struktury/klasy (z wyłączeniem pól) dostępne są dopiero od wersji C# 8.0.

#### 4. W deklaracji metody zwracającej referencję

```csharp =
modyfikatorDostepu ref readonly typ NazwaMetody(/*parametry*/)
{
    //cialo metody
}
```

Taki zapis oznacza, że nie możemy modyfikować obiektu na który wskazuje referencja zwrócona nam przez metodę. Ten obiekt jest tylko do odczytu (`readonly`). A dokładniej nie można tego obiektu modyfikować przy użyciu zwróconej referencji.

## [LEKCJA 7 – Klasy abstrakcyjne](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-7-klasy-abstrakcyjne/)
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

### Klasa abstrakcyjna
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

### Metoda abstrakcyjna
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

### Klasy "zapieczętowane"
Są to ostateczne (finalne) klasy, takie, po których nie można dziedziczyć. Tworzy się je dodając w definicji słowo kluczowe `sealed` (przed słowem kluczowym `class`).

```csharp =
ModyfikatorDostepu sealed class NazwaKlasy
{
    //cialo klasy
}
```

Stosuje się je raczej rzadko, gdy chcemy uniemożliwić tworzenie klas pochodnych od danej klasy. Próba utworzenia klasy dziedziczącej po klasie sealed będzie skutkować błędem kompilacji. Ponieważ nie można po niej dziedziczyć, nie może ona posiadać metod abstrakcyjnych, ani wirtualnych. Nie możemy jej już zmieniać (nadpisywać), ani rozszerzać.

## [LEKCJA 8 – Interfejsy](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-8-interfejsy/)
Typ w C# stosowany, podobnie jak klasa abstrakcyjna, do tworzenia szkieletu elementów. Interfejs jest to zbiór deklaracji związanych ze sobą funkcjonalności. Składa się wyłącznie z deklaracji właściwości, metod, indeksatorów i zdarzeń (od C# 8.0 interfejs może zawierać również implementacje metod, choć na razie stosowanie tego nie jest zbyt popularne). Nie może jednak zawierać pól. Interfejs może dziedziczyć jeden lub więcej interfejsów. 

### Tworzenie interfejsu
Interfejs tworzymy przy pomocy słowa kluczowego `interface`.

```csharp =
ModyfikatorDostepu interface NazwaInterfejsu
{
    //skladniki interfejsu
    //np.:
    //wlasciwosci
    typ NazwaWlasciwosci { get; set; }
    //metody
    typ NazwaMetody(/*parametry metody*/);
}
```

Standardowo zaczynamy od pisania interfejsów, a dopiero potem tworzymy implementujące je klasy. Interfejsy nie posiadają konstruktorów, gdyż nie mogą zostać zainicjalizowane (nie można tworzyć obiektów ich typu). Podobnie jak w przypadku klas abstrakcyjnych, można jednak dokonywać rzutowania na ten typ. Czyli zmiennej interfejsu można przypisać obiekt klasy implementującej interfejs. Co ciekawe, można jej nawet przypisać obiekt klasy dziedziczącej po klasie implementującej interfejs. Ponieważ interfejs jest tylko szkieletem, a implementacja jest pozostawiona wyłącznie implementującym go klasom (lub strukturom), więc elementy interfejsu nie posiadają nawet modyfikatorów dostępu. Domyślnie wszystkie są jednak publiczne i abstrakcyjne.

#### Nazwa interfejsu
Jak już wspominaliśmy w 1. tygodniu przyjęło się, że nazwy interfejsów rozpoczynamy wielką literą "i" ("I"). Należy to do dobrych praktyk.

#### Składniki interfejsu

Interfejs może zawierać deklaracje:
* metod (_methods_),
* właściwości (_properties_),
* indeksatorów (_indexers_),
* zdarzeń (_events_).

Interfejs nie może zawierać:
* pól (_fields_),
* konstruktorów (_constructors_),
* przypisania modyfikatorów dostępu (_access modifiers_) do jego składników,
* implementacji.

Składniki interfejsu są domyślnie abstrakcyjne (`abstract`) i publiczne (`public`)

Począwszy od C# 8.0 interfejs może:
* zawierać metody wraz z implementacjami
    * metody wirtualne interfejsu (inaczej metody domyślne interfejsu)<br/>
      Jeżeli metoda posiada implementację w interfejsie i nie ma modyfikatora `sealed` lub `private` to jest metodą wirtualną (`virtual`). To znaczy, że klasy (struktury) implementujące interfejs nie muszą implementować już tej metody, mogą ją jednak nadpisać. Klasy nie dziedziczą jednak domyślnych metod interfejsu, co oznacza, że nie możemy uzyskać do nich dostępu bezpośrednio przez obiekt klasy, a dopiero po jego zrzutowaniu na typ interfejsu.
    * metody "zapieczętowane"<br/>
      Metoda z modyfikatorem `sealed` musi być zaimplementowana wewnątrz interfejsu.
    * metody prywatne<br/>
      Metody z modyfikatorem `private` muszą być zaimplementowane wewnątrz interfejsu.
* pod pewnymi warunkami używać modyfikatorów `private`, `protected`, `internal`, `public`, `virtual`, `abstract`, `sealed`, `static`, `extern` i `partial`<br/>
  Dostęp do metod statycznych (z modyfikatorem `static`) można uzyskać przez nazwę interfejsu.

#### Przykład
Napiszmy przykładowy interfejs deklarujący podstawowe właściwości do operacji na plikach (przykłady użyte w tej lekcji pochodzą ze strony [tutorialsteacher.com](https://www.tutorialsteacher.com/csharp/csharp-interface))

```csharp =
public interface IFile
{
    void ReadFile();
    void WriteFile(string text);
}
```

### Implementacja interfejsu
Jak już wiemy, klasa może dziedziczyć tylko po jednej klasie. Może ona jednak implementować wiele interfejsów. Klasa implementująca interfejs musi zaimplementować wszystkie jego składniki (również właściwości). Czyli, w odróżnieniu od dziedziczenia, gdzie klasa pochodna jedynie rozszerzała i nadpisywała klasę bazową, implementując interfejs, musimy w klasie umieścić wszystkie jego składniki (począwszy od C# 8.0 możemy pominąć zaimplementowane w interfejsie metody). Implementacji dokonujemy analogicznie jak to robiliśmy w przypadku dziedziczenia klas, czyli po nazwie klasy (struktury) wstawiamy dwukropek i podajemy nazwę interfejsu (interfejsów - w tym wypadku rozdzielone przecinkami).

```csharp =
ModyfikatorDostepu class NazwaKlasy : NazwaInterfejsu
{
    // cialo klasy
    // implementacja interfejsu
    // ewentualnie dodatkowe elementy
}
```

Implementacja interfejsu może odbywać się w sposób jawny lub niejawny.

#### Implementacja niejawna interfejsu
Implementując interfejs niejawnie musimy nadpisać wszystkie jego składniki używając modyfikatora dostępu `public` (inaczej wystąpi błąd kompilacji). Oczywiście poza składnikami interfejsu możemy również dodać do klasy inne (dodatkowe) elementy. Zaimplementujmy więc nasz przykładowy interfejs.

```csharp =
public class FileInfo : IFile
{
    public void ReadFile()
    {
        Console.WriteLine("Reading File");
    }

    public void WriteFile(string text)
    {
        Console.WriteLine("Writing to file");
    }
}
```

Spróbujmy teraz użyć tej implementacji tworząc dwa obiekty klasy `FileInfo`. Jeden obiekt przypiszemy do zmiennej typu `IFile`, a drugi do zmiennej typu `FileInfo`. Następnie wywołajmy na obu obiektach obie metody interfejsu.

```csharp =
public class Program
{
    public static void Main()
    {
        IFile file1 = new FileInfo();
        FileInfo file2 = new FileInfo();
		
        file1.ReadFile(); 
        file1.WriteFile("content"); 

        file2.ReadFile(); 
        file2.WriteFile("content");

        //  Output:
        //  Reading File
        //  Writing to file
        //  Reading File
        //  Writing to file
    }
}
```

Ponieważ podczas implementacji nadpisaliśmy w klasie wszystkie metody interfejsu, więc możemy uzyskać do nich dostęp zarówno za pośrednictwem zmiennej `IFile`, jak i zmiennej `FileInfo`.

#### Implementacja jawna interfejsu
Interfejs możemy również zaimplementować jawnie, stosując notację nazwa interfejsu, kropka, nazwa elementu interfejsu który chcemy zaimplementować (`NazwaInterfejsu.NazwaSkladnika`). Ten sposób jest szczególnie użyteczny w przypadku gdy implementujemy wiele interfejsów. Dzięki temu zwiększa się czytelność kodu. Niweluje to również problem z powtarzalnością nazw (gdy implementujemy jednocześnie interfejsy zawierające metody o takich samych nazwach). Używając implementacji jawnej nie stosujemy modyfikatora dostępu `public`, gdyż będzie to skutkować błędem kompilacji. Oczywiście w ty wypadku poza składnikami interfejsu (interfejsów) również możemy w klasie umieścić dodatkowe składniki. Zaimplementujmy więc nasz przykładowy interfejs (tym razem jawnie) i dodajmy jeszcze dodatkową metodę `Search`.

```csharp =
class FileInfo : IFile
{
    void IFile.ReadFile()
    {
        Console.WriteLine("Reading File");
    }

    void IFile.WriteFile(string text)
    {
        Console.WriteLine("Writing to file");
    }

    public void Search(string text)
    {
        Console.WriteLine("Searching in file");
    }
}
```

Spróbujmy użyć również tej implementacji, ponownie tworząc dwa obiekty klasy `FileInfo`. Jeden obiekt przypiszemy do zmiennej typu `IFile`, a drugi do zmiennej typu `FileInfo`. Następnie wywołajmy na obu obiektach utworzone metody. Implementując interfejs w sposób jawny mamy jednak dostęp do jego składników tylko za pośrednictwem instancji typu interfejsu (w naszym przypadku zmiennej typu `IFile`, nie zmiennej typu `FileInfo`).

```csharp =
public class Program
{
    public static void Main()
    {
        IFile file1 = new FileInfo();
        FileInfo file2 = new FileInfo();
		
        file1.ReadFile(); 
        file1.WriteFile("content"); 
        // file1.Search("text to be searched")   // blad kompilacji
        
        file2.Search("text to be searched");
        // file2.ReadFile();     // blad kompilacji
        // file2.WriteFile("content");   // blad kompilacji
    }
}
```

W przypadku implementacji jawnej zmienna typu interfejsu może uzyskać dostęp wyłącznie do składników interfejsu, a zmienna klasy do składników klasy (elementy zaimplementowane jawnie są składnikami interfejsu, ale nie klasy).

#### Implementacja kilku interfejsów
Jak już wspominaliśmy implementując kilka interfejsów zaleca się zrobienie tego w sposób jawny. Już omówiliśmy ten sposób, zobaczmy więc tylko przykład implementacji dwóch interfejsów tym właśnie sposobem.

```csharp =
interface IFile
{
    void ReadFile();
}

interface IBinaryFile
{
    void OpenBinaryFile();
    void ReadFile();
}

class FileInfo : IFile, IBinaryFile
{
    void IFile.ReadFile()
    {
        Console.WriteLine("Reading Text File");
    }

    void IBinaryFile.OpenBinaryFile()
    {
        Console.WriteLine("Opening Binary File");
    }

    void IBinaryFile.ReadFile()
    {
        Console.WriteLine("Reading Binary File");
    }

    public void Search(string text)
    {
        Console.WriteLine("Searching in File");
    }
}

public class Program
{
    public static void Main()
    {
        IFile file1 = new FileInfo();
        IBinaryFile file2 = new FileInfo();
        FileInfo file3 = new FileInfo();
		
        file1.ReadFile(); 
        // file1.OpenBinaryFile();     // blad kompilacji
        // file1.SearchFile("text to be searched");     // blad kompilacji
        
        file2.OpenBinaryFile();
        file2.ReadFile();
        // file2.SearchFile("text to be searched");     // blad kompilacji
    
        file3.Search("text to be searched");
        // file3.ReadFile();     // blad kompilacji
        // file3.OpenBinaryFile();     // blad kompilacji
    }
}
```

### Interfejs vs. klasa abstrakcyjna

| Cecha | Interfejs | Klasa abstrakcyjna |
| :--- | :---: | :---: |
| Służy jako szkielet dla innych elementów | tak | tak |
| Można tworzyć obiekty tego typu | nie | nie |
| Posiada konstruktor | nie | tak |
| Może posiadać pola | nie | tak |
| Można rzutować na ten typ inne obiekty | obiekty klas implementujących lub dziedziczących po klasach implementujących interfejs | klasy dziedziczące (bezpośrednio lub pośrednio) po tej klasie |
| Po zrzutowaniu na ten typ dostęp tylko do jego składników (nie do składników klasy która go implementuje/dziedziczy) | tak | tak |
| Klasa może implementować/dziedziczyć | wiele | jedną |
| Zawiera wyłącznie definicje | tak (przed C# 8.0) | może implementować składniki |
| Składniki posiadają modyfikatory dostępu | nie | tak |
| Może posiadać elementy o różnych modyfikatorach dostępu | nie (przed C# 8.0 tylko `public`) | tak |
| Klasa musi zawierać | wszystkie elementy interfejsu | tylko elementy abstrakcyjne i te, które chcemy nadpisać |

### Zastosowanie
Interfejsy są szeroko stosowane w aplikacjach webowych, są podstawą ich architektury. Więcej na ten temat dowiemy się w dalszej części kursu. Warto jednak pamiętać, że bez interfejsów nie powstaje obecnie żadna aplikacja internetowa. Są niezbędne do zachowania wielu zasad nowoczesnego programowania.

## [LEKCJA 9 – Typy generyczne](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-9-typy-generyczne/)
**Typy generyczne** - są to typy mogące obsługiwać różne typy danych. Stosuje się je, gdy na różnych typach danych chcemy wykonywać takie same operacje. Przykładem typu generycznego jest poznana już przez nas lista (`List<T>`) służąca do tworzenia i obsługi kolekcji obiektów danego typu (`T` - zamiast T wstawiamy dowolny typ danych), określonego w definicji. Wszystkie predefiniowane typy generyczne należą do namespace'u `System.Collection.Generic`. Typy generyczne często stosuje się w aplikacjach webowych do obsługi operacji CRUD-owych na klasach.

### Operacje CRUD
Czyli operacje Create (tworzenia) - Read (odczytywania) - Update (aktualizacji) - Delete (usuwania). Są to najczęściej operacje związane z obsługą bazy danych. Właściwie na każdej tabeli z bazy danych wykonuje się takie operacje. Aby nie implementować tych metod dla każdej klasy osobno, często stosuje się właśnie klasy generyczne, które będą wykonywały te operacje podczas pracy programu. Będziemy musieli jedynie podać typ (klasę) dla jakiego te operacje mają być wykonane.

### Definicja typu generycznego
Typ generyczny definiujemy prawie tak samo jak każdą inną klasę, dodając jedynie, za nazwą klasy, nawias ostry z literką `T` w środku.

```csharp =
ModyfikatorDostepu class NazwaKlasyGenerycznej<T>
{
    // cialo klasy
}
```

#### Przykład
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

### Korzystanie z typów generycznych
Z typów generycznych korzystamy prawie tak samo jak z każdego innego typu, dodając jedynie po nazwie typu generycznego w nawiasach ostrych nazwę typu który chcemy, aby nasz typ generyczny obsłużył.

```csharp =
NazwaKlasyGenerycznej<typ> nazwaZmiennej = new NazwaKlasyGenerycznej<typ>();
```

#### Przykład
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

### Przykłady typów generycznych C#
Język C# ma już zdefiniowane wiele typów generycznych, z których możemy korzystać. Są to np. typy:
* `List<T>` - listy,
* `Dictionary<TKey, TValue>` - słowniki,
* `SortedList<TKey, TValue>` - uporządkowane listy,
* `SortedDictionary<TKey, TValue>` - uporządkowane słowniki,
* `LinkedList<T>` - podwójne listy połączone

Więcej informacji o tych i innych typach generycznych można znaleźć w [dokumentacji](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic?view=net-7.0).

### Ograniczanie klas generycznych
Definiując typ generyczny możemy ograniczyć obsługi jakich typów będzie on dotyczył. Możemy np. stworzyć klasę generyczną tylko dla typów referencyjnych, albo tylko dla typów implementujących jakiś interfejs itd. Służy do tego słowo kluczowe `where`. W definicji klasy generycznej, za nawiasem ostrym z literką T (`<T>`) piszemy `where T : warunek`.
#### Przykłady
Typ generyczny obsługujący tylko klasy (gdy spróbujemy użyć go np. dla typu `int`, otrzymamy błąd kompilacji)

```csharp =
public class GenericService<T> where T : class
{
    // cialo klasy
}
```

albo tylko klasy implementujące interfejs `IItem` (gdy spróbujemy użyć go dla typu, który nie implementuje tego interfejsu otrzymamy błąd kompilacji)

```csharp =
public class GenericService<T> where T : IItem
{
    // cialo klasy
}
```

itd.

## [LEKCJA 10 – Refaktoryzacja](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-10-refaktoryzacja/)
W tej lekcji zajmiemy się refaktoryzacją naszego programu, utworzonego w poprzednim tygodniu, z wykorzystaniem nabytej w tym tygodniu wiedzy. Skupimy się na trzech aspektach:
1. Podział logiczny aplikacji - podzielimy solucję na dodatkowe projekty
2. Wykorzystanie interfejsów - utworzymy interfejsy dla serwisów
3. Wykorzystanie dziedziczenia i klas abstrakcyjnych - utworzenie bazowego serwisu i modelu
### 1. Podzielić solucję na projekty
Aby aplikacja była łatwiejsza w zarządzaniu i dalszym rozwoju, przeniesiemy część jej funkcjonalności do odrębnych projektów. Nie będą to jednak projekty konsolowe, a biblioteki klas (_Class Library (Standard .NET)_). Są to projekty, które nie kompilują się do pliku wykonywalnego (_.exe_) i co za tym idzie nie mogą funkcjonować jako samodzielne programy. Kompilują się natomiast do plików _.dll_, czyli właśnie bibliotek, i służą do implementacji funkcjonalności potrzebnych w programie. Takie biblioteki można później ponownie wykorzystywać w kolejnych aplikacjach. Utworzymy dwie biblioteki i usuniemy przykładową klasę z każdej z nich.
#### 1. _NazwaAplikacji.App_
Po utworzeniu projektu i usunięci przykładowej klasy, jeszcze zanim utworzymy własną klasę, zajmijmy się zależnościami między projektami (ang. _dependencies_). Wiemy na pewno, że nasz główny projekt będzie korzystał z tego projektu. W naszym _Solution Explorer_ kliknijmy więc prawym przyciskiem myszki na _Dependencies_ naszego głównego projektu i wybierzmy _Add Project Reference..._. Otworzy nam się _Reference Manager_. Powinien automatycznie otworzyć się on na zakładce _Projects_ -> _Solution_, czyli liście projektów w naszej solucji (z wyłączeniem projektu, dla którego menadżer otworzyliśmy). Na liście zaznaczamy check box naszego nowo utworzonego projektu, dodając do niego referencję (informację dla kompilatora, że w danym projekcie będziemy korzystać z innego projektu), i klikamy _OK_.

W tym projekcie znajdować się będzie cała logika naszej aplikacji. Umieścimy w niej wszystkie serwisy (klasy serwisowe) służące do sterowania naszą aplikacją.

Podzielmy jeszcze projekt na foldery:
##### 1. _Abstract_
Tu znajdą się interfejsy dla serwisów. Serwisy będą zajmować się manipulacją naszych obiektów entity. Ich dodawaniem, usuwaniem, aktualizacją, czy zwracaniem.
##### 2. _Common_
Tu umieścimy bazowy serwis, implementujący interfejs bazowy dla serwisów.
##### 3. _Concrete_
Tu znajdą się konkretne implementacje serwisów.
##### 4. _Managers_
Tu znajdą się nasze menadżery. Menadżery, są to swoiste kontrolery, decydujące co użytkownik chce zrobić i na tej podstawie wykonujące odpowiednie akcje (być może wywołujące odpowiednie metody z serwisu). W nich będą znajdywać się wszystkie kroki decyzyjne. Być może będzie to wyświetlenie dodatkowego menu i wybranie odpowiedniej opcji, czy przekazywanie do serwisu odpowiedniego obiektu, tak, aby dodać go do listy. Menadżery będziemy tworzyć dla konkretnych modeli.
#### 2. _NazwaAplikacji.Domain_
W tym projekcie znajdować się będą wszystkie modele (klasy reprezentujące obiekt, który w przyszłości mógłby być obiektem bazodanowym).

Podzielmy projekt na foldery według podobnej zasady jak powyższy.
##### 1. _Common_
Tu umieścimy klasę bazową dla modeli.
##### 2. _Entity_
Tu będą się znajdywać implementacje konkretnych modeli.
### 2. Dodać interfejsy dla serwisów
W projekcie _NazwaAplikacji.App_ w folderze _Abstract_ utwórzmy interfejsy dla naszych serwisów.
#### 1. `IService<T>`
Zacznijmy od utworzenia interfejsu bazowego dla wszystkich serwisów. Będzie to publiczny (`public`) interfejs generyczny. Umieścimy w nim listę elementów typu `T` oraz podstawowe metody zwracające wszystkie elementy listy, pozwalające na dodanie, aktualizację lub usunięcie elementu listy.
##### Przykład
```csharp =
namespace NazwaAplikacji.App.Abstract
{
    public interface IService<T>
    {
        List<T> Items { get; set; }
        
        List<T> GetAllItems();      // zwracanie wszystkich elementów listy
        int AddItem(T item);        // dodawanie elementu do listy
        int UpdateItem(T item);     // aktualizacja elementu znajdującego się na liście
        void RemoveItem(T item);    // usunięcie elementu z listy
    }
}
```
### 3. Dodać bazowy serwis i bazowy model
W naszej aplikacji utwórzmy bazowy serwis, po którym dziedziczyć będą inne klasy serwisowe, implementujące logikę związaną z konkretnym modelem. Następnie stwórzmy również bazowy model, czyli klasę będącą szkieletem dla wszystkich klas implementujących modele bazodanowe.
#### `BaseService`
W projekcie _NazwaAplikacji.App_ w folderze _Common_ stwórzmy plik _BaseService.cs_ w którym umieścimy publiczną klasę generyczną będącą klasą bazową dla serwisów i implementującą bazowy interfejs (`IService<T>`). Tu możemy już stworzyć podstawowy konstruktor, w którym zainicjalizujemy naszą listę. Możemy również zaimplementować podstawowe metody.
##### Przykład
W przykładzie będziemy korzystać z utworzonego poniżej modelu bazowego. Ponieważ serwisy będą obsługiwać konkretne modele, więc chcielibyśmy zaznaczyć to w serwisie bazowym. Ponieważ jest to klasa generyczna, więc typ `T` będzie nam wskazywać, który model obsługujemy. `T` musi więc być typu `BaseEntity` (lub jakiejkolwiek klasy po nim dziedziczącej). Umieśćmy więc w definicji klauzulę `where`. Aby móc korzystać w naszym projekcie _NazwaAplikacji.App_ z elementów projektu  _NazwaAplikacji.Domain_, musimy w tym pierwszym dodać referencję do tego drugiego. Możemy to zrobić za pośrednictwem _Dependencies_ projektu _NazwaAplikacji.App_, analogicznie jak poprzednio, lub podczas pisania kodu, korzystając z inteligentnych podpowiedzi. Kiedy napiszemy nazwę klasy `BaseEntity` przed dodaniem referencji, Visual Studio zaznaczy nam błąd kompilacji, gdyż taka klas nie jest mu znana. Wówczas możemy uruchomić, na będzie, inteligentne podpowiedzi, np. korzystając ze skrótu klawiszowego **Alt + Enter** (lub **Ctrl + .**) i wybrać z listy _Add reference to 'NazwaAplikacji.Domain'_. Wybranie tej opcji spowoduje automatyczne dodanie odpowiedniej pozycji w _Dependencies_ projektu.
```csharp =
namespace NazwaAplikacji.App.Common
{
    public class BaseService<T> : IService<T> where T : BaseEntity
    {
        public List<T> Items { get; set; }

        public BaseService()
        {
            Items = new List<T>();
        }

        public int GetLastId() // get highest Id on the list
        {
            int lastId;
            if(Items.Any()) // list is not empty
            {
                lastId = Items.OrderBy(p => p.Id).LastOrDefault().Id; // sort list and get Id of the last element
            }
            else // no elements on the list
            {
                lastId = 0;
            }

            return lastId;
        }

        public int AddItem(T item)
        {
            Items.Add(item);
            return item.Id;
        }

        public List<T> GetAllItems()
        {
            return Items;
        }

        public void RemoveItem(T item)
        {
            Items.Remove(item);
        }
        
        public int UpdateItem(T item)
        {
            var entity = Items.FirstOrDefault(p => p.Id == item.Id);
            if(entity != null)
            {
                entity = item;
            }

            return entity.Id;
        }
    }
}
```
#### `BaseEntity`
W projekcie _NazwaAplikacji.Domain_ w folderze _Common_ stwórzmy plik _BaseEntity.cs_. Umieśćmy w nim publiczną klasę, która będzie klasą bazową dla wszystkich modeli (wszystkie modele będą po niej dziedziczyć).
##### Przykład
W przykładzie skorzystamy z utworzonej poniżej klasy `AuditableModel`. Ponieważ chcemy, aby wszystkie modele zawierały zdefiniowane w niej pola, więc nasz model bazowy będzie po niej dziedziczył.
```csharp =
namespace NazwaAplikacji.Domain.Common
{
    public class BaseEntity : AuditableModel
    {
        public int Id { get; set; }
    }
}
```
#### `AuditableModel`
W projekcie _NazwaAplikacji.Domain_ w folderze _Common_ stwórzmy jeszcze jedną klasę, `AuditableModel`. Mówiliśmy już o niej przy okazji omawiania dziedziczenia (Tydzień 3., lekcja 4.). Przypomnijmy. Jest to popularna klasa bazowa dla modeli, często tworzona w aplikacjach webowych. Ponieważ modele służą najczęściej do odzwierciadlania tabel w bazie danych, a większość z nich zawiera współcześnie informacje o tym kto i kiedy utworzył/zmodyfikował dany rekord, to często tworzy się odrębną klasę przechowującą takie informacje, z której potem mogą korzystać nasze modele (mogą po niej dziedziczyć).
##### Przykład
```csharp =
namespace NazwaAplikacji.Domain.Common
{
    public class AuditableModel
    {
        public int CreatedById { get; set; }            // kto utworzył rekord
        public DateTime CreatedDateTime { get; set; }   // kiedy rekord został utworzony
        public int? ModifiedById { get; set; }          // jezeli rekord zostal zmodyfikowany, to kto go zmodyfikowal (jezeli nie zostal zmodyfikowany to null, dlatego typ int?, czyli nullowalny int)
        public DateTime? ModifiedDateTime { get; set; } // jezeli rekord zostal zmodyfikowany, to kiedy (jezeli nie zostal zmodyfikowany to null, dlatego typ DateTime?, czyli nullowalna struktura DateTime)
    }
}
```
### Uporządkowanie wcześniej utworzonych klas
Kiedy skończymy tworzyć nowe projekty, podzielimy je na foldery i utworzymy bazowe klasy i interfejsy należałoby dopasować utworzone wcześniej klasy do nowego porządku. Jeżeli jakaś klasa niema jeszcze własnego pliku, to go stwórzmy. Umieśćmy zaimplementowane serwisy w folderze _Concrete_ projektu _NazwaAplikacji.App_, a modele w folderze _Entity_ projektu _NazwaAplikacji.Domain_. Przenosząc pliki z jednego projektu do drugiego, musimy pamiętać o kilku rzeczach:
#### 1. Zmiana namespace'u
Na górze pliku zmieniamy namespace z nazwy projektu z której plik pochodzi na nazwę projektu do którego został przeniesiony, kropkę i nazwę folderu.
#### 2. Dodanie dziedziczenia
Do naszych serwisów i modeli dodajemy dziedziczenie po nowozaimplementowanych klasach bazowych (serwisy - `BaseService<NazwaOdpowiedniegoModelu>`, modele - `BaseEntity`).
#### 3. Dodanie właściwych usingów
Na górze plików dodajemy odpowiednie pozycje using potrzebne do korzystania z nowo utworzonych plików.
#### 4. Aktualizacja klas uwzględniająca klasy bazowe
Z klas pochodnych usuwamy właściwości i metody, które zaimplementowaliśmy w nowoutworzonych klasach bazowych. Jeżeli chcemy natomiast nadpisać któreś z metod, to dodajemy odpowiednie słowa kluczowe. Dopasowujemy nazwy klas i właściwości.
### Dodanie folderu _Helpers_ w głównym projekcie aplikacji
W głównym projekcie aplikacji, w razie potrzeby, możemy utworzyć dodatkowy folder _Helpers_. Możemy w nim przechowywać elementy pomocnicze dla naszej aplikacji. Mogą to być np. utworzone na potrzeby tej aplikacji enumy, czy klasy, porządkujące nam wyświetlanie w konsoli.
### Poprawa metody `Main`
Po zakończeniu porządkowania klas, musimy poprawić kod w klasie `Main` naszej aplikacji, uwzględniając wprowadzone zmiany.
#### Uprośćmy nasz kod
Metoda `Main` powinna zawierać możliwie najmniej elementów. Powinny być one za to jak najbardziej podstawowe.
##### 1. Korzystajmy z klas bazowych.
Gdzie to tylko możliwe, korzystajmy z klas bazowych, aby zwiększyć elastyczność naszego kodu.
##### 2. Przenieśmy co można do innych klas
Jeśli jest to możliwe, to kod dotyczący konkretnej klasy, przenieśmy do tej właśnie klasy. Stwórzmy np. odpowiednie konstruktory i wywoływane w nich metody do wypełniania obiektów klasy danymi inicjalizacyjnymi. Wykorzystajmy menadżery. Przenieśmy do nich interakcję z użytkownikiem.

## [LEKCJA 11 – Błędy początkujących](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-11-bledy-poczatkujacych/)
Aby unikać błędów pamiętaj o stosowaniu się do wszystkich poznanych w tym tygodniu zasad. Nawet jeśli nie jest jeszcze do końca jasne dlaczego tak robimy i część z nich może na razie wydawać się bez sensu, to przestrzegaj ich wszystkich, a zaprocentuje to w przyszłości. W miarę nabierania doświadczenia stanie się jasne dlaczego tak robimy, a stosowanie się teraz do zasad ułatwi nam dalszą pracę z programem.
### 1. Dziedziczenie i polimorfizm
Nawet jeśli nie jest to jeszcze do końca jasne, pamiętaj, aby tworzyć klasy tak, aby dziedziczyły one po sobie. Zwróć przy tym uwagę, aby to dziedziczenie odbywało się w sposób analogiczny do tego, jak dzieje się to w świecie realnym. Czyli, obiekt klasy pochodnej jest również obiektem klasy bazowej (ryba jest zwierzęciem, krzesło jest meblem itp.). Klasa pochodna jedynie uszczegóławia i rozszerza klasę bazową (ryba jest zwierzęciem żyjącym w wodzie i oddychającym skrzelami (ale każde zwierzę żyje i oddycha), krzesło jest meblem służącym do siedzenia (ale każdy mebel ma jakąś funkcję)). Klasa `Person` (z ang. osoba) nie może natomiast dziedziczyć po klasie `File` (z ang. plik), tylko dla tego, że mamy jakiś plik będący spisem osób, bo osoba nie jest plikiem. Przy dziedziczeniu pamiętajmy również o nadpisywaniu metod. Stosujmy słowa kluczowe takie jak `override`, `new`, `virtual`, `abstract` itd. zgodnie z ich przeznaczeniem.
### 2. Stosowanie modyfikatorów dostępu, enkapsulacja
Pamiętaj o enkapsulacji. Przed przystąpieniem do refaktoryzacji przemyśl, które elementy powinny być publiczne (`public` lub publiczne, ale dostępne tylko w danym projekcie - `internal`), a które prywatne (`private`) lub chronione (`protected`). Udostępnij publicznie tylko te elementy, które rzeczywiście muszą być publiczne. Jeżeli nie chcesz, aby jakaś właściwość mogła być zmieniana poza daną klasą, to ją przed tym zabezpiecz. Używaj również innych modyfikatorów: `sealed`, `readonly`, `const`, `static` itd., zgodnie z ich przeznaczeniem.

## [LEKCJA 12 – Praca domowa](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-12-praca-domowa/)
Pracą domową na ten tydzień jest oczywiście dalszy rozwój aplikacji. Zacznij od refaktoryzacji. Podziel aplikację na projekty w analogiczny sposób, jak to zostało przedstawione w lekcji 10. tego tygodnia. Utwórz odpowiednie interfejsy. Przydadzą nam się one już w kolejnym tygodniu podczas testowania. Dobrze zaprojektowana aplikacja jest łatwiejsza do przetestowania. Po zakończeniu refaktoryzacji, można zająć się dalszym rozwojem aplikacji. Dodać nowe funkcjonalności. Najeży jednak pamiętać o przestrzeganiu wszystkich poznanych dotychczas zasad.


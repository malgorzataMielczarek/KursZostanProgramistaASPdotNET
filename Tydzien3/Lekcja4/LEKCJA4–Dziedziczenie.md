# [LEKCJA 4 – Dziedziczenie](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-4-polimorfizm/)
**Dziedziczenie** jest to jeden z trzech paradygmatów (filarów) programowania obiektowego. Pozwala ona na przejście od ogółu do szczegółu. Tzn. mamy jakąś ogólną klasę (np. Ssak) i klasy szczegółowe (np. Pies, Kot, Małpa, Człowiek itp.), które będą w sobie zawierać wszystkie cechy klasy ogólnej (pola, właściwości, metody). Dodatkowo każda z tych bardziej szczegółowych klas będzie posiadała własne, charakterystyczne tylko dla siebie cechy (pola, właściwości, metody). Oczywiście dziedziczenie możemy realizować wielopoziomowo. Możemy np. mieć klasę Zwierzę, po której będą dziedziczyć klasy Ssak, Ptak, Gad, Płaz itd. Później np. po klasie Ssak będą dziedziczyć klasy Naczelny, Gryzoń itd., a po klasie Gryzoń np. Wiewiórka, Szczur itd. Wówczas klasa Szczur będzie posiadać wszystkie składniki klas Zwierzę, Ssak, Gryzoń i Szczur. Dziedziczenie może odbywać się jednak tylko po jednej klasie (dana bardziej szczegółowa klasa może mieć tylko jedną klasę bazową). Czyli np. nasza klasa Szczur, ponieważ dziedziczy już po klasie Gryzoń, nie mogłaby jeszcze dodatkowo dziedziczyć po klasie ZwierzątkoDomowe. Klasa może natomiast implementować kilka interfejsów, ale o tym w dalszych lekcjach tego tygodnia. Pokażmy więc jak wygląda dziedziczenie na przykładzie.

## Przykład
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

### Konstruktor
Ponieważ klasa bazowa (`Rectangle`) niema zdefiniowanego konstruktora domyślnego (bezargumentowego), a jedynie konstruktor przyjmujący dwa argumenty, więc w klasie `Square` również musimy zdefiniować konstruktor. Nie musi on przyjmować takich samych argumentów jak konstruktor klasy `Rectangle`, musi jednak go wywoływać. Aby to zrobić po definicji konstruktora klasy `Square` piszemy dwukropek, słowo kluczowe `base` i w nawiasie odpowiednie argumenty, które wyślemy do konstruktora klasy bazowej. Nie musimy więc jeszcze raz przepisywać kodu, który zawarliśmy w konstruktorze klasy bazowej. Jeżeli w klasie `Square` mielibyśmy jakieś dodatkowe pola/właściwości, które chcielibyśmy zainicjalizować, albo jakieś inne czynności jakie chcielibyśmy wykonać podczas tworzenia obiektu tego typu, to możemy dopisać odpowiedni kod w ciele konstruktora. Podczas tworzenia obiektu najpierw zostanie wywołany konstruktor klasy bazowej, a następnie konstruktor klasy pochodnej (najpierw konstruktor `Rectangle`, a potem `Square`).

### Przykrywanie metod
W klasie pochodnej poza dodatkowymi metodami niewystępującymi w klasie bazowej możemy również utworzyć metody o takich samych nazwach jak te występujące w klasie bazowej. Takie metody przykryją te występujące w klasie bazowej (dla obiektów klasy pochodnej będą wywoływane zamiast nich). W implementacji klasy pochodnej możemy jednak odwołać się również do oryginalnych implementacji z klasy bazowej. W tym celu ponownie używamy słowa kluczowego `base` i po kropce podajemy nazwę metody klasy bazowej, którą chcemy wywołać i w nawiasie odpowiednie argumenty.

### Przykład użycia obiektów
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

## Popularna klasa bazowa w aplikacjach webowych
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

# [LEKCJA 16 – Pola i właściwości](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-16-pola-i-wlasciwosci/)
## Pola
Mówiliśmy już, że klasa składa się z właściwości i metod. Nic nie stoi jednak na przeszkodzie, aby w klasie stworzyć też zwykłą zmienną. Robimy to podając modyfikator dostępu, typ i nazwę zmiennej. Na końcu oczywiście stawiamy średnik. Zmienną będącą składnikiem klasy nazywamy polem. Np.:

```csharp =
public class Cube
{
    public int EdgeLength;
    private int volume;
    private int lateralSurface;
    public Cube (int a = 0) 
    { 
        EdgeLength = a;
        volume = a * a * a;
        lateralSurface = a * a * 6;
    }
    public int Volume()
    {
        volume = EdgeLength * EdgeLength * EdgeLength;
        return volume;
    }
    public int LateralSurface()
    {
        lateralSurface = EdgeLength * EdgeLength * 6;
        return lateralSurface;
    }
}
```

Dostęp do pól publicznych możemy uzyskać w ten sam sposób, jak do wszystkich innych publicznych składników klasy:

```csharp =
Cube cube = new Cube();
Console.WriteLine("Domyślna długość krawędzi: " + cube.EdgeLength);
cube.EdgeLength = 10;
Console.WriteLine("Nowa długość krawędzi: " + cube.EdgeLength);
Console.WriteLine("Objętość kostki: " + cube.Volume());
Console.WriteLine("Pole powierzchni bocznej kostki" + cube.LateralSurface());
```

## Właściwości
Właściwości są to tak na prawdę struktury składające się z pola prywatnego i dwóch publicznych metod (`get` i `set`). Czyli np. właściwość:

```csharp =
public class Person
{
    public int Id { get; set; }
}
```

można by tak na prawdę zapisać jako:

```csharp =
public class Person
{
    private int id;
    public int getId()
    {
        return id;
    }
    public void setId(int newId)
    {
        id = newId;
    }
}
```

C# zajmuje się jednak całą tą mechaniką za nas. Może jednak zdarzyć się, że będziemy chcieli jawnie zaimplementować `get`tery i `set`tery. Np. chcemy mieć pewność, że nasze id będzie mieścić się w przedziale od 1 do 10 oraz wyświetlić odpowiednią informację w konsoli zarówno przy jego pobieraniu jak i zwracaniu. Możemy to zrobić np. tak:

```csharp =
public class Person
{
    private int _id;
    public int Id 
    { 
        get
        {
            Console.WriteLine($"Pobrano wartość Id równą {_id}.");
            return _id;
        }
        set
        {
            _id = value % 10 + 1;
            Console.WriteLine($"Ustawiono Id na {_id}.");
        } 
    }
}
```

Jak widać nazwę prywatnego pola ustawia się na ogół na taką samą jak nazwa właściwości, tylko pisaną z małej litery i poprzedzoną podkreślnikiem (_). W `set`terze natomiast użyliśmy słowa kluczowego `value`, które oznacza wartość jaką do niego przekazaliśmy. Czyli jeżeli stworzyliśmy sobie obiekt `Person person = new Person();` i zapisaliśmy niżej `person.Id = 5;`, to `value` będzie równe właśnie to `5`.

### Właściwości read-only
Może się też tak zdarzyć, że chcemy, aby można było odczytać wartość naszej właściwości, jednak nie chcemy dawać możliwości jej jawnej zmiany. Np. nasze Id będzie ustawiane automatycznie w momencie tworzenia obiektu (zaprogramujemy to w kodzie klasy) i nie chcemy, aby ktokolwiek mógł nam tam mieszać. Oby to zrobić wystarczy w definicji właściwości pominąć sekcję `set`. Czyli w podstawowej formie będzie to wyglądało:

```csharp =
public class Person
{
    public int Id { get; }
}
```

natomiast gdy musimy skorzystać z jawnej deklaracji pola prywatnego:

```csharp =
public class Person
{
    private int _id;
    public int Id 
    { 
        get
        {
            return _id;
        }
    }

    //możemy to też zapisać w jednej linii: public int Id { get { return _id; } }
}
```

W nowszych wersjach języka C# (od C# 7.0) możemy skrócić ten zapis i napisać:

```csharp =
public class Person
{
    private int _id;
    public int Id => _id;
}
```

Taki skrócony zapis można również stosować przy właściwościach read-write (przy jawnej deklaracji zarówno `get`tera jak i `set`tera):

```csharp =
public class Person
{
    private int _id;
    public int Id 
    { 
        get => _id;
        set => _id = value % 10 + 1;
    }
}
```

Można też oczywiście mieszać oba zapisy (np. skrócony zapis dla `get`tera i normalny dla `set`tera).

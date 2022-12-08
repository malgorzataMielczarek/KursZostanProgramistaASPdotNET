# [LEKCJA 2 – Konstruktory](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-2-konstruktory/)
**Konstruktor** jest to specjalny rodzaj metody, wywoływanej podczas inicjalizacji obiektu danej klasy.

## Tworzenie konstruktora
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

## Wywoływanie konstruktora
Standardowo konstruktor jest wywoływany w momencie tworzenia nowego obiektu danej klasy, czyli gdy piszemy np.:

```csharp =
MyClass newObject = new MyClass();
```

## Konstruktor domyślny
Jeżeli w naszej klasie nie zadeklarujemy konstruktora to zostaje wywoływany tzw. domyślny bezparametrowy konstruktor. Jego zadaniem jest zainicjalizowanie wartościami domyślnymi wszystkich pól i właściwości klasy. Czyli np. do pól i właściwości typu `int` podstawiane jest `0`, do typów referencyjnych `null` itd.

## Konstruktor z parametrami
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

## Konstruktor z listą inicjalizacyjną
Gdybyśmy chcieli zapewnić nadanie wartości określonym parametrom, ale jednocześnie dać możliwość tworzenia obiektów bez podawania parametrów możemy wówczas skorzystać z konstruktora z listą inicjalizacyjną. Możemy np. Do powyższej klasy `User` dopisać drugi konstruktor:

```csharp
public User() : this("guest", "1234"){}
```

Wówczas możemy stworzyć obiekt tej klasy nie podając żadnych parametrów:

```csharp =
User guest = new User();
```

W ten sposób, wywołamy konstruktor bezparametrowy, który wywoła z kolei wywoła konstruktor z parametrami, przypisując argumentom odpowiednie wartości podane w liście inicjalizacyjnej (`: this("guest", "1234")`). Czyli pod parametr `login` podstawi wartość `"guest"`, a pod parametr `pwd` wartość `"1234"`.

Stosowanie konstruktorów z listą inicjalizacyjną jest dobrą metodą, jeżeli mamy możliwość nadania naszym wymaganym do wypełnienia właściwością wartości domyślnych.

## Tworzenie wielu konstruktorów
Możemy stworzyć dowolną ilość konstruktorów, jednak muszą się one różnić ilością i/lub typem danych przekazywanych przez parametry. Jest to tzw. przeciążanie metod, o którym więcej nauczymy się w kolejnej lekcji (lekcja 3).

## Kiedy tworzyć własne konstruktory
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

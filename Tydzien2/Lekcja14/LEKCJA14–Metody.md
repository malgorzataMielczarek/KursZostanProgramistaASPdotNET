# [LEKCJA 14 – Metody](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-14-metody/)
Jak wspomnieliśmy w poprzedniej lekcji, klasy składają się z właściwości i metod.

Współcześnie często tworzymy osobne klasy z samymi właściwościami, gdyż są one najczęściej odzwierciedleniem bazy danych, więc ma to sens, aby nie zawierały żadnych metod.

## Klasy serwisowe
Do manipulacji danymi przechowywanymi w tych klasach tworzy się najczęściej tzw. klasy serwisowe. Nadaje się im najczęściej nazwy takie jak nazwa klasy podstawowej z dodanym na końcu słowem `Service`. Klasy serwisowe posiadają najczęściej obiekt lub listę obiektów klasy podstawowej oraz metody do manipulacji nimi. 

## Metody
Wróćmy jednak do metod. Służą one do wykonywania jakichś operacji na właściwościach klasy i/lub danych podanych im jako parametry (dane które przekazujemy do metody w momencie jej wywołania). Metody definiujemy w następujący sposób: podajemy modyfikator dostępu (`public`, `private` itp.), następnie, jeśli chcemy aby była to metoda statyczna, piszemy słowo kluczowe `static`, typ zwracanych danych (lub `void`, jeśli nie chcemy aby nasza metoda cokolwiek zwracała), w końcu podajemy nazwę metody, w nawiasach okrągłych parametry (typ i nazwa, kolejne parametry oddzielone przecinkami lub puste nawiasy, jeżeli nie chcemy przekazywać do metody żadnych danych) a w klamrach ciało metody (instrukcje wykonywane gdy wywołamy metodę).
### Instrukcja skoku `return`
Jak wspomnieliśmy metoda, kiedy skończy się wykonywać, może zwracać jakąś daną. Służy do tego słowo kluczowe `return`. Oznacza ono natychmiastowe zakończenie wykonywania danego elementu (w tym wypadku metody). Po nim podajemy daną którą chcemy zwrócić (otrzymać jako wynik działania metody) i średnik. Jeżeli chcemy zakończyć wykonywanie metody, ale nie chcemy zwracać żadnych danych (metoda typu `void`), to piszemy tylko `return;`. Jeżeli zadeklarowaliśmy, że nasza metoda będzie zwracać dane jakiegoś typu, to zawsze (nie zależnie od wariantu jej zakończenia) musi zwracać jakąś daną tego typu.

Stwórzmy więc dwie przykładowe klasy, jedną składającą się z samych właściwości i klasę serwisową do jej obsługi:

```csharp =
public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Surname { get; set; }
    public int Age { get; set; }
}
```

```csharp =
public class PersonService
{
    public List<Person> People { get; set; }

    public PersonService()
    {
        People = new List<Person>();
    }

    public int AddNewPerson(string name, string surname, int age)
    {
        int id;
        if(People.Count == 0) id = 1;
        else id = People.Max(person => person.Id) + 1;
        People.Add(new Person() {Id = id, Name = name, Surname = surname, Age = age });
        return id;
    }

    public List<Person> GetAllPeople()
    {
        return People;
    }
    public void DisplayAllPeople()
    {
        Console.WriteLine("----------------------------------");
        foreach (var person in People)
        {
            Console.WriteLine("Id: " + person.Id);
            Console.WriteLine("Imię: " + person.Name);
            Console.WriteLine("Nazwisko: " + person.Surname);
            Console.WriteLine("Wiek: " + person.Age);
            Console.WriteLine("----------------------------------");
        }
    }
}
```

Pamiętajmy, że definicja (i implementacja) każdej klasy powinna się znaleźć w osobnym pliku (jest to dobra praktyka ułatwiająca nawigowanie po kodzie).
### Metody typu `void`
Wróćmy jeszcze na chwilę do metod typu `void`. Są to metody, które nic nie zwracają. Wykonują one operacje wyłącznie wewnątrz klasy, na składnikach obiektu, których rezultat działania właściwie nas nie interesuje. Rezultatu działanie takich metod nie będziemy w dalszych etapach używać.

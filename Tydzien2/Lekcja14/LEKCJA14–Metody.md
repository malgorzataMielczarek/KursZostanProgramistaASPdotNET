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
### Wywoływanie metod
Po utworzeniu obiektu danej klasy, możemy swobodnie korzystać w kodzie programu ze wszystkich jego metod publicznych (z modyfikatorem dostępu `public`). Korzystamy z nich tak jak robiliśmy to ze wszystkimi metodami do tej pory. Podajemy ścieżką dostępu, nazwę metody i jej parametry (argumenty), oddzielone przecinkami, ujęte w nawias okrągły. Ścieżką dostępu będzie w tym wypadku utworzony przez nas obiekt (czyli po prostu nazwa utworzonego przez nas obiektu danej klasy). Użyjmy więc dla przykładu metod klasy `PersonService`. Po utworzeniu obiektu tej klasy, najpierw dopiszemy kilka osób do listy, następnie wyświetlimy wpisane dane, a na koniec policzymy średnią wieku osób z listy.
 
```csharp
PersonService people = new PersonService();

people.AddNewPerson("Jan", "Kowalski", 33);
people.AddNewPerson("Piotr", "Nowak", 88);
people.AddNewPerson("Agnieszka", "Luźna", 45);
people.AddNewPerson("Jolanta", "Piątek", 20);
people.AddNewPerson("Max", "Pierwszy", 15);

people.DisplayAllPeople();

List<Person> peopleList = people.GetAllPeople();
int sumAge = 0;
foreach (Person person in peopleList)
{
    sumAge += person.Age;
}
decimal average = (decimal)sumAge / (decimal)peopleList.Count;
Console.WriteLine("Średnia wieku: " + average);
```

### Metody i klasy statyczne
Jak już wspominaliśmy tworzone przez nas metody mogą być statyczne (dopisujemy słowo kluczowe `static` pomiędzy modyfikator dostępu a zwracany typ). Oznacza to, że istnieje jeden egzemplarz takiej metody (niezależnie ile obiektów danej klasy utworzymy) i można jej używać, nawet gdy żaden obiekt tej klasy nie istnieje. Taka metoda jest wywoływana nie na obiekcie, a na klasie (ścieżką dostępu będzie nazwa klasy, a nie tak jak w pozostałych przypadkach nazwa obiektu). W związku z tym nie można w takiej metodzie modyfikować żadnych właściwości klasy, gdyż nie ma ona do nich dostępu (chyba, że są to właściwości statyczne). Metoda statyczna jest niezależna od obiektów danej klasy. Właśnie z tych względów metod statycznych prawie nigdy nie stosuje się w klasach serwisowych. Najczęściej używa się ich do zapisania wielokrotnie wykonywanych obliczeń, walidacji itp. i umieszcza w specjalnej pomocniczej klasie statycznej. Na przykład możemy używać ich do przeliczeń jednostek. Załóżmy więc, że chcielibyśmy wiek osób zapisywać w miesiącach, a nie w latach. Wówczas moglibyśmy stworzyć metodę statyczną:

```csharp =
public static int YearsToMonths(int years)
{
    return years * 12;
}
```

Oczywiście jest to przeliczenie wieku w miesiącach zaokrąglonego do pełnych lat i takie jego zapisywanie niema zbyt wielkiego sensu. Moglibyśmy sobie jednak wyobrazić podobną metodę przeliczającą aktualny wiek na podstawie daty urodzenia. Moglibyśmy oczywiście umieścić tą metodę w klasie `PersonService` lub `Person` (wówczas jej wywołanie wyglądałoby np. `PersonService.YearsToMonths(45);` lub `Person.YearsToMonths(58);`). Najprawdopodobniej jednak umieścilibyśmy ją w osobnej klasie statycznej przechowującej różne przeliczenia tego typu, gdyż mogą one dotyczyć nie tylko wieku osób, ale też np. częstotliwości raportowania itd.

**Klasy statyczne** są to klasy których wszystkie składniki (zarówno właściwości jak i metody) muszą być statyczne. Klasy statyczne tworzymy dodając słowo kluczowe `static` pomiędzy modyfikator dostępu (np. `public`) a słowo kluczowe `class`. Nie można tworzyć obiektów klas statycznych.

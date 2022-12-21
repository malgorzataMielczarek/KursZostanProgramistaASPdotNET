# [LEKCJA 8 – Interfejsy](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-8-interfejsy/)
Typ w C# stosowany, podobnie jak klasa abstrakcyjna, do tworzenia szkieletu elementów. Interfejs jest to zbiór deklaracji związanych ze sobą funkcjonalności. Składa się wyłącznie z deklaracji właściwości, metod, indeksatorów i zdarzeń (od C# 8.0 interfejs może zawierać również implementacje metod, choć na razie stosowanie tego nie jest zbyt popularne). Nie może jednak zawierać pól. Interfejs może dziedziczyć jeden lub więcej interfejsów. 

## Tworzenie interfejsu
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

### Nazwa interfejsu
Jak już wspominaliśmy w 1. tygodniu przyjęło się, że nazwy interfejsów rozpoczynamy wielką literą "i" ("I"). Należy to do dobrych praktyk.

### Składniki interfejsu

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

### Przykład
Napiszmy przykładowy interfejs deklarujący podstawowe właściwości do operacji na plikach (przykłady użyte w tej lekcji pochodzą ze strony [tutorialsteacher.com](https://www.tutorialsteacher.com/csharp/csharp-interface))

```csharp =
public interface IFile
{
    void ReadFile();
    void WriteFile(string text);
}
```

## Implementacja interfejsu
Jak już wiemy, klasa może dziedziczyć tylko po jednej klasie. Może ona jednak implementować wiele interfejsów. Klasa implementująca interfejs musi zaimplementować wszystkie jego składniki (również właściwości). Czyli, w odróżnieniu od dziedziczenia, gdzie klasa pochodna jedynie rozszerzała i nadpisywała klasę bazową, implementując interfejs, musimy w klasie umieścić wszystkie jego składniki (począwszy od C# 8.0 możemy pominąć zaimplementowane w interfejsie metody). Implementacji dokonujemy analogicznie jak to robiliśmy w przypadku dziedziczenia klas, czyli po nazwie klasy (struktury) wstawiamy dwukropek i podajemy nazwę interfejsu (interfejsów - w tym wypadku rozdzielone przecinkami).

```csharp =
ModyfikatorDostepu class NazwaKlasy : NazwaInterfejsu
{
    //cialo klasy
    //implementacja interfejsu
    //ewentualnie dodatkowe elementy
}
```

Implementacja interfejsu może odbywać się w sposób jawny lub niejawny.

### Implementacja niejawna interfejsu
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

### Implementacja jawna interfejsu
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
        //file1.Search("text to be searched")   //bład kompilacji
        
        file2.Search("text to be searched");
        //file2.ReadFile();     //blad kompilacji
        //file2.WriteFile("content");   //blad kompilacji
    }
}
```

W przypadku implementacji jawnej zmienna typu interfejsu może uzyskać dostęp wyłącznie do składników interfejsu, a zmienna klasy do składników klasy (elementy zaimplementowane jawnie są składnikami interfejsu, ale nie klasy).

### Implementacja kilku interfejsów
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
        //file1.OpenBinaryFile();     //blad kompilacji
        //file1.SearchFile("text to be searched");     //blad kompilacji
        
        file2.OpenBinaryFile();
        file2.ReadFile();
        //file2.SearchFile("text to be searched");     //blad kompilacji
    
        file3.Search("text to be searched");
        //file3.ReadFile();     //blad kompilacji
        //file3.OpenBinaryFile();     //blad kompilacji
    }
}
```

## Interfejs vs. klasa abstrakcyjna

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

## Zastosowanie
Interfejsy są szeroko stosowane w aplikacjach webowych, są podstawą ich architektury. Więcej na ten temat dowiemy się w dalszej części kursu. Warto jednak pamiętać, że bez interfejsów nie powstaje obecnie żadna aplikacja internetowa. Są niezbędne do zachowania wielu zasad nowoczesnego programowania.

# [LEKCJA 5 – Manipulacje plikami](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-5-praca-z-danymi/lekcja-5-manipulacje-plikami/)
Aplikacje często korzystają z plików zewnętrznych. Pobieramy z nich dodatkowe informacje potrzebne do działania programu albo generujemy pliki zewnętrzne np. raporty, które będziemy chcieli potem rozesłać. Dlatego ważne jest, aby nauczyć się poprawnej pracy z plikami. Poznajmy więc podstawy odczytu i zapisu informacji do plików.
## Przykładowy plik
Zrobimy to na podstawie pliku CSV (_Comma Separated Value_), czyli formatu pliku w którym przechowywane wartości oddzielone są od siebie przecinkami, a w jednej linii umieszcza się informacje dotyczące tylko jednego elementu. W naszym pliku będziemy przechowywać informacje potrzebne do stworzenia kilku obiektów typu `Item`.
### Klasa `Item`
Dla przypomnienia klasa Item wyglądała następująco:
```csharp =
using System;

namespace Warehouse.Domain.Entity
{
    public class Item
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public int Quantity { get; set; }
        public int TypeId { get; set; }
        public DateTime CreateDate { get; set; }
        public string CreatedBy { get; set; }

        public Item(int id, string name, int typeId)
        {
            Id = id;
            Name = name;
            Quantity = 0;
            TypeId = typeId;
            CreateDate = DateTime.Now;
            CreatedBy = "Me";
        }
    }
}
```
### Plik CSV
Nasz przykładowy plik nazwiemy _items.csv_ i umieścimy w folderze _Temp_ na dysku _C_. Pełna ścieżka do pliku będzie więc wyglądać następująco: _C:\Temp\items.csv_. Przykładowa treść pliku:
```
1,Apple,2
2,Strawberry,2
3,Pineapple,2
1,TShirt,3
2,Jeans,3
1,Laptop,1
2,Screen,1
```
## Strumień pliku
### Instrukcja _using_
Instrukcja, która pomaga w zarządzaniu pamięcią naszego programu. Zapewnia ona poprawne użycie obiektów `IDisposable`. W obrębie instrukcji `using` taki obiekt jest tylko do odczytu. `IDisposable` jest interfejsem udostępniającym mechanizmy zwalniania niezarządzanych zasobów, czyli takich których usuwanie nie odbywa się automatycznie. Przykładem klasy implementującej ten interfejs jest klasa `Stream` (strumień), czyli abstrakcyjna klasa zajmująca się obsługą strumieni danych. Jedną z jej klas pochodnych jest natomiast klasa `FileStream`, której zaraz użyjemy.

Tradycyjnie instrukcja `using` składa się ze słowa kluczowego `using` po którym w nawiasie okrągłym umieszczamy obiekt `IDisposable`, którego będziemy używać. Na końcu znajduje się umieszczony w klamrach blok instrukcji wykonywanych na tym obiekcie. Po zakończeniu ich wykonywania odpowiednia informacja trafia do _garbage collector_ i obiekt ten zostaje usunięty z pamięci programu.

Począwszy od C# 8.0 możliwy jest uproszczony zapis instrukcji `using`, bez nawiasów. Składa się on wówczas jedynie ze słowa kluczowego `using` i obiektu `IDisposable`. Na końcu instrukcji stawiamy tu oczywiście średnik. W tym wypadku blok instrukcji związanych z tą instrukcją `using` kończy się wraz z końcem bloku zawierającego instrukcję `using` (czyli najczęściej metody). Tam też właśnie zostaje automatycznie wywołana metoda `Dispose` obiektu `IDisposable`, odpowiedzialna za zwolnienie zajmowanej przez niego pamięci.

### `FileStream`
Jak już wspominaliśmy przykładową klasą implementującą interfejs `IDisposable` jest klasa `FileStream`. Zajmuje się ona obsługą strumienia danych związanego z plikami zewnętrznymi. Kiedy nasz program uzyskuje dostęp do jakiegoś pliku znajdującego się na dysku, taki dostęp jest równocześnie zablokowany dla wszystkich innych. To znaczy, że jeżeli otworzymy w naszej aplikacji jakiś plik, to nie będziemy mogli tego pliku otworzyć i modyfikować w żadnym innym programie. Ponadto mogą to być bardzo duże pliki, a co za tym idzie, kiedy wczytamy je do programu, zajmą one dużo jego pamięci. Ze względu na te dwa powody, bardzo ważne jest aby kontrolować jak używamy plików w programie (kiedy dokładnie powinniśmy je otworzyć, a kiedy zamknąć i zwolnić pamięć). Klasa `FileStream`, jak każda klasa dziedzicząca po klasie `Stream` zawiera dane w postaci sekwencji bajtów. Odczytując dane z plików będziemy więc je uzyskiwać właśnie w takiej formie. Aby były one użyteczne w naszym programie będziemy musieli je najpierw zdekodować, a następnie odpowiednio obrobić.
### Przykład
Zobaczmy więc jak wygląda instrukcja `using` i działa strumień plików na przykładzie prostego odczytu danych z naszego pliku _items.csv_.

#### Stary zapis instrukcji using
```csharp =
public void Method()
{
    using(FileStream fs = File.Open(@"C:\Temp\items.csv", FileMode.Open, FileAccess.Read))
    {
        byte[] buf = new byte[1024];
        int c;
        while ((c = fs.Read(buf, 0, buf.Length)) > 0)
        {
            string text = Encoding.UTF8.GetString(buf, 0, c);

            // Instrukcje do obrobki danych
        }

        // Tu zostanie wywolana metoda fs.Dispose();
    }

    // Ewentualne inne instrukcje
}
```
#### Skrócony zapis instrukcji using
```csharp =
public void Method()
{
    using FileStream fs = File.Open(@"C:\Temp\items.csv", FileMode.Open, FileAccess.Read);

    byte[] buf = new byte[1024];

    int c;
    while ((c = fs.Read(buf, 0, buf.Length)) > 0)
    {
        string text = Encoding.UTF8.GetString(buf, 0, c);

        // Instrukcje do obrobki danych
    }

    // Ewentualne inne instrukcje

    // Tu zostanie wywolana metoda fs.Dispose();
}
```

#### Wykonane czynności
1. Utworzenie strumienia pliku</br>
Pracę z plikiem rozpoczynamy od utworzenia związanego z nim strumienia danych.
2. Deklaracja bufora do odczytu danych</br>
Następnie deklarujemy bufor, w którym będziemy zapisywać porcje danych pobrane ze strumienia. Ponieważ pliki mogą być bardzo duże, wiec nie chcemy pobierać ich w całości do pamięci programu. Lepiej zrobić to mniejszymi porcjami. Ponieważ nasz plik jest krótki, wiec zadeklarowany bufor pomieści cały plik naraz, jednak nie zawsze będziemy mieć taką sytuację.
3. Deklaracja licznika odczytanych bajtów</br>
Deklarujemy również zmienną `c`, w której będziemy zapisywać ile danych faktycznie pobraliśmy ze strumienia. Jest to ważne, ponieważ, jeżeli w strumieniu pozostało mniej danych niż próbujemy pobrać, to ta wartość pomoże nam ustalić do którego miejsca bufora znajdują się nowe dane. Pozwoli nam ona również ustalić kiedy skończy się plik, gdyż ilość pobranych bajtów wyniesie wówczas `0`.
4. Pobieranie danych</br>
Kiedy mamy już otwarty strumień i zadeklarowane odpowiednie zmienne, możemy przejść do pobierania danych. Odczytane ze strumienia dane zapisujemy do bufora (od początku do końca bufora), dopóki nie dojdziemy do końca pliku (niema już czego pobierać, `c == 0`).
5. Dekodowanie danych
Każdą porcję pobranych bajtów, musimy zdekodować na bardziej użyteczny dla nas format. Używając odpowiedniego kodowania (najczęściej UTF-8) przekształcamy strumień bajtów na `string`.
6. Obróbka danych</br>
W końcu możemy przejść do obróbki pobranej przez nas porcji danych. Prawdopodobnie wykorzystamy jakieś wyrażenie regularne do wyszukania końca linii i podzielenia otrzymanego `string`a na elementy. Następnie zapewne użyjemy kolejnych wyrażeń regularnych do wyszukania przecinków w każdym elemencie i uzyskania pojedynczych wartości. Na koniec będziemy musieli sparsować dane z typu `string` na typ odpowiedniej właściwości (pola) i utworzyć nasze obiekty `Item`.
#### Użyte klasy i metody
* **`File`** - klasa związana z obsługą plików (otwieranie, zmykanie itd.).<br/>
* **`File.Open(string path, FileMode mode, FileAccess access, FileShare share)`** - metoda klasy `File` przyjmująca dwa (`path`, `mode`), trzy (`path`, `mode`, `access`) lub cztery (`path`, `mode`, `access`, `share`) argumenty, otwierająca plik, blokująca dostęp do niego dla innych programów i zwracająca obiekt typu `FileStream`.
    * `path` - `string` przedstawiający ścieżkę do pliku, który chcemy otworzyć.
    * `mode` - tryb otwieranego pliku, tzn. czy chcemy stworzyć nowy plik, otworzyć już istniejący, nadpisać plik lub otworzyć plik ustawiając kursor na jego końcu.
    * `access` - jaki dostęp do pliku chcemy uzyskać, tzn., czy chcemy z niego tylko odczytywać informacje, tylko zapisywać, a może jedno z drugim.
    * `share` - jakiego dostępu do plików chcemy udzielić wątkom pochodnym (odczyt, zapis, oba, usuwanie, żaden, dziedziczony).

* **`fs.Read(byte[] array, int offset, int count)`** - metoda klasy `FileStream` odczytująca `count` bajtów danych ze strumienia i zastępująca nimi dane z tablicy `array` począwszy od indeksu `offset`. Metoda zwraca liczbę rzeczywiście odczytanych bajtów.
* **`Encoding.UTF8.GetString(byte[] bytes, int index, int count)`** - metoda klasy `System.Text.Encoding`, zaimplementowana przez klasę `System.Text.UTF8Encoding`, dekodująca sekwencję bajtów do typu `string`. Korzystając z właściwości `Encoding.UTF8` uzyskujemy dostęp do obiektu klasy `UTF8Encoding`, implementującej metodę `GetString`, wykorzystując do dekodowania kodowanie UTF-8. Metoda zwraca `string` zawierający rezultat dekodowania podanej sekwencji bajtów. Występuje w kilku wariantach (może przyjmować różne argumenty). W zastosowanym, podajemy kolejno:
    * `bytes` - tablicę bajtów, zawierającą sekwencję bajtów, która ma zostać zdekodowana,
    * `index` - numer indeksu pierwszego bajtu do zdekodowania,
    * `count` - liczbę bajtów do zdekodowania.

### `FileMode`
Enum określający jak system operacyjny powinien otworzyć plik.
```csharp =
namespace System.IO
{
    [Serializable]
    [ComVisible(true)]
    public enum FileMode
    {
        CreateNew = 1,
        Create,
        Open,
        OpenOrCreate,
        Truncate,
        Append
    }
}
```
#### 1. `CreateNew`
Oznacza, że system operacyjny powinien stworzyć nowy plik. Jeżeli plik już istnieje, to zostaje wyrzucony wyjątek `System.IO.IOException`. Użycie tej opcji wymaga uprawnienia do zapisu (`System.Security.Permissions.FileIOPermissionAccess.Write`).
#### 2. `Create`
Oznacza, że system operacyjny powinien stworzyć nowy plik. Jeżeli natomiast plik już istnieje, to zostanie on nadpisany. Użycie tej opcji również wymaga uprawnienia do zapisu (`System.Security.Permissions.FileIOPermissionAccess.Write`). Wybór tej opcji jest równoznaczny ze stwierdzeniem: jeżeli plik jeszcze nie istnieje to wybierz opcję `CreateNew`, w przeciwnym razie wybierz opcję `Truncate`. Jeżeli plik już istnieje, ale jest to plik ukryty zostaje wyrzucony wyjątek `System.UnauthorizedAccessException`, o nieautoryzowanym dostępie.
#### 3. `Open`
Oznacza, że system operacyjny powinien otworzyć istniejący plik. Jeżeli plik nie istnieje, zostaje wyrzucony wyjątek `System.IO.FileNotFoundException`. To jaki dostęp mamy do otwartego pliku (odczyt, zapis, oba) określa enum `System.IO.FileAccess`.
#### 4. `OpenOrCreate`
Oznacza, że jeżeli plik istnieje, system operacyjny powinien go otworzyć, w przeciwnym razie powinien stworzyć nowy plik. Jeżeli otwieramy plik do odczytu (`FileAccess.Read`), to jest wymagane uprawnienie do odczytu (`System.Security.Permissions.FileIOPermissionAccess.Read`). Jeżeli otwieramy plik do zapisu (`FileAccess.Write`), to wymagane jest uprawnienie do zapisu (`System.Security.Permissions.FileIOPermissionAccess.Write`). Gdy natomiast otwieramy plik do odczytu i zapisu (`FileAccess.ReadWrite`), to oba uprawnienia są wymagane (`System.Security.Permissions.FileIOPermissionAccess.Read` i `System.Security.Permissions.FileIOPermissionAccess.Write`).
#### 5. `Truncate`
Oznacza, że system operacyjny powinien otworzyć istniejący plik, a następnie wyczyścić całą jego zawartość, tak, aby jego rozmiar wynosił zero bajtów. Użycie tej opcji wymaga uprawnienia do zapisu (`System.Security.Permissions.FileIOPermissionAccess.Write`). Próba odczytu z pliku otwartego z opcją `Truncate` wywołuje wyjątek `System.ArgumentException`.
#### 6. `Append`
Oznacza, że system operacyjny powinien otworzyć plik, jeśli istnieje, i wyszukać jego koniec, lub stworzyć nowy plik. Wybór tej opcji wymaga pozwolenia do dołączania (`System.Security.Permissions.FileIOPermissionAccess.Append`). Może być ona stosowana wyłącznie w połączeniu z modyfikatorem dostępu do zapisu (`FileAccess.Write`). Po wybraniu tej opcji, próba wyszukania pozycji przed końcem pliku skutkuje wyjątkiem `System.IO.IOException`, natomiast próba odczytu z pliku wyjątkiem `System.NotSupportedException`.

### `FileAccess`
Enum określający tryb dostępu do pliku.
```csharp
namespace System.IO
{
    [Serializable]
    [Flags]
    [ComVisible(true)]
    public enum FileAccess
    {
        Read = 0x1,
        Write = 0x2,
        ReadWrite = 0x3
    }
}
```

#### 1. `Read`
Oznacza dostęp do pliku w trybie odczytu. Dane mogą być więc odczytywane z pliku. Można zastosować go w połączeniu z trybem `Write`, aby umożliwić zarówno odczyt jaki zapis informacji do pliku.
#### 2. `Write`
Oznacza dostęp do pliku w trybie zapisu. Dane mogą być zapisywane do pliku. Można zastosować go w połączeniu z trybem `Read`, aby umożliwić zarówno zapis, jak i odczyt informacji z pliku.
#### 3. `ReadWrite`
Oznacza dostęp do pliku w trybie odczytu i zapisu. Dane mogą być zarówno zapisywane jak i odczytywane z pliku.
### `FileShare`
Enum zawierający stałe określające jaki dostęp mogą mieć inne obiekty `System.IO.FileStream` do tego samego pliku.
```csharp =
namespace System.IO
{
    [Serializable]
    [Flags]
    [ComVisible(true)]
    public enum FileShare
    {
        None = 0x0,
        Read = 0x1,
        Write = 0x2,
        ReadWrite = 0x3,
        Delete = 0x4,
        Inheritable = 0x10
    }
}
```

#### 0. `None`
Odmawia dzielenia się dostępem do tego pliku. Jakiekolwiek próby otwarcia tego pliku przez ten lub inny proces nie powiodą się, dopóki plik nie zostanie zamknięty.
#### 1. `Read`
Pozwala na dalsze otwieranie pliku do odczytu. Jeśli ten znacznik nie jest określony, to wszelkie próby otwarcia pliku do odczytu przez ten lub inny proces nie powiodą się, dopóki plik nie zostanie zamknięty. Jednakże, nawet jeżeli ta flaga jest określona, to do otwarcia pliku wciąż mogą być potrzebne dodatkowe pozwolenia.
#### 2. `Write`
Pozwala na dalsze otwieranie pliku do zapisu. Jeśli ten znacznik nie jest określony, to wszelkie próby otwarcia pliku do zapisu przez ten lub inny proces nie powiodą się, dopóki plik nie zostanie zamknięty. Jednakże, nawet jeżeli ta flaga jest określona, to do otwarcia pliku wciąż mogą być potrzebne dodatkowe pozwolenia.
#### 3. `ReadWrite`
Pozwala na dalsze otwieranie pliku do odczytu lub zapisu. Jeśli ten znacznik nie jest określony, to wszelkie próby otwarcia pliku do odczytu lub zapisu przez ten lub inny proces nie powiodą się, dopóki plik nie zostanie zamknięty. Jednakże, nawet jeżeli ta flaga jest określona, to do otwarcia pliku wciąż mogą być potrzebne dodatkowe pozwolenia.
#### 4. `Delete`
Pozwala na późniejsze usunięcie pliku.
#### 16. `Inheritable`
Sprawia, że deskryptor pliku (ang. _file handle_) jest dziedziczony przez procesy potomne. Nie jest to bezpośrednio obsługiwane przez Win32.

## Odczyt (_Read_) danych z pliku
Pierwszą metodę do odczytu poznaliśmy w przykładzie wyżej. Omówmy ją sobie dokładniej.
### Metoda `Read` klasy `FileStream`
```csharp
public override int Read([In][Out] byte[] array, int offset, int count)
```
Metoda odczytuje blok bajtów ze strumienia i zapisuje dane w danym buforze.
#### Parametry:
1. `array` - gdy metoda wraca, parametr zawiera określoną tablicę bajtów z wartościami pomiędzy `offset` i `(offset + count - 1)` zastąpionymi przez bajty odczytane ze strumienia. Jeżeli potok zawierał mniej bajtów niż `count`, zamienione zostanie jedynie tyle wartości, ile zostało odczytanych (czyli między `offset` i `(offset + count - 1)` mogą znajdować się również stare wartości).
2. `offset` - przesunięcie bajtów w tablicy, w którym zostaną umieszczone odczytane bajty (numer indeksu od którego wartości zaczną być podmieniane).
3. `count` - maksymalna liczba bajtów do odczytu (ile bajtów chcemy odczytać). Zostanie odczytane `count` bajtów lub tyle ile zostało, jeżeli zostało mniej niż `count`.
#### Zwraca:
Metoda zwraca całkowitą liczbę bajtów wczytanych do bufora. Może to być mniej niż chcieliśmy, jeżeli taka liczba bajtów nie jest aktualnie dostępna, lub zero, jeżeli dotarliśmy do końca pliku.
#### Wyjątki:
1. `System.ArgumentNullException` - jeśli `array` jest `null`.
2. `System.ArgumentOutOfRangeException` - jeśli `offset` lub `count` są ujemne.
3. `System.NotSupportedException` - jeśli strumień danych nie wspiera odczytu danych.
4. `System.IO.IOException` - jeśli wystąpił błąd wejścia/wyjścia.
5. `System.ArgumentException` - jeśli `offset` i `count` opisują nieprawidłowy zakres w tablicy.
6. `System.ObjectDisposedException` - jeśli metoda była wywołana po zamknięciu strumienia (pliku).

### Metoda `ReadLines` klasy `File`
Kolejną metodę do odczytu danych z pliku dostarcza nam bezpośrednio klasa `System.IO.File`. W odróżnieniu od poprzedniej nie wymaga ona utworzenia strumienia pliku. Ma ona również wewnętrznie zaszyte dekodowanie i podział pobranych danych na linie. Występuje w dwóch wariantach.

Jeżeli używamy kodowania UTF-8, to możemy użyć wersji, przyjmującej tylko jeden argument - ścieżkę do pliku.

```csharp =
public static System.Collections.Generic.IEnumerable<string> ReadLines (string path);
```

Jeżeli używamy innego kodowania, to musimy je również podać, stosując metodę:
```csharp =
public static System.Collections.Generic.IEnumerable<string> ReadLines (string path, System.Text.Encoding encoding);
```

#### Parametry:
1. `path` - ścieżka do pliku, który ma zostać odczytany.
2. `[encoding = System.Text.Encoding.UTF8]` - kodowanie, które zostało zastosowane w odczytywanym pliku.
#### Zwraca:
Metoda zwraca `IEnumerable<String>`, w związku z tym można ją stosować w połączeniu z LINQ. Zastosowana samodzielnie zawiera kolekcję wszystkich linii znajdujących się w podanym pliku. Zastosowana wewnątrz zapytania LINQ, zwraca tylko te linie, które są wynikiem zapytania. Przy czym, można rozpocząć wyliczanie, przed zwróceniem całej kolekcji. Od razu można więc przefiltrować zawartość pliku, tak, aby wyciągnąć z niego tylko interesujące nas linie tekstu.
#### Wyjątki:
1. `ArgumentException` - w .NET Framework i .NET Core w wersjach starszych niż 2.1: jeżeli `path` ma długość `0`, składa się wyłącznie z białych znaków lub zawieraj przynajmniej jeden nieprawidłowy znak (jeden ze znaków zdefiniowanych przez metodę `GetInvalidPathChars()`).
2. `ArgumentNullException` - jeśli `path` jest `null`.
3. `DirectoryNotFoundException` - jeśli podana ścieżka jest nieprawidłowa (np. prowadzi do niezmapowanego dysku).
4. `FileNotFoundException` - jeśli plik wskazany przez ścieżkę `path` nie został znaleziony.
5. `IOException` - jeśli wystąpił błąd I/O (wejścia/wyjścia), podczas otwierania pliku.
6. `PathTooLongException` - jeśli `path` jest dłuższe niż zdefiniowana przez system maksymalna długość.
7. `SecurityException` - jeśli użytkownik nie posiada wymaganych uprawnień.
8. `UnauthorizedAccessException` - jeśli wskazany plik jest _read-only_ lub ta operacja nie jest wspierana przez używaną platformę, lub `path` wskazuje na folder, a nie plik, lub użytkownik nie ma wymaganych uprawnień.
#### Przykład:
Załóżmy, że z naszego pliku chcemy pobrać wyłącznie informacje dotyczące produktów spożywczych. Wówczas moglibyśmy napisać:
```csharp =
public void Method()
{
    var lines = File.ReadLines(@"C:\Temp\items.csv").Where(line => line.EndsWith(",2"));
    // Dalsza obrobka danych
}
```
#### Zastosowanie:
1. Wykonywanie zapytań LINQ (tzw. typu _LINQ to Objects_ (LINQ do obiektów), czyli wykonywanych bezpośrednio na kolekcjach `IEnumerable` lub `IEnumerable<T>`) do otrzymania z pliku przefiltrowanego zestawu linii.
2. Do zapisu otrzymanej kolekcji linii do pliku przy pomocy metody `File.WriteAllLines(String, IEnumerable<String>) ` lub dodania jej na końcu pliku, przy użyciu metody `File.AppendAllLines(String, IEnumerable<String>)`.
3. Do stworzenia natychmiast wypełnionej instancji kolekcji, która posiada konstruktor, przyjmujący jako argument kolekcję `IEnumerable<string>`, np. `IList<string>` lub `Queue<string>`.
### Metoda `ReadAllLines` klasy `File`
Klasy `System.IO.File` dostarcza nam jeszcze jedną metodę do odczytu linii tekstu z pliku. Otwiera ona podany plik, odczytuje z niego wszystkie linie tekstu do tablicy `string`ów i zamyka plik. Podobnie jak metoda `ReadLines` występuje ona w dwóch wersjach:
```csharp =
public static string[] ReadAllLines (string path);
```
i
```csharp =
public static string[] ReadAllLines (string path, System.Text.Encoding encoding);
```

#### Porównanie metody `ReadAllLines` z metodą `ReadLines`
| Cecha | `ReadAllLines` | `ReadLines` |
| :--- | :---: | :---: |
| Parametry | analogiczne | analogiczne |
| Zwracany typ | tablica (`string[]`) | typ wyliczeniowy (`IEnumerable<string>`) |
| Zwracane linie pliku | wszystkie | wszystkie lub wybrane |
| Wyjątki | analogiczne | analogiczne |
| Użycie w zapytaniach LINQ | nie | tak |
| Możliwość filtrowanie | dopiero po odczycie całości | podczas odczytu |
| Domyślne kodowanie | próbuje automatycznie wykryć kodowanie pliku, może wykryć kodowanie UTF-8 i UTF-32 | UTF-8 |
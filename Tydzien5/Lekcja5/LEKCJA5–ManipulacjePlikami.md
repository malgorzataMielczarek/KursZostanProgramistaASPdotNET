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

## Odczyt danych z pliku
Pierwszą metodę do odczytu poznaliśmy w przykładzie wyżej. Omówmy ją sobie dokładniej.
### Metoda `Read` klasy `FileStream`
```csharp
public override int Read (byte[] buffer, int offset, int count);
```
Metoda odczytuje blok bajtów ze strumienia i zapisuje dane w danym buforze.
#### Parametry:
1. `buffer` - gdy metoda wraca, parametr zawiera określoną tablicę bajtów z wartościami pomiędzy `offset` i `(offset + count - 1)` zastąpionymi przez bajty odczytane ze strumienia. Jeżeli potok zawierał mniej bajtów niż `count`, zamienione zostanie jedynie tyle wartości, ile zostało odczytanych (czyli między `offset` i `(offset + count - 1)` mogą znajdować się również stare wartości).
2. `offset` - przesunięcie bajtów w tablicy, w którym zostaną umieszczone odczytane bajty (numer indeksu od którego wartości zaczną być podmieniane).
3. `count` - maksymalna liczba bajtów do odczytu (ile bajtów chcemy odczytać). Zostanie odczytane `count` bajtów lub tyle ile zostało, jeżeli zostało mniej niż `count`.
#### Zwraca:
Metoda zwraca całkowitą liczbę bajtów wczytanych do bufora. Może to być mniej niż chcieliśmy, jeżeli taka liczba bajtów nie jest aktualnie dostępna, lub zero, jeżeli dotarliśmy do końca pliku.
#### Wyjątki:
1. `System.ArgumentNullException` - jeśli `buffer` jest `null`.
2. `System.ArgumentOutOfRangeException` - jeśli `offset` lub `count` są ujemne.
3. `System.NotSupportedException` - jeśli strumień danych nie wspiera odczytu danych.
4. `System.IO.IOException` - jeśli wystąpił błąd wejścia/wyjścia.
5. `System.ArgumentException` - jeśli `offset` i `count` opisują nieprawidłowy zakres w tablicy.
6. `System.ObjectDisposedException` - jeśli metoda była wywołana po zamknięciu strumienia (pliku).

### Klasa `StreamReader`
Aby ułatwić sobie nieco zapis możemy skorzystać z klasy `System.IO.StreamReader`, implementującej klasę `System.IO.TextReader`, która odczytuje znaki za strumienia bajtów, używając odpowiedniego kodowania. Domyślnie jest to kodowanie UTF-8, o ile nie podano inaczej. Klasa implementuje interfejs `IDisposable`, będziemy więc tworzyć jej obiekty w połączeniu z instrukcją `using`. Do utworzenia obiektu tej klasy mamy do dyspozycji np. konstruktor przyjmujący jako argument strumień danych, ale także taki, przyjmujący ścieżkę do pliku:
```csharp =
using FileStream fs = File.Open(@"C:\Temp\items.csv", FileMode.Open, FileAccess.Read);
using StreamReader sr = new StreamReader(fs);
```
lub krócej
```csharp =
using StreamReader sr = new StreamReader(@"C:\Temp\items.csv");
```
Można go również utworzyć wywołując metodę `OpenText` na obiekcie klasy `FileInfo`:
```csharp =
FileInfo file = new FileInfo(@"C:\Temp\items.csv");
using StreamReader sr = file.OpenText();
```
lub jej statycznego odpowiednika klasy `File`
```csharp =
using StreamReader sr = File.OpenText(@"C:\Temp\items.csv");
```
Klasa posiada różne metody do odczytu tekstu ze strumienia bajtów.
#### `Read`
Podobnie jak klasa `FileStream`, posiada ona metodę `Read`. Może ona również występować w bardzo zbliżonej formie:
```csharp =
public override int Read (char[] buffer, int index, int count);
```
Zachowuje się ona analogicznie do przedstawionej wyżej metody, z tą różnicą, że dane zwracane do bufora są już zdekodowane i są typu `char`, a nie `byte`.

Powyższe przeciążenie występuje również pod nazwą `ReadBlock`:
```csharp =
public override int ReadBlock (char[] buffer, int index, int count);
```

Metoda `Read` ma również inne przeciążenia, np. bezargumentowe:
```csharp =
public override int Read ();
```
W tym wypadku metoda zwraca następny znak ze strumienia danych, przedstawiony jako `int` lub `-1`, jeżeli w strumieniu niema więcej znaków.
#### `Peek`
Inną ciekawą metodą jest metoda `Peek`:
```csharp =
public override int Peek ();
```
Zwraca ona wartości analogiczne do bezargumentowej metody `Read`. W odróżnieniu od niej nie przesuwa ona pozycji strumienia, czyli wywołując wielokrotnie metodę `Peek` lub wywołując metodę `Peek`, a następnie `Read` otrzymamy w rezultacie tą samą wartość.
#### `ReadLine`
Kolejną metodą jest `ReadLine`, która umożliwia pobieranie z pliku całych linii tekstu. Dzięki przedstawionym powyżej metodą klasy `StreamReader` zaoszczędziliśmy sobie pisanie implementacji dekodowania ciągu bajtów. Możemy jednak pójść jeszcze krok dalej. W późniejszej obróbce uzyskanych danych będziemy zapewne chcieli umieścić każdą linię tekstu z naszego pliku w osobnym `string`u, aby uzyskać informację dotyczącą jednego produktu. Funkcja `ReadLine` zrobi to już za nas.
```csharp =
public override string? ReadLine ();
```
Metoda zwraca następną linię ze strumienia wejściowego (pliku) lub `null`, gdy doszliśmy do końca pliku (lub innego używanego strumienia danych).
##### Przykład:
```csharp =
public void Method()
{
    using StreamReader sr = new StreamReader(@"C:\Temp\items.csv");

    string? itemInfo;
    while ((itemInfo = sr.ReadLine()) != null)
    {
        // stworzenie nowego obiektu Item przy uzyciu danych zawartych w itemInfo
        // dodanie obiektu do kolekcji
    }
}
```

### Metody klasy `File`
Kolejne metody do odczytu danych z pliku dostarcza nam bezpośrednio klasa `System.IO.File`. W odróżnieniu od poprzednich nie wymagają ona jawnego użycia strumienia. Podobnie jak metoda powyżej, mają one wewnętrznie zaszyte dekodowanie i, w przypadku dwóch pierwszych metod, podział pobranych danych na linie. Wszystkie wymienione niżej metody występują w dwóch wariantach. Jeżeli używamy kodowania UTF-8, to możemy użyć wersji, przyjmującej tylko jeden argument - ścieżkę do pliku. Inaczej musimy również podać użyte w nim kodowanie.
#### Parametry:
1. `string path` - ścieżka do pliku, który ma zostać odczytany.
2. `[Encoding encoding = System.Text.Encoding.UTF8]` - kodowanie, które zostało zastosowane w odczytywanym pliku.
#### Wyjątki:
1. `ArgumentException` - w .NET Framework i .NET Core w wersjach starszych niż 2.1: jeżeli `path` ma długość `0`, składa się wyłącznie z białych znaków lub zawieraj przynajmniej jeden nieprawidłowy znak (jeden ze znaków zdefiniowanych przez metodę `GetInvalidPathChars()`).
2. `ArgumentNullException` - jeśli `path` jest `null`.
3. `DirectoryNotFoundException` - jeśli podana ścieżka jest nieprawidłowa (np. prowadzi do niezmapowanego dysku).
4. `FileNotFoundException` - jeśli plik wskazany przez ścieżkę `path` nie został znaleziony.
5. `IOException` - jeśli wystąpił błąd I/O (wejścia/wyjścia), podczas otwierania pliku.
6. `PathTooLongException` - jeśli `path` jest dłuższe niż zdefiniowana przez system maksymalna długość.
7. `SecurityException` - jeśli użytkownik nie posiada wymaganych uprawnień.
8. `UnauthorizedAccessException` - jeśli wskazany plik jest _read-only_ lub ta operacja nie jest wspierana przez używaną platformę, lub `path` wskazuje na folder, a nie plik, lub użytkownik nie ma wymaganych uprawnień.

#### `ReadLines` 
```csharp =
public static System.Collections.Generic.IEnumerable<string> ReadLines (string path);
```
```csharp =
public static System.Collections.Generic.IEnumerable<string> ReadLines (string path, System.Text.Encoding encoding);
```
##### Zwraca:
Metoda zwraca `IEnumerable<String>`, w związku z tym można ją stosować w połączeniu z LINQ. Zastosowana samodzielnie zawiera kolekcję wszystkich linii znajdujących się w podanym pliku. Zastosowana wewnątrz zapytania LINQ, zwraca tylko te linie, które są wynikiem zapytania. Przy czym, można rozpocząć wyliczanie, przed zwróceniem całej kolekcji. Od razu można więc przefiltrować zawartość pliku, tak, aby wyciągnąć z niego tylko interesujące nas linie tekstu.
##### Przykład:
Załóżmy, że z naszego pliku chcemy pobrać wyłącznie informacje dotyczące produktów spożywczych. Wówczas moglibyśmy napisać:
```csharp =
public void Method()
{
    var lines = File.ReadLines(@"C:\Temp\items.csv").Where(line => line.EndsWith(",2"));
    // Dalsza obrobka danych
}
```
##### Zastosowanie:
1. Wykonywanie zapytań LINQ (tzw. typu _LINQ to Objects_ (LINQ do obiektów), czyli wykonywanych bezpośrednio na kolekcjach `IEnumerable` lub `IEnumerable<T>`) do otrzymania z pliku przefiltrowanego zestawu linii.
2. Do zapisu otrzymanej kolekcji linii do pliku przy pomocy metody `File.WriteAllLines(String, IEnumerable<String>) ` lub dodania jej na końcu pliku, przy użyciu metody `File.AppendAllLines(String, IEnumerable<String>)`.
3. Do stworzenia natychmiast wypełnionej instancji kolekcji, która posiada konstruktor, przyjmujący jako argument kolekcję `IEnumerable<string>`, np. `IList<string>` lub `Queue<string>`.

#### `ReadAllLines`
```csharp =
public static string[] ReadAllLines (string path);
```
```csharp =
public static string[] ReadAllLines (string path, System.Text.Encoding encoding);
```
##### Zwraca:
Metoda zwraca tablicę `string`ów, zawierającą wszystkie linie znajdujące się w tekście. Wszelkie operacje na pobranych danych, ich filtrowanie itp. można wykonywać dopiero po pobraniu z pliku wszystkich plików, na gotowej tablicy.
##### Wyjątki:
Poza wyjątkami opisanymi wyżej, metoda może jeszcze zwrócić wyjątek `NotSupportedException`, jeżeli `path` jest w nieprawidłowym formacie.
##### Zastosowanie:
Kiedy potrzebujemy pobrać do pamięci naszej aplikacji całą zawartość pliku tekstowego i podzielić ją na pojedyncze linie.
#### `ReadAllText`
```csharp =
public static string ReadAllText (string path);
```
```csharp =
public static string ReadAllText (string path, System.Text.Encoding encoding);
```
##### Zwraca:
W odróżnieniu do dwóch poprzednich metod, metoda `ReadAllText`, nie dzieli treści pliku na linie. Cała zawartość strumienia, po zdekodowaniu, zostaje zapisana do pojedynczego `string`a.
##### Wyjątki:
Podobnie jak metoda `ReadAllLines`, poza wyliczonymi wyżej wyjątkami, może zwrócić wyjątek `NotSupportedException`, jeżeli `path` jest w nieprawidłowym formacie.
##### Zastosowanie:
Kiedy chcemy pobrać całą zawartość pliku, ale nie chcemy dzielić go na pojedyncze linie. Np. kiedy chcemy tylko wyświetlić zawartość pliku.
#### Porównanie metod
| Cecha | `ReadLines` | `ReadAllLines` | `ReadAllText` |
| :--- | :---: | :---: | :---: |
| Parametry | analogiczne | analogiczne | analogiczne |
| Zwracany typ | typ wyliczeniowy (`IEnumerable<string>`) | tablica (`string[]`) | `string` |
| Zwracane linie pliku | wszystkie lub wybrane | wszystkie | cała zawartość pliku, bez podziału na linie |
| Wyjątki | analogiczne | analogiczne + `NotSupportedException` | analogiczne + `NotSupportedException` |
| Użycie w zapytaniach LINQ | tak, nawet podczas pobierania | po pobraniu całości, jak każdą tablica | po pobraniu całości, jak każdy `string` |
| Możliwość filtrowanie | podczas odczytu | dopiero po odczycie całości | dopiero po odczycie całości |
| Domyślne kodowanie | UTF-8 | próbuje automatycznie wykryć kodowanie pliku, może wykryć kodowanie UTF-8 i UTF-32 (big-endian i little-endian) | próbuje automatycznie wykryć kodowanie pliku, może wykryć kodowanie UTF-8, UTF-16 (big-endian i little-endian) i UTF-32 (big-endian i little-endian), jeżeli plik rozpoczyna się odpowiednimi znacznikami kolejności bajtów (**BOM** - _**B**yte **O**rder **M**ark_) |

#### `ReadAllBytes`
Jeżeli nie chcemy dekodować treści pliku, bo np. zawiera on obrazek, a nie tekst, to klasa `File` posiada metodę również do tego:
```csharp =
public static byte[] ReadAllBytes (string path);
```
Otwiera ona plik binarny o podanej ścieżce. Następnie odczytuje cała zawartość pliku i zapisuje ją w tablicy bajtów. Na koniec, zamyka plik i zwraca utworzoną tablicę.

## Zapis danych do pliku
Do zapisu danych do pliku istnieję analogiczne metody jak w przypadku odczytu.
### Metoda `Write` klasy `FileStream`
Analogicznie do metody `Read`, mamy w klasie `System.IO.FileStream` metodę `Write`.
```csharp =
public override void Write (byte[] buffer, int offset, int count);
```
Przyjmuje ona analogiczne argumenty, choć w tym wypadku `buffer`, w podanym przedziale, zawiera oczywiście dane do zapisu i nie zostaje zmieniony przez metodę. W odróżnieniu od metody `Read`, metoda `Write` nie zwraca żadnej wartości.
#### Przykład:
```csharp =
public void Method()
{
    using FileStream fs = File.Open(@"C:\Temp\items.csv", FileMode.Append, FileAccess.Write);

    byte[] buf = Encoding.UTF8.GetBytes("4,Watermelon,2");

    fs.Write(buf, 0, buf.Length);
}
```
1. Podobnie jak przy odczycie, najpierw musimy utworzyć strumień danych. W tym wypadku otwieramy go jednak z dostępem do zapisu (`Write`).
2. Następnie również tworzymy bufor bajtów. Tym razem będzie on jednak zawierał informacje, które chcemy zapisać. Aby zakodować `string`, jako tablicę bajtów, ponownie sięgamy do klasy `System.Text.Encoding`, a właściwie do dziedziczącej po niej klasy `System.Text.UTF8Encoding`. Implementuje ona metodę `GetBytes`, która koduje podany `string` na bajty, przy pomocy kodowania UTF-8 i zwraca uzyskane wartości jako tablicę.
3. Tym razem nie musimy więc tworzyć pętli, tylko możemy od razu zapisać wszystkie informacje.

| :warning:**UWAGA!** |
| :---: |
| Należy pamiętać, że metoda `Write` zapisuje dane do strumienia rozpoczynając od aktualnej pozycji strumienia. Ponieważ otwarcie strumienia pliku metodą `File.Open` w trybie `FileMode.Open` otwiera plik i ustawia pozycję strumienia na jego początku, oznacza to, że dane znajdujące się na początku pliku zostaną nadpisane przez nowe dane, a dalsza część pliku pozostanie bez zmian. Kiedy chcemy dopisać dane na końcu pliku, tak, aby obecna zawartość pliku pozostała nienaruszona, musimy zastosować modyfikator `Append`, jak to zrobiliśmy w powyższym przykładzie. Możemy również użyć opcji `Truncate`, gdybyśmy chcieli usunąć zawartość pliku i wstawić do niego wyłącznie nowe dane. |

Po zakończeniu zapisu metodą `Write`, pozycja strumienia jest ustawiona w miejscu zakończenia zapisu.

### Klasa `StreamWriter`
Podobnie jak do odczytu danych ze strumienia danych mieliśmy do dyspozycji klasę `System.IO.StreamReader`, do zapisu istnieje klasa `System.IO.StreamWriter` implementująca klasę `System.IO.TextWriter`. Służy ona do zapisu znaków do strumienia bajtów, używając odpowiedniego kodowania. Domyślnie jest to ponownie kodowanie UTF-8, o ile nie podano inaczej. Ta klasa również implementuje interfejs `IDisposable`, będziemy więc tworzyć jej obiekty w połączeniu z instrukcją `using`. Do utworzenia obiektu tej klasy mamy do dyspozycji np. konstruktor przyjmujący jako argument strumień danych, ale także taki, przyjmujący ścieżkę do pliku:
```csharp =
using FileStream fs = File.Open(@"C:\Temp\items.csv", FileMode.Open, FileAccess.Read);
using StreamWriter sw = new StreamWriter(fs);
```
lub
```csharp =
using StreamWriter sw = new StreamWriter(@"C:\Temp\items.csv");
```
| :warning:**UWAGA!** |
| :---: |
| Jeżeli używamy konstruktora przyjmującego ścieżkę do pliku i taki plik już istnieje, to zostanie on nadpisany. W przeciwnym razie, plik zostanie utworzony. |

Obiekt klasy `StreamWriter` można również utworzyć wywołując metodę `CreateText` na obiekcie klasy `FileInfo`:
```csharp =
FileInfo file = new FileInfo(@"C:\Temp\items.csv");
using StreamWriter sw = file.CreateText();
```
lub jej statycznej wersji z klasy `File`:
```csharp =
using StreamWriter sw = File.CreateText(@"C:\Temp\items.csv");
```
| :warning:**UWAGA!** |
| :---: |
| Wywołanie tej metody powoduje utworzenie nowego pliku (nadpisanie pliku, jeżeli już istnieje). |

Jeżeli chcemy dopisać tekst do istniejącego pliku możemy natomiast skorzystać z metody `AppendText`:
```csharp =
FileInfo file = new FileInfo(@"C:\Temp\items.csv");
using StreamWriter sw = file.AppendText();
```
lub w wersji statycznej:
```csharp =
using StreamWriter sw = File.AppendText(@"C:\Temp\items.csv");
```
| :warning:**UWAGA!** |
| :---: |
| Wywołanie tej metody powoduje otwarcie istniejącego pliku i ustawienie się na jego końcu lub, gdy jeszcze nie istnieje, utworzenie nowego pliku. |

Klasa posiada różne metody do zapisu tekstu do strumienia bajtów.
#### `Write`
Podobnie jak klasa `FileStream`, posiada ona metodę `Write`. Może ona również występować w bardzo zbliżonej formie:
```csharp =
public override void Write (char[] buffer, int index, int count);
```
Zachowuje się ona analogicznie do przedstawionej wyżej metody, z tą różnicą, że dane w buforze są typu `char`, a nie `byte`.

Do metody `Write` możemy również dostarczyć dane, które chcemy zapisać w strumieniu, w postaci `string`a, sformatowanego `string`a, tablicy znaków `char[]` (jeśli podajemy tylko tablicę znaków, bez `index` i `count`, to do strumienia zostaną dopisane znaki z całej tablicy), pojedynczego znaku `char`, czy zakresu znaków `ReadOnlySpan<char>`.
#### `WriteLine`
Podobnie jak w klasie `StreamRead` mieliśmy metodę `ReadLine`, w klasie `StreamWriter` mamy metodę `WriteLine`. Przyjmuje ona jako argument `string` (sformatowany `string` lub `ReadOnlySpan<char>`), który chcemy dopisać do strumienia danych. Jak można się domyślić metoda, używając odpowiedniego kodowania, koduje podany ciąg znaków oraz znak nowej linii i zapisuje otrzymane bajty w strumieniu danych. Metoda nic nie zwraca.
##### Przykład:
```csharp =
public void Method()
{
    FileInfo file = new FileInfo(@"C:\Temp\items.csv");
    using StreamWriter sw = file.AppendText();

    sw.WriteLine("5,Melon,2");
}
```

### Metody klasy `File`
Jak już wspominaliśmy przy okazji omówienia metody `ReadLines`, klasa `System.IO.File` posiada do zapisu danych do pliku m.in. metody `WriteAllLines` i `AppendAllLines`.
#### `WriteAllLines`
Jak można się domyślić metoda tworzy nowy plik (jeżeli plik już istnieje, to zostaje nadpisany), zapisuje do niego podaną kolekcję `string`ów, dodając po każdym z nich znak nowej linii i zamyka plik. Może ona przyjmować różne argumenty:
```csharp =
public static void WriteAllLines (string path, System.Collections.Generic.IEnumerable<string> contents);
```
```csharp =
public static void WriteAllLines (string path, string[] contents);
```
```csharp =
public static void WriteAllLines (string path, System.Collections.Generic.IEnumerable<string> contents, System.Text.Encoding encoding);
```
```csharp =
public static void WriteAllLines (string path, string[] contents, System.Text.Encoding encoding);
```
##### Parametry:
1. `path` - ścieżka do pliku, który ma zostać utworzony i do którego chcemy zapisać podane dane.
2. `contents` - linie tekstu, które chcemy zapisać w pliku.
3. `[encoding = System.Text.Encoding.UTF8]` - kodowanie, zgodnie z którym mają zostać zakodowane zapisywane w pliku informacje.

Jeżeli kodowanie nie zostało sprecyzowane, to domyślnie jest stosowane kodowanie UTF-8, bez znacznika kolejności bajtów (**BOM** - **B**yte **O**rder **M**ark). Jeżeli jest niezbędne umieszczenie identyfikatora kodowania UTF-8 (takiego jak BOM) na początku pliku, to trzeba jawnie podać kodowanie (użyć jednego z przeciążeń z parametrem `encoding`). Musimy to również zrobić, jeżeli chcemy użyć jakiegokolwiek innego kodowania.
##### Wyjątki:
1. `ArgumentException` - w .NET Framework i .NET Core w wersjach starszych niż 2.1: jeżeli `path` ma długość `0`, składa się wyłącznie z białych znaków lub zawieraj przynajmniej jeden nieprawidłowy znak (jeden ze znaków zdefiniowanych przez metodę `GetInvalidPathChars()`).
2. `ArgumentNullException` - jeśli `path` lub `contents` jest `null`.
3. `DirectoryNotFoundException` - jeśli podana ścieżka jest nieprawidłowa (np. prowadzi do niezmapowanego dysku).
4. `IOException` - jeśli wystąpił błąd I/O (wejścia/wyjścia), podczas otwierania pliku.
5. `PathTooLongException` - jeśli `path` jest dłuższe niż zdefiniowana przez system maksymalna długość.
6. `NotSupportedException` - jeśli `path` ma nieprawidłowy format.
7. `SecurityException` - jeśli użytkownik nie posiada wymaganych uprawnień.
8. `UnauthorizedAccessException` - jeśli wskazany plik jest _read-only_ lub ścieżka wskazuje na ukryty plik lub ta operacja nie jest wspierana przez używaną platformę, lub `path` wskazuje na folder, a nie plik, lub użytkownik nie ma wymaganych uprawnień.
#### `AppendAllLines`
Metoda otwiera plik (jeśli plik nie istnieje, to go tworzy), dopisuje podane informacje na jego końcu i zamyka go. Metoda ma tylko dwa przeładowania:
```csharp =
public static void AppendAllLines (string path, System.Collections.Generic.IEnumerable<string> contents);
```
```csharp =
public static void AppendAllLines (string path, System.Collections.Generic.IEnumerable<string> contents, System.Text.Encoding encoding);
```
##### Parametry:
1. `path` - ścieżka do pliku, do którego chcemy dopisać podane linie. Jeżeli plik o podanej ścieżce nie istnieje, zostanie on utworzony. Metoda może utworzyć nowy plik, ale nie nowe foldery. Tak więc ścieżka może zawierać wyłącznie istniejące foldery.
2. `contents` - linie tekstu, które chcemy dopisać do pliku.
3. `[encoding = System.Text.Encoding.UTF8]` - kodowanie, zgodnie z którym mają zostać zakodowane zapisywane w pliku informacje.
##### Wyjątki:
1. `ArgumentException` - jeżeli `path` ma długość `0`, składa się wyłącznie z białych znaków lub zawieraj przynajmniej jeden nieprawidłowy znak (jeden ze znaków zdefiniowanych przez metodę `GetInvalidPathChars()`).
2. `ArgumentNullException` - jeśli `path` lub `contents` jest `null`.
3. `DirectoryNotFoundException` - jeśli podana ścieżka jest nieprawidłowa (np. podany folder nie istnieje lub jest na niezmapowanym dysku).
4. `FileNotFoundException` - plik wskazany przez `path` nie został znaleziony.
5. `IOException` - jeśli wystąpił błąd I/O (wejścia/wyjścia), podczas otwierania pliku.
6. `PathTooLongException` - jeśli `path` jest dłuższe niż zdefiniowana przez system maksymalna długość.
7. `NotSupportedException` - jeśli `path` ma nieprawidłowy format.
8. `SecurityException` - jeśli użytkownik nie posiada uprawnienia do zapisu do pliku.
9. `UnauthorizedAccessException` - jeśli wskazany plik jest _read-only_ lub ta operacja nie jest wspierana przez używaną platformę, lub `path` wskazuje na folder, a nie plik.
##### Zastosowanie:
Można użyć tej metody, aby stworzyć plik zawierający:
1. wyniki zapytania LINQ to Objects na wierszach pliku, uzyskane przy użyciu metody `ReadLines`.
2. zawartość kolekcji implementującej interfejs `IEnumerable<T>` dla `string`ów.
#### `WriteAllText`
Metoda tworzy plik o podanej ścieżce, zapisuje w nim podaną treść i zamyka plik. Jeżeli wskazany plik już istnieje, to zostaje on nadpisany. Metoda występuje w dwóch przeładowaniach:
```csharp =
public static void WriteAllText (string path, string? contents);
```
```csharp =
public static void WriteAllText (string path, string? contents, System.Text.Encoding encoding);
```
##### Parametry:
1. `path` - ścieżka do pliku, który ma zostać utworzony i do którego chcemy zapisać podaną treść.
2. `contents` - tekst, który chcemy zapisać w pliku.
3. `[encoding = System.Text.Encoding.UTF8]` - kodowanie, zgodnie z którym ma zostać zakodowany zapisywany w pliku tekst.

Jeżeli kodowanie nie zostało sprecyzowane, to domyślnie jest stosowane kodowanie UTF-8, bez znacznika kolejności bajtów (**BOM** - **B**yte **O**rder **M**ark). Jeżeli jest niezbędne umieszczenie identyfikatora kodowania UTF-8 (takiego jak BOM) na początku pliku, to trzeba jawnie podać kodowanie (użyć jednego z przeciążeń z parametrem `encoding`). Musimy to również zrobić, jeżeli chcemy użyć jakiegokolwiek innego kodowania.
##### Wyjątki:
1. `ArgumentException` - w .NET Framework i .NET Core w wersjach starszych niż 2.1: jeżeli `path` ma długość `0`, składa się wyłącznie z białych znaków lub zawieraj przynajmniej jeden nieprawidłowy znak (jeden ze znaków zdefiniowanych przez metodę `GetInvalidPathChars()`).
2. `ArgumentNullException` - jeśli `path` jest `null`.
3. `DirectoryNotFoundException` - jeśli podana ścieżka jest nieprawidłowa (np. prowadzi do niezmapowanego dysku).
4. `IOException` - jeśli wystąpił błąd I/O (wejścia/wyjścia), podczas otwierania pliku.
5. `PathTooLongException` - jeśli podana ścieżka, nazwa pliku lub oba są dłuższe niż zdefiniowana przez system maksymalna długość.
6. `NotSupportedException` - jeśli `path` ma nieprawidłowy format.
7. `SecurityException` - jeśli użytkownik nie posiada wymaganych uprawnień.
8. `UnauthorizedAccessException` - jeśli wskazany plik jest _read-only_ lub ścieżka wskazuje na ukryty plik lub ta operacja nie jest wspierana przez używaną platformę, lub `path` wskazuje na folder, a nie plik, lub użytkownik nie ma wymaganych uprawnień.
#### `AppendAllText`
Metoda otwiera plik, dopisuje na jego końcu podany tekst i zamyka plik. Jeżeli plik o podanej ścieżce nie istnieje metoda najpierw go tworzy. Ma ona, podobnie jak metoda `WriteAllText`, dwa przeciążenia:
```csharp =
public static void AppendAllText (string path, string? contents);
```
```csharp =
public static void AppendAllText (string path, string? contents, System.Text.Encoding encoding);
```
##### Parametry:
1. `path` - ścieżka do pliku, do którego chcemy dopisać podane linie. Jeżeli plik o podanej ścieżce nie istnieje, zostanie on utworzony. Metoda może utworzyć nowy plik, ale nie nowe foldery. Tak więc ścieżka może zawierać wyłącznie istniejące foldery.
2. `contents` - tekst, który chcemy dopisać do pliku.
3. `[encoding = System.Text.Encoding.UTF8]` - kodowanie, zgodnie z którym ma zostać zakodowany zapisywany w pliku tekst.
##### Wyjątki:
1. `ArgumentException` - w .NET Framework i .NET Core w wersjach starszych niż 2.1: jeżeli `path` ma długość `0`, składa się wyłącznie z białych znaków lub zawieraj przynajmniej jeden nieprawidłowy znak (jeden ze znaków zdefiniowanych przez metodę `GetInvalidPathChars()`).
2. `ArgumentNullException` - jeśli `path` jest `null`.
3. `DirectoryNotFoundException` - jeśli podana ścieżka jest nieprawidłowa (np. folder nie istnieje lub znajduje się na niezmapowanym dysku).
4. `IOException` - jeśli wystąpił błąd I/O (wejścia/wyjścia), podczas otwierania pliku.
5. `PathTooLongException` - jeśli podana ścieżka, nazwa pliku lub oba są dłuższe niż zdefiniowana przez system maksymalna długość.
6. `NotSupportedException` - jeśli `path` ma nieprawidłowy format.
7. `SecurityException` - jeśli użytkownik nie posiada wymaganych uprawnień.
8. `UnauthorizedAccessException` - jeśli wskazany plik jest _read-only_ lub ta operacja nie jest wspierana przez używaną platformę, lub `path` wskazuje na folder, a nie plik, lub użytkownik nie ma wymaganych uprawnień.
#### `WriteAllBytes`
Podobnie jak w przypadku odczytywania, klasa `File` posiada również metodę do zapisywanie informacji przedstawionej w postaci bajtów. W odróżnieniu jednak od wszystkich innych sposobów zapisu informacji mamy tu dostępną tylko jedną metodę:
```csharp =
public static void WriteAllBytes (string path, byte[] bytes);
```
Metoda `WriteAllBytes` tworzy nowy plik, zapisuje w nim podane bajty informacji i zamyka go. Jeżeli wskazany plik już istnieje, to zostaje on nadpisany.

Klasa `File` nie udostępnia metody umożliwiającej dopisywanie informacji, przedstawionej w postaci tablicy bajtów, na końcu istniejącego pliku.
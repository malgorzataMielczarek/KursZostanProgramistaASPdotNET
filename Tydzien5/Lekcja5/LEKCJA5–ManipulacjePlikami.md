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
```csharp =
public void Method()
{
    //ewentualne inne instrukcje

    // Stary zapis instrukcji using
    //utworzenie strumienia pliku
    using(FileStream fs = File.Open(@"C:\Temp\items.csv", FileMode.Open, FileAccess.Read))
    {
        //deklaracja bufora do odczytu porcji danych ze strumienia
        //poniewaz pliki moga byc bardzo duze, wiec nie chcemy pobierac ich calych na raz do pamieci programu, a mniejszymi porcjami
        byte[] buf = new byte[1024];
        //zmienna w ktorej bedziemy zapisywac ile danych faktycznie pobralismy ze strumienia
        //jezeli w strumieniu pozostalo mniej danych niz probujemy pobrac, to ta wartosc pomoze nam ustalic do ktorego miejsca bufora znajduja sie nowe dane
        //oraz ustalic kiedy skonczy sie plik
        int c;
        //pobieramy dane do bufora (od poczatku do konca bufora), dopoki nie dojdziemy do konca pliku (niema juz czego pobierac)
        while ((c = fs.Read(buf, 0, buf.Length)) > 0)
        {
            //dekodujemy dane zapisane w postaci bajtow, uzywajac odpowiedniego kodowania
            //najczesciej bedzie to kodowanie UTF8
            //i przeksztalcamy strumien bajtow na string
            string text = Encoding.UTF8.GetString(buf, 0, c); //poniewaz nasz plik jest krotki, wiec bufor pomiesci caly plik naraz

            //obrabiamy dane
            //instrukcje do obrobki danych
            //zapewne wyrazenie regularne do wyszukania konca linii i podzielenia stringu na elementy
            //nastepnie zapewne ryrazenie regularne do wyszukania przecinkow w kazdym elemencie i uzystakania pojedynczych danych
            //na koniec sparsowanie danych z typu string na wlasciwy typ i otworzenie naszych obiektow Item
        }

        //tu zostanie wywolana metoda fs.Dispose();
    }

    //ewentualne inne instrukcje
}
```
```csharp =
public void Method()
{
    //ewentualne inne instrukcje

    // Skrocony zapis instrukcji using
    using FileStream fs = File.Open(@"C:\Temp\items.csv", FileMode.Open, FileAccess.Read);
    byte[] buf = new byte[1024];
    int c;
    while ((c = fs.Read(buf, 0, buf.Length)) > 0)
    {
        string text = Encoding.UTF8.GetString(buf, 0, c);
        //instrukcje do obrobki danych
    }

    //ewentualne inne instrukcje

    //tu zostanie wywolana metoda fs.Dispose();
}
```

**`File`** - klasa związana z obsługą plików (otwieranie, zmykanie itd.).<br/>
**`File.Open(string path, FileMode mode, FileAccess access, FileShare share)`** - metoda klasy `File` przyjmująca dwa (`path`, `mode`), trzy (`path`, `mode`, `access`) lub cztery (`path`, `mode`, `access`, `share`) argumenty, otwierająca plik, blokująca dostęp do niego dla innych programów i zwracająca obiekt typu `FileStream`.
* `path` - `string` przedstawiający ścieżkę do pliku, który chcemy otworzyć.
* `mode` - tryb otwieranego pliku, tzn. czy chcemy stworzyć nowy plik, otworzyć już istniejący, nadpisać plik lub otworzyć plik ustawiając kursor na jego końcu.
* `access` - jaki dostęp do pliku chcemy uzyskać, tzn., czy chcemy z niego tylko odczytywać informacje, tylko zapisywać, a może jedno z drugim.
* `share` - jakiego dostępu do plików chcemy udzielić wątkom pochodnym (odczyt, zapis, oba, usuwanie, żaden, dziedziczony).

**`fs.Read(byte[] array, int offset, int count)`** - metoda klasy `FileStream` odczytująca `count` bajtów danych ze strumienia i zastępująca nimi dane z tablicy `array` począwszy od indeksu `offset`. Metoda zwraca liczbę rzeczywiście odczytanych bajtów.

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

## Odczyt (_Read_)
### Metoda `Read` klasy `FileStream`
```csharp
public override int Read([In][Out] byte[] array, int offset, int count)
```
Metoda odczytuje blok bajtów ze strumienia i zapisuje dane w danym buforze.
#### Parametry:
1. `array`:<br/>
Gdy metoda wraca, parametr zawiera określoną tablicę bajtów z wartościami pomiędzy `offset` i `(offset + count - 1)` zastąpionymi przez bajty odczytane ze strumienia.
2. `offset`:<br/>
Przesunięcie bajtów w tablicy, w którym zostaną umieszczone odczytane bajty (numer indeksu od którego wartości zaczną być podmieniane).
3. `count`:<br/>
Maksymalna liczba bajtów do odczytu (ile bajtów chcemy odczytać). Zostanie odczytane `count` bajtów lub tyle ile zostało, jeżeli zostało mniej niż `count`.
#### Zwraca:
Metoda zwraca całkowitą liczbę bajtów wczytanych do bufora. Może to być mniej niż chcieliśmy, jeżeli taka liczba bajtów nie jest aktualnie dostępna, lub zero, jeżeli dotarliśmy do końca pliku.
#### Wyjątki:
1. `System.ArgumentNullException`:<br/>
Jeśli `array` jest `null`.
2. `System.ArgumentOutOfRangeException`:<br/>
Jeśli `offset` lub `count` są ujemne.
3. `System.NotSupportedException`:<br/>
Jeśli strumień danych nie wspiera odczytu danych.
4. `System.IO.IOException`:<br/>
Jeśli wystąpił błąd wejścia/wyjścia.
5. `System.ArgumentException`:<br/>
Jeśli `offset` i `count` opisują nieprawidłowy zakres w tablicy.
6. `System.ObjectDisposedException`:<br/>
Jeśli metoda była wywołana po zamknięciu strumienia (pliku).

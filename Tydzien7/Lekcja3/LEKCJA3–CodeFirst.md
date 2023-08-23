# [LEKCJA 3 – Code first](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-3-code-first/)
_Code first_ jest metodą tworzenia baz danych prawie całkowicie z poziomu kodu aplikacji.

Kiedyś standardowo mieliśmy bardzo duże rozróżnienie pomiędzy programistami webowymi a programistami baz danych. Oczywiście w dużych projektach wymagających optymalizacji praca programistów baz danych dalej jest nieoceniona. Jednak dzięki nowoczesnym narzędziom takim jak _Entity Framework Core_ w prostszych projektach programiści webowi radzą sobie sami, nawet przy niewielkiej znajomości SQL-a.

Kiedyś aby utworzyć bazę danych standardowo łączyliśmy się bezpośrednio z naszym silnikiem bazodanowym (np. Microsoft SQL Server np. przez Microsoft SQL Server Management Studio) i tworzyliśmy tam bezpośrednio nową bazę danych i nasze tabele. Następnie można było wrócić do kodu naszej aplikacji, gdzie przy pomocy _Entity Framework_ łączyliśmy się z utworzoną bazą danych i generowaliśmy modele. Takie podejście nazywa się _database first_. Czyli najpierw tworzymy wszystkie tabele i relacje między nimi w naszej bazie danych, a dopiero potem przechodzimy do kodu. Programistą jednak takie podejście się nie podobało. Nie chcieli oni być skazani na pracę bezpośrednio na silniku bazodanowym, a zlecanie tworzenia klas modeli zewnętrznemu narzędzi. Szczególnie, że _Entity Framework_ działało według własnych schematów i wygenerowane przy jego pomocy klasy nie do końca odpowiadały wymaganiom programistów. Trzeba je było zatem jeszcze później poprawiać. Dlatego w nowej wersji _Entity Framework_ (_Core_) zaimplementowano podejście _code first_.

Podejście _code first_ pozwala na napisanie przez programistów modeli w naszej aplikacji i automatyczne utworzenie na ich podstawie bazy danych z odpowiednimi tabelami, za co odpowiada _Entity Framework Core_.

## Tworzenie modeli
W katalogu _Model_ projektu _.Domain_ tworzymy klasy z naszymi modelami. Modele będą wyglądać podobnie jak te, które pisaliśmy do tej pory. Będą to po prostu publiczne klasy zawierające publiczne właściwości, na podstawie których zostaną utworzone tabele w bazie danych (jedna klasa = jedna tabela, jedna właściwość klasy = jedna kolumna tabeli).
### Mapowanie
#### Nazwy
Automatycznie, po mapowaniu, tabele w bazie danych będą miały takie nazwy jak nasze klasy, natomiast kolumny, takie jak nasze właściwości. Jeżeli jednak z jakiegoś powodu (choć nie jest to zalecane), chcielibyśmy, aby elementy w bazie danych miały inne nazwy niż odpowiadające im elementy w modelach, to z pomocą przychodzą nam odpowiednie atrybuty. Są to odpowiednio `TableAttribute` i `ColumnAttribute`, które przyjmują żądane nazwy, jako argumenty konstruktora. Np.:
```csharp =
[Table("Subjects")]
public class Item
{
    public int Id { get; set; }

    [Column("Title")]
    public string Name { get; set; }
}
```
co spowoduje utworzenie tabeli o nazwie _Subjects_, z dwoma kolumnami: _Id_ i _Title_.
#### Wskazanie klucza głównego
Jeżeli pierwsza właściwość w klasie nosi nazwę `Id`, to _Entity Framework Core_ automatycznie rozpozna ją i ustawi jako klucz główny tabeli. Możemy jednak również jawnie wskazać właściwość, którą chcemy oznaczyć jako klucz główny. Służy do tego atrybut `KeyAttribute`. Np. model:
```csharp =
public class Language
{
    [Key]
    public string Code { get; set; }
    public string Name { get; set; }
}
```
zostanie zmapowany na tabelę _Language_ z dwoma kolumnami: _Code_ i _Name_, w której _Code_ będzie zawierał klucze główne (unikatowe wartości pozwalające na identyfikację jednego konkretnego elementu tabeli).
#### Relacje
Załóżmy, że mamy dwa modele:
```csharp =
public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int TypeId {get; set; }
}
```
```csharp =
public class Type
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```
Jak widać chcemy, do obiektów `Item` przyporządkowywać jakiś obiekt `Type`. Tak napisalibyśmy je do tej pory. _Entity Framework Core_ wymaga od nas jeszcze stworzenia właściwości na obiekty znajdujące się w relacji. Będziemy je nazywać nawigacjami. Dzięki nim będziemy mogli np. sprawdzić `Name` obiektu `Type` powiązanego z naszym obiektem `Item` bez tworzenia dodatkowych zapytań. Do klasy `Item` musimy zatem dopisać jeszcze jedną linijkę:
```csharp =
public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int TypeId {get; set; }

    public virtual Type Type { get; set; }
}
```
Widzimy tu, że każdy obiekt `Item` jest powiązany z dokładnie jednym obiektem `Type`. Relacja jest jednak dwustronna. Tutaj mamy do czynienia z najbardziej podstawowym typem relacji, czyli tzw. relacja jeden do wielu. Oznacza to, że jeden obiekt `Type` może być w relacji z wieloma obiektami `Item`. Musimy więc jeszcze zmodyfikować naszą klasę `Type`, aby móc w niej przechowywać powiązaną z danym obiektem `Type` kolekcję obiektów `Item`:
```csharp =
public class Type
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Item> Items { get; set; }
}
```
Wewnątrz naszej bazy danych na relację będzie jedynie wskazywać obecność kluczy (id) z tabeli _Type_ w tabeli _Item_. Jeżeli właściwość zawierającą taki klucz nazwiemy w tworzonej klasie według wzoru _NazwaTabeliZKtórejPochodziKluczNazwaWłaściwościKtóraJestKluczem_ tak jak to zrobiliśmy w naszym przypadku, _Entity Framework Core_ wie, że przechowujemy tu klucz obcy z tej a tej tabeli.
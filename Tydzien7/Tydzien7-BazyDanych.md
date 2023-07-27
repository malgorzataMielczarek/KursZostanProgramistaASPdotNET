# TYDZIEŃ 7 - Bazy danych
## Spis treści
### [LEKCJA 1 – Powitanie](#lekcja-1--powitanie-1)
### [LEKCJA 2 – Entity Framework](#lekcja-2--entity-framework-1)
### [LEKCJA 3 – Code first](#lekcja-3--code-first-1)

## [LEKCJA 1 – Powitanie](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-1-powitanie/)
W tym tygodniu zajmiemy się tworzeniem bazy danych dla aplikacji webowej.
1. _Entity Framework Core_<br />
Nauczymy się korzystać z najczęściej używanego w .NET ORM (_**O**bject-**r**elational **M**aping_ - mapowanie obiektowo-relacyjne) - Entity Framework Core.
2. _Code First_ vs. _Database First_<br />
Poznamy różnice między podejściami _Code First_ i _Database First_.
3. Migracje<br />
Nauczymy się migrować tabele i modele.

## [LEKCJA 2 – Entity Framework](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-2-entity-framework/)
Do połączenia naszej aplikacji z bazą danych będziemy używać odpowiedniej biblioteki nazywanej ORM. ORM, czyli _**O**bject-**r**elational **M**aping_ (mapowanie obiektowo-relacyjne), nazywamy biblioteki przeznaczone do połączenia aplikacji z odpowiednią bazą danych. Jak sama nazwa wskazuje będziemy jej używać do przetrzymywania obiektów, które będą reprezentowały tabele z naszej bazy danych oraz definiowania występujących między nimi relacji. W środowisku .NET najczęściej stosowaną biblioteką jest _Entity Framework_ (obecnie w wersji _Core_, stosowana w _ASP.NET Core_). Istnieją oczywiście inne ORMy używane w różnych środowiskach. Innym popularnym rozwiązaniem dla środowiska .NET jest _NHibernate_. Wywodzi się on ze stworzonej dla Javy biblioteki _Hibernate_ dla której powstał tzw. port dla .NET-a, jednak obecnie są to zupełnie oddzielne projekty (niezależnie rozwijane i utrzymywane). Programiści chętnie używają również projektu _Dapper_, który jest tzw. małym ORM-em. Docenia się go za szybkość działania. _Entity Framework Core_ jest jednak obecnie mocno rozwijany przez firmę Microsoft, więc chociażby przewaga w szybkości działania jaką jeszcze niedawno miał _Dapper_ coraz bardziej się zaciera. Dodatkowo jest on świetnie zintegrowany z platformą .NET, dla której od początku był tworzony. Możemy przy jego pomocy tworzyć połączenie z bazą danych w aplikacjach na wszystkie platformy, nie tylko webowe, ale również desktopowe, czy nawet mobilne, pisane w Xamarinie. Mamy również szeroki wachlarz możliwości, jeśli chodzi o typ bazy danych z jakim chcemy się połączyć. Może to być chociażby Microsoft SQL Server (MS SQL), MySQL, PostgreSQL, SQLite.
### Instalacja biblioteki
1. Otwieramy menadżer paczek NuGet<br />
_Tools_ -> _NuGet Package Manager_ -> _Manage NuGet Packages for Solution..._<br />
lub<br />
_Solution Explorer_ -> prawym przyciskiem myszki na naszym projekcie _.Infrastructure_ -> _Manage NuGet Packages..._
2. Wyszukujemy paczki do instalacji<br />
Ogólnie będziemy chcieli zainstalować 3 paczki:
    1. _Microsoft.EntityFrameworkCore_ - nasze ORM
    2. _Microsoft.EntityFrameworkCore.SqlServer_ - rozszerzenie biblioteki dla odpowiedniego systemu zarządzania bazą danych. W tym wypadku jest to Microsoft SQL Server, ale jeżeli będziemy używać czegoś innego, to wyszukujemy odpowiednią inną paczkę.
    3. _Microsoft.EntityFrameworkCore.Tools_ - ta paczka nie jest obowiązkowa. Rozszerza nam ona nasz ORM o dodatkowe funkcje. Jest wykorzystywana do migracji.
3. Instalacja<br />
Po wyszukaniu odpowiedniej paczki instalujemy ją. Jeżeli otworzyliśmy menadżer dla całej solucji (pierwsza opcja z pierwszego punktu), to musimy jeszcze wybrać projekt dla którego chcemy wykonać instalację. Zaznaczamy wówczas nasz projekt _.Infrastructure_.

### _Entity Framework Core_ vs. _Entity Framework_
Zanim powstał .NET Core mieliśmy do dyspozycji _Entity Framework_. Obecnie został on zastąpiony przez napisany od nowa ORM _Entity Framework Core_. Przynajmniej na razie nie zostały w nim zaimplementowane wszystkie funkcjonalności z ostatniej (6.) wersji _Entity Framework_. Najważniejsze elementy, takie jak kontekst bazy danych, DbSety, czyli obiekty, które mapowane są na tabele bazy danych (dzięki nim mamy prostą możliwość dodawania, usuwania i wyszukiwania elementów w bazie danych), oczywiście działają. Dodatkowo mamy możliwość migracji, co ułatwia podejście _Code First_. Nie mamy natomiast kilku elementów takich jak chociażby automatyczne migracje (podczas uruchamiania projektu), czy część funkcjonalności związanych z leniwym ładowaniem (_lazy loading_) dla relacyjnych tabel. Istnieją jednak sposoby na obejście tych braków. Jest jednak kilka rzeczy, których nie było w _Entity Framework_, a które dodano do _Entity Framework Core_. Jest to między innymi baza danych _In-Memory_, czyli prosty framework do testowania połączenia z bazą danych (Obecnie trwa dyskusja, czy programiści testować części aplikacji w jakikolwiek sposób związane z _Entity Framework Core_, gdyż są to już wówczas de facto testy integracyjne, a nie jednostkowe.). Innym ważnym elementem, który dodano, jest standardowe wsparcie dla _Inversion of Control_, czyli dla implementacji _Dependenci Injection_, dzięki czemu można ją używać bez żadnych dodatkowych konfiguracji.

## [LEKCJA 3 – Code first](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-3-code-first/)
_Code first_ jest metodą tworzenia baz danych prawie całkowicie z poziomu kodu aplikacji.

Kiedyś standardowo mieliśmy bardzo duże rozróżnienie pomiędzy programistami webowymi a programistami baz danych. Oczywiście w dużych projektach wymagających optymalizacji praca programistów baz danych dalej jest nieoceniona. Jednak dzięki nowoczesnym narzędziom takim jak _Entity Framework Core_ w prostszych projektach programiści webowi radzą sobie sami, nawet przy niewielkiej znajomości SQLa.

Kiedyś aby utworzyć bazę danych standardowo łączyliśmy się bezpośrednio z naszym silnikiem bazodanowym (np. Microsoft SQL Server np. przez Microsoft SQL Server Management Studio) i tworzyliśmy tam bezpośrednio nową bazę danych i nasze tabele. Następnie można było wrócić do kodu naszej aplikacji, gdzie przy pomocy _Entity Framework_ łączyliśmy się z utworzoną bazą danych i generowaliśmy modele. Takie podejście nazywa się _database first_. Czyli najpierw tworzymy wszystkie tabele i relacje między nimi w naszej bazie danych, a dopiero potem przechodzimy do kodu. Programistą jednak takie podejście się nie podobało. Nie chcieli oni być skazani na pracę bezpośrednio na silniku bazodanowym, a zlecanie tworzenia klas modeli zewnętrznemu narzędzi. Szczególnie, że _Entity Framework_ działało według własnych schematów i wygenerowane przy jego pomocy klasy nie do końca odpowiadały wymaganiom programistów. Trzeba je było zatem jeszcze później poprawiać. Dlatego w nowej wersji _Entity Framework_ (_Core_) zaimplementowano podejście _code first_.

Podejście _code first_ pozwala na napisanie przez programistów modeli w naszej aplikacji i automatyczne utworzenie na ich podstawie bazy danych z odpowiednimi tabelami, za co odpowiada _Entity Framework Core_.

### Tworzenie modeli
W katalogu _Model_ projektu _.Domain_ tworzymy klasy z naszymi modelami. Modele będą wyglądać podobnie jak te, które pisaliśmy do tej pory. Będą to po prostu publiczne klasy zawierające publiczne właściwości, na podstawie których zostaną utworzone tabele w bazie danych (jedna klasa = jedna tabela, jedna właściwość klasy = jedna kolumna tabeli).
#### Mapowanie
##### Nazwy
Automatycznie, po mapowaniu, tabele w bazie danych będą miały takie nazwy jak nasze klasy, natomiast kolumny, takie jak nasze metody. Jeżeli jednak z jakiegoś powodu (choć nie jest to zalecane), chcielibyśmy, aby elementy w bazie danych miały inne nazwy niż odpowiadające im elementy w modelach, to z pomocą przychodzą nam odpowiednie atrybuty. Są to odpowiednio `TableAttribute` i `ColumnAttribute`, które przyjmują żądane nazwy, jako argumenty konstruktora. Np.:
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
##### Wskazanie klucza głównego
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
##### Relacje
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
Jak widać chcemy, do obiektów `Item` przyporządkowywać jakiś obiekt `Type`. Tak napisalibyśmy je do tej pory. _Entity Framework Core_ wymaga od nas jeszcze stworzenia właściwości na obiekty znajdujące się w relacji. Dzięki temu będziemy mogli np. sprawdzić `Name` obiektu `Type` powiązanego z naszym obiektem `Item` bez tworzenia dodatkowych zapytań. Do klasy `Item` musimy zatem dopisać jeszcze jedną linijkę:
```csharp =
public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int TypeId {get; set; }

    public virtual Type Type { get; set; }
}
```
Widzimy tu, że każdy obiekt `Item` jest powiązany z dokładnie jednym obiektem `Type`. Relacja jest jednak dwustronna. Tutaj mamy do czynienia z najbardziej podstawowym typem relacji, czyli tzw. relacja jeden do wielu. Oznacza to, że Jeden obiekt `Type` może być w relacji z wieloma obiektami `Item`. Musimy więc jeszcze zmodyfikować naszą klasę `Type`, aby muc w niej przechowywać powiązaną z danym obiektem `Type` kolekcję obiektów `Item`:
```csharp =
public class Type
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Item> Items { get; set; }
}
```
Wewnątrz naszej bazy danych na relację będzie jedynie wskazywać obecność kluczy (id) z tabeli _Type_ w tabeli _Item_. Jeżeli właściwość zawierającą taki klucz nazwiemy w tworzonej klasie według wzoru _NazwaTabeliZKtórejPochodziKluczNazwaWłaściwościKtóraJestKluczem_ tak jak to zrobiliśmy w naszym przypadku, _Entity Framework Core_ wie, że przechowujemy tu klucz obcy z tej a tej tabeli.


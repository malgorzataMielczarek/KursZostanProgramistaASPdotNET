# TYDZIEŃ 7 - Bazy danych
## Spis treści
### [LEKCJA 1 – Powitanie](#lekcja-1--powitanie-1)
### [LEKCJA 2 – Entity Framework](#lekcja-2--entity-framework-1)
### [LEKCJA 3 – Code first](#lekcja-3--code-first-1)
### [LEKCJA 4 – Zasady tworzenia baz danych](#lekcja-4--zasady-tworzenia-baz-danych-1)
### [LEKCJA 5 – Context](#lekcja-5--context-1)
### [LEKCJA 6 – Repository](#lekcja-6--repository-1)
### [LEKCJA 7 – Abstract](#lekcja-7--abstract-1)
### [LEKCJA 8 – Instalacja SQL Server](#lekcja-8--instalacja-sql-server-1)
### [LEKCJA 9 – Migracje](#lekcja-9--migracje-1)
### [LEKCJA 10 – Błędy początkujących](#lekcja-10--błędy-początkujących-1)
### [LEKCJA 11 – Praca domowa](#lekcja-11--praca-domowa-1)

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
Zanim powstał .NET Core mieliśmy do dyspozycji _Entity Framework_. Obecnie został on zastąpiony przez napisany od nowa ORM _Entity Framework Core_. Przynajmniej na razie nie zostały w nim zaimplementowane wszystkie funkcjonalności z ostatniej (6.) wersji _Entity Framework_. Najważniejsze elementy, takie jak kontekst bazy danych, DbSety, czyli obiekty, które mapowane są na tabele bazy danych (dzięki nim mamy prostą możliwość dodawania, usuwania i wyszukiwania elementów w bazie danych), oczywiście działają. Dodatkowo mamy możliwość migracji, co ułatwia podejście _Code First_. Nie mamy natomiast kilku elementów takich jak chociażby automatyczne migracje (podczas uruchamiania projektu), czy część funkcjonalności związanych z leniwym ładowaniem (_lazy loading_) dla relacyjnych tabel. Istnieją jednak sposoby na obejście tych braków. Jest jednak kilka rzeczy, których nie było w _Entity Framework_, a które dodano do _Entity Framework Core_. Jest to między innymi baza danych _In-Memory_, czyli prosty framework do testowania połączenia z bazą danych (Obecnie trwa dyskusja, czy programiści powinni testować części aplikacji w jakikolwiek sposób związane z _Entity Framework Core_. Są to bowiem de facto testy integracyjne, a nie jednostkowe.). Innym ważnym elementem, który dodano, jest standardowe wsparcie dla _Inversion of Control_, czyli dla implementacji _Dependenci Injection_, dzięki czemu można ją używać bez żadnych dodatkowych konfiguracji.

## [LEKCJA 3 – Code first](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-3-code-first/)
_Code first_ jest metodą tworzenia baz danych prawie całkowicie z poziomu kodu aplikacji.

Kiedyś standardowo mieliśmy bardzo duże rozróżnienie pomiędzy programistami webowymi a programistami baz danych. Oczywiście w dużych projektach wymagających optymalizacji praca programistów baz danych dalej jest nieoceniona. Jednak dzięki nowoczesnym narzędziom takim jak _Entity Framework Core_ w prostszych projektach programiści webowi radzą sobie sami, nawet przy niewielkiej znajomości SQL-a.

Kiedyś aby utworzyć bazę danych standardowo łączyliśmy się bezpośrednio z naszym silnikiem bazodanowym (np. Microsoft SQL Server np. przez Microsoft SQL Server Management Studio) i tworzyliśmy tam bezpośrednio nową bazę danych i nasze tabele. Następnie można było wrócić do kodu naszej aplikacji, gdzie przy pomocy _Entity Framework_ łączyliśmy się z utworzoną bazą danych i generowaliśmy modele. Takie podejście nazywa się _database first_. Czyli najpierw tworzymy wszystkie tabele i relacje między nimi w naszej bazie danych, a dopiero potem przechodzimy do kodu. Programistą jednak takie podejście się nie podobało. Nie chcieli oni być skazani na pracę bezpośrednio na silniku bazodanowym, a zlecanie tworzenia klas modeli zewnętrznemu narzędzi. Szczególnie, że _Entity Framework_ działało według własnych schematów i wygenerowane przy jego pomocy klasy nie do końca odpowiadały wymaganiom programistów. Trzeba je było zatem jeszcze później poprawiać. Dlatego w nowej wersji _Entity Framework_ (_Core_) zaimplementowano podejście _code first_.

Podejście _code first_ pozwala na napisanie przez programistów modeli w naszej aplikacji i automatyczne utworzenie na ich podstawie bazy danych z odpowiednimi tabelami, za co odpowiada _Entity Framework Core_.

### Tworzenie modeli
W katalogu _Model_ projektu _.Domain_ tworzymy klasy z naszymi modelami. Modele będą wyglądać podobnie jak te, które pisaliśmy do tej pory. Będą to po prostu publiczne klasy zawierające publiczne właściwości, na podstawie których zostaną utworzone tabele w bazie danych (jedna klasa = jedna tabela, jedna właściwość klasy = jedna kolumna tabeli).
#### Mapowanie
##### Nazwy
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

## [LEKCJA 4 – Zasady tworzenia baz danych](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-4-zasady-tworzenia-baz-danych/)
### Klucze
W bazie danych występują tzw. klucze, czyli identyfikatory elementów w bazie danych. Klucze muszą być unikatowe. Tzn. w obrębie jednej tabeli wartość klucza dla każdego wiersz musi być inna. Wyróżniamy dwa podstawowe rodzaje kluczy: główny i obcy.
#### Klucz główny
Jest to identyfikator elementów w danej tabeli. Najczęściej będzie to nasze id. Za każdym razem dodając nowy element do bazy danych będziemy sobie taki klucz inkrementować, tak aby różnił się od tych będących już w tabeli. Oczywiście nie musi być to koniecznie `int`. Klucz może być dowolnego typu. Ważne, aby jego wartość była unikatowa. **W tabeli nie mogą znajdować się dwa elementy o takim samym kluczu.**
#### Klucz obcy
Używa się go do tworzenia relacji pomiędzy danymi. Klucz obcy jest to klucz główny innego elementu (najczęściej z innej tabeli).
### Relacje
Pomiędzy danymi w bazach danych mogą występować trzy rodzaje relacji: jeden do jednego, jeden do wielu i wiele do wielu.
#### Jeden do jednego
Relacja mówiąca, że element z jednej tabeli może mieć relację tylko i wyłącznie z jednym elementem z drugiej tabeli. Składa się z tabeli głównej (_principal_, _parent_) i zależnej (_dependent_, _child_). Tabela zależna, jest to tabela posiadająca klucz obcy. Jeżeli dopiero tworzymy bazę danych, to jako tabelę zależną powinniśmy wybrać tabele, której istnienie niema logicznego sensu bez tabeli głównej. Jeżeli pomiędzy tabelami występuje naturalna relacja dziecko-rodzic, to "dziecko" powinno być tabelą zależną, a "rodzic" główną.
##### Przykład
Załóżmy, że klientami naszej aplikacji są firmy. Każda z nich ma wyznaczoną dokładnie jedną osobę do kontaktu. Klienci, będą więc naszym modelem głównym. Istnienie informacji kontaktowych dla nieistniejącego klienta niema bowiem sensu. Tak więc klasa wskazująca osobę do kontaktu będzie zawierać klucz obcy (klucz główny klienta). Np.:
```csharp =
// model glowny
public class Customer
{
    public int Id { get; set; } // klucz glowny
    public string Name { get; set; }
    public string NIP { get; set; }

    // Relacja jeden do jednego
    public CustomerContactInformation CustomerContactInformation { get; set; } // powiązany obiekt, nawigacja referencyjna
}
```
```csharp =
// model zalezny
public class CustomerContactInformation
{
    public int Id { get; set; } // klucz glowny
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Position { get; set; }

    // Relacja jeden do jednego
    public int CustomerRef { get; set; } // referencja do klienta, klucz obcy
    public Customer Customer { get; set; } // powiązany obiekt, nawigacja referencyjna
}
```
Dodatkowo będziemy musieli stworzyć klasę definiującą na podstawie jakich pól osiągamy relacją. Zajmiemy się tym jednak w kolejnej lekcji, podczas omawiania tworzenia kontekstów.
#### Jeden do wielu
Tą relację omówiliśmy już pokrótce w poprzedniej lekcji. Jest to najczęściej występujący typ relacji. Dlatego jest on wspierany przez _Entity Framework Core_. Dzięki temu nie wymaga dodatkowego tworzenia kontekstu, o ile odpowiednio nazwiemy właściwości. Relacja jeden do wielu oznacza, że element z jednej tabeli, jest powiązany z jednym lub więcej elementami z drugiej tabeli. Element z drugiej tabeli musi być natomiast powiązany z dokładnie jednym elementem z pierwszej tabeli.
##### Przykład
Przykład tej relacji omawialiśmy w poprzedniej lekcji. Dla przypomnienia, element `Item` jest jakiegoś typu `Type` (powiązany z elementem `Type`). Może istnieć wiele elementów `Item` tego samego typu `Type` (jeden element `Type` może być powiązany z wieloma elementami `Item`):
```csharp =
public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }

    // Relacja jeden do wielu
    public int TypeId {get; set; } // referencja do powiązanego obiektu, klucz obcy
    public virtual Type Type { get; set; } // powiązany obiekt, nawigacja referencyjna
}
```
```csharp =
public class Type
{
    public int Id { get; set; }
    public string Name { get; set; }

    // Relacja jeden do wielu
    public virtual ICollection<Item> Items { get; set; } // kolekcja powiązanych obiektów, nawigacja po kolekcji
}
```
#### Wiele do wielu
Oznacza, że element z jednej tabeli może być powiązany z wieloma elementami z drugiej tabeli. Element z drugiej tabeli również może być powiązany z wieloma elementami z pierwszej tabeli. Jest to najtrudniejszy do zrealizowania w bazie danych typ. Wymaga on bowiem utworzenia dodatkowej tabeli, której zadaniem będzie tworzenie połączeń między parą obiektów.
##### Przykład
Załóżmy, że aby ułatwić filtrowanie obiektów `Item` w naszej aplikacji, do każdego z nich przypiszemy zestaw tagów. Każdy `Item` może mieć przypisane wiele tagów. Każdy tag może mieć przypisanych wiele obiektów `Item`. Jest więc to relacja wiele do wielu. Stwórzmy więc odpowiednie klasy.
```csharp =
public class ItemTag // klasa pomocnicza
{
    // Relacja wiele do wielu
    public int ItemId { get; set; } // klucz obcy, klucz główny obiektu Item
    public Item Item { get; set; } // jeden z obiektow Item powiazanych z Tag
    public int TagId {get; set; } // klucz obcy, klucz główny obiektu Tag
    public virtual Tag Tag { get; set; } // jeden z obiektow Tag powiazanych z Item
}
```
```csharp =
public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }

    // Relacja jeden do wielu
    public int TypeId {get; set; } // referencja do powiązanego obiektu, klucz obcy
    public virtual Type Type { get; set; } // powiązany obiekt

    // Relacja wiele do wielu
    public ICollection<ItemTag> ItemTags { get; set; } // kolekcja z powiazaniami
}
```
```csharp =
public class Tag
{
    public int Id { get; set; }
    public string Name { get; set; }

    // Relacja wiele do wielu
    public ICollection<ItemTag> ItemTags {get; set; } // kolekcja z powiązaniami
}
```
Poza stworzeniem odpowiednich właściwości w klasach, podobnie jak w przypadku relacji jeden do jednego, konieczne będzie jeszcze stworzenie odpowiednich kontekstów, o czym w następnej lekcji.
### Normalizacja bazy danych
Normalizacja jest to proces organizowania danych w bazie danych. Ma ona na celu wyeliminowanie powtarzających się danych, zwiększenie elastyczności bazy danych, ułatwienie zarządzania nią i aktualizacji danych. Istnieją trzy podstawowe zasady normalizacji (zwane formami normalnymi). Istnieje jeszcze czwarta (zwana postacią normalną Boyce'a-Codda - _Boyce-Codd Normal Form (BCNF)_) i piąta postać normalna. Są one jednak sporadycznie rozważane w praktyce. O bazie danych spełniającej pierwszą zasadę mówi się, że jest w pierwszej formie normalnej. Jeśli spełnia trzy pierwsze reguły, mówi się, że jest w trzeciej formie normalnej itd.
#### Pierwsza forma normalna
* Wyeliminuj powtarzające się grupy w indywidualnych tabelach.
* Stwórz osobną tabelę dla każdego zestawu powiązanych danych.
* Stwórz klucz identyfikujący każdy zestaw powiązanych danych.

Nie używaj w pojedynczej tabeli wielu pól do przechowywania podobnych danych.
##### Przykład
Każdy z naszych klientów ma jakiś adres. Moglibyśmy więc do naszej tabeli klient dodać odpowiednie pola: miasto, adres, kod pocztowy itp. Załóżmy teraz jednak, że nasz klient ma dwa adresy (np. dwie filie firmy). Moglibyśmy więc w każdym z pól wpisać odpowiednio dwa miasta, dwa adresy itd. oddzielone np. przecinkami. Zmniejszy to jednak czytelność naszych danych. Moglibyśmy również, przewidując taką sytuację, od razu w naszej bazie danych stworzyć po drugim polu: miasto2, adres2 itd. Co się jednak stanie gdy w pewnym momencie będziemy potrzebować trzech adresów? Wówczas będziemy musieli przebudowywać nie tylko bazę danych, ale również korzystające z niej aplikacje. Zgodnie z pierwszą formą normalną w tabeli klientów mielibyśmy po jednym polu na miasto, adres itd. Kolejne adresy będzie się natomiast dodawać jako nowe rekordy. Czyli zamiast:
Id|Name|NIP|City1|Address1|ZIP1|City2|Address2|ZIP2
--:|:-:|:-:|:--|:--|:--|:--|:--|:--
1|Examplex|154742373|Przykładno|ul. Źródłowa 15f/4|54-294|Probowo|up. Druga 2|93-850

będziemy mieć:
Id|Name|NIP|City|Address|ZIP
--:|:-:|:-:|:--|:--|:--
1|Examplex|154742373|Przykładno|ul. Źródłowa 15f/4|54-294
1|Examplex|154742373|Probowo|up. Druga 2|93-850

W tym przykładzie będziemy musieli mieć klucz złożony.
#### Druga forma normalna
* Stwórz osobne tabele dla zbiorów danych, odnoszących się do wielu rekordów.
* Powiąż te tabele przy pomocy klucza obcego.

Rekordy (wiersze tabeli) nie powinny zależeć od niczego innego, niż od klucza głównego.
##### Przykład
Znormalizujmy dalej naszą przykładową tabelę do drugiej postaci normalnej. Wyodrębnijmy więc adresy do oddzielnej tabeli. Czyli zamiast tabeli Customers:
Id|Name|NIP|City|Address|ZIP
--:|:-:|:-:|:--|:--|:--
1|Examplex|154742373|Przykładno|ul. Źródłowa 15f/4|54-294
1|Examplex|154742373|Probowo|up. Druga 2|93-850

będziemy mieć dwie tabele, Customers:
Id|Name|NIP
--:|:-:|:-:
1|Examplex|154742373

i Addresses:
Id|City|Address|ZIP|CustomerId
--:|:-|:--|:--|:--:
1|Przykładno|ul. Źródłowa 15f/4|54-294|1
2|Probowo|up. Druga 2|93-850|1

powiązane relacją jeden do wielu.
#### Trzecia forma normalna
* Wyeliminuj dane, które nie zależą od klucza głównego.

Miejsce wartości w tabeli, które nie zależą od klucza, nie jest w tej tabeli. Generalnie, jeżeli istnieje prawdopodobieństwo, że zawartość grupy pól może mieć zastosowanie do więcej niż jednego rekordu w tabeli, należy zastanowić się nad umieszczeniem ich w osobnej tabeli.
##### Przykład
Gdybyśmy chcieli znormalizować nasz przykład do trzeciej postaci normalnej moglibyśmy wyodrębnić np. nazwy miast, kody pocztowe i nazwy ulic do osobnych tabeli, gdyż możemy mieć wiele adresów z tego samego miasta, kodu pocztowego i tej samej ulicy. Czyli zamiast Addresses:
Id|City|Address|ZIP|CustomerId
--:|:-|:--|:--|:--:
1|Przykładno|ul. Źródłowa 15f/4|54-294|1
2|Probowo|up. Druga 2|93-850|1

mielibyśmy Cities:
Id|Name
--:|:--
1|Przykładno
2|Probowo

Zips:
Id|No|CityId
--:|:--|:-:
1|54-294|1
2|93-850|2

Streets:
Id|Name|ZIPId
--:|:--|:--:
1|ul. Źródłowa|1
2|ul. Druga|2

i Addresses:
Id|StreetId|Local|CustomerId
--:|:-:|:--|:--:
1|1|15f/4|1
1|2|2|1

| :warning:**UWAGA!** |
| :---: |
|Do powyższych zasad należy się stosować, gdy ma to sens. Trzecią formę normalną stosuje się przykładowo tylko dla często zmieniających się danych. Np. przeprowadzona w powyższym przykładzie normalizacja do trzeciej postaci normalnej niema sensu. Miałaby ono sens, gdyby nazwy ulic, czy miejscowości często się zmieniały. Wówczas takie poprawki musielibyśmy nanieść tylko w jednym miejscu, zamiast w wielu rekordach. Ponieważ jednak zdarza się to sporadycznie i prawdopodobieństwo konieczności naniesienia takich poprawek jest bliskie zeru, więc lepiej pozostawić naszą tabelę Addresses w drugiej postaci normalnej. Szczególnie, że wiele małych tabel może obniżać wydajność lub przekraczać pojemność otwartych plików i pamięci. Wyodrębnienie nazw miast, kodów pocztowych itd. mogłoby mieć jeszcze sens, gdybyśmy mieli w bazie z góry zdefiniowaną listę miast, kodów itp. Wówczas wprowadzając nowy adres klienta będziemy np. wybierać dane z rozwijanej listy, zamiast wpisywać ręcznie, co zmniejszy ryzyko błędu.|
#### Konwencje
Aby _EF Core_ automatycznie wykrywał relacje i dokonywał mapowania, pisany przez nas kod musi przestrzegać ustalonych konwencji.
##### Wykrywanie nawigacji
Wykrywanie relacji rozpoczyna się wykrywaniem nawigacji pomiędzy typami encji.
###### Referencje
Właściwość typu encji jest wykrywana jako nawigacja referencyjna gdy:
* Właściwość jest publiczna (`public`).
* Właściwość ma getter i setter.
    * Setter nie musi być publiczny, może być prywatny lub mieć inny modyfikator dostępu.
    * Setter może być `init`.
* Typ właściwości jest, lub może być, typem encji. Oznacza to, że typ właściwości:
    * Musi być typem referencyjnym.
    * Nie może być jawnie skonfigurowany jako prymitywny typ danych (`bool`, `int`, `string` itd.).
    * Nie może być zmapowany jako prymitywny typ danych przez używany silnik bazodanowy.
    * Nie może być automatycznie konwertowalny do prymitywnego typu danych  mapowanego przez użyty silnik bazodanowy.
* Właściwość nie jest statyczna.
* Właściwość nie jest właściwością indeksatora.
####### Przykład
```csharp =
public class Blog
{
    // Not discovered as reference navigations:
    public int Id { get; set; }
    public string Title { get; set; } = null!;
    public Uri? Uri { get; set; }
    public ConsoleKeyInfo ConsoleKeyInfo { get; set; }
    public Author DefaultAuthor => new() { Name = $"Author of the blog {Title}" };

    // Discovered as a reference navigation:
    public Author? Author { get; private set; }
}

public class Author
{
    // Not discovered as reference navigations:
    public Guid Id { get; set; }
    public string Name { get; set; } = null!;
    public int BlogId { get; set; }

    // Discovered as a reference navigation:
    public Blog Blog { get; init; } = null!;
}
```
_Źródło: https://learn.microsoft.com/en-us/ef/core/modeling/relationships/conventions#reference-navigations_

W powyższym przykładzie `Blog.Author` i `Author.Blog` są rozpoznane jako nawigacje referencyjne (_reference navigations_). Z drugiej strony, pozostałe właściwości nie zostały rozpoznane jako nawigacje, gdyż:
* `Blog.Id`, bo `int` jest mapowany jako typ prymitywny
* `Blog.Title`, bo `string` jest mapowany jako typ prymitywny
* `Blog.Uri`, bo `Uri` jest automatycznie konwertowany do mapowanego typu prymitywnego
* `Blog.ConsoleKeyInfo`, bo `ConsoleKeyInfo` jest typem wartościowym C#
* `Blog.DefaultAuthor`, bo właściwość nie ma settera
* `Author.Id`, bo `Guid` jest mapowany do typu prymitywnego
* `Author.Name`, bo `string` jest mapowany do typu prymitywnego
* `Author.BlogId`, bo `int` jest mapowany do typu prymitywnego

###### Kolekcje
Właściwość typu encji jest wykrywana jako nawigacja po kolekcji gdy:
* Właściwość jest publiczna.
* Właściwość ma getter. Właściwość może mieć również setter, jednak nie jest on wymagany.
* Typem właściwości jest lub implementuje `IEnumerable<TEntity>`, gdzie `TEntity` jest lub mogłoby być typem encji. To oznacza, że `TEntity`:
    * Musi być typem referencyjnym.
    * Nie może być jawnie skonfigurowany jako prymitywny typ danych (`bool`, `int`, `string` itd.).
    * Nie może być zmapowany jako prymitywny typ danych przez używany silnik bazodanowy.
    * Nie może być automatycznie konwertowalny do prymitywnego typu danych  mapowanego przez użyty silnik bazodanowy.
* Właściwość nie jest statyczna.
* Właściwość nie jest właściwością indeksatora.
####### Przykład
```csharp =
public class Blog
{
    public int Id { get; set; }
    public List<Tag> Tags { get; set; } = null!;
}

public class Tag
{
    public Guid Id { get; set; }
    public IEnumerable<Blog> Blogs { get; } = new List<Blog>();
}
```
_Źródło: https://learn.microsoft.com/en-us/ef/core/modeling/relationships/conventions#collection-navigations_

W powyższym przykładzie zarówno `Blog.Tags`, jak i `Tag.Blogs` są wykryte jako nawigacje po kolekcjach.
###### Parowanie nawigacji
Kiedy wykryto już nawigację (np. dla encji `A` do encji `B`), to jest sprawdzane, czy istnieje nawigacja w drugą stronę (dla `B` do `A`). Jeżeli tak, to obie nawigacje są ze sobą parowane, tworząc jedną relację dwukierunkową. Typ relacji zależy, czy nawigacja i odwrotna nawigacja są referencjami, czy kolekcjami. Dokładniej:
* Jeśli jedna nawigacja jest kolekcją, a druga referencją, to jest to relacja jeden do wielu.
```csharp =
public class Blog
{
    public int Id { get; set; }
    public ICollection<Post> Posts { get; } = new List<Post>(); // nawigacja po kolekcji
}

public class Post
{
    public int Id { get; set; }
    public int? BlogId { get; set; }
    public Blog? Blog { get; set; } // nawigacja referencyjna
}
```
_Źródło: https://learn.microsoft.com/en-us/ef/core/modeling/relationships/conventions#pairing-navigations_
* Jeśli obie nawigacje są referencyjne, to jest to relacja jeden do jednego.
```csharp =
public class Blog
{
    public int Id { get; set; }
    public Author? Author { get; set; } // nawigacja referencyjna
}

public class Author
{
    public int Id { get; set; }
    public int? BlogId { get; set; }
    public Blog? Blog { get; set; } // nawigacja referencyjna
}
```
_Źródło: https://learn.microsoft.com/en-us/ef/core/modeling/relationships/conventions#pairing-navigations_
* Jeżeli obie nawigacje są kolekcjami, to jest to relacja wiele do wielu.
```csharp =
public class Post
{
    public int Id { get; set; }
    public ICollection<Tag> Tags { get; } = new List<Tag>(); // nawigacja po kolekcji
}

public class Tag
{
    public int Id { get; set; }
    public ICollection<Post> Posts { get; } = new List<Post>(); // nawigacja po kolekcji
}
```
_Źródło: https://learn.microsoft.com/en-us/ef/core/modeling/relationships/conventions#pairing-navigations_

| :warning:**UWAGA!** |
| :---: |
|Parowanie nawigacji może być niepoprawne, jeżeli dwie nawigacje reprezentują dwie różne jednokierunkowe relacje. W tym wypadku, te dwie relacje muszą być jawnie skonfigurowane w kontekście (o czym w następnej lekcji).|
|Parowanie relacji działa tylko, gdy jest tylko jedna relacja pomiędzy dwoma typami. Wiele relacji między dwoma typami musi być jawnie skonfigurowanych.|
|Powyższe przykłady dotyczyły relacji między dwoma różnymi typami. Jednak, jest również możliwe, aby ten sam typ był na obu końcach relacji (jeden element tego typu był w relacji z innym elementem tego typu). Pojedynczy typ może więc mieć dwie nawigacje sparowane ze sobą nawzajem. Nazywamy to _self-referencing relationship_.|

##### Wykrywanie właściwości z kluczami obcymi
Gdy nawigacje dla relacji zostaną wykryte lub jawnie skonfigurowane, służą one do wykrycia odpowiednich właściwości klucza obcego dla relacji. Właściwość jest wykryta jako klucz obcy gdy:
* Typ właściwości jest kompatybilny z kluczem głównym lub alternatywnym typu głównej encji.
    * Typy są kompatybilne, gdy są takie same lub gdy typ klucza obcego jest nullowalną wersją typu klucza głównego lub alternatywnego.
* Nazwa właściwości pasuje do jednej z konwencji nazewnictwa właściwości klucza obcego. Konwencje nazewnicze to:
    * `<nazwa właściwości nawigacji><nazwa właściwości klucza głównego>`
    * `<nazwa właściwości nawigacji><Id\id\ID>`
    * `<nazwa typu encji głównej><nazwa właściwości klucza głównego>`
    * `<nazwa typu encji głównej><Id\id\ID>`
* Dodatkowo, jeżeli koniec zależny został jawnie skonfigurowany przy użyciu API do budowy modeli i klucz główny encji zależnej jest kompatybilny, wówczas klucz główny końca zależnego zostanie również użyty jako klucz obcy.
###### Przykłady
_Źródło: https://learn.microsoft.com/en-us/ef/core/modeling/relationships/conventions#discovering-foreign-key-properties_

Przykład dla konwencji nazewniczej `<nazwa właściwości nawigacji><nazwa właściwości klucza głównego>`:
```csharp =
public class Blog
{
    public int Key { get; set; }
    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }
    public int? TheBlogKey { get; set; }
    public Blog? TheBlog { get; set; }
}
```
Przykład dla konwencji nazewniczej `<nazwa właściwości nawigacji><Id\id\ID>`:
```csharp =
public class Blog
{
    public int Key { get; set; }
    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }
    public int? TheBlogID { get; set; }
    public Blog? TheBlog { get; set; }
}
```
Przykład dla konwencji nazewniczej `<nazwa typu encji głównej><nazwa właściwości klucza głównego>`:
```csharp =
public class Blog
{
    public int Key { get; set; }
    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }
    public int? BlogKey { get; set; }
    public Blog? TheBlog { get; set; }
}
```
Przykład dla konwencji nazewniczej `<nazwa typu encji głównej><Id\id\ID>`:
```csharp =
public class Blog
{
    public int Key { get; set; }
    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }
    public int? Blogid { get; set; }
    public Blog? TheBlog { get; set; }
}
```
| :warning:**UWAGA!** |
| :---: |
|W przypadku nawigacji jeden do wielu, właściwości klucza obcego muszą być w tym samym typie, co nawigacja referencyjna, gdyż to będzie encja zależna.|
|W przypadku relacji jeden do jednego, wykrycie klucza obcego jest użyte do określenia który typ reprezentuje stronę zależną relacji. Jeśli nie wykryto właściwości klucza obcego, koniec zależny musi zostać skonfigurowany przy użyciu metody `HasForeignKey`.|
##### Wykrywanie liczebności relacji (typu)
EF używa wykrytych właściwości nawigacji i klucza obcego do ustalenia liczebności relacji, wraz z ich końcami (głównym i zależnym).
* Jeśli występuje jedna, niesparowana nawigacja referencyjna, wówczas relacja jest konfigurowana jako jednokierunkowa, jeden do wielu, z nawigacją referencyjną na końcu zależnym.
* Jeśli występuje jedna niesparowana nawigacja po kolekcji, wówczas relacja jest konfigurowana jako jednokierunkowa, jeden do wielu, z nawigacją po kolekcji na końcu głównym.
* Jeśli występują sparowane nawigacje referencyjna i po kolekcji, wówczas relacja jest konfigurowana jako dwukierunkowa, jeden do wielu, z nawigacją po kolekcji na końcu głównym.
* Jeśli nawigacja referencyjna jest sparowana z inną nawigacją referencyjną, wówczas:
    * Jeśli wykryto właściwość klucza obcego tylko po jednej stronie, to relacja jest konfigurowana jako dwukierunkowa, jeden do jednego, z właściwością klucza obcego na końcu zależnym.
    * Jeśli nie wykryto właściwości klucza obcego, lub wykryto ją na obu końcach, strona zależna nie może zostać określona. EF wyrzuca wówczas wyjątek wskazujący, że koniec zależny musi być jawnie skonfigurowany.
* Jeśli nawigacja po kolekcji jest sparowana z inną nawigacją po kolekcji wówczas relacja jest konfigurowana jako dwukierunkowa, wiele do wielu.
##### Właściwości cienia klucza obcego
Jeśli EF wykrył koniec zależny relacji, ale nie znalazł właściwości klucza obcego, wówczas tworzy tzw. właściwość cienia (ang. _shadow property_), reprezentującą klucz obcy.

**Właściwości cienia** są to właściwości, które nie są zdefiniowane w naszych .NET-owych klasach encji, ale są zdefiniowane dla tych typów encji w modelach EF Core. Wartość i stan tych właściwości są utrzymywane wyłącznie w _Change Tracker_. Są one użyteczne, gdy w bazie danych znajdują się dane, które nie powinny być ujawnione na zmapowanych typach encji.

Właściwość cienia klucza obcego:
* Ma typ właściwości klucza głównego lub alternatywnego, głównego końca relacji.
    * Typ jest domyślnie ustawiany jako nullowalny, co sprawia, że relacja jest domyślnie opcjonalna.
* Jest nazywana używając nazwy nawigacji końca zależnego połączonej z nazwą właściwości klucza obcego lub alternatywnego, pod warunkiem, że na końcu zależnym jest nawigacja.
* Jeśli na końcu zależnym nie ma nawigacji, wówczas nazwa jest tworzona przez połączenia nazwy typu (klasy) głównej encji i właściwości klucza głównego lub alternatywnego.
##### Usuwanie kaskadowe
Zgodnie z konwencją, wymagane relacje (te gdzie typ właściwości klucza obcego nie jest nullowalny) są konfigurowane do usuwania kaskadowego (ang. _cascade delete_). Relacje opcjonalne (te gdzie typ właściwości klucza obcego jest nullowalny) są konfigurowane, aby nie usuwały się kaskadowo.

**Usuwanie kaskadowe** oznacza, że gdy usuniemy główną encję (rodzica), to encja od niego zależna (dziecko) również zostanie usunięta. Jeżeli dziecko, jest encją główną dla innej encji zależnej (i mamy dla nich również skonfigurowane usuwanie kaskadowe), to wówczas ta encja zależna również jest usuwana itd.
##### Wiele do wielu
Relacja wiele do wielu nie ma końca głównego i zależnego. Żadna strona nie posiada klucza obcego. Zamiast tego używa typu encji łączenia. Zawiera ona pary kluczy obcych wskazujących na każdy z końców relacji wiele do wielu.
###### Przykład
```csharp =
public class Post
{
    public int Id { get; set; }
    public ICollection<Tag> Tags { get; } = new List<Tag>();
}

public class Tag
{
    public int Id { get; set; }
    public ICollection<Post> Posts { get; } = new List<Post>();
}
```
_Źródło: https://learn.microsoft.com/en-us/ef/core/modeling/relationships/conventions#many-to-many_

Do tworzonej encji łączenia zastosowanie mają następujące konwencje:
* Typ encji łączenia zostanie nazwany zgodnie z konwencją `<nazwa typu lewej encji><nazwa typu prawej encji>`, czyli `PostTag`.
    * Tabela łączenia, która zostanie utworzona w bazie danych, będzie miała taką samą nazwę jak typ encji łączenia.
* Typ encji łączenia będzie mieć właściwości klucza obcego dla każdego kierunku relacji. Są one nazwane zgodnie z konwencją `<nazwa nawigacji><nazwa klucza głównego>`, czyli `PostsId` i `TagsId`.
    * Dla jednokierunkowych relacji wiele do wielu, właściwość klucza obcego bez powiązanej nawigacji jest nazywana zgodnie z konwencją `<nazwa typu głównej encji><nazwa klucza głównego>`.
* Właściwości kluczy obcych są nienullowalne. Obie relacje encji łączenia są więc wymagane.
    * Konwencje usuwania kaskadowego oznaczają, że te relacje zostaną skonfigurowane do usuwania kaskadowego.
* Typ encji łączenia jest skonfigurowany z kluczem głównym złożonym, który składa się z dwóch właściwości kluczy obcych, czyli z `PostsId` i `TagsId`.

## [LEKCJA 5 – Context](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-5-context/)
Kontekst jest to specjalna klasa wymagana przez _Entity Framework Core_ definiująca tabele i relacje jakie mają zostać utworzone w bazie danych. Pozwala ona na zmapowanie stworzonych przez nas modeli domenowych na tabele bazodanowe. Tworzy sesję z bazą danych i umożliwia wysyłanie zapytań, pobieranie i modyfikację danych w bazie danych.

Kontekst zależy od typu używanego silnika bazodanowego (np. od paczki _Microsoft.EntityFrameworkCore.SqlServer_) oraz od użytego ORM-a (np. _Entity Framework Core_). Dlatego też utworzymy go w projekcie _.Infrastructure_ (bezpośrednio w projekcie, a nie w folderze). Dzięki temu nasze modele mogą być bez problemu używane przez aplikację, nawet jeśli w pewnym momencie zdecydujemy się na użycie innego silnika bazodanowego, czy innego ORM-a. Będziemy wówczas musieli jedynie podmienić nasz projekt _.Infrastructure_, bez wpływu na resztę aplikacji.

Stworzymy zwykłą publiczną klasę i nazwiemy ją `Context`. W przypadku _Entity Framework Core_ musi ona dziedziczyć po klasie _Microsoft.EntityFrameworkCore.DbContext_. Zawiera ona najbardziej podstawową implementację kontekstu. Ponieważ jednak zaznaczyliśmy, że chcemy przeprowadzać autoryzację użytkowników przy pomocy _Individual Accounts_, to nasza klasa `Context` będzie dziedziczyć po bardziej rozbudowanej klasie bazowej `Microsoft.AspNetCore.Identity.EntityFrameworkCore.IdentityDbContext`. Poza implementacją podstawowych funkcjonalności kontekstu, zawiera ona odpowiednią implementację wspierającą ten typ autoryzacji. Stwórzmy więc odpowiednią klasę, np.:
```csharp =
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using TitlesOrganizer.Domain.Models;

namespace TitlesOrganizer.Infrastructure
{
    public class Context : IdentityDbContext
    {
    }
}
```
Będzie to wymagało doinstalowania jeszcze jednej paczki (_Microsoft.AspNetCore.Identity.EntityFrameworkCore_).

W naszej nowej klasie musimy utworzyć jeszcze odpowiedni konstruktor, który umożliwi inicjalizację podstawowych funkcjonalności kontekstu zaimplementowaną w konstruktorze bazowej klasy `DbContext`:
```csharp =
public Context(DbContextOptions options) : base(options)
{
}
```
Jeżeli nasza baza danych może być zmapowana wyłącznie przy użyciu konwencji _Entity Framework Core_ to wystarczy jeszcze tylko dodać właściwości z naszymi DbSetami.
### `DbSet<TEntity>`
Jest to klasa pozwalająca na zapytywanie i zapisywanie instancji `TEntity`. Zapytania LINQ wykonywane na `DbSet<TEntity>` będą tłumaczone na zapytania na bazie danych. Pozwala więc nam na utworzenie na podstawie modeli domenowych odpowiednich tabel w bazie danych i wykonywanie na nich operacji. W naszej klasie `Context` tworzymy więc właściwość `DbSet` dla każdej tabeli bazodanowej (dla każdego modelu domenowego). Np. jeśli mamy modele

`Book` (książka):
```csharp =
namespace TitlesOrganizer.Domain.Models
{
    public class Book
    {
        public int Id { get; set; }
        public string? Title { get; set; }
        public string? OriginalTitle { get; set; }
        public string? OriginalLanguage { get; set; }
        public int? Year { get; set; }
        public string? Edition { get; set; }
        public string? Description { get; set; }

        // relacja jeden do jeden
        public Cover? Cover { get; set; }

        // relacja jeden do wielu
        public int? SeriesId { get; set; }
        public Series? Series { get; set; }

        // relacje wiele do wielu
        public virtual ICollection<BookAuthor>? BookAuthors { get; set; }
        public virtual ICollection<BookGenre>? BookGenres { get; set; }
    }
}
```
`Cover` (okładka książki):
```csharp =
namespace TitlesOrganizer.Domain.Models
{
    public class Cover
    {
        public int Id { get; set; }
        public string? Name { get; set; }
        public string? Description { get; set; }

        // relacja jeden do wielu
        public int DesignerId { get; set; }
        public Designer Designer { get; set; }

        // relacja jeden do jednego
        public int BookId { get; set; }
        public Book Book { get; set; }
    }
}
```
`Designer` (twórca okładki):
```csharp =
namespace TitlesOrganizer.Domain.Models
{
    public class Designer
    {
        public int Id { get; set; }
        public string? Name { get; set; }
        public string? LastName { get; set; }
        public string? Pseudonim { get; set; }

        public virtual ICollection<Cover> Covers { get; set; }
    }
}
```
`Author` (autor książki):
```csharp =
namespace TitlesOrganizer.Domain.Models
{
    public class Author
    {
        public int Id { get; set; }
        public string? Name { get; set; }
        public string? LastName { get; set; }

        public virtual ICollection<BookAuthor>? BookAuthors { get; set; }
    }
}
```
`Genre` (gatunek do którego należy książka):
```csharp =
namespace TitlesOrganizer.Domain.Models
{
    public class Genre
    {
        public int Id { get; set; }
        public required string Name { get; set; }

        public virtual ICollection<BookGenre>? BookGenres { get; set; }
    }
}
```
`Series` (do tworzenia serii książek, np. trylogie):
```csharp =
namespace TitlesOrganizer.Domain.Models
{
    public class Series
    {
        public int Id { get; set; }
        public string? Title { get; set; }
        public string? Description { get; set; }
        public int? ExpectedLength { get; set; }

        public ICollection<Book>? Books { get; set; }
    }
}
```
oraz pomocnicze klasy do stworzenia relacji wiele do wielu między książką, a autorem (`BookAuthor`):
```csharp =
namespace TitlesOrganizer.Domain.Models
{
    public class BookAuthor
    {
        public int BookId { get; set; }
        public Book Book { get; set; }
        public int AuthorId { get; set; }
        public Author Author { get; set; }
    }
}
```
i książką a gatunkiem (`BookGenre`):
```csharp =
namespace TitlesOrganizer.Domain.Models
{
    public class BookGenre
    {
        public int BookId { get; set; }
        public Book Book { get; set; }
        public int GenreId { get; set; }
        public Genre Genre { get; set; }
    }
}
```
W klasie `Context` tworzymy dla nich odpowiednie właściwości `DbSet`:
```csharp =
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using TitlesOrganizer.Domain.Models;

namespace TitlesOrganizer.Infrastructure
{
    public class Context : IdentityDbContext
    {
        public DbSet<Author> Authors { get; set; }
        public DbSet<Book> Books { get; set; }
        public DbSet<BookAuthor> BookAuthor { get; set; }
        public DbSet<BookGenre> BookGenre { get; set; }
        public DbSet<Cover> Covers { get; set; }
        public DbSet<Designer> Designers { get; set; }
        public DbSet<Genre> Genres { get; set; }
        public DbSet<Series> Serieses { get; set; }

        public Context(DbContextOptions options) : base(options)
        {
        }
    }
}
```
Tradycyjnie nazwa właściwości `DbSet` odpowiada liczbie mnogiej nazwy modelu, którego dotyczy. Wyjątek stanowią modele pomocnicze do tworzenia relacji wiele do wielu, w których używamy liczby pojedynczej, właśnie, aby wyróżnić, że są to tabele z relacjami.

Gdyby wszystkie klasy i relacje były utworzone zgodnie z konwencją, to na tym moglibyśmy zakończyć tworzenie naszej klasy `Context`. Jak już jednak wspominaliśmy konwencja wspiera wyłącznie tworzenie relacji jeden do wielu (już nie tylko, ale nadal może istnieć konieczność skonfigurowania rzeczy niezgodnych z konwencją lub przez nią nieuwzględnionych, np. jednokierunkowa relacja wiele do wielu). W naszym przykładzie występują jednak również relacje jeden do jednego i wiele do wielu. Musimy więc je jeszcze zdefiniować (nie trzeba już jawnie konfigurować relacji jeden do jednego i wiele do wielu, ale ponieważ tu mamy utworzone modele dla pomocniczych tabeli łączących, więc musimy przynajmniej zdefiniować dla nich klucze złożone). Posłuży do tego tzw. _fluent API_.
### _Fluent API_
Tym mianem określamy metody klasy `ModelBuilder`. Pozwalają one na nadpisanie jakichkolwiek domyślnych konfiguracji mapowania _Entity Framework Core_. Zmiany wykonane przy pomocy _Fluent API_ są nadrzędne względem wszystkich innych konfiguracji. Modyfikacji `ModelBuilder`a dokonujemy przez nadpisanie metody `OnModelCreating(ModelBuilder modelBuilder)` klasy `DbContext`.
#### `OnModelCreating`
Metodę nadpisuje się, gdy chce się dalej skonfigurować model utworzony przy pomocy konwencji na podstawie modeli domenowych wskazanych przez `DbSet`y. Ponieważ `IdentityDbContext` nadpisuje tą metodę, aby skonfigurować modele związane z użytkownikami, więc na początku metody musimy wywołać jej bazową wersję. Następnie przechodzimy do konfiguracji utworzonych przez nas modeli.
#### `Entity`
Najpierw na naszym `modelBuilder`ze wywołujemy metodę:
```csharp =
public virtual Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder<TEntity> Entity<TEntity> () where TEntity : class;
```
Zwraca ona obiekt `EntityTypeBuilder<TEntity>`, który może zostać użyty do skonfigurowania modelu `TEntity`.
#### `HasKey`
Metoda klasy `EntityTypeBuilder<TEntity>` pozwalająca na ustawienie właściwości modelu `TEntity`, które będą tworzyć klucz główny. Wywołujemy ją na obiekcie zwróconym przez metodę `Entity<TEntity>`.
#### `HasOne<TRelatedEntity>`
Metoda klasy `EntityTypeBuilder<TEntity>` konfigurująca relację, gdzie `TEntity` posiada referencję wskazującą na pojedynczą instancję `TRelatedEntity`. Zwraca obiekt typu `ReferenceNavigationBuilder<TEntity,TRelatedEntity>`.
#### `HasMany<TRelatedEntity>`
Metoda klasy `EntityTypeBuilder<TEntity>` konfigurująca relację, gdzie `TEntity` posiada kolekcję zawierającą instancje `TRelatedEntity`. Zwraca obiekt typu `CollectionNavigationBuilder<TEntity,TRelatedEntity>`.
#### `WithOne`
Metoda klasy `ReferenceNavigationBuilder<TEntity,TRelatedEntity>` lub `CollectionNavigationBuilder<TEntity,TRelatedEntity>`. Konfiguruje ona odpowiednią relację i zwraca obiekt umożliwiający jej dalszą konfigurację.
Klasa|Relacja|Typ zwracany
---|---|---
`ReferenceNavigationBuilder <TEntity,TRelatedEntity>`|jeden do jednego|`ReferenceReferenceBuilder <TEntity,TRelatedEntity>`
`CollectionNavigationBuilder <TEntity,TRelatedEntity>`|jeden do wielu|`ReferenceCollectionBuilder <TEntity,TRelatedEntity>`
#### `WithMany`
Metoda klasy `ReferenceNavigationBuilder<TEntity,TRelatedEntity>` lub `CollectionNavigationBuilder<TEntity,TRelatedEntity>`. Konfiguruje ona odpowiednią relację i zwraca obiekt umożliwiający jej dalszą konfigurację.
Klasa|Relacja|Typ zwracany
---|---|---
`ReferenceNavigationBuilder <TEntity,TRelatedEntity>`|jeden do wielu|`ReferenceCollectionBuilder <TRelatedEntity,TEntity>`
`CollectionNavigationBuilder <TEntity,TRelatedEntity>`|wiele do wielu|`CollectionCollectionBuilder <TRelatedEntity,TEntity>`
#### `HasForeignKey`
Metoda klas `ReferenceCollectionBuilder<TPrincipalEntity,TDependentEntity>` i `ReferenceReferenceBuilder<TEntity,TRelatedEntity>`. Konfiguruje właściwość (właściwości), którą (które) należy użyć, jako klucza obcego tej relacji.
#### Przykład
Skonfigurujmy więc relacje jeden do jednego i wiele do wielu, dla naszych przykładowych modeli. Zauważmy, że relacja wiele do wielu, jest w praktyce relacją jeden do wielu, pomiędzy modelami, a klasą pomocniczą. Czyli np. relacja wiele do wielu pomiędzy `Book` i `Author`, jest tak na prawdę relacją jeden do wielu między `Book` i `BookAuthor` oraz `Author` i `BookAuthor`. Zwróćmy jeszcze uwagę, że dla modeli pomocniczych musimy skonfigurować klucze główne, gdyż będą to tzw. klucze złożone, składające się z dwóch właściwości, będących kluczami obcymi. Nadpiszmy więc metodę `OnModelCreating`:
```csharp =
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using TitlesOrganizer.Domain.Models;

namespace TitlesOrganizer.Infrastructure
{
    public class Context : IdentityDbContext
    {
        public DbSet<Author> Authors { get; set; }
        public DbSet<Book> Books { get; set; }
        public DbSet<BookAuthor> BookAuthor { get; set; }
        public DbSet<BookGenre> BookGenre { get; set; }
        public DbSet<Cover> Covers { get; set; }
        public DbSet<Designer> Designers { get; set; }
        public DbSet<Genre> Genres { get; set; }
        public DbSet<Series> Serieses { get; set; }

        public Context(DbContextOptions options) : base(options)
        {
        }
        
        protected override void OnModelCreating(ModelBuilder builder)
        {
            // konfiguracja identity
            base.OnModelCreating(builder);

            // konfiguracja relacji wiele do wielu Book - Author
            builder.Entity<BookAuthor>()
                .HasKey(it => new { it.AuthorId, it.BookId }); // na klucz glowny entity BookAuthor skladaja sie wlasciwosci AuthorId i BookId

            builder.Entity<BookAuthor>()         // entity BookAuthor
                .HasOne<Book>(it => it.Book)     // jest w relacji z jednym entity Book (przechowywanym we wlasciwosci Book)
                .WithMany(i => i.BookAuthors)    // jest to relacja jeden do wielu (entity Book posiada kolekcje BookAuthors obiektow BookAuthor)
                .HasForeignKey(it => it.BookId); // kluczem obcym tworzacym ta releacje jest wlasciwosc BookId

            builder.Entity<BookAuthor>()            // entity BookAuthor
                .HasOne<Author>(it => it.Author)    // jest w relacji z jednym entity Author (przechowywanej we wlasciwosci Author)
                .WithMany(i => i.BookAuthors)       // jest to relacja typu jeden do wielu (entity Author posiada kolekcje BookAuthors obiektow BookAuthor)
                .HasForeignKey(it => it.AuthorId); // kluczem obcym tworzacym ta relacje jest wlasciwosc AuthorId

            // konfiguracja relacji wiele do wielu Book - Genre
            builder.Entity<BookGenre>()
                .HasKey(it => new { it.BookId, it.GenreId }); // na klucz glowny entity BookGenre skladaja sie wlasciwosci BookId i GenreId

            builder.Entity<BookGenre>()          // entity BookGenre
                .HasOne<Book>(it => it.Book)     // jest w relacji z jednym entity Book (przechowywanym we wlasciwosci Book)
                .WithMany(i => i.BookGenres)     // jest to relacja typu jeden do wielu (entity Book posiada kolekcje BookGenres obiektow BookGenre)
                .HasForeignKey(it => it.BookId); // kluczem obcym tworzacym ta relacje jest wlasciwosc BookId

            builder.Entity<BookGenre>()           // entity BookGenre
                .HasOne<Genre>(it => it.Genre)    // jest w relacji z jednym entity Genre (przechowywanym we wlasciwosci Genre)
                .WithMany(i => i.BookGenres)      // jest to relacja typu jeden do wielu (entity Genre posiada kolekcje BookGenres obiektow BookGenre)
                .HasForeignKey(it => it.GenreId); // kluczem obcym tworzacym ta relacje jest wlasciwosc GenreId

            // konfiguracja relacji jeden do jednego Book - Cover
            builder.Entity<Book>()                  // entity Book
                .HasOne(it => it.Cover)      // jest w relacji z jednym entity Cover (przechowywanym we wlaciwosci Cover)
                .WithOne(i => i.Book)               // jest to relacja jeden do jeden (entity Cover posiada wlasciwosc Book, przechowujaca obiekt Book)
                .HasForeignKey<Cover>(it => it.BookId); // kluczem obcym tworzacym ta relacje jest wlasciwosc BookId entity Cover
        }
    }
}
```
### Konfiguracja UI
Kiedy tworzyliśmy nasz projekt _.Web_ wybraliśmy opcję autoryzacji użytkowników przy pomocy _Individual Accounts_. Przez to, został dla nas automatycznie utworzony podstawowy `IdentityDbContext`. Znajduje się on w katalogu _Data_ projektu _.Web_. Ponieważ aplikacja MVC ma być tylko frontem naszej aplikacji, więc nie chcemy się w niej łączyć z bazą danych. Usuwamy więc cały katalog _Data_. Teraz, musimy jeszcze zaktualizować konfigurację naszej aplikacji. W pliku _Program.cs_ (lub _Startup.cs_, jeśli istnieje) aplikacji webowej podmieniamy usunięty przed chwilą kontekst (`ApplicationDbContext`), na ten przez nas utworzony (`TitlesOrganizer.Infrastructure.Context`).

Więc kod:
```csharp =
using TitlesOrganizer.Web.Data; // ZMIANIC REFERENCJE

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
var connectionString = builder.Configuration.GetConnectionString("DefaultConnection") ?? throw new InvalidOperationException("Connection string 'DefaultConnection' not found.");
builder.Services.AddDbContext<ApplicationDbContext>(options => // ZMIENIC TUTAJ
    options.UseSqlServer(connectionString));
builder.Services.AddDatabaseDeveloperPageExceptionFilter();

builder.Services.AddDefaultIdentity<IdentityUser>(options => options.SignIn.RequireConfirmedAccount = true)
    .AddEntityFrameworkStores<ApplicationDbContext>(); // ZMIENIC TUTAJ
builder.Services.AddControllersWithViews();

var app = builder.Build();
```
zamieniamy na:
```csharp =
using TitlesOrganizer.Infrastructure; // PO ZMIANIE

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
var connectionString = builder.Configuration.GetConnectionString("DefaultConnection") ?? throw new InvalidOperationException("Connection string 'DefaultConnection' not found.");
builder.Services.AddDbContext<Context>(options => // PO ZMIANIE
    options.UseSqlServer(connectionString));
builder.Services.AddDatabaseDeveloperPageExceptionFilter();

builder.Services.AddDefaultIdentity<IdentityUser>(options => options.SignIn.RequireConfirmedAccount = true)
    .AddEntityFrameworkStores<Context>(); // PO ZMIANIE
builder.Services.AddControllersWithViews();

var app = builder.Build();
```

## [LEKCJA 6 – Repository](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-6-repository/)
Kiedy mamy już utworzony kontekst ze wszystkimi naszymi tabelami bazodanowymi, możemy przejść do implementacji operacji na danych. Poprzednio, w aplikacji konsolowej, używaliśmy w tym celu serwisów. W aplikacji webowej stworzymy do tego celu repozytoria w naszym projekcie _.Infrastructure_.
### Czy trzeba tworzyć repozytoria?
Od kiedy powstał _Entity Framework Core_ takie operacje możemy wykonywać bezpośrednio na utworzonych w kontekście `DbSet`ach. Teoretycznie nie musielibyśmy więc tworzyć repozytoriów. Warto jednak oddzielnie zaimplementować operacje na danych z bazy danych. Pozwala nam to na kontrolę nad tym, jak te operacje są wykonywane, co zmniejsza ryzyko błędu. Poprawia to również podział logiczny aplikacji. Serwisy nie będą wykonywać operacji na bazie danych, a jedynie otrzymywać i przesyłać odpowiednie dane i wywoływać odpowiednie operacje.
### Jak tworzyć repozytoria?
Istnieją dwa podejścia do tworzenia repozytoriów. Po pierwsze możemy tworzyć osobne repozytorium dla każdej tabeli. Często jednak mamy powiązane z sobą tabele, które zawsze będziemy używać razem. Np. w jednej tabeli przechowujemy książki, a w drugiej autorów. Wyświetlając, tworząc lub edytując książki będziemy również podawać autorów. Samych autorów też nie będziemy raczej używać nie w odniesieniu do książek. Tworzenie osobnych repozytoriów dla obu tych tabel może nie mieć więc sensu. Zgodnie z drugim podejściem tworzymy więc repozytoria zgodnie z logiką biznesową. Wówczas w jednym repozytorium możemy wykonywać operacje na kilku `DbSet`ach.
### Tworzenie repozytoriów
Repozytoria tworzymy w naszym projekcie _.Infrastructure_ w katalogu _Repositories_. Będą to po prostu zwykłe klasy. Będziemy im nadawać nazwy z przyrostkiem _Repository_, np. _BookRepository_. Do każdego repozytorium będziemy musieli wstrzyknąć nasz kontekst, np.:
```csharp =
using TitlesOrganizer.Domain.Models;

namespace TitlesOrganizer.Infrastructure.Repositories
{
    public class BookRepository
    {
        // wstrzykujemy kontekst do repozytorium
        private readonly Context _context;

        public BookRepository(Context context)
        {
            _context = context;
        }
    }
}
```
Teraz będziemy już mogli odwoływać się w naszym repozytorium do `DbSet`ów naszego kontekstu i wykonywać na nich operacje. Robimy to przy użyciu LINQ, analogicznie jak to robiliśmy na listach w aplikacji konsolowej. Dodatkowo, kiedy wykonamy pożądane zmiany w naszym kontekście (dodamy/usuniemy/zmodyfikujemy dane w jednym lub kilku `DbSet`ach) i chcemy zapisać je w bazie danych musimy wywołać funkcję `SaveChanges` kontekstu. Konteksty działają na zasadzie transakcji. Tzn. zbierane są wszystkie informacje które zostały zmienione w kontekście. Są one wykonywane na bazie danych, ale jeżeli któraś z operacji na bazie się nie powiedzie, to wszystkie te zmiany zostaną cofnięte i nasza baza pozostanie bez zmian.

Kiedy tworzymy metodę pobierającą z bazy zestaw (kolekcję) danych, to powinna ona zawsze mieć zwracany typ danych `IQueryable<T>`. Dzięki temu metoda ta nie zwraca tak na prawdę fizycznej kolekcji obiektów, a formułuje jedynie zapytanie do bazy danych. Jeżeli później w serwisie będziemy wywoływać taką metodę i wykonywać jakieś dalsze filtrowanie, to nasze zapytanie zostanie zmodyfikowane i dopiero wtedy wykonane. W efekcie z bazy danych zostaną pobrane wyłącznie dane, których naprawdę potrzebujemy.

Dodając nowe obiekty do bazy nie nadajemy im numerów id (o ile są one kluczami głównymi), jak to robiliśmy w naszej aplikacji konsolowej. Zostaną one nadane automatycznie przez naszą bazę danych, przez autoinkrementację klucza głównego tabeli.
### Przykład
Wykorzystajmy kontekst utworzony w poprzedniej lekcji. Stwórzmy repozytorium związane z książkami. Zaimplementujmy w nim kilka podstawowych metod.
```csharp =
using TitlesOrganizer.Domain.Models;

namespace TitlesOrganizer.Infrastructure.Repositories
{
    public class BookRepository
    {
        private readonly Context _context;

        public BookRepository(Context context)
        {
            _context = context;
        }

        public int AddBook(Book book)
        {
            _context.Books.Add(book);
            _context.SaveChanges();

            return book.Id;
        }

        public void DeleteBook(int bookId)
        {
            Book? book = _context.Books.Find(bookId);
            if (book != null)
            {
                _context.Books.Remove(book);
                _context.SaveChanges();
            }
        }

        public Book? GetBookById(int bookId)
        {
            return _context.Books.FirstOrDefault(book => book.Id == bookId);
        }

        public IQueryable<Book> GetBooksByAuthor(int authorId)
        {
            return _context.Books.Where(b => b.BookAuthors.Select(ba => ba.AuthorId).Contains(authorId));
        }
    }
}
```

## [LEKCJA 7 – Abstract](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-7-abstract/)
Jak wspominaliśmy podczas omawiania architektury naszej aplikacji, w warstwie domenowej naszej aplikacji poza modelami będziemy mieć interfejsy, które będą implementowane przez repozytoria warstwy infrastruktury. Interfejsy te nazywamy kontraktami. Są one bardzo ważne, gdyż dzięki nim będziemy mogli w warstwie aplikacji korzystać z tych metod bez znajomości i zależności od implementacji, a co za tym idzie użytego silnika bazodanowego.
### Tworzenie kontraktów dla repozytoriów
W naszej aplikacji _.Domain_ w katalogu _Interfaces_ stwórzmy interfejs dla każdego repozytorium, będący jego szablonem. Są to zwyczajne interfejsy. Ich nazwy są zgodne z nazwą odpowiedniego repozytorium, poprzedzając je wielką literę "i", np. _IBookRepository_.

Po stworzeniu kontraktów pamiętajmy jeszcze, aby w repozytoriach dopisać jakie interfejsy implementują.
### Przykład
Stwórzmy kontrakt dla uproszczonego repozytorium z poprzedniej lekcji:
```csharp =
using TitlesOrganizer.Domain.Models;

namespace TitlesOrganizer.Domain.Interfaces
{
    public interface IBookRepository
    {
        int AddBook(Book book);

        void DeleteBook(int bookId);

        Book? GetBookById(int bookId);

        IQueryable<Book> GetBooksByAuthor(int authorId);
    }
}
```
Dopiszmy jeszcze do repozytorium jaki interfejs implementuje:
```csharp =
using TitlesOrganizer.Domain.Interfaces; // Dopisz odpowiedni using
using TitlesOrganizer.Domain.Models;

namespace TitlesOrganizer.Infrastructure.Repositories
{
    public class BookRepository : IBookRepository // dopisz implementowany interfejs
    {
        ...
    }
}
```

## [LEKCJA 8 – Instalacja SQL Server](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-8-instalacja-sql-server/)
Na początku kursu zainstalowaliśmy narzędzie Microsoft SQL Server Management Studio, którego będziemy używać do pracy z naszą bazą danych. Nie zainstalowaliśmy jednak jeszcze samego silnika bazodanowego. Zróbmy to teraz.
### Instalacja silnika bazodanowego
Na stronie: https://www.microsoft.com/pl-pl/sql-server/sql-server-downloads wyszukujemy sekcję zatytułowaną "Lub pobierz bezpłatną, specjalistyczną edycję" i pobieramy wersję _Express_. Po pobraniu i uruchomieniu instalatora wybieramy w nim opcję _Basic_, akceptujemy wszelkie zgody i klikamy _Install_. Po zakończeniu instalacji pojawi nam się ekran z podsumowaniem:

![Podsumowanie instalacji SQL Server](Lekcja8/Ilustracje/PodsumowanieInstalacjiSQLServer.png)<br />
_Rysunek 1. Przykładowy ekran podsumowania instalacji silnika bazodanowego SQL Server_

Znajduje się tu kilka ważnych informacji. Po pierwsze mamy nazwę instancji (_INSTANCE NAME_), która zaraz nam się przyda. Po drugie mamy _CONNECTION STRING_, który pozwoli nam na łączenie się z naszą bazą danych. Jest on bardzo ważny, dla tego od razy go sobie skopiujmy i zapiszmy w jakimś osobnym pliku. Reszta informacji na razie nas specjalnie nie interesuje. Gdy zapiszemy _CONNECTION STRING_ możemy zamknąć instalator klikając _Close_.
### Połączenie aplikacji z utworzoną instancją serwera bazodanowego
Kiedy zainstalowaliśmy już nasz lokalny serwer połączmy z nim od razu naszą aplikację.

Kiedy zmienialiśmy konfigurację naszej aplikacji MVC, tak aby korzystała z utworzonego przez nas kontekstu, mieliśmy tam m.in. taki kod:
```csharp =
var connectionString = builder.Configuration.GetConnectionString("DefaultConnection") ?? throw new InvalidOperationException("Connection string 'DefaultConnection' not found.");
builder.Services.AddDbContext<Context>(options =>
    options.UseSqlServer(connectionString));
```
Widzimy tu zmienną `connectionString`, tworzoną z jakiegoś skonfigurowanego `"DefaultConnection"` (domyślnego połączenia). W kolejnej instrukcji z kolei wskazywaliśmy builderowi, aby nasz kontekst korzystał ze wskazanego przez tą zmienną serwera SQL. W tym właśnie miejscu w kodzie łączyliśmy nasz kontekst z serwerem bazodanowym. `"DefaultConnection"` jest skonfigurowane w pliku JSON _appsettings.json_ naszego projektu _.Web_. Na początku tego pliku powinniśmy mieć coś takiego:
```json =
"ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=aspnet-TitlesOrganizer.Web-730df0ff-8ddf-4542-9564-b259fc927467;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
```
czyli właśnie konfigurację domyślnego połączenia. Zawiera ona w tej chwili automatycznie wygenerowany podczas tworzenia naszej aplikacji ciąg połączenia. Zastąpmy go ciągiem wygenerowanym podczas instalacji dla naszego serwera (_CONNECTION STRING_, który przed chwilą zapisywaliśmy w oddzielnym pliku). Czyli będziemy mieć teraz np.:
```json =
"ConnectionStrings": {
    "DefaultConnection": "Server=localhost\\SQLEXPRESS;Database=master;Trusted_Connection=True;"
  }
```
I już.
### Otwarcie serwera w Microsoft SQL Server Management Studio
Nasz serwer możemy teraz spróbować otworzyć w programie Microsoft SQL Server Management Studio. Otwieramy program. Powinno wyświetlić nam się od razu okienko _Connect to Server_. Musimy podać tu kilka informacji.
* _Server type:_ wybieramy _Database Engine_
* _Server name:_ musimy wpisać nazwę naszego serwera. Składa się ona z nazwy naszego komputera, backslasha i nazwy instancji naszego serwera. Jeśli nie znamy nazwy naszego komputera, możemy zamiast niej wstawić kropkę, lub słowo _localhost_. Nazwa instancji naszego serwera była podana w podsumowaniu instalacji. Prawdopodobnie jest to _SQLEXPRESS_. Pełna nazwa serwera jest też podana na początku naszego connection stringa. Czyli np. wpisujemy: _localhost\SQLEXPRESS_.
* _Authentication:_ powinno być ustawione na _Windows Authentication_, ponieważ jest to lokalny serwer na naszym komputerze.

Gdy wszystko uzupełnimy klikamy _Connect_.

Jeżeli okno _Connect to Server_ nie otworzyło nam się automatycznie po uruchomieniu programu, możemy je otworzyć wybierając w _Object Explorer_ -> _Connect_ -> _Database Engine..._, klikając w _Object Explorer_ ikonkę wtyczki (_Connect Object Explorer_) lub wybierając z menu _File_ -> _Connect Object Explorer..._

Kiedy połączymy się już z naszym serwerem w _Object Explorer_ (F8, jeśli się nie wyświetla) możemy zobaczyć co się na nim znajduje. Nasze bazy danych zostaną umieszczone w folderze _Databases_, kiedy już je utworzymy.

## [LEKCJA 9 – Migracje](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-9-migracje/)
Migracje, jest to mechanizm pozwalający na automatyczne przeniesienie struktury bazy danych, stworzonej w podejściu _code first_, do silnika bazodanowego (naszej instancji serwera), na faktyczną bazę danych.

Moglibyśmy oczywiście ręcznie stworzyć całą naszą bazę danych, przy pomocy silnika bazodanowego i odpowiednich zapytań SQL lub wyklikując odpowiednie opcje np. w Microsoft SQL Server Management Studio. Aby jednak zaoszczędzić programistom czasu i pracy stworzono właśnie migracje.

Komendy związane z migracjami znajdują się w paczce _Microsoft.EntityFrameworkCore.Tools_, którą zainstalowaliśmy w naszym projekcie na początku tego tygodnia. Będziemy ich używać w konsoli _Package Manager Console_ w Visual Studio. Konsolę otworzymy wybierając z menu _Tools_(Alt+T) -> _NuGet Package Manager_(N) -> _Package Manager Console_(O). W konsoli w _Default project:_ musimy wybrać projekt, który będzie związany z migracjami, a co za tym idzie z bazą danych (projekt, w którym znajduje się kontekst). Będzie to oczywiście nasz projekt _.Infrastructure_. Komendy będziemy wywoływać podając po prostu nazwę i po spacji ewentualne parametry. Jeśli będą to parametry obowiązkowe i pozycyjne, to wystarczy, że podamy wartość, którą chcemy im nadać. W przeciwnym razie najpierw musimy podać nazwę parametru poprzedzoną myślnikiem, a po spacji wartość. W tabeli poniżej podano parametry wspólne dla wszystkich komend EF Core.
### Wspólne parametry komend EF Core
Parametr|Opis
:--|:--
`-Context <String>`|Klasa `DbContext`, którą chcemy użyć. Tylko nazwa klasy lub pełna nazwa łącznie z nazwą przestrzeni nazw. Jeśli ten parametr jest pominięty EF Core znajduje klasę kontekstową. Jeżeli istnieje kilka kontekstów, to ten parametr jest wymagany.
`-Project <String>`|Projekt docelowy. Jeżeli ten parametr jest pominięty jako projekt docelowy zostaje użyty domyślny projekt konsoli (_Default project:_, który jest ustawiony w naszej konsoli _Package Manager Console_).
`-StartupProject <String>`|Projekt startowy. Jeśli ten parametr zostanie pominięty, jako projekt docelowy zostanie użyty projekt startowy ustawiony we właściwościach solucji.
`-Args <String>`|Argumenty przekazane do aplikacji.
`-Verbose`|Pokaż szczegółowe dane wyjściowe. Jest to opcja, którą możemy włączyć. Nie podajemy po niej żadnych wartości.

Parametry `Context`, `Project` i `StartupProject` wspierają tzw. _tab-extension_ (rozszerzenie tabulacji), czyli automatyczne wyszukiwanie odpowiedniej wartości parametru przez wciśnięcie klawisza Tab.

Żeby zobaczyć informacje pomocnicze na temat jakiejś komendy możemy użyć komendy `Get-Help` PowerShell'a. W paczce _Tools_ znajdują się takie komendy jak:
### `Add-Migration`
Służy do dodania nowej migracji. Komenda obsługuje wszystkie parametry wspólne wymienione w tabeli powyżej oraz dodatkowo parametry z tabeli poniżej. Posiada jeden obowiązkowy parametr pozycyjny, `Name`. Możemy go więc użyć baz podawania nazwy parametru. Tak więc po nazwie komendy, po spacji, podajemy nazwę jaką chcemy nadać naszej migracji, np. _InitialCreate_. 
Parametr|Opis
:--|:--
`-Name <String>`|Nazwa migracji. Jest to parametr pozycyjny i jest on wymagany.
`-OutputDir <String>`|Katalog, w którym zostanie zapisany plik z migracją. Ścieżki są względne w stosunku do katalogu projektu docelowego. Domyślnie jest to katalog _Migrations_.
`-Namespace <String>`|Przestrzeń nazw, która ma być używana dla wygenerowanych klas. Domyślnie generowane z katalogu wyjściowego (czyli np. _TitlesOrganizer.Infrastructure.Migrations_, jeżeli nie zmienialiśmy domyślnej wartości parametru `OutputDir`).
### `Bundle-Migration`
Tworzy plik wykonywalny do aktualizacji bazy danych. Poza parametrami wspólnymi, posiada jeszcze parametry wymienione w tabeli poniżej.
Parametr|Opis
:--|:--
`-Output <String>`|Ścieżka tworzonego pliku wykonywalnego.
`-Force`|Zastąp istniejące pliki.
`-SelfContained`|Zapakuj również pakiet środowiska uruchomieniowego .NET, żeby nie trzeba go było instalować na maszynie.
`-TargetRuntime <String>`|Docelowe środowisko uruchomieniowe na które ma być utworzony plik.
`-Framework <String>`|Docelowy framework. Domyślnie, pierwszy w projekcie.
### `Drop-Database`
Usuwa bazę danych. Poza wskazanymi wyżej parametrami wspólnymi dla wszystkich komend EF Core, posiada jeszcze jeden, `WhatIf`. Żaden z parametrów nie jest obowiązkowy, ani pozycyjny.
Parametr|Opis
:--|:--
`-WhatIf`|Pokazuje, która baza danych będzie usunięta, ale bez jej faktycznego usuwania. Jeżeli przed usunięciem bazy danych, chcemy się upewnić, czy odpowiednia baza danych zostanie usunięta możemy wywołać komendę `Drop-Database -WhatIf`.
### `Get-DbContext`
Wyświetla listę i pobiera informacje o dostępnych typach `DbContext`. Posiada tylko parametry wspólne dla wszystkich metod EF Core.
### `Get-Migration`
Wyświetla listę dostępnych migracji. Poza parametrami wspólnymi posiada jeszcze wymienione w tabeli poniżej.
Parametr|Opis
:--|:--
`-Connection <String>`|Connection string do połączenia się z bazą danych. Domyślnie używany string określony w `AddDbContext` lub `OnConfiguring`.
`-NoConnect`|Nie łącz z bazą danych.
### `Optimize-DbContext`
Generuje skompilowaną wersję modelu użytego w `DbContext`. Poza wspólnymi parametrami są jeszcze:
Parametr|Opis
:--|:--
`-OutputDir <String>`|Katalog, w którym mają zostać umieszczone pliki. Ścieżki są względne w stosunku do głównego katalogu projektu.
`-Namespace <String>`|Przestrzeń nazwy, w której mają się znaleźć wygenerowane klasy. Domyślnie nazwa jest generowana przez połączenie nazwy głównej przestrzeni nazw, katalogu określonego przez parametr `OutputDir` i przyrostka `CompiledModels`.
### `Remove-Migration`
Usuwa ostatnią migrację (wycofuje zmiany kodu, które zostały wykonane podczas migracji). Poza wspólnymi parametrami posiada jeszcze parametr `Force`.
Parametr|Opis
:--|:--
`-Force`|Wymuś cofnięcie migracji (wycofaj zmiany, które zostały zastosowane w bazie danych).
### `Scaffold-DbContext`
Generuje kod dla `DbContext` i typy encji dla bazy danych (o ile tabela bazodanowa ma klucz główny). Komenda może oczywiście używać parametrów wspólnych i ponadto parametrów z tabeli poniżej. Ma ona dwa wymagane parametry pozycyjne: `Connection` i `Provider`. Czyli po nazwie komendy po spacji podajemy wartość dla parametru `Connection` i znowu po spacji wartość dla parametru `Provider`.
Parametr|Opis
:--|:--
`-Connection <String>`|Connection string do bazy danych. W przypadku projektów ASP.NET Core 2.x wartość może być `"name=<nazwa connection string>"` (np. `"Name=ConnectionStrings:Blogging"`). W takim przypadku nazwa pochodzi od źródeł konfiguracji skonfigurowanych dla projektu. Jest to parametr pozycyjny i jest wymagany.
`-Provider <String>`|Dostawca do użycia. Zazwyczaj jest to nazwa pakietu NuGet, na przykład: Microsoft.EntityFrameworkCore.SqlServer. Jest to parametr pozycyjny i jest wymagany.
`-OutputDir <String>`|Katalog, w którym mają zostać umieszczone pliki klas entity. Ścieżki są względne w stosunku do katalogu projektu.
`-ContextDir <String>`|Katalog, w którym ma zostać umieszczony plik `DbContext`. Ścieżki są względne w stosunku do katalogu projektu.
`-Namespace <String>`|Przestrzeń nazw, która ma być używana dla wszystkich wygenerowanych klas. Domyślnie generowane z głównej przestrzeni nazw i katalogu wyjściowego.
`-ContextNamespace <String>`|Przestrzeń nazw, która ma być używana dla wygenerowanej klasy `DbContext`. **Uwaga!** Nadpisuje wartość parametru `Namespace`.
`-Context <String>`|Nazwa generowanej klasy `DbContext`.
`-Schemas <String[]>`|Schematy tabel i widoków, dla których mają być generowane typy entity. Jeśli ten parametr zostanie pominięty, uwzględnione zostaną wszystkie schematy. W przypadku użycia tej opcji wszystkie tabele i widoki w schematach zostaną uwzględnione w modelu, nawet jeśli nie zostały one wyraźnie uwzględnione za pomocą parametru `Table`. 
`-Tables <String[]>`|Tabele i widoki, dla których mają zostać wygenerowane typy entity. Tabele i widoki z konkretnych schematów mogą być dodane używając formatu "Schemat.Tabela" lub "Schemat.Widok". Jeżeli ten parametr jest pominięty, wszystkie tabele i widoki zostaną uwzględnione.Jeżeli chcemy wskazać kilka tabel/widoków to ich nazwy (zawarte w cudzysłowach) oddzielamy od siebie przecinkami.
`-DataAnnotations`|Użyj atrybutów do skonfigurowania modelu (tam gdzie jest to możliwe). Jeśli ten parametr jest pominięty, tylko _fluent API_ jest użyte.
`-UseDatabaseNames`|Użyj nazw tabel, widoków, sekwencji i kolumn dokładnie takich, jakie są użyte w bazie danych. Jeśli ten parametr jest pominięty nazwy z bazy danych są zmieniane, tak aby były bardziej zgodne z konwencjami stylów nazw języka C#.
`-Force`|Nadpisz istniejące pliki.
`-NoOnConfiguring`|Nie generuj `DbContext.OnConfiguring`.
`-NoPluralize`|Nie używaj `IPluralizer`. Nie zmieniaj nazw identyfikatorów (klas, parametrów itd.) odpowiednio na liczbę pojedynczą lub mnogą.
### `Script-Migration`
Generuje skrypt SQL, który zastosowuje wszystkie zmiany z jednej wybranej migracji w innej wybranej migracji. Można z nią używać wszystkich wspólnych parametrów oraz parametrów z poniższej tabeli. Komenda posiada dwa parametry pozycyjne: `From` i `To`. Można więc po nazwie komendy i spacji podać wartość dla parametru `From` i, po spacji, wartość dla parametru `To`. Będziemy używać tej komendy, gdy będziemy chcieli zrobić migrację do bazy danych, która z jakichś względów nie wspiera migracji automatycznych.
Parametr|Opis
:--|:--
`-From <String>`|Początkowa migracja. Migracje mogą być identyfikowane według nazwy lub ID. Cyfra 0 to szczególny przypadek, który oznacza przed pierwszą migracją. Domyślnie 0.
`-To <String>`|Końcowa migracja. Domyślnie ostatnia migracja.
`-Idempotent`|Wygeneruj skrypt, którego można użyć w bazie danych podczas dowolnej migracji.
`-NoTransactions`|Nie generuj instrukcji transakcji SQL.
`-Output <String>`|Plik, w którym ma zostać zapisany wynik. Jeśli ten parametr zostanie pominięty, plik zostanie utworzony z wygenerowaną nazwą w tym samym folderze, w którym tworzone są pliki wykonawcze aplikacji, np.: _/obj/Debug/netcoreapp2.1/ghbkztfz.sql/_.

Parametry `To`, `From` i `Output` wspierają tzw. _tab-extension_ (rozszerzenie tabulacji), czyli automatyczne wyszukiwanie odpowiedniej wartości parametru przez wciśnięcie klawisza Tab.
### `Update-Database`
Aktualizuje bazę danych do ostatniej lub określonej migracji. Poza wspólnymi parametrami posiada jeszcze dwa, z czego jeden (`Migration`) jest parametrem pozycyjnym. Możemy więc po nazwie komendy i spacji bezpośrednio podać jej wartość.
Parametr|Opis
:--|:--
`-Migration <String>`|Migracja docelowa. Migracje mogą być identyfikowane według nazwy lub ID. Cyfra 0 to szczególny przypadek, który oznacza przed pierwszą migracją i powoduje cofnięcie wszystkich migracji. Jeśli nie zostanie określona żadna migracja, komenda domyślnie przyjmuje ostatnią migrację.
`-Connection <String>`|Connection string do połączenia się z bazą danych. Domyślnie używany string określony w `AddDbContext` lub `OnConfiguring`.

Parametr `Migration` wspieraj tzw. _tab-extension_ (rozszerzenie tabulacji), czyli automatyczne wyszukiwanie odpowiedniej wartości parametru przez wciśnięcie klawisza Tab.
### `Script-DbContext`
Komenda zdefiniowana w paczce _Microsoft.EntityFrameworkCore.Design_, od której zależy paczka _Tools_. Generuje skrypt SQL z DbContext. Pomija wszelkie migracje. Poza parametrami wspólnymi, ma jeszcze parametr `Output`.
Parametr|Opis
:--|:--
`-Output <String>`|Plik, do którego ma zostać zapisany rezultat.
### Tworzenie bazy danych na podstawie kontekstu
#### Budowanie migracji
Jeżeli nasz program jest w stanie się zbudować (nie ma w nim żadnych błędów) i zachowaliśmy wszystkie standardy wymagane przez EF Core (przestrzegamy standardów związanych z nazewnictwem, stworzyliśmy odpowiednie relacje itd.) to możemy stworzyć naszą pierwszą migrację, którą nazwiemy _InitialCreate_. W tym celu otwieramy _Package Manager Console_ i wpisujemy w niej komendę `Add-Migration InitialCreate`. Powinno to spowodować zbudowanie migracji (chyba, że jednak mamy błędy). Jeżeli tworzenie migracji powiodło się (w konsoli wyświetlił się komunikat _Build succeeded._), to w projekcie _.Infrastructure_ stworzył nam się nowy folder, _Migrations_. W nim znajduje się plik, ze zbudowaną przed chwilą migracją. Nazwa pliku składa się z dwóch części. Najpierw mamy datę utworzenia migracji, a następnie po podkreślniku podaną przez nas nazwę (_InitialCreate_). Jest to plik .cs. Powinien się on automatycznie otworzyć po zbudowaniu migracji. W nim możemy podejrzeć, co zostanie stworzone w naszej bazie danych. Poza tabelami, które sami zdefiniowaliśmy znajdziemy tam m.in. takie tabele jak `"AspNetRoles"`, czy `"AspNetUsers"`. Zostały one zdefiniowane przez `IdentityDbContext`, którego użyliśmy, gdyż chcemy aby autoryzacja użytkowników odbywała się przy pomocy standardowej struktury zdefiniowanej przez ASP.NET. Ogólnie, stworzona migracja powinna się bez problemu przenieść do bazy danych.
#### Aktualizacja bazy danych
Po zbudowaniu migracji, możemy zaktualizować do niej naszą bazę danych. Zanim to jednak zrobimy sprawdźmy jeszcze kilka rzeczy:
1. Upewnijmy się czy migracja została stworzona.
2. Sprawdźmy, czy connection string `"DefaultConnection"` (w _appsettings.json_) wskazuje na właściwą bazę danych.
    1. Sprawdźmy, czy wskazuje na zainstalowaną przez nas w poprzedniej lekcji instancję serwera.
    2. Sprawdźmy, czy wskazuje na właściwą bazę danych. Jeżeli wkleiliśmy skopiowany z podsumowania instalacji connection string, to musimy zmienić jeszcze nazwę bazy danych. Wewnątrz stringa prawdopodobnie mamy wówczas coś takiego `"Database=master"`, czyli nasz connection string wskazuje na bazę danych o nazwie _master_. Jest to baza wewnętrzna naszej instancji serwera. Jest ona nadrzędna nad wszystkimi innymi bazami danych. Tam znajdują się m.in. nazwy baz danych znajdujących się w tej instancji i niektóre logi. Nie powinniśmy więc do niej niczego dopisywać. Musimy więc zmienić nazwę _master_ na inną, którą chcielibyśmy nadać nowotworzonej przez nas bazie danych. Może to być np. nazwa naszej solucji, np. _TitlesOrganizer_ lub jakaś inna nazwa, wskazująca na to, co znajduje się w tej bazie danych.

Po sprawdzeniu, że wszystko się zgadza, możemy wrócić do konsoli ( _Package Manager Console_) i stworzyć bazę danych. Robimy to przy pomocy komendy `Update-Database`. Spowoduje to ponowne zbudowanie naszego projektu (upewnijmy się, że mamy wybrany projekt _.Infrastructure_), aby sprawdzić, czy nie ma żadnych błędów w migracji. Po czym nastąpi aktualizacja (lub, jak w naszym przypadku, utworzenie) bazy danych. Jeżeli wszystko się uda powinniśmy zobaczyć w konsoli komunikat _Done._ Jeżeli otrzymamy błąd `A connection was successfully established with the server, but then an error occurred during the login process.`, to dodajmy do naszego connection string w _appsettings.json_ parametr `TrustServerCertificate=True;` i spróbujmy ponownie. Po udanym utworzeniu bazy danych, możemy przejść do Microsoft SQL Server Management Studio i podejrzeć naszą bazę danych. W folderze _Databases_ powinna pojawić się nasza baza danych, ze wszystkimi zdefiniowanymi przez nas tabelami (w folderze _Tables_ naszej bazy danych).

## [LEKCJA 10 – Błędy początkujących](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-10-bledy-poczatkujacych/)
Podstawowym błędem na tym etapie jest źle zaplanowany schemat bazy danych. Należy na spokojnie zaplanować strukturę bazy danych. Pamiętajmy przy tym o zasadach normalizacji. Dobrze zaplanowana baza danych ułatwi nam dalszą pracę. Błędy popełnione na tym etapie trudno będzie natomiast później naprawić. Będzie to wymagało dużo pracy. Pamiętajmy więc, że gdy np. istnieje jakakolwiek szansa, że będziemy mieć kilka danych tego samego typu, dotyczące jednego rekordu (np. kilka numerów telefonów), to nigdy nie możemy przechowywać takich danych w jednej kolumnie. Wyodrębnijmy osobną tabelę na takie dane. Da nam to większą elastyczność. Dobrze zaplanujmy więc strukturę bazy danych, a dopiero później stwórzmy modele metodą code first.

## [LEKCJA 11 – Praca domowa](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-11-praca-domowa/)
Pracą domową na ten tydzień jest stworzenie całego projektu _.Domain_, a także kontekstu i repozytoriów projektu _.Infrastructure_. Być może w późniejszym czasie trzeba będzie jeszcze dorobić jakieś tabele, zmienić jakieś nazwy, albo usunąć/dodać jakieś dane. Jest to całkowicie normalne podczas tworzenia aplikacji. Za każdym razem gdy będziemy wprowadzać zmiany w strukturze bazy danych pamiętajmy o wykonaniu migracji (komendy `Add-Migration` i `Update-Database`), tak jak to robiliśmy za pierwszym razem.


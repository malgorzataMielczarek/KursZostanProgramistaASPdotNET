# [LEKCJA 4 – Zasady tworzenia baz danych](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-4-zasady-tworzenia-baz-danych/)
## Klucze
W bazie danych występują tzw. klucze, czyli identyfikatory elementów w bazie danych. Klucze muszą być unikatowe. Tzn. w obrębie jednej tabeli wartość klucza dla każdego wiersz musi być inna. Wyróżniamy dwa podstawowe rodzaje kluczy: główny i obcy.
### Klucz główny
Jest to identyfikator elementów w danej tabeli. Najczęściej będzie to nasze id. Za każdym razem dodając nowy element do bazy danych będziemy sobie taki klucz inkrementować, tak aby różnił się od tych będących już w tabeli. Oczywiście nie musi być to koniecznie `int`. Klucz może być dowolnego typu. Ważne, aby jego wartość była unikatowa. **W tabeli nie mogą znajdować się dwa elementy o takim samym kluczu.**
### Klucz obcy
Używa się go do tworzenia relacji pomiędzy danymi. Klucz obcy jest to klucz główny innego elementu (najczęściej z innej tabeli).
## Relacje
Pomiędzy danymi w bazach danych mogą występować trzy rodzaje relacji: jeden do jednego, jeden do wielu i wiele do wielu.
### Jeden do jednego
Relacja mówiąca, że element z jednej tabeli może mieć relację tylko i wyłącznie z jednym elementem z drugiej tabeli. Składa się z tabeli głównej (_principal_, _parent_) i zależnej (_dependent_, _child_). Tabela zależna, jest to tabela posiadająca klucz obcy. Jeżeli dopiero tworzymy bazę danych, to jako tabele zależną powinniśmy wybrać tabele, której istnienie niema logicznego sensu bez tabeli głównej. Jeżeli pomiędzy tabelami występuje naturalna relacja dziecko-rodzic, to "dziecko" powinno być tabelą zależną, a "rodzic" główną
#### Przykład
Załóżmy, że klientami naszej aplikacji są firmy. Każda z nich ma wyznaczoną dokładnie jedną osobę do kontaktu. Klienci, będą więc naszym modelem głównym. Istnienie informacji kontaktowych dla nieistniejącego klienta niema bowiem sensu. Tak więc klasa wskazująca osobę do kontaktu będzie zawierać klucz obcy (klucz główny klienta). Np.:
```csharp =
// model glowny
public class Customer
{
    public int Id { get; set; } // klucz glowny
    public string Name { get; set; }
    public string NIP { get; set; }

    // Relacja jeden do jednego
    public CustomerContactInformation CustomerContactInformation { get; set; } // powiązany obiekt
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
    public Customer Customer { get; set; } // powiązany obiekt
}
```
Dodatkowo będziemy musieli stworzyć klasę definiującą na podstawie jakich pól osiągamy relacją. Zajmiemy się tym jednak w kolejnej lekcji, podczas omawiania tworzenia kontekstów.
### Jeden do wielu
Tą relację omówiliśmy już pokrótce w poprzedniej lekcji. Jest to najczęściej występujący typ relacji. Dlatego jest on wspierany przez _Entity Framework Core_. Dzięki temu nie wymaga dodatkowego tworzenia kontekstu, o ile odpowiednio nazwiemy właściwości. Relacja jeden do wielu oznacza, że element z jednej tabeli, jest powiązany z jednym lub więcej elementami z drugiej tabeli. Element z drugiej tabeli musi być natomiast powiązany z dokładnie jednym elementem z pierwszej tabeli.
#### Przykład
Przykład tej relacji omawialiśmy w poprzedniej lekcji. Dla przypomnienia, element `Item` jest jakiegoś typu `Type` (powiązany z elementem `Type`). Może istnieć wiele elementów `Item` tego samego typu `Type` (jeden element `Type` może być powiązany z wieloma elementami `Item`):
```csharp =
public class Item
{
    public int Id { get; set; }
    public string Name { get; set; }

    // Relacja jeden do wielu
    public int TypeId {get; set; } // referencja do powiązanego obiektu, klucz obcy
    public virtual Type Type { get; set; } // powiązany obiekt
}
```
```csharp =
public class Type
{
    public int Id { get; set; }
    public string Name { get; set; }

    // Relacja jeden do wielu
    public virtual ICollection<Item> Items { get; set; } // kolekcja powiązanych obiektów
}
```
### Wiele do wielu
Oznacza, że element z jednej tabeli może być powiązany z wieloma elementami z drugiej tabeli. Element z drugiej tabeli również może być powiązany z wieloma elementami z pierwszej tabeli. Jest to najtrudniejszy do zrealizowania w bazie danych typ. Wymaga on bowiem utworzenia dodatkowej tabeli, której zadaniem będzie tworzenie połączeń między parą obiektów.
#### Przykład
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
## Normalizacja bazy danych
Normalizacja jest to proces organizowania danych w bazie danych. Ma ona na celu wyeliminowanie powtarzających się danych, zwiększenie elastyczności bazy danych, ułatwienie zarządzania nią i aktualizacji danych. Istnieją trzy podstawowe zasady normalizacji (zwane formami normalnymi). Istnieje jeszcze czwarta (zwana postacią normalną Boyce'a-Codda - _Boyce-Codd Normal Form (BCNF)_) i piąta postać normalna. Są one jednak sporadycznie rozważane w praktyce. O bazie danych spełniającej pierwszą zasadę mówi się, że jest w pierwszej formie normalnej. Jeśli spełnia trzy pierwsze reguły, mówi się, że jest w trzeciej formie normalnej itd.
### Pierwsza forma normalna
* Wyeliminuj powtarzające się grupy w indywidualnych tabelach.
* Stwórz osobną tabelę dla każdego zestawu powiązanych danych.
* Stwórz klucz identyfikujący każdy zestaw powiązanych danych.

Nie używaj w pojedynczej tabeli wielu pól do przechowywania podobnych danych.
#### Przykład
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
### Druga forma normalna
* Stwórz osobne tabele dla zbiorów danych, odnoszących się do wielu rekordów.
* Powiąż te tabele przy pomocy klucza obcego.

Rekordy (wiersze tabeli) nie powinny zależeć od niczego innego, niż od klucza głównego.
#### Przykład
Znormalizujmy dalej naszą przykładową tabelę do drugiej postaci normalnej. Wyodrębnijmy więc adresy do oddzielnej tabeli. Czyli zamiast tabel Customer:
Id|Name|NIP|City|Address|ZIP
--:|:-:|:-:|:--|:--|:--
1|Examplex|154742373|Przykładno|ul. Źródłowa 15f/4|54-294
1|Examplex|154742373|Probowo|up. Druga 2|93-850

będziemy mieć dwie tabele, Customer:
Id|Name|NIP
--:|:-:|:-:
1|Examplex|154742373

i Address:
Id|City|Address|ZIP|CustomerId
--:|:-|:--|:--|:--:
1|Przykładno|ul. Źródłowa 15f/4|54-294|1
2|Probowo|up. Druga 2|93-850|1

powiązane relacją jeden do wielu.
### Trzecia forma normalna
* Wyeliminuj dane, które nie zależą od klucza głównego.

Miejsce wartości w tabeli, które nie zależą od klucza, nie jest w tej tabeli. Generalnie, jeżeli istnieje prawdopodobieństwo, że zawartość grupy pól może mieć zastosowanie do więcej niż jednego rekordu w tabeli, należy zastanowić się nad umieszczeniem ich w osobnej tabeli.
#### Przykład
Gdybyśmy chcieli znormalizować nasz przykład do trzeciej postaci normalnej moglibyśmy wyodrębnić np. nazwy miast, kody pocztowe i nazwy ulic do osobnych tabeli, gdyż możemy mieć wiele adresów z tego samego miasta, kodu pocztowego i tej samej ulicy. Czyli zamiast Address:
Id|City|Address|ZIP|CustomerId
--:|:-|:--|:--|:--:
1|Przykładno|ul. Źródłowa 15f/4|54-294|1
2|Probowo|up. Druga 2|93-850|1

mielibyśmy City:
Id|Name
--:|:--
1|Przykładno
2|Probowo

Zip:
Id|No|CityId
--:|:--|:-:
1|54-294|1
2|93-850|2

Street:
Id|Name|ZIPId
--:|:--|:--:
1|ul. Źródłowa|1
2|ul. Druga|2

i Address:
Id|StreetId|Local|CustomerId
--:|:-:|:--|:--:
1|1|15f/4|1
1|2|2|1

| :warning:**UWAGA!** |
| :---: |
|Do powyższych zasad należy się jednak stosować, gdy ma to sens. Trzecią formę normalną stosuje się przykładowo tylko dla często zmieniających się danych. Np. przeprowadzona w powyższym przykładzie normalizacja do trzeciej postaci normalnej niema sensu. Miało by ono sens, gdyby nazwy ulic, czy miejscowości często się zmieniały. Wówczas takie poprawki musielibyśmy nanieść tylko w jednym miejscu, zamiast w wielu rekordach. Ponieważ jednak zdarza się to sporadycznie i prawdopodobieństwo konieczności naniesienia takich poprawek jest bliskie zeru, więc lepiej pozostawić naszą tabelę Address w drugiej postaci normalnej. Szczególnie, że wiele małych tabel może obniżać wydajność lub przekraczać pojemność otwartych plików i pamięci. Wyodrębnienie nazw miast, kodów pocztowych itd. mogłoby mieć jeszcze sens, gdybyśmy mieli w bazie z góry zdefiniowaną listę miast, kodów itp. Wówczas wprowadzając nowy adres klienta będziemy np. wybierać dane z rozwijanej listy, zamiast wpisywać ręcznie, co zmniejszy ryzyko błędu.|
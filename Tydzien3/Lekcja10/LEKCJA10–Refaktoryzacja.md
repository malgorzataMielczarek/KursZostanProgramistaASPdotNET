# [LEKCJA 10 – Refaktoryzacja](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-10-refaktoryzacja/)
W tej lekcji zajmiemy się refaktoryzacją naszego programu, utworzonego w poprzednim tygodniu, z wykorzystaniem nabytej w tym tygodniu wiedzy. Skupimy się na trzech aspektach:
1. Podział logiczny aplikacji - podzielimy solucję na dodatkowe projekty
2. Wykorzystanie interfejsów - utworzymy interfejsy dla serwisów
3. Wykorzystanie dziedziczenia i klas abstrakcyjnych - utworzenie bazowego serwisu i modelu
## 1. Podzielić solucję na projekty
Aby aplikacja była łatwiejsza w zarządzaniu i dalszym rozwoju, przeniesiemy część jej funkcjonalności do odrębnych projektów. Nie będą to jednak projekty konsolowe, a biblioteki klas (_Class Library (Standard .NET)_). Są to projekty, które nie kompilują się do pliku wykonywalnego (_.exe_) i co za tym idzie nie mogą funkcjonować jako samodzielne programy. Kompilują się natomiast do plików _.dll_, czyli właśnie bibliotek, i służą do implementacji funkcjonalności potrzebnych w programie. Takie biblioteki można później ponownie wykorzystywać w kolejnych aplikacjach. Utworzymy dwie biblioteki i usuniemy przykładową klasę z każdej z nich.
### 1. _NazwaAplikacji.App_
Po utworzeniu projektu i usunięci przykładowej klasy, jeszcze zanim utworzymy własną klasę, zajmijmy się zależnościami między projektami (ang. _dependencies_). Wiemy na pewno, że nasz główny projekt będzie korzystał z tego projektu. W naszym _Solution Explorer_ kliknijmy więc prawym przyciskiem myszki na _Dependencies_ naszego głównego projektu i wybierzmy _Add Project Reference..._. Otworzy nam się _Reference Manager_. Powinien automatycznie otworzyć się on na zakładce _Projects_ -> _Solution_, czyli liście projektów w naszej solucji (z wyłączeniem projektu, dla którego menadżer otworzyliśmy). Na liście zaznaczamy check box naszego nowo utworzonego projektu, dodając do niego referencję (informację dla kompilatora, że w danym projekcie będziemy korzystać z innego projektu), i klikamy _OK_.

W tym projekcie znajdować się będzie cała logika naszej aplikacji. Umieścimy w niej wszystkie serwisy (klasy serwisowe) służące do sterowania naszą aplikacją.

Podzielmy jeszcze projekt na foldery:
#### 1. _Abstract_
Tu znajdą się interfejsy dla serwisów. Serwisy będą zajmować się manipulacją naszych obiektów entity. Ich dodawaniem, usuwaniem, aktualizacją, czy zwracaniem.
#### 2. _Common_
Tu umieścimy bazowy serwis, implementujący interfejs bazowy dla serwisów.
#### 3. _Concrete_
Tu znajdą się konkretne implementacje serwisów.
#### 3. _Managers_
Tu znajdą się nasze menadżery. Menadżery, są to swoiste kontrolery, decydujące co użytkownik chce zrobić i na tej podstawie wykonujące odpowiednie akcje (być może wywołujące odpowiednie metody z serwisu). W nich będą znajdywać się wszystkie kroki decyzyjne. Być może będzie to wyświetlenie dodatkowego menu i wybranie odpowiedniej opcji, czy przekazywanie do serwisu odpowiedniego obiektu, tak, aby dodać go do listy. Menadżery będziemy tworzyć dla konkretnych modeli.
### 2. _NazwaAplikacji.Domain_
W tym projekcie znajdować się będą wszystkie modele (klasy reprezentujące obiekt, który w przyszłości mógłby być obiektem bazodanowym).

Podzielmy projekt na foldery według podobnej zasady jak powyższy.
#### 1. _Common_
Tu umieścimy klasę bazową dla modeli.
#### 2. _Entity_
Tu będą się znajdywać implementacje konkretnych modeli.
## 2. Dodać interfejsy dla serwisów
W projekcie _NazwaAplikacji.App_ w folderze _Abstract_ utwórzmy interfejsy dla naszych serwisów.
### 1. `IService<T>`
Zacznijmy od utworzenia interfejsu bazowy dla wszystkich serwisów. Będzie to publiczny (`public`) interfejs generyczny. Umieścimy w nim listę elementów typu `T` oraz podstawowe metody zwracające wszystkie elementy listy, pozwalające na dodanie, aktualizację lub usunięcie elementu listy.
#### Przykład
```csharp =
namespace NazwaAplikacji.App.Abstract
{
    public interface IService<T>
    {
        List<T> Items { get; set; }
        
        List<T> GetAllItems();      // zwracanie wszystkich elementów listy
        int AddItem(T item);        // dodawanie elementu do listy
        int UpdateItem(T item);     // aktualizacja elementu znajdującego się na liście
        void RemoveItem(T item);    // usunięcie elementu z listy
    }
}
```
## 3. Dodać bazowy serwis i bazowy model
W naszej aplikacji utwórzmy bazowy serwis, po którym dziedziczyć będą inne klasy serwisowe, implementujące logikę związaną z konkretnym modelem. Następnie stwórzmy również bazowy model, czyli klasę będącą szkieletem dla wszystkich klas implementujących modele bazodanowe.
### `BaseService`
W projekcie _NazwaAplikacji.App_ w folderze _Common_ stwórzmy plik _BaseService.cs_ w którym umieścimy publiczną klasę generyczną będącą klasą bazową dla serwisów i implementującą bazowy interfejs (`IService<T>`). Tu możemy już stworzyć podstawowy konstruktor, w którym zainicjalizujemy naszą listę. Możemy również zaimplementować podstawowe metody.
#### Przykład
W przykładzie będziemy korzystać z utworzonego poniżej modelu bazowego. Ponieważ serwisy będą obsługiwać konkretne modele, więc chcielibyśmy zaznaczyć to w serwisie bazowym. Ponieważ jest to klasa generyczna, więc typ `T` będzie nam wskazywać, który model obsługujemy. `T` musi więc być typu `BaseEntity` (lub jakiejkolwiek klasy po nim dziedziczącej). Umieśćmy więc w definicji klauzulę `where`. Aby móc korzystać w naszym projekcie _NazwaAplikacji.App_ z elementów projektu  _NazwaAplikacji.Domain_, musimy w tym pierwszym dodać referencję do tego drugiego. Możemy to zrobić za pośrednictwem _Dependencies_ projektu _NazwaAplikacji.App_, analogicznie jak poprzednio, lub podczas pisania kodu, korzystając z inteligentnych podpowiedzi. Kiedy napiszemy nazwę klasy `BaseEntity` przed dodaniem referencji, Visual Studio zaznaczy nam błąd kompilacji, gdyż taka klas nie jest mu znana. Wówczas możemy uruchomić, na będzie, inteligentne podpowiedzi, np. korzystając ze skrótu klawiszowego **Alt + Enter** (lub **Ctrl + .**) i wybrać z listy _Add reference to 'NazwaAplikacji.Domain'._. Wybranie tej opcji spowoduje automatyczne dodanie odpowiedniej pozycji w _Dependencies_ projektu.
```csharp =
namespace NazwaAplikacji.App.Common
{
    public class BaseService<T> : IService<T> where T : BaseEntity
    {
        public List<T> Items { get; set; }
        public BaseService()
        {
            Items = new List<T>();
        }
        public int GetLastId() //pobierz najwyższe Id n liście
        {
            int lastId;
            if(Items.Any()) //list is not empty
            {
                lastId = Items.OrderBy(p => p.Id).LastOrDefault().Id; //sort list and get Id of the last element
            }
            else //no elements on the list
            {
                lastId = 0;
            }
            return lastId;
        }
        public int AddItem(T item)
        {
            Items.Add(item);
            return item.Id;
        }
        public List<T> GetAllItems()
        {
            return Items;
        }
        public void RemoveItem(T item)
        {
            Items.Remove(item);
        }
        public int UpdateItem(T item)
        {
            var entity = Items.FirstOrDefault(p => p.Id == item.Id);
            if(entity != null)
            {
                entity = item;
            }
            return entity.Id;
        }
    }
}
```
### `BaseEntity`
W projekcie _NazwaAplikacji.Domain_ w folderze _Common_ stwórzmy plik _BaseEntity.cs_. Umieśćmy w nim publiczną klasę, która będzie klasą bazową dla wszystkich modeli (wszystkie modele będą po niej dziedziczyć).
#### Przykład
W przykładzie skorzystamy z utworzonej poniżej klasy `AuditableModel`. Ponieważ chcemy, aby wszystkie modele zawierały zdefiniowane w niej pola, więc nasz model bazowy będzie po niej dziedziczył.
```csharp =
namespace NazwaAplikacji.Domain.Common
{
    public class BaseEntity : AuditableModel
    {
        public int Id { get; set; }
    }
}
```
### `AuditableModel`
W projekcie _NazwaAplikacji.Domain_ w folderze _Common_ stwórzmy jeszcze jedną klasę, `AuditableModel`. Mówiliśmy już o niej przy okazji omawiania dziedziczenia (Tydzień 3., lekcja 4.). Przypomnijmy. Jest to popularna klasa bazowa dla modeli, często tworzona w aplikacjach webowych. Ponieważ modele służą najczęściej do odzwierciadlania tabel w bazie danych, a większość z nich zawiera współcześnie informacje o tym kto i kiedy utworzył/zmodyfikował dany rekord, to często tworzy się odrębną klasę przechowującą takie informacje, z której potem mogą korzystać nasze modele (mogą po niej dziedziczyć).
#### Przykład
```csharp =
namespace NazwaAplikacji.Domain.Common
{
    public class AuditableModel
    {
        public int CreatedById { get; set; }            // kto utworzył rekord
        public DateTime CreatedDateTime { get; set; }   // kiedy rekord został utworzony
        public int? ModifiedById { get; set; }          // jezeli rekord zostal zmodyfikowany, to kto go zmodyfikowal (jezeli nie zostal zmodyfikowany to null, dlatego typ int?, czyli nullowalny int)
        public DateTime? ModifiedDateTime { get; set; } // jezeli rekord zostal zmodyfikowany, to kiedy (jezeli nie zostal zmodyfikowany to null, dlatego typ DateTime?, czyli nullowalna struktura DateTime)
    }
}
```
## Uporządkowanie wcześniej utworzonych klas
Kiedy skończymy tworzyć nowe projekty, podzielimy je na foldery i utworzymy bazowe klasy i interfejsy należałoby dopasować utworzone wcześniej klasy do nowego porządku. Jeżeli jakaś klasa niema jeszcze własnego pliku, to go stwórzmy. Umieśćmy zaimplementowane serwisy w folderze _Concrete_ projektu _NazwaAplikacji.App_, a modele w folderze _Entity_ projektu _NazwaAplikacji.Domain_. Przenosząc pliki z jednego projektu do drugiego, musimy pamiętać o kilku rzeczach:
### 1. Zmiana namespace'u
Na górze pliku zmieniamy namespace z nazwy projektu z której plik pochodzi na nazwę projektu do którego został przeniesiony, kropkę i nazwę folderu.
### 2. Dodanie dziedziczenia
Do naszych serwisów i modeli dodajemy dziedziczenie po nowozaimplementowanych klasach bazowych (serwisy - `BaseService<NazwaOdpowiedniegoModelu>`, modele - `BaseEntity`).
### 3. Dodanie właściwych usingów
Na górze plików dodajemy odpowiednie pozycje using potrzebne do korzystania z nowo utworzonych plików.
### 4. Aktualizacja klas uwzględniająca klasy bazowe
Z klas pochodnych usuwamy właściwości i metody, które zaimplementowaliśmy w nowoutworzonych klasach bazowych. Jeżeli chcemy natomiast nadpisać któreś z metod, to dodajemy odpowiednie słowa kluczowe. Dopasowujemy nazwy klas i właściwości.
## Dodanie folderu _Helpers_ w głównym projekcie aplikacji
W głównym projekcie aplikacji, w razie potrzeby, możemy utworzyć dodatkowy folder _Helpers_. Możemy w nim przechowywać elementy pomocnicze dla naszej aplikacji. Mogą to być np. utworzone na potrzeby tej aplikacji enumy, czy klasy, porządkujące nam wyświetlanie w konsoli.
## Poprawa metody `Main`
Po zakończeniu porządkowania klas, musimy poprawić kod w klasie `Main` naszej aplikacji, uwzględniając wprowadzone zmiany.
### Uprośćmy nasz kod
Metoda `Main` powinna zawierać możliwie najmniej elementów. Powinny być one za to jak najbardziej podstawowe.
#### 1. Korzystajmy z klas bazowych.
Gdzie to tylko możliwe, korzystajmy z klas bazowych, aby zwiększyć elastyczność naszego kodu.
#### 2. Przenieśmy co można do innych klas
Jeśli jest to możliwe, to kod dotyczący konkretnej klasy, przenieśmy do tej właśnie klasy. Stwórzmy np. odpowiednie konstruktory i wywoływane w nich metody do wypełniania obiektów klasy danymi inicjalizacyjnymi. Wykorzystajmy menadżery. Przenieśmy do nich interakcję z użytkownikiem.

# [LEKCJA 4 – Typy referencyjne](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-4-typy-referencyjne/)

Zmienne w programie są przechowywane na dwóch typach pamięci: na stosie i na stercie.

## Stos
ang. _stack_, jak mówiliśmy w poprzedniej lekcji, jest to fragment bardzo uporządkowanej pamięci wirtualnej, przydzielonej aplikacji podczas uruchamiania. Jest strukturą typu LIFO (Last-In-First-Out - ostatni włożony, pierwszy wyjęty). Można go sobie wyobrazić jako górę pudełek, ułożonych równo jedno na drugim. Pudełka te są naszymi zmiennymi. Mogą się one od siebie różnić, mieć różną pojemność, ale każde pudełko ma z góry znane wymiary i można wsadzić do niego tylko tyle ile zmieści się do środka. Niezależnie jednak co do nich włożymy (jaka wartość jest aktualnie przypisana do zmiennej), jak bardzo je wypełnimy, pudełko zawsze będzie zajmowało taką samą przestrzeń w naszej wierzy (w pamięci, na stosie). Tworząc nową zmienną "dokładamy nowe pudełko na szczyt naszej wierzy". Jeżeli będziemy sięgnąć "do wnętrza jakiegoś pudełka" będziemy to robić sięgając "od góry po kolejne pudełka". Oznacza to, że najszybciej uzyskamy dostęp do zmiennej, którą ostatnio utworzyliśmy lub użyliśmy. Najczęściej są to właśnie te zmienne do których chcemy się dostać. Przeszukiwanie stosu jest więc bardzo efektywne i umożliwia szybki dostęp do zmiennych. Można jednak przechowywać na nim wyłącznie zmienne o z góry określonym, stałym rozmiarze, alokowane statycznie (kiedy budujemy wierzę, musimy mieć wcześniej przygotowane pudełka). Przechowuje się więc w niej stałe i zmienne typów wartościowych, które zawsze mają taki sam, z góry ustalony, rozmiar, nie zależnie od przechowywanych w nich aktualnie wartości. Poza nimi na stosie znajdują się również referencje do zmiennych referencyjnych, czyli jakby odnośniki do miejsca na stercie, w którym faktycznie przechowywana jest nasza zmienna referencyjna (a dokładniej, gdzie się zaczyna).

## Sterta
ang. _heap_, jest to fragment pamięci wirtualnej nieuporządkowanej aplikacji, o bardziej złożonej strukturze. Służy do przechowywania zmiennych typów referencyjnych, czyli obiektów (ang. _objects_). W odróżnieniu od stosu, który umożliwia tylko dodawanie/usuwanie zmiennych "z wierzchu", na stercie możemy uzyskać dostęp do dowolnego elementu w dowolnym czasie. Oznacza to jednak, że struktura i zarządzanie stosem są o wiele bardziej złożone, co łączy się również z mniejszą wydajnością. Sterta umożliwia dynamiczne alokowanie pamięci i służy do przechowywanie zmiennych typów referencyjnych. Oznacza to, że nie musimy z góry znać wielkości naszej zmiennej, a pamięć jest nam  przydzielana/odbierana na bieżąco wedle potrzeb. Za zarządzanie alokowaniem i zwalnianiem pamięci na stercie w C# odpowiada program zwany _garbage collector_ (GC).

## Zmienne typów referencyjnych
Zmienne typów referencyjnych, inaczej obiekty, deklarujemy analogicznie jak typy wartościowe. Załóżmy, że tworzymy obiekt będący instancją klasy _Item_ (zmienną typu _Item_):

```csharp =
Item item;
```

Powyższy kod spowodował utworzenie na stosie zmiennej typu _Item_. Zmienna ta nie może jednak przechowywać żadnej wartości, a jedynie adres pamięci sterty, gdzie przetrzymywany będzie nasz obiekt. Na razie jednak na stercie nie został jeszcze zarezerwowany żaden fragment pamięci, więc w zmiennej _item_ nie możemy przetrzymywać jeszcze żadnych danych. Aby zaalokować na stercie pamięć dla naszej zmiennej, musimy posłużyć się słowem kluczowym `new` i konstruktorem (specjalną metodą, tworzącą obiekt danego typu) danej klasy, czyli np.:

```csharp =
item = new Item();
```

Możemy oczywiście dwie powyższe czynności wykonać od razu, w jednej linii:

```csharp =
Item item = new Item();
```

Teraz nasza zmienna istnieje zarówno na stosie, jak i na stercie. Możemy więc już korzystać z metod danej klasy, wypełniać jej pola danymi itd. Jeżeli zmienne tworzymy wewnątrz jakiejś metody, po wyjściu z tej metody usunięte zostaną wszystkie utworzone na stosie, również zmienna typu referencyjnego. Obiekt znajdujący się na stercie jednak dalej tam pozostanie. Oby wyczyścić pamięć sterty z niepotrzebnych już obiektów możemy skorzystać z dostarczanego przez .NET, utworzonego w tym celu mechanizmu _garbage collector_ lub podobnie jak w innych językach programowania zrobić to osobiście, używając odpowiedniej metody.

## Typy referencyjne

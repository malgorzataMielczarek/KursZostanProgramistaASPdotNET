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
Do typów referencyjnych zaliczamy przede wszystkim typy dziedziczące po klasie `System.Object`. Są to przede wszystkim elementy tworzone przez samego programistę, czyli klasy, interfejsy. Dodatkowo powszechnie używanym typem referencyjnym jest typ `string`, służący do przechowywania i obsługi tekstu.

### Tworzenie własnego typu referencyjnego - klasy
Aby utworzyć klasę podajemy po kolei modyfikator dostępu, ewentualnie dodatkowe modyfikatorów, słowo kluczowe `class`, nazwę klasy i w nawiasach klamrowych wnętrze klasy. Poniżej pokazano prosty przykład stworzonej przez programistę klasy:

```csharp =
namespace Application
{
	public class Item
	{
		public int Id { get; set; }
		public string Name { get; set }
	}
}
```

Przypominamy, że do dobrych praktyk należy tworzenie każdej klasy w osobnym pliku.<br/>
Teraz już możemy tworzyć obiekty tej klasy. Np. w metodzie `Main` naszej aplikacji możemy wpisać kod:

```csharp =
Item item = new Item() { Id = 1, Name = "Apple" };
```

Jak widać, na podstawie powyższego fragmentu kodu, podczas tworzenia obiektu możemy od razu przypisać wartości jego właściwością. Wartości te możemy później zmieniać, np.:

```csharp =
item.Id = 5;
item.Name = "Banana";
```

### Przypisywanie zmiennych typów referencyjnych
Jeżeli mamy dwie zmienne tego samego typu wartościowego i dokonamy przypisania jednej zmiennej do drugiej, to przypiszemy jedynie aktualnie przechowywaną tam wartość. Czyli np.:

```csharp =
int a = 5;
int b = 6;
Console.WriteLine(a);	//wyświetli się: 5
Console.WriteLine(b);	//wyświetli się: 6

b = a;
Console.WriteLine(a);	//wyświetli się: 5
Console.WriteLine(b);	//wyświetli się: 5

b = 4;
Console.WriteLine(a);	//wyświetli się: 5
Console.WriteLine(b);	//wyświetli się: 4

b = a;
a = 3;
Console.WriteLine(a);	//wyświetli się: 3
Console.WriteLine(b);	//wyświetli się: 5
```

Jeżeli natomiast analogiczne przypisanie wykonalibyśmy między dwoma zmiennymi tego samego typu referencyjnego, to tak na prawdę nie dokonamy przypisania wartości (nie będziemy mieć dwóch obiektów z takimi samymi wartościami właściwości), a jedynie referencji (adresu, pod którym obiekt istnieje na stercie). Oznacza to, że tak na prawdę istnieje tylko jeden obiekt do którego mamy dwie referencje (aliasy), istnieje pod dwoma nazwami. Czyli np.:

```csharp =
Item item = new Item() { Id = 1, Name = "Apple" };
Item item2 = new Item() { Id = 2; Name = "Orange" };
Console.WriteLine(item.Name);	//wyświetli się: Apple
Console.WriteLine(item2.Name);	//wyświetli się Orange

item2 = item;
Console.WriteLine(item.Name);	//wyświetli się: Apple
Console.WriteLine(item2.Name);	//wyświetli się Apple

item2.Name = "Banana";
Console.WriteLine(item.Name);	//wyświetli się: Banana
Console.WriteLine(item2.Name);	//wyświetli się Banana

item.Name = "Melon";
Console.WriteLine(item.Name);	//wyświetli się: Melon
Console.WriteLine(item2.Name);	//wyświetli się Melon
```

W powyższym kodzie mamy więc dwie referencje do obiektu utworzonego w pierwszej linijce, pod nazwami `item` i `item2`. Obiekt utworzony w drugiej linijce dalej istnieje na stercie, ale nie ma już do niego żaden referencji.

Jest możliwe, aby znak przypisania między zmiennymi typu referencyjnego, zamiast przypisania referencji, powodował utworzenie kopii obiektu (drugiego obiektu z przypisanymi takimi samymi wartościami), jednak wymaga to celowej implementacji w klasie.

### String
Klasa `System.String` służąca do przechowywania sekwencji znaków UNICODE UTF-16, czyli sekwencję `char`ów. Zmienne tego typu można również tworzyć używając aliasu `string`, analogicznie do zmiennych typów wartościowych. Tekst który chcemy umieścić w obiekcie typu `string` umieszczamy w cudzysłowach. W _stringach_
występują również tzw. znaki specjalne, np.

| Znak specjalny | Opis |
| ---: | :--- |
| \r | powrót karetki, pozostałość po maszynie do pisania, oznacza powrót na początek linii |
| \n | nowa linia |
| \t | tabulacja |
| \b | backspace |
| \ | backslash jest tzw. znakiem ucieczki, oznacza to, że umożliwia on wstawianie w tekście znaków używanych w innym kontekście, jak np. cudzysłowów |

| Sekwencja wpisana w stringu | wyświetlany tekst | Opis|
| ---: | :---: | :--- |
| `\'` | ' | pojedynczy cudzysłów, apostrof |
| `\"` | " | cudzysłów |
| `\\` | \ | backslash |

Możemy również "wyłączyć" interpretowanie znaków specjalnych. W tym celu przed cudzysłowem rozpoczynającym nasz string wstawiamy znak `@`. Oczywiście wstawienie kolejnego znaku `"` będzie dalej oznaczać zakończenie naszego stringa, a znak `\` nie będzie już interpretowany jako znak ucieczki, tylko po prostu backslash. W tak tworzonym stringu nie da się więc wstawić znaku cudzysłowa. W wielu sytuacjach zapis taki może być jednak bardzo pomocny, np. przy zapisywaniu ścieżek do plików, aby zwiększyć czytelność. Np.:

```csharp
const string FILE = "C:\\Users\\adam\\Images\\smiley.bmp";
// można też zapisać np.
const string SMILEY_PATH = @"C:\Users\adam\Images\smiley.bmp";
```

Wstawiony przed rozpoczynającym string cudzysłowem znaku `$` daje znać kompilatorowi, że chcemy w stringu używać wartości innych zmiennych. Możemy wówczas wpisać wewnątrz stringu nazwę zmiennej otoczoną klamrami. Kompilator zastąpi nam tą sekwencję aktualną wartością zmiennej przekonwertowaną do typu `string`;

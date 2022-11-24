# [LEKCJA 10 – Tablice](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-10-tablice/)
Obiekty służące do przetrzymywania z góry określonej liczby danych tego samego typu. Klasą bazową dla tablic jest zawsze klasa `System.Array` (nie zależnie od tego jakiego typu dane przechowuje tablica). 

## Tworzenie tablic
Tablicę możemy sobie wyobrazić jako mebel na książki. Budując mebel (tablicę) z góry wiemy ile książek (danych) danego typu chcemy w nim umieścić - jakie wymiary ma mieć tablica. Wiemy też, czy ma to być półka (tablica jednowymiarowa), czy może regał (tablica dwuwymiarowa), albo mebel jeszcze innego typu (o innej liczbie wymiarów), czyli z góry wiemy ile wymiarów ma nieć nasza tablica. Oczywiście jeśli jest to regał to wiemy nie ile książek łącznie ma się na nim zmieścić, ale raczej ile ma mieć półek i ile książek zmieści się na każdej półce, a jeżeli jest to cała biblioteka z przesuwnymi regałami, to ile takich regałów ma się w niej znajdować. Inaczej mówiąc budując tablicę podajemy wielkość każdego jej wymiaru. 
### 1. Deklaracja
Deklaracja zmiennej typu tablicowego składa się z typu danych jakie będziemy w niej przechowywać, nawiasu kwadratowego z wymiarami tablicy oddzielonymi przecinkami i nazwy zmiennej. Spowoduje to utworzenie zmiennej na stosie, ale bez rezerwacji miejsca na stercie (czyli nie możemy w niej jeszcze niczego przechowywać - na razie zmówiliśmy mebel, wiemy już czy będzie to półka, szafa, czy jeszcze co innego, ale na razie nikt jej nie buduje, więc nie musimy jeszcze znać jej dokładnych wymiarów). Na tym etapie nie musimy jeszcze znać konkretnych wartości jakie będzie mieć każdy z wymiarów, musimy jednak zaznaczyć ile tych wymiarów będzie. To znaczy w nawiasie kwadratowym nie wpisujemy liczb, ale musimy pozostawić przecinki.
### 2. Inicjalizacja
Inicjalizację pętli wykonuje się z użyciem słowa kluczowego `new`, po którym następuje typ przechowywanych w niej danych i nawias kwadratowy z podanymi wielkościami wymiarów, oddzielonymi od siebie przecinkami (podajemy dokładne wymiary i mebel zostaje zbudowany). Tym razem musimy określić z ilu konkretnie elementów będzie się składał każdy z wymiarów (podać nieujemne wartości typu `int`). Tablic została już utworzona (zarezerwowano pamięć na stercie - mebel jest już zbudowany), ale na razie jest pusta. Pusta, to znaczy jeżeli dany typ danych przechowywany w tablicy może przyjmować wartość `null`, to właśnie taką wartość ma każdy z elementów tablicy. Gdy natomiast dany typ nie może być równy `null`, to każdy z elementów tablicy przyjmuje wartość domyślną dla danego typu (np. dla typu `int` jest to wartość `0`, a dla typu `string` tzw. pusty string - `String.Empty`, czyli inaczej `""`). Teraz powinniśmy więc wypełniać tablicę żądanymi wartościami.
### 3. Wypełnienie tablicy danymi
Możemy to zrobić na kilka sposobów:
1. razem z inicjalizacją, czyli po nawiasie kwadratowym w klamrach podać wartości wszystkich kolejnych elementów oddzielone od siebie przecinkami. Jeżeli tablica jest wielowymiarowa to będziemy zagnieżdżać w sobie nawiasy klamrowe. Np. dla tablicy dwuwymiarowej piszemy nawias klamrowy w jego wnętrzu wstawiamy nawiasy klamrowe oddzielone przecinkami - tyle nawiasów klamrowych ile mamy rzędów (wielkość pierwszego wymiaru), a w każdym z tych nawiasów wypisujemy tyle przedzielonych przecinkami wartości ile rzędów ma nasza tablica (wielkość drugiego wymiaru). Czyli wracając do naszego przykładu z regałem - zewnętrzne klamry będą naszym regałem. Wewnątrz regału mamy półki (nawiasy klamrowe) które są porozdzielane (przecinki). Na każdej półce znajdują się książki (wartości jakie chcemy wpisać do tablicy), które również są pooddzielane przecinkami (nie chcemy przecież aby książki na półce skleiły się w jedną całość). Przy tym sposobie możemy pominąć podawanie wielkości tablicy w wyrażeniu inicjalizacyjnym, gdyż zostanie ona sprecyzowana przez ilość wartości które przypisujemy.
2. po inicjalizacji, w dowolnym miejscu programu. Wpisujemy wartość do konkretnej komórki tabeli. W tym celu podajemy nazwę tabeli, ujęty w nawias kwadratowy indeks, znak przypisania (`=`) i wartość jaką chcemy przypisać. Indeks mówi nam w którym miejscu tabeli mamy wpisać daną wartość (na której półce, w którym miejscu mam wsadzić tą książkę). Jeżeli tablica jest wielowymiarowa, to indeksy poszczególnych wymiarów rozdzielamy przecinkami. Indeksację w tablicach zaczynamy od zera tzn. pierwszy element w tablicy jednowymiarowej będzie miał indeks `[0]`, w dwuwymiarowej `[0, 0]`, t trójwymiarowej `[0, 0, 0]` itd. Możemy oczywiście ręcznie podawać indeksy wszystkich kolejnych elementów tablicy, ale przy wypełnianiu najczęściej robi się to przy pomocy pętli `for`, gdzie za indeks służy zmienna będąca iteratorem pętli. Jeżeli tablica jest wielowymiarowa, to stosuje się zagnieżdżone pętle 'for'. Przypisanie wartości do konkretnej komórki tabeli stosuje się też później w programie do zmian wartości konkretnej komórki.

Pokażmy więc przykład tablicy jedno-, dwu- i trzywymiarowej:

```csharp =
string[] animals;
animals = new string[10];
animals[0] = "cat";
animals[1] = "dog";
animals[2] = "dolphin";
animals[3] = "monkey";
animals[4] = "duck";
animals[5] = "bat";
animals[6] = "hamster";
animals[7] = "rat";
animals[8] = "wolf";
animals[9] = "tiger";

//number of animals in shelter1 - first column, and in shelter2 - second column
int[,] qtty = new int[10, 2] { { 2, 5 }, { 3, 8 }, { 4, 10 }, { 6, 2 }, { 9, 15 }, { 1, 0 }, { 5, 7 }, { 8, 0 }, { 0, 1 }, { 0, 0 } };

//animal index in animals, animal qtty from qtty for shelter1 and shelter2
int[,,] animalQtty = new int[2, 10, 2];
for(int i = 0; i < 2; i++)
{
	//shelter i
	for(int j = 0; j < 10; j++)
	{
		animalQtty[i, j, 0] = j;
		animalQtty[i, j, 1] = qtty[j, i];
	}
}

for(int i = 0; i < 2; i++)
{
	Console.WriteLine("--------------------------------------------------------------");
	Console.WriteLine("Shelter" + (i+1));
	for(int j = 0; j < 10; j++)
	{
		Console.Write(animals[j] + ":\t");
		Console.WriteLine(qtty[j, i]);
	}

}
Console.WriteLine("--------------------------------------------------------------");

/*output:
--------------------------------------------------------------
Shelter1
cat:    2
dog:    3
dolphin:        4
monkey: 6
duck:   9
bat:    1
hamster:        5
rat:    8
wolf:   0
tiger:  0
--------------------------------------------------------------
Shelter2
cat:    5
dog:    8
dolphin:        10
monkey: 2
duck:   15
bat:    0
hamster:        7
rat:    0
wolf:   1
tiger:  0
--------------------------------------------------------------
*/
int[] qtty1 = new int[10], qtty2 = new int[10], indexes1 = new int[10], indexes2 = new int[10];
for(int i = 0; i < 10; i++)
{
	qtty1[i] = qtty[i, 0];
	qtty2[i] = qtty[i, 1];
	indexes1[i] = i;
	indexes2[i] = i;
}
Array.Sort(qtty1, indexes1);
Array.Sort(qtty2, indexes2);

int[,,] animalsQttySorted = new int[2, 10, 2];
for( int i = 0; i < 10; i++)
{
	for (int j = 0; j < 2; j++)
		animalsQttySorted[0, i, j] = animalQtty[0, indexes1[i], j];
}
for (int i = 0; i < 10; i++)
{
	for (int j = 0; j < 2; j++)
		animalsQttySorted[1, i, j] = animalQtty[1, indexes2[i], j];
}
Console.WriteLine("\n\nAfter sorting");
for (int i = 0; i < 2; i++)
{
	Console.WriteLine("--------------------------------------------------------------");
	Console.WriteLine("Shelter" + (i+1));
	for (int j = 0; j < 10; j++)
	{
		Console.Write(animals[animalsQttySorted[i, j, 0]] + ":\t");
		Console.WriteLine(animalsQttySorted[i, j, 1]);
	}
}
Console.WriteLine("--------------------------------------------------------------");
/*output:


After sorting
--------------------------------------------------------------
Shelter1
wolf:   0
tiger:  0
bat:    1
cat:    2
dog:    3
dolphin:        4
hamster:        5
monkey: 6
rat:    8
duck:   9
--------------------------------------------------------------
Shelter2
bat:    0
rat:    0
tiger:  0
wolf:   1
monkey: 2
cat:    5
hamster:        7
dog:    8
dolphin:        10
duck:   15
--------------------------------------------------------------
*/

```

## Właściwości i metody klasy `System.Array`
Jak już wspomnieliśmy klasą bazową dla wszystkich tablic jest klasa `System.Array`. Możemy więc korzystać ze zdefiniowanych w niej właściwości (ang. _properties_) i metod (ang. _methods_). Jedną z podstawowych  właściwości jest `Length`. Przechowuje ona łączną liczbę wszystkich elementów tablicy. Jej wartość możemy uzyskać podając nazwę tablicy, kropkę i nazwę właściwości (`nazwaTablic.Length`). Możemy z niej korzystać np. iterując po wszystkich elementach tablicy, czy chcąc otrzymać ostatni element tablicy. Jednak **UWAGA**, `Length` zwraca liczbę elementów, czyli to co wpisaliśmy w nawiasie klamrowym przy inicjalizacji (a w przypadku tablicy wielowymiarowej wszystkie wartości przemnożone przez siebie). Natomiast indeksowanie w tablicach rozpoczyna się od zera. Oznacza to, że ostatni element tablicy będzie mieć indeks o jeden mniejszy niż długość (wielkość) tablicy (`tablica[tablica.Length - 1]`). Często stosowanymi metodami są np.:
1. `IndexOf` - metoda zwracająca numer indeksu pierwszego napotkanego elementu tablicy jednowymiarowej o podanej wartości (dokładnie tej wartości włącznie np. z wielkością liter). Stosuje się ją w następujący sposób: `Array.IndexOf(nazwaTablicy, szukanaWartosc)`. Czyli np. chcąc w tablicy `animals` z naszego przykładu sprawdzić pod jakim indeksem jest przechowywana małpa (`"monkey"`) napiszemy:

```csharp =
int monkeyIndex = Array.IndexOf(animals, "monkey");
```

2. `Sort` - metoda sortująca elementy w tablicy jednowymiarowej. Sortowanie odbywa się dla typów numerycznych w kolejności od najmniejszego do największego, dla typów znakowych w kolejności alfabetycznej, a dla innych obiektów według specjalnie zdefiniowanych zasad (implementacja interfejsu `IComparer`). W podstawowej wersji Użycie metody sort wygląda następująco: `Array.Sort(nazwaTablicy)`. Np. gdybyśmy nazwy zwierząt z tablicy `animals` (z naszego przykładu) uporządkować w kolejności alfabetycznej napisalibyśmy:

```csharp = 
//skopiujmy najpierw tablicę
string [] animalsSorted = new string[animals.Length];
animals.CopyTo(animalsSorted, 0); //lub Array.Copy(animals, animalsSorted, animals.Length);
//posortujmy nazwy zwierząt w tablicy animalsSorted
Array.Sort(animalsSorted);
```

3. `Clear` - metoda służąca do czyszczenia ("zerowania") tablicy. Przez czyszczenie rozumie się przypisanie takich wartości jak w przypadku inicjalizacji pustej tablicy (czyli dla typu `int` wartości `0`, dla typu `string` `String.Empty`, dla typów mogących przyjmować wartość `null` wartość `null` itd.). Można stosować ją na dwa sposoby. Aby wyczyścić całą tablicę napiszemy: `Array.Clear(nazwaTablicy);`. Gdy natomiast chcemy wyczyścić tylko część tablicy, możemy to zrobić podając dodatkowo jako parametr indeks elementu od którego chcemy zacząć czyszczenie i liczbę elementów które chcemy wyczyścić (`Array.Clear(nazwaTablicy, indeksElementuOdKtoregoZaczynamyCzyszczenie, liczbaElementowDoWyczyszczenia);`).

Metody `IndexOf` i `Sort` można stosować tylko w przypadku tablic jednowymiarowych. Mogą one przyjmować też inne argumenty (inna liczba i/lub typ). Np. w przykładowym kodzie użyliśmy metody `Sort` przyjmującej dwa argumenty typu `Array` (tablice). Pierwszy przechowuje klucze (identyfikatory), a drugi wartości. Pierwsza tablica zostanie posortowana normalnie, a druga według tej samej kolejności co pierwsza (jeżeli pierwszy element tablicy kluczy stanie się teraz jej trzecim elementem, to pierwszy element tablicy wartości również stanie się jej trzecim elementem). Więcej informacji dotyczących tych metod można znaleźć w dokumentacji ([`IndexOf`](https://learn.microsoft.com/en-us/dotnet/api/system.array.indexof?view=net-7.0), [`Sort`](https://learn.microsoft.com/en-us/dotnet/api/system.array.sort?view=net-7.0), [`Clear`](https://learn.microsoft.com/en-us/dotnet/api/system.array.clear?view=net-7.0)). Można tam też znaleźć więcej właściwości i metod klasy [`System.Array`](https://learn.microsoft.com/en-us/dotnet/api/system.array?view=net-7.0), np. użytą w przykładzie `Sort` metodę do kopiowania tablic [`CopyTo`](https://learn.microsoft.com/en-us/dotnet/api/system.array.copyto?view=net-7.0) lub [`Copy`](https://learn.microsoft.com/en-us/dotnet/api/system.array.copy?view=net-7.0).

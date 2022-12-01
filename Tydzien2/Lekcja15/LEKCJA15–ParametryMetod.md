# [LEKCJA 15 – Parametry metod](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-15-parametry-metod/)
Jak wiemy z poprzednich lekcji metody mogą przyjmować dowolną liczbę argumentów. Jeżeli jednak nasza funkcja musi przyjąć ich wiele (powyżej 7), to dobrą praktyką jest, aby utworzyć nową klasę przechowującą wartości tych argumentów i przekazywać do metody obiekt tej klasy. Argumenty podajemy w nawiasie okrągłym za nazwą funkcji i oddzielamy od siebie przecinkami. Istnieją one tylko w obrębie metody.

## Parametry w definicji metody
W definicji funkcji każdy parametr składa się z typu i nazwy (deklarujemy zmienne, których będziemy używać w funkcji).

```csharp =
modyfikatorDostepu typ nazwaMetody (typ1 nazwaParametru1, typ2 nazwaParametru2, ..., typN nazwaParametruN)
{
    typ ret;
    //operacje z wykozystaniem parametrow
    //nadanie zmiennej ret wartosci
    return ret;
}
```

## Parametry w wywołaniu metody
W wywołaniu metody są to wartości (literały, zmienne, obiekty), które chcemy przekazać do metody (inicjalizujemy zmienne zadeklarowane w definicji metody, przypisując im konkretne wartości). Możemy to zrobić wypisując same wartości. Należy jednak uważać, oby kolejność wypisywanych wartości była zgodna z kolejnością parametrów w definicji metody (pierwsza wartość zostanie przypisana do pierwszego parametru, druga do drugiego itd.). C# daje nam również możliwość przekazywania argumentów w dowolnej kolejności. Wówczas musimy jednak dla każdego parametru podać jego nazwę (użytą w definicji metody), dwukropek i wartość jaką chcemy temu parametrowi przypisać. W obu przypadkach kolejne parametry oddzielamy od siebie przecinkami. Musimy też zawsze podać wartości dla wszystkich obowiązkowych parametrów metody (parametry opcjonalne opcjonalne opisane są niżej).

```csharp =
NazwaKlasy objekt = new NazwaKlasy();
typ1 parametr1;
typ2 parametr2;
...
typN paremetrN;

//nadanie wartosci parametrom

//wywolanie metody
typ rezultat = objekt.nazwaMetody(parametr1, parametr2, ..., parametrN);
//lub
typ rezultat2 = objekt.nazwaMetody(nazwaParametruN: parametrN, nazwaParametru1: parametr1, nazwaParametru2: parametr2, ...);
```

## Typy parametrów
Możemy zadeklarować parametry dowolnego typu. Mogą to być zarówno typy wartościowe, jak i klasy. Należy jednak pamiętać, że typ przekazywanej wartości musi być zgodny z zadeklarowanym typem parametru (lub musi być możliwość niejawnej konwersji do tego typu).

## Parametry opcjonalne
1. Definicja<br/>
Są to parametry, którym w definicji metody przypisujemy wartość domyślną. Robimy to dodając w liście parametrów za nazwą argumentu znak `=` i wartość którą chcemy przypisać. Wartość ta musi być stałą (wartością znaną w momencie kompilacji). Parametry domyślne muszą być podane na końcu listy parametrów (za parametrem opcjonalnym nie może znaleźć się żaden parametr obowiązkowy).

```csharp =
public void Metoda(typ1 nazwaParametru1, typ2 nazwaParametry2 = wartoscTypu2, ..., typN nazwaParametruN = wartoscTypuN)
{
    //ciało metody
}
```

2. Wywołanie<br/>
W wywołaniu metody z parametrami opcjonalnymi nie musimy przypisywać im żadnej wartości. To znaczy, że jeżeli nie podamy wartości dla tych parametrów, to przyjmą one wartość domyślną z definicji, natomiast jeżeli je podamy, to przyjmą one podane przez nas wartości. Możemy też podać wartości tylko wybranym parametrom domyślnym. Jeżeli w wywołaniu metody używamy sposobu z wypisaniem kolejno samych wartości parametrów, to musimy podać wszystkie wartości z ewentualnym pominięciem parametrów domyślnych na końcu. To znaczy, że jeżeli chcielibyśmy przekazać wartość do trzeciego z kolei parametru domyślnego, to musimy również podać wartości pierwszego i drugiego parametru domyślnego. Jeżeli natomiast użyjemy sposobu z użyciem nazw parametrów, to musimy przypisać wartości wszystkim parametrom obowiązkowym i możemy przypisać wartości tylko tym argumentom obowiązkowym, którym chcemy, bez względu na ich kolejność. Można też połączyć oba sposoby, np. wypisując po kolei wartości wszystkich parametrów obowiązkowych, a następnie podać wartości parametrów z nazwami dla wybranych parametrów opcjonalnych.

```csharp =
NazwaKlasy objekt = new NazwaKlasy();
typ1 parametr1;
typ2 parametr2;
...
typN paremetrN;

//nadanie wartosci parametrom

//przykłady wywołania metody z parametrami opcjonalnymi
objekt.Metoda(parametr1);
objekt.Metoda(parametr1, parametr2, ..., parametrN);
objekt.Metoda(parametr1, parametr2);
objekt.Metoda(nazwaParametruN: parametrN, nazwaParametru1: parametr1, nazwaParametru2: parametr2);
objekt.Metoda(parametr1, parametr2, nazwaParametruN: parametrN);
```

## Słowa kluczowe `out` i `ref`
Tradycyjnie metoda może zwrócić tylko jedną wartość. Gdybyśmy jednak potrzebowali uzyskać z niej kilka wartości możemy użyć tzw. argumentów wyjściowych. Jak już wspomnieliśmy normalnie argumenty istnieją tylko wewnątrz metody (są usuwana po zakończeniu jej wykonywania). Używając jednak słów kluczowych `out` lub `ref` możemy stworzyć argumenty, które będą istnieć nawet po zakończeniu wykonywania metody. Robimy to dodając przed nazwą parametru (w definicji metody) i przypisywaną mu wartością (w wywołaniu metody, niezależnie którym sposobem) wybrane słowo kluczowe. Sposób ten jest jednak rzadko stosowany. Kiedy musimy zwrócić kilka wartości najczęściej tworzy się nową klasą, która będzie je przechowywać i zwraca się obiekt tej klasy. Czasami jednak spotkamy się z użyciem parametrów wyjściowych. Mogliśmy to już np. zobaczyć w funkcjach do parsowania `string`ów na inne typy. Np:

```csharp =
int age;
Console.Write("Wiek: ");
string ageString = Console.ReadLine();
Int32.TryParse(ageString, out age);
```

Zobaczmy jednak co użycie tych słów kluczowych właściwie oznacza.
### Przekazywanie argumentów przez wartość
Normalnie parametry metod są przekazywane przez wartość. To znaczy, że przekazujemy samą liczbę (typy liczbowe), znak (`char`), referencję do obiektu na stercie (typy referencyjne) itd., a nie zmienną. Jeżeli więc w metodzie przypiszemy parametrowi inną wartość (lub zmodyfikujemy dotychczasową), to zmiany te nie będą widoczne poza metodą. Oczywiście jeżeli używamy typu referencyjnego i nie zmieniając przechowywanej w argumencie referencji dokonujemy zmian we wskazywanym przez nią obiekcie na stosie, to zmiany te będą widoczne również gdy odwołamy się do tego obiektu przez zmienną spoza metody. Jeżeli jednak w metodzie przypiszemy argumentowi nową referencję (inny obiekt), to nie zmieni to obiektu na który wskazuje zmienna, którą użyliśmy w wywołaniu. 
### Przekazywanie argumentów przez referencję
Użycie słów kluczowych `ref` lub `out` oznacza, że ten argument przekazujemy przez referencję, a nie przez wartość. Czyli nasz argument nie jest nową zmienną, a jedynie aliasem do zmiennej przekazanej w wywołaniu (w wywołaniu musimy użyć zmiennej, czegoś co ma zarezerwowany konkretny adres w pamięci, a nie np. literału). Oznacza to, że argument jest tak na prawdę tą samą zmienną, co ta przekazana w wywołaniu (wskazuje na ten sam obszar w pamięci), występuje ona tylko pod nową nazwą. Wszelkie zmiany jakie dokonamy w takim argumencie będą więc też widoczne w oryginalnej zmiennej (użytej w wywołaniu) po zakończeniu działania metody.
### Porównanie argumentów `out` i `ref`

| Cecha | `ref` | `out` |
| :--- | :---: | :---: |
| Inicjalizacja zmiennej, przed przekazaniem jako argument metody | musi być zainicjalizowana | nie musi być zainicjalizowana |
| Inicjalizacja lub przypisanie wartości do parametru wewnątrz metody | nie wymagana | wymagana przed zakończeniem metody |
| Zastosowanie | gdy metoda powinna zmodyfikować wartość przekazanej zmiennej | gdy metoda ma zwrócić więcej niż jedną wartość |
| Inicjalizacja wartości argumentu wewnątrz metody przed jej użyciem (wewnątrz metody) | opcjonalna | wymagana |
| Kierunek przekazywania danych | dane mogą być przekazane zarówno do metody, jak i z metody (dwukierunkowe) | dane są przekazywane tylko z metody (jednokierunkowe) |

1. Oba rodzaje argumentów są traktowane inaczej podczas wykonywania, ale tak samo podczas kompilacji. Oznacza to, że nie będziemy mogli przeciążyć metod, gdzie jedyną różnicą będzie to, że jedna będzie przyjmować argument jako `ref`, a druga jako `out` (o przeciążaniu metod będzie w przyszłym tygodniu).
2. Pamiętajmy również, że właściwości metod nie są zmiennymi. Nie możemy więc ich przekazać jako parametrów `out` lub `ref`.
3. Argumenty `out` i `ref` nie mogą być również argumentami domyślnymi.

# [LEKCJA 5 – Warunki](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-5-warunki/)
Instrukcje warunkowe umożliwiają wykonanie określonych instrukcji lub zwrócenie określonej wartości w zależności od spełnienia jakiegoś warunku.
## if
Podstawową instrukcją warunkową jest instrukcja `if`. Ma ona postać:

```csharp =
if (warunek)
{
	//instrukcje wykonywane jeśli warunek jest spełniony (ma wartość bool true)
}
```

Działa ona w następujący sposób. Jeżeli warunek, będący wyrażeniem typu `bool`, ma wartość `true`, to zostają wykonane kolejno instrukcje znajdujące się pomiędzy klamrami. Jeśli natomiast warunek ma wartość `false` instrukcje znajdujące się pomiędzy klamrami są pomijane i program przechodzi bezpośrednio do kodu znajdującego się po instrukcji `if`.

Ta instrukcja warunkowa może mieć bardziej rozbudowaną formę tzw. `if else`. Wygląda wówczas następująco:

```csharp =
if (warunek)
{
	// pierwszy blok instrukcji - instrukcje wykonywane jeśli warunek jest spełniony (ma wartość bool true)
}
else
{
	//drugi blok instrukcji - instrukcje wykonywane jeśli warunek nie jest spełniony (ma wartość bool false)
}
```

W powyższym przypadku, jeżeli warunek ma wartość `true` wykonują się instrukcje z pierwszego bloku (znajdujące się między klamrami po słowie kluczowym `if`), natomiast blok drugi (instrukcje między klamrami po słowie kluczowym `else`) jest ignorowany. Natomiast jeśli warunek ma wartość `false`, pierwszy blok kodu jest ignorowany, a wykonuje się drugi blok.

Możemy również dokonywać wyborów wielowariantowych. Wówczas polecenie `if else` rozbudowujemy o sekcję `else if`, czyli:

```csharp =
if (warunek1)
{
	//pierwszy blok instrukcji - instrukcje wykonywane jeśli warunek1 jest spełniony (ma wartość bool true)
}
else if (warunek2)
{
	//drugi blok instrukcji - instrukcje wykonywane jeśli warunek1 ma wartość false, ale warunek2 jest spełniony (ma wartość bool true)
}
else
{
	//ostatni blok instrukcji - instrukcje wykonywane jeśli żaden z warunków nie jest spełniony (zarówno warunek1, jak i warunek2 mają wartość bool false)
}
```

Sekcji `else if` może być dowolnie dużo (możemy więc sprawdzać dowolną ilość warunków). Powyższa instrukcja działa analogicznie jak poprzednie, z zastrzeżeniem, że po napotkaniu pierwszego warunku o wartości `true`, kolejne warunki nie są nawet sprawdzane. Tak więc najpierw obliczana jest wartość warunku `warunek1`. Jeżeli wynosi ona `true` to wykonuje się pierwszy blok instrukcji, a reszta instrukcji warunkowej jest ignorowana. Natomiast gdy `warunek1` ma aktualnie wartość `false`, pominięty zostaje pierwszy blok instrukcji i przechodzimy do obliczania wartości `warunek2`. Jeżeli wynosi ona `true` wykonuje się drugi blok instrukcji, a pozostała część instrukcji warunkowej jest ignorowana. Gdy natomiast ma ona wartość `false`, drugi blok instrukcji jest ignorowany i jeżeli jest więcej sekcji `else if` to postępujemy z nimi analogicznie, a jak nie to wykonują się instrukcje z ostatniego bloku, po słowie kluczowym `else`. Instrukcja warunkowa wielowariantowa nie musi zawierać sekcji `else`. Wówczas, jeżeli żaden z warunków nie jest spełniony to żadna z instrukcji wewnątrz instrukcji warunkowej się nie wykona.

We wszystkich powyższych przypadkach, jeżeli któryś z bloków instrukcji składa się tylko z jednego polecenia to otaczające je nawiasy klamrowe można pominąć.

Możliwe jest również zagnieżdżanie instrukcji `if` dowolnych wariantów. To znaczy, że blok instrukcji może również zawierać instrukcję `if` (`if else`, `if else if else` itd.).

## Operator trójargumentowy
ang. _conditional operator_ lub _ternary conditional operator_ (operator warunkowy, operator trójargumentowy), czyli operator `?:` zwracający różne wartości w zależności od spełnienia określonego warunku. Ma on następującą budowę:

```csharp =
var zmienna = warunek ? wartosc1 : wartosc2;
```

Jeżeli wyrażenie warunkowe ma wartość `true` ( `warunek` jest równy `true`) `zmienna` przyjmie wartość `wartosc1`. Gdy natomiast ma wartość `false`, to `zmienna` będzie mieć wartość `wartosc2`. Operatory mogą być również w sobie zagnieżdżone. Tą samą operację można przeprowadzić używając instrukcji `if else`, jednak użycie operatora trójargumentowego skraca zapis. Np. operacja:

```csharp =
int index = item.Name == 'Banana' ? 1 : 0;
```

zapisana przy pomocy instrukcji `if else` będzie wyglądać następująco:

```csharp =
int index;
if (item.Name == "Banana")
{
	index = 1;
}
else
{
	index = 0;
}
```

## switch
Instrukcja warunkowa sprawdzająca czy jej argument jest równy konkretnej, z góry znanej wartości. Ma ona następującą budowę:

```csharp =
switch (argument)
{
	case wartosc1:
		//blok 1 - instrukcje wykonywane gdy argument == wartosc1
		break;
	case wartosc2:
		//blok 2 - instrukcje wykonywane gdy argument == wartosc2
		break;
	.
	.
	.
	default:
		//ostatni blok - instrukcje wykonywane gdy argument nie jest równy żadnej z wymienionych wyżej wartości
		break;
}
```

Powyższa instrukcja działa tak samo jak następująca instrukcja `if`:

```csharp =
if (argument == wartosc1)
{
	//blok 1
}
else if (argument == wartosc2)
{
	//blok 2
}
.
.
.
else
{
	//ostatni blok
}
```

Operator `==` jest operatorem porównania, czyli wyrażenie `a == b` ma wartość `true`, jeżeli zmienna `a` ma taką samą wartość jak zmienna `b`. W przeciwnym razie wyrażenie ma wartość `false`. Słowo kluczowe `break` oznacza przerwanie wykonywania danej instrukcji. Czyli po dojściu do instrukcji `break` program natychmiast przerywa wykonywanie instrukcji `switch` i przechodzi do wykonywania kolejnych poleceń po niej występujących. W odróżnieniu np. od instrukcji `if` bloki instrukcji nie są otoczone nawiasami klamrowymi. Dany blok zaczyna się za znakiem `:` i kończy poleceniem `break` (zamiast `break` możliwe jest również użycie instrukcji `return` lub `throw`). Taki zapis jest wynikiem zaszłości z języków poprzedzających język C#. W odróżnieniu od np. C++, w C# umieszczenie instrukcji `break` jest obligatoryjne. W instrukcji `switch` analogicznie do instrukcji `if` może być sprawdzana dowolna liczba przypadków (dowolna liczba bloków `case`). Można również pominąć blok `default` (czyli odpowiednik `else` z `if`a). Instrukcja `switch` ma ograniczone działanie w stosunku do instrukcji `if`. Tradycyjnie po pierwsze jej warunki są wyłącznie operacjami porównania dwóch wartości typów podstawowych (typy wartościowe, stringi), `argument` musi więc być zmienną/wyrażeniem typu podstawowego. Sytuacja ta zmieniła się nieco od wersji C# 7, od której składnia i możliwości instrukcji `switch` zostały znacznie rozbudowane i są jeszcze bardziej rozszerzane w kolejnych wersjach języka (dla zainteresowanych ciekawy artykuł o `switch` w C# 7 i C# 8 - [link](https://geek.justjoin.it/nowy-switch-w-c-8-0-jak-dziala-property-pattern/)). Drugim ograniczeniem jest, że wartości z którymi porównywany jest nasz `argument` muszą być stałymi (wartościami znanymi jeż na etapie kompilacji), w odróżnieniu od instrukcji `if`, gdzie wartości te obliczane są dopiero podczas wykonywania instrukcji (sprawdzania warunku). Przez te ograniczenia instrukcja `switch` jest dość rzadko stosowana. Może nam się jednak przydać np. do obsługi menu naszej aplikacji konsolowej, co jest dobrym przykładem jej zastosowania.

## switch expressions
Występujące od C# 8 wyrażenie o działaniu podobnym do instrukcji `switch`, które jednak podobnie jak operator trójargumentowy zwraca jakąś wartość (zależną od tego który warunek jest spełniony). Ma ono następującą budowę:

```csharp =
var zmienna = argument switch
{
	wzorzec1 => wartosc1,
	wzorzec2 => wartosc2,
	.
	.
	.
	_ => wartoscN
};
```

Ma ona następujące działanie:
1. sprawdzamy, czy `argument` jest zgodny ze wzorcem `wzorzec1`
	1. jeżeli tak, to `switch` zwraca wartość `wartosc1` i kończy swoje działanie
	2. jeżeli nie, to przechodzimy do punktu 2.
2. sprawdzamy, czy `argument` jest zgodny z kolejnym wzorcem
	1. jeżeli tak, to `switch` zwraca odpowiednią wartość i kończy swoje działanie
	2. jeżeli nie, to
		1. jeżeli są jeszcze jakieś wzorce do sprawdzenia, wracamy do punktu 2.
		2. jeżeli nie ma już żadnych wzorców przechodzimy do punktu 3.
3. ostatnim elementem wyrażenia `switch` może być `_`, choć nie jest to element obligatoryjny. Oznacza on dowolną wartość i jest równoważny sekcji `default` instrukcji `switch`. Jeżeli ten element istnieje i `argument` nie pasował do żadnego ze wzorców, wyrażenie zwraca wartość `wartoscN`.


Począwszy od C# 7 `argument` zarówno w instrukcji jak i wyrażeniu `switch` może być obiektem lub tulpem (zestawem wartości, zamiast jedną wartością). Począwszy od C# 9 możliwe jest również poza prostym porównaniem, porównanie relacyjne (<, <=, >, >=).

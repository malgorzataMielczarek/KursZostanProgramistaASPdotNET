# [LEKCJA 6 – Operatory](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-6-operatory/)

Kolejność wykonywania działań z użyciem operatorów jest zgodna z zasadami matematyki. Jeżeli kilka operacji jest równoważnych (tak samo ważnych jeśli chodzi o kolejność wykonywania działań) to są one wykonywane od lewej do prawej. Jeżeli ostateczny wynik będziemy znać po wykonaniu tylko części operacji (będzie to czasem miało miejsce np. w przypadku operacji logicznych), to dalsze operacje nie są wykonywane.

## Arytmetyczne
Zdefiniowanie zmiennych do przykładów z poniższej tabeli:

```csharp =
int a = 5;
int b = 10;
```

| Operator | Znaczenie | Przykład |
| :---: | :--- | :--- |
| `+` | dodawanie | `int c = a + b; // wynik: 15` |
| `-` | odejmowanie | `int c = b - a; // wynik: 5` |
| `*` | mnożenie | `int c = a * b; // wynik: 50` |
| `/` | dzielenie | `int c = b / a; // wynik: 2` |
| `%` | modulo - reszta z dzielenia | `int c = b % a; // wynik: 0` |

Jeżeli wynik operacji chcemy przypisać do zmiennej znajdującej się po lewej stronie operatora, zapis operacji możemy skrócić stosując operatory (`+=`, `-=`, `*=`, `/=` i `%=`), np.:

```csharp =
int a = 0;
int b = 10;
a += 2; // to samo co a = a + 2;
// czyli teraz a == 2, b == 10
a -= b; // to samo co a = a - b;
//teraz a == -8, b == 10
a *= -1; // to samo co a = a * (-1);
// a == 8, b == 10
b %= a; // to samo co b = b % a;
// a == 8, b == 2
a /= b; // to samo co a = a / b;
// a == 4, b == 2
```

## Inkrementacji/dekrementacji
1. **Inkrementacja** - zwiększenie wartości o jeden. Jest to operacja jednoargumentowa, której operatorem są dwa znaki plus (`++`). Może występować w dwóch wersjach:
	1. postinkrementacja -  operator wstawiamy po argumencie. W tym wypadku wyrażenie ma wartość argumentu (najpierw następuje przypisanie, a potem inkrementacja). Np.:
	
	```csharp =
	int i = 0;
	i++; // to samo co i = i + 1; lub i += 1;
	// wyrażenie ma wartość i
	```
	
	2. preinkrementacja -  operator wstawiamy przed argumentem. W tym wypadku wyrażenie ma wartość argumentu powiększonego o jeden (najpierw następuje inkrementacja, a potem przypisanie). Np.:
	
	```csharp =
	int i = 0;
	++i; // to samo co i = i + 1; lub i += 1;
	// wyrażenie ma wartość i + 1
	```
	
	Przykład pokazujący różnicę między postinkrementacją i preinkrementacją:
	
	```csharp =
	int i = 0, post, pre;
	
	// Postinkrementacja
	post = i++;
	/*
	Inaczej można to zapisać:
	post = i;
	i = i + 1;
	czyli teraz post == 0, a i == 1
	*/
	
	i = 0;
	//Preinkrementacja
	pre = ++i;
	/*
	Inaczej można to zapisać:
	i = i + 1;
	pre = i;
	czyli teraz pre == 1, a i == 1
	*/
	```

2. **Dekrementacja** - zmniejszenie wartości o jeden. Jest to operacja jednoargumentowa, której operatorem są dwa znaki minus (`--`). Podobnie jak inkrementacja, może występować w dwóch wersjach, o analogicznym działaniu:
	1. postdekrementacja -  operator wstawiamy po argumencie. W tym wypadku wyrażenie ma wartość argumentu (najpierw następuje przypisanie, a potem dekrementacja). Np.:
	
	```csharp =
	int i = 0;
	i--; // to samo co i = i - 1; lub i -= 1;
	// wyrażenie ma wartość i
	```
	
	2. predekrementacja -  operator wstawiamy przed argumentem. W tym wypadku wyrażenie ma wartość argumentu pomniejszonego o jeden (najpierw następuje dekrementacja, a potem przypisanie). Np.:
	
	```csharp =
	int i = 0;
	--i; // to samo co i = i - 1; lub i -= 1;
	// wyrażenie ma wartość i - 1
	```
	
	Przykład pokazujący różnicę między postdekrementacją i predekrementacją:
	
	```csharp =
	int i = 0, post, pre;
	
	// Postdekrementacja
	post = i--;
	/*
	Inaczej można to zapisać:
	post = i;
	i = i - 1;
	czyli teraz post == 0, a i == -1
	*/
	
	i = 0;
	//Predekrementacja
	pre = --i;
	/*
	Inaczej można to zapisać:
	i = i - 1;
	pre = i;
	czyli teraz pre == -1, a i == -1
	*/
	```
	
Podsumowanie:

| Operator | Znaczenie | Przykład |
| :---: | :--- | :--- |
| `++` | inkrementacja | `i++; // postinkrementacja`<br/>`++i; // preinkrementacja` |
| `--` | dekrementacja | `i--; // postdekrementacja`<br/>`--i; // predekrementacja` |

## Przypisania
Operator pozwalający nadać zmiennej wartość. Jest to po prostu znak równa się (`=`) i był już przez nas wielokrotnie stosowany, np.:

```csharp =
int a, b;
a = 5; // nadanie zmiennej a wartości 5
b = a; // nadanie zmiennej b takiej samej wartości jaką ma zmienna a, czyli 5
```

| Operator | Znaczenie | Przykład |
| :---: | :--- | :--- |
| `=` | przypisanie wartości do zmiennej | `int i = 0;` |

## Relacji
Operatory porównujące dwie wartości i zwracające wartość `bool` `true` jeśli zależność jest prawdziwa lub `false`, jeśli nie. Zdefiniowanie zmiennych do przykładów z poniższej tabeli:

```csharp =
int a = 5;
int b = 5;
```

| Operator | Znaczenie | Przykład |
| :---: | :--- | :--- |
| `==` | czy równe | `bool c = (a == b); // wynik: true` |
| `!=` | czy różne | `bool c = (a != b); //wynik: false` |
| `>` | większe niż | `bool c = (a > b); // wynik: false` |
| `<` | mniejsze niż | `bool c = (a < b); // wynik: false` |
| `>=` | większe równe | `bool c = (b >= a); // wynik: true` |
| `<=` | mniejsze równe | `bool c = (b <= a); // wynik: true` |

Przykład użycia:

```csharp =
int a = 5;
int b = 0;
int result;

if (b != 0)
{
	result = a / b;
	Console.WriteLine($"{a} // {b} = {result}");
}
else
{
	Console.WriteLine("Nie można dzielić przez zero.");
}
```	

## Logiczne warunkowe
Zostaną omówione w kolejnej lekcji (lekcja 7).

## Konkatenacji
czyli łączenia łańcuchów tekstowych. Operatorem konkatenacji jest znak plus (`+`). Np.:

```csharp =
string person = "Ala";
string action = "ma kota.";
string sentence = person + " " + action;
Console.WriteLine(sentence); //zostanie wypisane: Ala ma kota.
sentence = person + " miała kota";
sentence += ", ale zdechł.";
Console.WriteLine(sentence); //zostanie wypisane: Ala miała kota, ale zdechł.
```

Podobnie jak w przypadku operatora dodawania, można go łączyć z operatorem przypisania (`+=`), np.:

```csharp =
string a = "Ala";
a += " ma kota."; // to samo co a = a + " ma kota."
// wynik: Ala ma kota.
```

| Operator | Znaczenie | Przykład |
| :---: | :--- | :--- |
| `+` | dla argumentów typu `string` oznacza konkatenację, połączenie dwóch stringów w jeden | `string a = "Ala" + " miała kota." // wynik: Ala miała kota.` |

## Łączenia wartości null
ang. _null coalescing operator_. Operatorem łączenia wartości null jest podwójny znak zapytania (`??`). Jest to operator dwuargumentowy.

Budowa wyrażenia: `x ?? y`, gdzie `x` jest wyrażeniem dowolnego typu, który może przyjmować wartość `null`, a `y` typu, który może zostać przypisany do `x` (nie musi móc przyjmować wartości `null`).

Działanie:</br>
1. Obliczenie wartości wyrażenia `x`
2. Jeżeli `x` wynosi `null` to patrz 3., przeciwnie patrz 4.
3. Obliczanie wyrażenia `y`. `x ?? y` ma wartość wyrażenia `y`.
4. `x ?? y` ma wartość wyrażenia `x`.

Np.:
```csharp =
string? nullableString = null;
string notNullString = nullableString ?? "Tu jest NULL";
Console.WriteLine(notNullString); //zostanie wypisane: Tu jest NULL

nullableString = "Już niema NULL";
notNullString = nullableString ?? "Tu jest NULL";
Console.WriteLine(notNullString); //zostanie wypisane: Już niema NULL
```

Począwszy od C# 8.0 operator łączenia wartości null można również stosować w połączeniu z operatorem przypisania (`??=`). Wówczas wartość wyrażenia łączenia wartości null jest przypisywana do lewego argumentu tego wyrażenia. W tym wypadku lewy argument musi być zmienną, właściwością lub indeksatorem. Np.:

```csharp =
int? a = null;

a ??= 5; //to samo co a = a ?? 5;, czyli a jest teraz równe 5
a ??= 7; //to samo co a = a ?? 7;, czyli a jest nadal równe 5
```

Operatory `??` i `??=` są prawostronnie asocjacyjne. Oznacza to, że operatory te są grupowane od prawej do lewej. Czyli wyrażenie `a ?? b ?? c`, jest przetwarzane jako `a ?? (b ?? c)`, a wyrażenie `d ??= e ??= f`, jako `d ??= (e ??= f)`. Ogólnie wyrażenie postaci `E1 ?? E2 ?? ... ?? EN` zwraca wartość pierwszego argumentu różnego niż `null` lub `null`, jeżeli wszystkie argumenty mają wartość `null`. Analogicznie w wyrażeniu `E1 ??= E2 ??= ... ??= EN` `E1` będzie miał wartość pierwszego argumentu różnego od `null` lub `null`, jeżeli wszystkie argumentu mają wartość `null`.

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

## switch


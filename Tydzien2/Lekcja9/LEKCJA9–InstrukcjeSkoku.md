# [LEKCJA 9 – Instrukcje skoku](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-9-instrukcje-skoku/)
Instrukcje składające się z określonego słowa kluczowego i średnika. Najogólniej mówiąc służą do przenoszenia do innej części kodu. Najczęściej stosuje się je w połączeniu z instrukcją warunkową `if`. Wyróżniamy następujące instrukcje skoku:

## break
Stosowana głównie w pętlach, gdy istnieje potrzeba przerwania działania pętli, nawet jeśli warunek zakończenia pętli nie został jeszcze spełniony (warunek dalej jest równy `true`). Kiedy program napotyka na instrukcję `break` wykonywana właśnie pętla (lub inna instrukcja np. `switch`) zostaje natychmiast przerwana (przeskakujemy do kodu wykonywanego po danej instrukcji). Weźmy np., użytą w poprzedniej lekcji do pokazania różnicy między pętlą `while` i `do while`, pętlę nieskończoną `do while` wypisującą podaną liczbę i kolejne liczby, jeżeli były one dodatnie. Użyjmy instrukcji `break`, aby wypisywać tą pętlą nie więcej niż 10 cyfr:

```csharp =
Console.Write("Podaj liczbę: ");
int j = Int32.Parse(Console.ReadLine());
int i = 0;
do
{
	Console.Write(j);
	if( i == 9) break;
	i++;
	j++;
}
while(j > 0);
Console.WriteLine();
//np. dla j = 0
//zostanie wypisane: 0123456789
//a dla j = 5
//zostanie wypisane: 567891011121314
//a dla j = -1
//zostanie wypisane: -1
```

Teraz powyższa pętla nie jest już pętlą nieskończoną.

## continue
Stosowana w pętlach, gdy istnieje potrzeba pominięcia części kodu w bloku kodu pętli. Po napotkaniu tej instrukcji program nie wykonuje następujących po niej instrukcji bloku kodu, a przeskakuje prosto do sprawdzenia warunku wykonywania pętli. Np.:

```csharp =
for(int i = 0; i < 10; i++)
{
	Console.Write("+");
	
	if (i%2 == 1) continue;
	
	Console.WriteLine(";");
	Console.Write(i/2);
}
//program wypisze w konsoli:
//	+;
//	0++;
//	1++;
//	2++;
//	3++;
//	4+
```

Oczywiście powyższy program niema większego sensu i można go zaprogramować z użyciem samej instrukcji warunkowej (bez instrukcji `continue`), ale chodziło nam tu tylko o pokazanie działania instrukcji `continue`.

## goto
Służy do przejścia w zupełne inne miejsce w kodzie. Składa się ze słowa kluczowego `goto` i nazwy etykiety oraz etykiety umieszczonej w kodzie w miejscu do którego chcemy się przenieść. Program po natrafieniu na instrukcję `goto` przeskakuje do miejsca w kodzie gdzie znajduje się etykieta o podanej nazwie i nie wraca już do miejsca w kodzie gdzie znajduje się instrukcja `goto`:

```csharp =
...
goto Etykieta;
.
.
.
Etykieta:
//kod wykonywany po napotkaniu instrukcji goto lub gdy program dotarł do niego w skutek normalnego wykonywania kolejnych instrukcji
```

Instrukcja `goto` jest pozostałością po językach niższego poziomu i nie jest stosowana we współczesnym programowaniu. Jeżeli jest to tylko możliwe (a praktycznie zawsze jest) unikaj więc jej stosowania, gdyż takie skakanie po kodzie zmniejsza jego spójność i czytelność.

## return
Stosowana głównie w metodach. Zostanie omówiona później w tym tygodniu w lekcji dotyczącej metod (Lekcja 14).

## throw
Stosowana do wyrzucania wyjątków. Zostanie omówiona później w kursie, przy okazji omawiania obsługi wyjątków.

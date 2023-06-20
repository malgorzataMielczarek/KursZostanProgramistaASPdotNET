# [LEKCJA 9 – Konwencje pisania (Dobre praktyki programowania)](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-1-plan-gry/lekcja-10-kompilator/)
Konwencje programowania, są to ogólnie przyjęte zasady pisania kodu. Nie mają one wpływu na działanie programu. Stosowanie ich zwiększa jednak czytelność i ułatwia programistą czytanie cudzego kodu. Konwencji programowania jest bardzo dużo. W tej lekcji poznamy tylko kilka z nich. Warto od razu wyrobić sobie nawyk ich stosowania, aby ułatwić sobie pracę w przyszłości.

## Konwencje nazywania elementów programu

### **PascalCase**

| Wygląd | Zastosowanie |
|:---|:---|
| Wszystkie wyrazy nazwy piszemy razem, bez żadnych przerw. Pierwsza litera każdego wyrazu jest wielka a kolejne małe. | Nazywanie klas, plików, przestrzeni nazw (`namespace`), metod. |
|Np.: `CaloriesCalculator`, `HelloWorld`, `AddYears`. ||

### **camelCase**

| Wygląd | Zastosowanie|
|:---|:---|
| Wszystkie wyrazy nazwy piszemy razem, bez żadnych przerw. Pierwsza litera prawie każdego wyrazu jest wielka a kolejne małe. Wyjątek stanowi pierwszy wyraz, który w całości jest pisany małymi literami. | Nazywanie lokalnych zmiennych i prywatnych elementów programu. |
| Np.: `addedYears`, `displayScreen`, `numberOfCircles`. ||

### **UPPER_CASE**

| Wygląd | Zastosowanie |
|:---|:---|
| Nazwę piszemy w całości wielkimi literami (wszystkie litery). Wyrazy oddzielamy od siebie przy pomocy podkreślnika („_”). | Nazywanie stałych. |
| Np.: `PI_NUMBER`, `AGE_OF_CONSENT`, `NUMBER_OF_THREADS`. ||

## Konwencja zapisu nawiasów klamrowych
Nawiasy klamrowe są częstym elementem kodu w języku C#. Definiują zasięgi przestrzeni nazw, klas, metod oraz instrukcji warunkowych i pętli. W odróżnieniu od większości instrukcji, po nawiasie klamrowym nie stawia się średnika. W języku C# przyjęło się, że nawias klamrowy stawiamy w kolejnej linii niż instrukcja której dotyczy. Visual Studio domyślnie pomaga w utrzymaniu tej konwencji. Po zamykającym nawiasie klamrowym wstawiamy linię odstępu (pustą linię), chyba, że w kolejnej linijce jest kolejny zamykający nawias klamrowy. Wówczas nie ma pustej linii. W przypadku bloku instrukcji `if...else`, `if...else if...else` najlepiej zawsze stosować nawiasy klamrowe, nawet jeżeli blok instrukcji składa się tylko z jednego polecenia.

Np.:
```csharp
namespace HelloWorld
{
	public class Program
	{
		public static void Main(string[] args)
		{
			int a = GetA(args);
			int b = GetB(args);

			while (a > b)
			{
				// instrukcje wykonywane w przypadku spełnienia warunku;
				a--;
			}

			if (a == b)
			{
				// instrukcje wykonywane w przypadku spełnienia warunku;
			}

			// kolejne instrukcje
		}

		private static int GetA(string[] args)
		{
			// instrukcje do uzyskania wartości zmiennej a z argumentów wywołania programu
		}

		private static int GetB(string[] args)
		{
			// instrukcje do uzyskania wartości zmiennej b z argumentów wywołania programu
		}
	}
}
```

## Konwencja tworzenia nowych metod
Pomiędzy kolejnymi metodami umieszcza się maksymalnie jedną linię odstępu (pustą linię). Stosowanie większych odstępów zmniejsza czytelność kodu. Powoduje jego niepotrzebne wydłużenie. To z kolei utrudnia sprawną nawigację po kodzie.

## Konwencja pisania modyfikatorów dostępu
Zaleca się pisanie modyfikatora dostępu za każdym razem, nawet kiedy nie jest on wymagany. Standardowym i domyślnym modyfikatorem dostępu dla większości elementów języka C# jest `private` lub `internal`. W niektórych przypadkach można więc je pominąć, bez wpływu na funkcjonowanie programu. Zaleca się jednak zapisywanie ich w postaci jawnej, aby zwiększyć przejrzystość kodu. Związane jest to ze stosowaniem wielu słów kluczowych przed nazwami metod (takich jak `static`, czy typ zwracany przez metodę). Zapisanie modyfikatorów dostępu w postaci jawnej ułatwia przeglądanie kodu w poszukiwaniu metod o określonym typie dostępu.

## Konwencja tworzenia klas
Każda klasa powinna znajdować się w oddzielnym pliku (każdy plik powinien zawierać tylko jedną klasę). Język C# umożliwia umieszczenie wielu klas w jednym pliku. W miarę rozrastania się programu zmniejsza to jednak jego czytelność i utrudnia nawigację. Visual Studio umożliwia łatwe przeniesienie utworzonych już klas do nowych plików, przy pomocy Szybkich akcji.

## Konwencja stosowania typów w nazwach
W odróżnieniu od niektórych innych języków programowania, w języku C# w większości przypadków nazwy elementu NIE rozpoczyna się od jego typu (ani skrótu od nazwy typu elementu). Od tej zasady występuje jeden wyjątek, a mianowicie interfejs. Nazwy interfejsów zawsze rozpoczyna się od wielkiej litery „i” („I”, jak `interface`).

Np.:
```csharp
public interface IProgram
{
	// 
}
```

## Kolejność elementów w klasach (strukturach)
Elementy każdej klasy porządkujemy zgodnie z przynależnością do poniższych grup:
1. zmienne
2. właściwości
3. konstruktory
4. metody

Lub

1. zmienne
2. konstruktory
3. właściwości
4. metody

W obrębie każdej z tych grup porządkujemy elementy według modyfikatorów dostępu (od `public` do `private`), a następnie alfabetycznie wg. nazw.

## Komentarze
Pomiędzy znakami komentarza, a jego treścią stawiamy spację. Czyli
```csharp =
//źle
// dobrze
```

## Instrukcje warunkowe (`if`, `while`, `for` itd.)
W instrukcjach warunkowych pomiędzy słowem kluczowym, a nawiasem okrągłym z warunkiem stawiamy spację. Czyli:
```csharp =
if(a > b)
{
	// źle
}

if (a > b)
{
	// dobrze
}
```

# [LEKCJA 3 – Twój pierwszy test](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-3-twoj-pierwszy-test/)
W tej lekcji napiszemy nasz pierwszy test jednostkowy. Na razie będzie to najbardziej podstawowy test, nie powiązany w żaden sposób z tworzoną przez nas aplikacją. Zależy nam jedynie, aby nauczyć się w jaki sposób powinny być tworzone testy jednostkowe.
## Struktura testu jednostkowego - **AAA**
Każdy test jednostkowy powinien być zbudowany zgodnie ze strukturą, nazywaną w skrócie **AAA** (ang. _Arrange - Act - Assert_).
### 1. _Arrange_
Pierwszym krokiem testowym jest tzw. _Arrange_. Będziemy w nim przygotowywać dane, które będziemy testować.
### 2. _Act_
Drugim krokiem testowym jest wykonanie działania logicznego, które ma zostać sprawdzone w tym teście jednostkowym.
### 3. _Assert_
W ostatnim kroku testowym musi znajdować się dowód na to, że kod działa w sposób prawidłowy. Służy do tego klasa `Assert`, będąca częścią biblioteki `Xunit`. Każda biblioteka służąca do testowania posiada na ogół klasę `Assert`.
## Przykład
Stwórzmy dla przykładu prosty test sprawdzający, czy dodawanie dwóch liczb działa prawidłowo.
### 1. _Arrange_
W pierwszym kroku zadeklarujemy dane testowe. W naszym przypadku będę to dwie zmienne przechowujące liczby, które będziemy dodawać. Np.:
```csharp =
int a = 5;
int b = 3;
```
### 2. _Act_
Chcemy przetestować dodawanie dwóch zmiennych. Wykonajmy więc takie działanie, zapisując wynik w zmiennej `result`.
```csharp =
int result = a + b;
```
### 3. _Assert_

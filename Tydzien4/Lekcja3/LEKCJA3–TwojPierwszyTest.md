# [LEKCJA 3 – Twój pierwszy test](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-3-twoj-pierwszy-test/)
W tej lekcji napiszemy nasz pierwszy test jednostkowy. Na razie będzie to najbardziej podstawowy test, nie powiązany w żaden sposób z tworzoną przez nas aplikacją. Oczywiście, w kontekście tworzonego przez nas programu, nie będzie on miał sensu. Zależy nam jednak jedynie, aby nauczyć się w jaki sposób powinny być tworzone testy jednostkowe.
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
Chcemy sprawdzić, czy w wyniku działań podjętych w poprzednim kroku, otrzymaliśmy oczekiwany rezultat. Wykorzystamy do tego jedną z metod klasy `Assert`, a mianowicie `Equal` (z ang. równy).
W naszym przykładzie testujemy dodawanie liczb, na przykładzie liczb 5 i 3. Ponieważ wiemy, że
<p align="center">5 + 3 = 8,</p>

więc oczekujemy, że wynik, który uzyskaliśmy na skutek działań podjętych w kroku _Act_, i zapisaliśmy w zmiennej `result`, wyniesie właśnie 8. Zapiszmy to więc

```csharp =
Assert.Equal(8, result);
```

W naszym przykładzie, chcieliśmy sprawdzić czy uzyskany wynik jest równy konkretnej liczbie. Oczywiście biblioteka `Assert` posiada jednak wiele metod i umożliwia nie tylko porównywanie.
### Cały kod przykładowego testu
Zapiszmy jeszcze łącznie cały kod testu.
```csharp =
using System;
using Xunit;

namespace NazwaAplikacji.Tests
{
    public class UnitTest1
    {
        [Fact]
        public void Test1()
        {
            //Arrange
            int a = 5;
            int b = 3;
            //Act
            int result = a + b;
            //Assert
            Assert.Equal(8, result);
        }
    }
}
```
Po utworzeniu testu możemy go uruchomić. Oczywiście z godnie z oczekiwaniami powinien zakończyć się on sukcesem.
## Przykładowe metody klasy `Assert`
Poza metodą `Equal`, klasa `Assert` posiada wiele innych metod. Ciekawym przykładem jest metod 
### `Throws`
Służy ona do sprawdzenia, czy w określonej sytuacji nasz program zwrócił wyjątek. Sprawdza, czy aplikacja w odpowiedni sposób obsługuje błąd na który może natrafić.
### `NotNull`
Służy do sprawdzenia, czy jakaś wartość jest nullem.
### `NotEmpty`
Służy do sprawdzenia, czy dany `string` (lub kolekcja) jest pusty.
### `StartsWith`
Służy do sprawdzenia, czy dany `string` (lub kolekcja) rozpoczyna się danym elementem.
### `Contains`
Służy do sprawdzenia, czy jakiś `string` (lub kolekcja) zawiera jakiś element.
### `DoesNotContain`
Służy do sprawdzenia, czy jakiś `string` (lub kolekcja) nie zawiera jakiegoś elementu.
## Pisanie testów
Pisząc testy chcemy sprawdzić nie tylko nasz idealny wariant, który z założenia powinien być poprawnie wykonany. Powinniśmy sprawdzić wszystkie możliwości dotyczące naszego kodu. Im więcej wariantów sprawdzimy, im więcej testów napiszemy, tym mniej będziemy później musieli debugować nasz kod. Kiedy większość sytuacji zostanie przewidziana i sprawdzona w testach, to zamiast debugować kod, będziemy mogli uruchomić testy. Jeżeli któryś z nich wypadnie negatywnie, to będziemy wiedzieć w której części kodu znajduje się błąd. Nie będziemy więc musieli przechodzić debugerem krok po kroku przez cały kod, a jedynie najwyżej przez fragment w którym testy wykazały błąd.

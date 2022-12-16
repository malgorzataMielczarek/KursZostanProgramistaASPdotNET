# [LEKCJA 6 – Hermetyzacja](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-6-hermetyzacja/)
**Hermetyzacja**, inaczej **enkapsulacja**, jest trzecim i ostatnim paradygmatem (filarem) programowania obiektowego. Oznacza ona ograniczenie dostępu użytkownika (np. innej klasy) do elementów programu. Tworząc nową klasę musimy świadomie udzielać dostępu tylko do tych jej elementów, do których inne klasy powinny mieć dostęp, i tylko w takim zakresie jaki jest konieczny. To znaczy elementy związane z wewnętrznymi procesami zachodzącymi w klasie ukrywamy całkowicie, a inne możemy chcieć udostępnić tylko do odczytu, albo tylko za pośrednictwem określonych metod. Jest to możliwe przy pomocy modyfikatorów dostępu (`public`, `internal`, `protected`, `private`) oraz innych modyfikatorów (`readonly`, `const` itd.).

Wcześniej omówiliśmy już modyfikatory dostępu (tydzień 2., lekcja 17.) oraz stałe (tydzień 2., lekcja 2.). Powiedzmy więc jeszcze po krótce co oznacza słowo kluczowe `readonly`.

## Słowo kluczowe `readonly`
Słowem kluczowym `readonly` (z ang. tylko do odczytu), jak sama nazwa wskazuje, oznacza się element których odczyt chcemy umożliwić, ale chcemy je zabezpieczyć przed zmianą ich wartości. Tego słowa kluczowego możemy użyć w czterech sytuacjach:
### 1. W deklaracji pola klasy

```csharp =
modyfikatorDostepu readonly typ nazwaPola;
```

Oznacza to, że nie można modyfikować wartości przypisanej temu polu. Jedynymi miejscami w których można to zrobić jest deklaracja pola oraz konstruktor klasy. Później możemy już tylko odczytywać jego wartość.

#### Różnice między polem `readonly` a `const`
Stałej (zmienna/pole z modyfikatorem `const`) możemy przypisać wartość wyłącznie w momencie deklaracji. Ponadto wartość ta musi być znana już na etapie kompilacji. Nigdzie indziej w programie nie możemy już jej zmienić.

Polu `readonly` możemy przypisać wartość w momencie deklaracji i/lub w konstruktorze klasy, w momencie tworzenia obiektu. Wartość ta może więc się zmienić (jedną przypiszemy w deklaracji, a inną w konstruktorze). Co więcej, możemy przypisać jej różną wartość, np. w zależności od użytego konstruktora lub jego parametrów. Możemy więc tę wartość poznać dopiero w momencie wykonywania programu. Po utworzeniu obiektu danej klasy, nie możemy już jednak zmieniać wartości jego pola `readonly`. Możemy jedynie odczytywać jego wartość.

### 2. W deklaracji struktury

```csharp =
modytikatorDostepu readonly struct NazwaStruktury { //cialo struktury }
```

Oznaczenie struktury jako `readonly` oznacza, że wszystkie jej elementy (poza konstruktorem) są również `readonly`.

### 3. W deklaracji elementów struktury, klasy, np. metod, właściwości

```csharp =
modyfikatorDostepu struct NazwaStruktury
{
    ...
    modytikatorDostepu readonly typ NazwaMetody(/*parametry*/)
    {
        //cialo metody
    }
    modytikatorDostepu readonly typ NazwaWlasciwosci { get; set; }
    ...
}
```

Oznaczenie właściwości jako `readonly` oznacza, że wszystkie jej elementy są również `readonly`. Czyli tak na prawdę będziemy mieć wyłącznie metodę `get`.

Oznaczenie metody jako `readonly` oznacza, że metoda ta, nie będzie modyfikować struktury w której się znajduje. Tzn. może wykonywać różne obliczenia, zwracać jakąś wartość itd., jednak nie zmieni wartość żadnego pola struktury. Wewnątrz ciała metody `readonly` można wywołać jednak metodę bez takiego modyfikatora. Aby zapewnić niezmienność struktury, tworzona jest wówczas kopia struktury i na tej kopii wykonywana jest metoda bez modyfikatora `readonly` (kompilator niema więc pewności, czy nie zmieni ona struktury).

Elementy `readonly` struktury/klasy (z wyłączeniem pól) dostępne są dopiero od wersji C# 8.0.

### 4. W deklaracji metody zwracającej referencję

```csharp =
modyfikatorDostepu ref readonly typ NazwaMetody(/*parametry*/)
{
    //cialo metody
}
```

Taki zapis oznacza, że nie możemy modyfikować obiektu na który wskazuje referencja zwrócona nam przez metodę. Ten obiekt jest tylko do odczytu (`readonly`). A dokładniej nie można tego obiektu modyfikować przy użyciu zwróconej referencji.

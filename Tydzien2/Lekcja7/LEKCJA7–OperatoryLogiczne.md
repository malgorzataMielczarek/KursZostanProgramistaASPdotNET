# [LEKCJA 7 – Operatory Logiczne](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-7-operatory-logiczne/)
W lekcji tej poznamy operatory logiczne warunkowe. Wyróżniamy operatory logiczne jednoargumentowe i dwuargumentowe. Wszystkie argumenty są typu `bool`. Wynik operacji logicznych również jest tego typu. Operatory logiczne są często stosowane w wyrażeniach warunkowych instrukcji `if`, pętli itp.

## Operatory logiczne jednoargumentowe
Jest tylko jedna jednoargumentowa operacja logiczna - NOT, czyli zaprzeczenie. Zmienia on wartość logiczną wyrażenia na przeciwną (z `true` na `false`, a z `false` na `true`). Operatorem NOT jest wykrzyknik (`!`) stawiany przed argumentem (wyrażeniem/zmienną itd.). Umożliwia on np. wykonanie jakichś czynności, gdy warunek nie jest spełniony.

| Operator | Nazwa | Przykład | Uwagi |
| :---: | :---: | :--- | :--- |
| `!` | NOT (nie, zaprzeczenie, negacja) | `bool a = !true; // wynik: false` | jest to operator jednoargumentowy, stojący po lewej stronie argumentu, spośród operacji logicznych negacja jest wykonywana jako pierwsza (ma najwyższy priorytet, nie licząc nawiasów) |

| Wartość argumentu | Negacja | Wynik |
| :---: | :---: | :--- |
| `true` | `!true` | `false` |
| `false` | `!false` | `true` |

## Operatory logiczne dwuargumentowe
Operacje logiczne dwuargumentowe mają postać `argument1 operator argument2`. Zarówno oba argumenty, jak i całość operacji są typu `bool`. Jeżeli argumenty są wyrażeniami, a operator jest operatorem warunkowym (`&&` lub `||`), to gdy wynik operacji jest znany po obliczeniu pierwszego argumentu, to drugi argument nie jest już w ogóle liczony. Możemy wymienić następujące operatory logiczne dwuargumentowe:

| Operator | Nazwa | Przykład | Uwagi |
| :---: | :---: | :--- | :--- |
| `&` | logiczne AND (i, koniunkcja) | `bool a = false & true; // wynik: false` | Zawsze obliczane są oba argumenty operacji, niezależnie od wyniku. Można ten operator łączyć z operatorem przypisania (`&=`), wówczas wynik operacji jest przypisany do zmiennej będącej jej lewym argumentem. |
| `&&` | warunkowy operator logiczny AND | `bool a = false && true; // wynik: false` | Jeżeli lewy argument operacji jest równy `false`, to prawy argument nie jest liczony (czyli jeżeli byłaby to np. funkcja zwracająca wartość logiczną, to w ogóle by się ona nie wykonała), a całość operacji daje wynik `false`. Operatora nie można łączyć z operatorem przypisania. |
| `\|` | logiczne OR (lub, alternatywa) | `bool a = false \| true; // wynik: true` | Zawsze obliczane są oba argumenty operacji, niezależnie od wyniku. Można ten operator łączyć z operatorem przypisania (`\|=`), wówczas wynik operacji jest przypisany do zmiennej będącej jej lewym argumentem. |
| `\|\|` | warunkowy operator logiczny OR | `bool a = false \|\| true; // wynik: true` | Jeżeli lewy argument operacji jest równy `true`, to prawy argument nie jest liczony (czyli jeżeli byłaby to np. funkcja zwracająca wartość logiczną, to w ogóle by się ona nie wykonała), a całość operacji daje wynik `false`. Operatora nie można łączyć z operatorem przypisania. |
| `^` | logiczne XOR (albo, alternatywa rozłączna, wykluczająca) | `bool a = false ^ true; // wynik: true` | Zawsze obliczane są oba argumenty operacji, niezależnie od wyniku. Można ten operator łączyć z operatorem przypisania (`^=`), wówczas wynik operacji jest przypisany do zmiennej będącej jej lewym argumentem. |

Operacje z użyciem powyższy operatorów przyjmują następujące wartości dla różnych wartości argumentów:

| argument 1 (`x`) | argument 2 (`y`) | AND (`x & y` lub `x && y`) | OR (`x \| y` lub `x \|\| y`) | XOR (`x ^ y`) |
| :---: | :---: | :---: | :---: | :---: |
| `true` | `true` | `true` | `true` | `false` |
| `true` | `false` | `false` | `true` | `true` |
| `false` | `true` | `false` | `true` | `true` |
| `false` | `false` | `false` | `false` | `false` |

Operatory `&` i `|` mogą również obsługiwać logikę trójwartościową, czyli przyjmować argumenty i zwracać wartość typu `bool?` (mającego jedną z trzech wartości: `true`, `false` lub `null`). Wówczas przyjmują one następujące wartości:

| argument 1 (`x`) | argument 2 (`y`) | AND (`x & y`) | OR (`x \| y`) |
| :---: | :---: | :---: | :---: |
| `true` | `true` | `true` | `true` |
| `true` | `fałsz` | `fałsz` | `true` |
| `true` | `null` | `null` | `true` |
| `fałsz` | `true` | `fałsz` | `true` |
| `fałsz` | `fałsz` | `fałsz` | `fałsz` |
| `fałsz` | `null` | `fałsz` | `null` |
| `null` | `true` | `null` | `true` |
| `null` | `fałsz` | `fałsz` | `null` |
| `null` | `null` | `null` | `null` |

## Łączenie operacji logicznych
Operacje logiczne, podobnie jak arytmetyczne można ze sobą łączyć w jedno wyrażenie logiczne. Podobnie jak w przypadku operatorów arytmetycznych obowiązuje tu również matematyczna kolejność wykonywania działań (zgodnie z logiką matematyczną). Kolejność ta jest następująca:
1. Operator negacji logicznej (`!`)
2. Operator logiczny AND (`&`)
3. Logiczny wyłączny operator OR - XOR (`^`)
4. Operator logiczny OR (`|`)
5. Warunkowy operator logiczny AND (`&&`)
6. Warunkowy operator logiczny OR (`||`)

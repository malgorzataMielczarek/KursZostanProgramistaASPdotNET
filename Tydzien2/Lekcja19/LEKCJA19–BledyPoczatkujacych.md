# [LEKCJA 19 – Błędy początkujących](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-19-bledy-poczatkujacych/)
## Tworzenie metod
Nie twórz klas ze zbyt dużą liczbą argumentów. Jeżeli nie możesz zapamiętać kolejności w jakiej występują, to znaczy, że jest ich prawdopodobnie za dużo. Wtedy warto utworzyć dodatkową klasę pomocniczą, która zbierze wszystkie argumenty które chcieliśmy przekazać. Wówczas do naszej metody będziemy przekazywać tylko jeden argument typu tej klasy pomocniczej.
## Typy danych
Jeżeli masz problem jakiego typu powinna być dana zmienna lub właściwość, to zastanów się, co oznaczałaby ona w świecie realnym. Jeżeli jest to ilość czegoś, to najczęściej będzie to liczba całkowita, czyli użyjemy typu `int`. Gdy chcemy natomiast przechowywać cenę, to wiemy, że jest to wartość z dwoma miejscami dziesiętnymi. Użyjemy więc typu `decimal`, gdyż zależy nam tu na dużej dokładności obliczeń na liczbach dziesiętnych.
## Instrukcje warunkowe
Stosuj je często. Zawsze, gdy pobierasz dane od użytkownika, sprawdź, czy podał on takie dane jakich oczekiwaliśmy, zanim użyjesz ich dalej w programie. Jeżeli np. oczekiwaliśmy, że użytkownik wpisze cyfrę, a on np. wciśnie Enter, to próba rzutowania na typ `int` może zakończyć się błędem programu i zakończeniem jego pracy.

W instrukcjach warunkowych uważaj na wzajemnie się wykluczające warunki.
## Pętle nieskończone
Uważaj na pętle nieskończone. Stosuj je świadomie i pamiętaj, że są kosztowne dla programu.

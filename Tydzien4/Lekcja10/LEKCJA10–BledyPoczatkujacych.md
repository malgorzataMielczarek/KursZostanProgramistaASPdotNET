# [LEKCJA 10 – Błędy początkujących](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-10-bledy-poczatkujacych/)
Jeśli chodzi o testowanie, to możliwych błędów jest całkiem sporo. Pierwszy podstawowy to

## Brak testów
Wielu programistów w ogóle nie testuje swoich aplikacji, co jest pierwszym krokiem do ich klęski. Pamiętajmy, że każdy program powinien być odpowiednio przetestowany. Dzięki temu mamy pewność, że działa ona w taki sposób, jaki to zaplanowaliśmy. Dodatkowo, w przypadkach spornych (np. z administratorami sieci), pozwala nam to udowodnić, że nasz kod działa prawidłowo. W przypadku gdy natomiast coś cię wysypie, ułatwia to i przyśpiesza znalezienie źródła błędu. Pozwala nam również sprawdzić, że każda kolejna wersja aplikacji, działa w ten sam sposób.

## Testowanie nieodpowiednich elementów aplikacji
Po drugie, często testujemy nieodpowiednie elementy aplikacji.

**Nie powinniśmy testować:**
* kontrolerów (w przypadku programowania webowego)
* zewnętrznych bibliotek (zakładamy, że działają one poprawnie).

**Powinniśmy testować:**
* logikę biznesową aplikacji.

Skupmy się na testowaniu tego, nad czym mamy panowanie, co sami zaimplementowaliśmy i powinniśmy sprawdzić, czyli na logice biznesowej naszej aplikacji. W naszym przypadku będą to menadżery i być może serwisy. W tym momencie, prawdopodobnie nie używamy w nich jeszcze bibliotek zewnętrznych, więc warto je przetestować.

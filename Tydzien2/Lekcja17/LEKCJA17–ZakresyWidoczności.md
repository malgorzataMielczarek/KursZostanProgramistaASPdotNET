# [LEKCJA 17 – Zakresy widoczności](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-17-zakresy-widocznosci/)
Poszczególne elementy programu (klasy, metody, właściwości, zmienne) mają różną widoczność w zależności od miejsca w programie. Definiujemy ją przy pomocy tzw. modyfikatorów dostępu.
## Modyfikatory dostępu
Są to słowa kluczowe które umieszczaliśmy na początku każdej deklaracji. Podawanie ich jawnie za każdym razem, nawet w sytuacjach gdy nie są wymagane, zwiększa czytelność i przeszukiwanie kodu. Należy więc to do dobrych praktyk programowania. Wyróżniamy cztery podstawowe modyfikatory dostępu:
### `public`
Oznaczamy tak elementy do których chcemy mieć dostęp w każdym elemencie programu. Możemy sią do nich odwoływać zarówno w innych klasach, jak i nawet w innych projektach w danej solucji (wtedy musimy dodać sekcję using z nazwą projektu z elementem który chcemy użyć, lub odpowiedniej ścieżki dostępu).
### `private`
Oznaczamy tak elementy do których chcemy mieć dostęp tylko w obrębie elementu bezpośrednio go zawierającego (klasy, metody).To znaczy, że próba odwołania się do takich w innej klasie, zakończy się fiaskiem.
### `protected`
Do tak oznaczonych elementów klasy, mamy dostęp tylko wewnątrz tej klasy, oraz w klasach po niej dziedziczących (o dziedziczeniu powiemy więcej w przyszłym tygodniu).
### `internal`
Oznacza elementy do których mamy dostęp tylko wewnątrz danego projektu. To znaczy nie możemy się do nich odwołać z innych projektów jakie dodamy do naszej solucji. Jeżeli w definicji klasy nie ma podanego modyfikatora dostępu, to są one domyślnie `internal`.

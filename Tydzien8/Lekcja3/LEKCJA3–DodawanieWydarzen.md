# [LEKCJA 3 – Dodawanie wydarzeń](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-8-od-widoku-do-modelu/lekcja-3-dodawanie-wydarzen/)
Wydarzenia, inaczej akcje, są to metody metody wywoływane w reakcji na działanie użytkownika. Będziemy je dodawać w kontrolerach.
## Wydarzenie `Index`
Najbardziej podstawową akcją każdego kontrolera jest `Index`. Tradycyjnie nazywamy tak metodę, która odpowiada za wywołanie podstawowego widoku dla danego kontrolera. Najczęściej będzie to lista (może być przefiltrowana) np. produktów, akcji do wykonania itp. `Index` przekaże do serwisu ewentualne dane potrzebne do filtrowania. Następnie pobierze z niego odpowiednie dane. Na koniec wywoła odpowiedni widok, przekazując mu uzyskane dane.
## Typ danych zwracanych przez serwis
Serwisy nie będą nam zwracać całych modeli domenowych. Będą one przetwarzać dane uzyskane z repozytoriów i wyłuskiwać jedynie potrzebne w danej chwili informacje. Następnie zostaną one zwrócone w postaci specjalnie utworzonego dla danego wydarzenia modelu.
## Wydarzenia GET i POST
W jednym kontrolerze będziemy mieć często po dwa wydarzenia o tej samej nazwie. Jedno będzie odpowiedzią na zapytanie GET i będzie służyło do wyświetlenia jakichś danych, formularza do wypełnienia itd. Drugie będzie wywoływane żądaniem typu POST, przekazującym dane do zapisania. Każde takie wydarzenie będzie powodować wywołanie nowej strony (lub przeładowanie obecnej).
## Podział na kontrolery
Jak podzielić aplikację na kontrolery?
1. Kontroler na stronę<br />
**NIE** tworzymy jednego kontrolera na stronę, gdyż, jak wspominaliśmy wyżej, każda akcja kontrolera może wywoływać inną stronę.
2. Kontroler na model domenowy<br />
**NIE** tworzymy jednego kontrolera na jeden model domenowy. W jednym kontrolerze często będziemy bowiem łączyć informacje z kilku tabel.
3. Kontroler na serwis<br />
Podejście jeden kontroler na jeden serwis jest najbliższe prawdy, chociaż nie istnieje taka ścisła zależność. Może się zdarzyć, że w obrębie jednego kontrolera będziemy korzystać z kilku serwisów. Możemy również potrzebować utworzyć kilka kontrolerów dla jednego serwisu.
4. **Kontroler na moduł**<br />
Mówimy więc, że tworzymy jeden kontroler na moduł. Moduł są to powiązane ze sobą funkcjonalności naszego programu. Możemy mieć np. moduł klientów, moduł sprzedażowy, moduł zamówień.
# [LEKCJA 6 – Controller](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-6-aplikacje-webowe-w-asp-net-core/lekcja-6-controller/)
## Kody statusów HTTP
Definiują one odpowiedzi jakie może wysłać serwer w odpowiedzi na otrzymane zapytania. Kody podzielona na klasy kodów, zawierające odpowiedzi o podobnym zastosowaniu. W poniższej tabeli przedstawiono kody z podziałem na klasy. Pogrubiono numery kodów, które są najczęściej używane.

VebDav (_Web Distributed Authoring and Versioning_) jest rozszerzeniem protokołu HTTP pozwalającym na zarządzanie i kontrolę wersji plików na serwerze WWW. Standard ten dodaje do protokołu HTTP takie metody jak:
* PROPFIND - pobierz własności zasobu
* PROPPATCH - zmień lub skasuj różne własności zasobu w atomowej operacji
* MKCOL - utwórz "kolekcję" (katalog)
* COPY - skopiuj zasób z jednego adresu na drugi
* MOVE - przenieś zasób z jednego adresu na drugi
* LOCK - zablokuj zasób (zarówno dzielone jak i wyłączne blokady)
* UNLOCK - usuń blokadę z zasobu
<table>
<thead><tr><th style="text-align:center">Klasa kodów</th><th style="text-align:center">Nazwa EN</th><th style="text-align:center">Nazwa PL</th><th style="text-align:center">Opis</th></tr></thead>
<tbody>
<tr><th style="text-align:center">1xx</th><td style="text-align:center"><i>Informational</i></td><td style="text-align:center">Informacyjny</td><td>Klasa kodów statusów wskazująca prowizoryczną odpowiedź.</br>Odpowiedź składa się tylko ze Status-Line i opcjonalnego nagłówka i jest zakończona pustą linią. Ponieważ żaden z kodów tej klasy nie został zdefiniowany przez HTTP/1.0, serwery nie muszą wysyłać odpowiedzi tej klasy do klienta (aplikacji webowej). Klient musi być jednak przygotowany na przyjęcie jednej lub więcej odpowiedzi tej klasy, przed otrzymaniem odpowiedzi. User agent (aplikacja kliencka - przeglądarka) może zignorować nieoczekiwaną odpowiedź tej klasy.</td></tr>
<tr><td colspan=5><details>
<summary style="text-align:center">
Rozwiń kody klasy 1xx
</summary>

| Kod | Nazwa EN | Nazwa PL | Opis |
| ---: | :---: | :---: | --- |
| 100 | _Continue_ | Kontynuuj | Wskazuje, że klient powinien kontynuować swoje zapytanie.</br> Taka odpowiedź może zostać wysłana, aby poinformować klienta, że otrzymano początkową część zapytania i nie została ona jeszcze odrzucona przez serwer. Klient powinien kontynuować wysyłanie reszty zapytania, lub zignorować tą odpowiedź, jeżeli całe zapytanie zostało już wysłane. Po zakończeniu obsługi zapytania, serwer musi wysłać ostateczną odpowiedź. |
| 101 | _Switching Protocols_ | Przełącz protokoły |Oznacza, że serwer zrozumiał i jest gotowy zrealizować wysłaną prośbę o zmianę protokołu.</br>Serwer zmieni protokół na ten o który proszono, zaraz za pustą linią znajdującą się na końcu tej odpowiedzi.</br>Protokół powinien zostać zamieniony, gdy jest to korzystne, np. gdy poproszono o nowszą wersję HTTP. |
| 102 | _Processing (WebDAV)_ | Przetwarzanie | Jest to tymczasowa odpowiedź używana do poinformowania klienta, że serwer zaakceptował całe żądanie, ale jeszcze go nie ukończył.</br>Ten kod stanu powinien zostać wysłany tylko wtedy, gdy serwer ma uzasadnione oczekiwania, że wykonanie żądania zajmie dużo czasu. Jako wskazówka, jeśli wykonanie metody trwa dłużej niż 20 sekund, serwer powinien zwrócić tą odpowiedź. Serwer musi wysłać ostateczną odpowiedź po zakończeniu przetwarzania żądania. Jest ona np. wysyłana aby zapobiec automatycznemu wylogowaniu użytkownika z powodu przekroczenia czasu, gdy czeka on na odpowiedź. |
</details></td></tr>
<tr><th style="text-align:center">2xx</th><td style="text-align:center"><i>Success</i></td><td style="text-align:center">Sukces</td><td>Kody tej klasy oznaczają, że żądanie klienta zostało pomyślnie przyjęte, zrozumiane i przetworzone.</td></tr>
<tr><td colspan=5><details>
<summary style="text-align:center">Rozwiń kody klasy 2xx</summary>

| Kod | Nazwa EN | Nazwa PL | Opis |
| ---: | :---: | :---: | --- |
| **200** | _OK_ | OK |Oznacza, że żądanie zakończyło się sukcesem. Informacja zwrócona razem z odpowiedzią zależy od metody użytej podczas żądania. Przykładowo:<ul style="list-style-type: none;"><li>GET entity odpowiadające żądanemu zasobowi jest wysyłane w odpowiedzi.</li><li>HEAD w odpowiedzi zostają wysłane tylko pola nagłówka entity, odpowiadającego żądanemu zasobowi, bez body wiadomości.</li><li>POST przesłanie entity opisującego lub zawierającego rezultat operacji</li><li>TRACE wysłanie entity zawierającego treść żądania w formie, w jakiej otrzymał je serwer końcowy</li></ul>
| **201** | _Created_ | Utworzony |  |
| 202 | _Accepted_ | Zaakceptowany |  |
| 203 | _Non-Authoritative Information_ | Informacja nieautorytatywna |  |
| **204** | _No Content_ | Brak zawartości |  |
| 205 | _Reset Content_ | Resetuj zawartość |  |
| 206 | _Partial Content_ |  |  |
| 207 | _Multi-Status (WebDAV)_ |  |  |
| 208 | _Already Reported (WebDAV)_ |  |  |
| 226 | _IM Used_|  |  |
</details></td></tr>
<tr><th style="text-align:center">3xx</th><td style="text-align:center"><i>Redirection</i></td><td style="text-align:center"></td><td></td></tr>
<tr><td colspan=5><details>
<summary style="text-align:center">Rozwiń kody klasy 3xx</summary>

| Kod | Nazwa EN | Nazwa PL | Opis |
| ---: | :---: | :---: | --- |
| 300 | _Multiple Choices_ |  |  |
| 301 | _Moved Permanently_ |  |  |
| 302 | _Found_ |  |  |
| 303 | _See Other_ |  |  |
| **304** | _Not Modified_ |  |  |
| 305 | _Use Proxy_ |  |  |
| 306 | _(Unused)_ |  |  |
| 307 | _Temporary Redirect_ |  |  |
| 308 | _Permanent Redirect (experimental)_ |  |  |
</details></td></tr>
<tr><th style="text-align:center">4xx</th><td style="text-align:center"><i>Client Error</i></td><td style="text-align:center"></td><td></td></tr>
<tr><td colspan=5><details>
<summary style="text-align:center">Rozwiń kody klasy 4xx</summary>

| Kod | Nazwa EN | Nazwa PL | Opis |
| ---: | :---: | :---: | --- |
| **400** | _Bad Request_ |  |  |
| **401** | _Unauthorized_ |  |  |
| 402 | _Payment Required_ |  |  |
| **403** | _Forbidden_ |  |  |
| **404** | _Not Found_ |  |  |
| 405 | _Method Not Allowed_ |  |  |
| 406 | _Not Acceptable_ |  |  |
| 407 | _Proxy Authentication Required_ |  |  |
| 408 | _Request Timeout_ |  |  |
| **409** | _Conflict_ |  |  |
| 410 | _Gone_ |  |  |
| 411 | _Length Required_ |  |  |
| 412 | _Precondition Failed_ |  |  |
| 413 | _Request Entity Too Large_ |  |  |
| 414 | _Request-URI Too Long_ |  |  |
| 415 | _Unsupported Media Type_ |  |  |
| 416 | _Requested Range Not Satisfiable_ |  |  |
| 417 | _Expectation Failed_ |  |  |
| 418 | _I'm a teapot (RFC 2324)_ |  |  |
| 420 | _Enhance Your Calm (Twitter)_ |  |  |
| 422 | _Unprocessable Entity (WebDAV)_ |  |  |
| 423 | _Locked (WebDAV)_ |  |  |
| 424 | _Failed Dependency (WebDAV)_ |  |  |
| 425 | _Reserved for WebDAV_ |  |  |
| 426 | _Upgrade Required_ |  |  |
| 428 | _Precondition Required_ |  |  |
| 429 | _Too Many Requests_ |  |  |
| 431 | _Request Header Fields Too Large_ |  |  |
| 444 | _No Response (Nginx)_ |  |  |
| 449 | _Retry With (Microsoft)_ |  |  |
| 450 | _Blocked by Windows Parental Controls (Microsoft)_ |  |  |
| 451 | _Unavailable For Legal Reasons_ |  |  |
| 499 | _Client Closed Request (Nginx)_ |  |  |
</details></td></tr>
<tr><th style="text-align:center">5xx</th><td style="text-align:center"><i>Server Error</i></td><td style="text-align:center"></td><td></td></tr>
<tr><td colspan=5><details>
<summary style="text-align:center">Rozwiń kody klasy 5xx</summary>

| Kod | Nazwa EN | Nazwa PL | Opis |
| ---: | :---: | :---: | --- |
| **500** | _Internal Server Error_ |  |  |
| 501 | _Not Implemented_ |  |  |
| 502 | _Bad Gateway_ |  |  |
| 503 | _Service Unavailable_ |  |  |
| 504 | _Gateway Timeout_ |  |  |
| 505 | _HTTP Version Not Supported_ |  |  |
| 506 | _Variant Also Negotiates (Experimental)_ |  |  |
| 507 | _Insufficient Storage (WebDAV)_ |  |  |
| 508 | _Loop Detected (WebDAV)_ |  |  |
| 509 | _Bandwidth Limit Exceeded (Apache)_ |  |  |
| 510 | _Not Extended_ |  |  |
| 511 | _Network Authentication Required_ |  |  |
| 598 | _Network read timeout error_ |  |  |
| 599 | _Network connect timeout error_ |  |  |
</details></td></tr>
</tbody>
</table>



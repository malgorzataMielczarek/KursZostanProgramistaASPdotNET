# [LEKCJA 6 – Controller](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-6-aplikacje-webowe-w-asp-net-core/lekcja-6-controller/)
Kontrolery odpowiadają za odebranie z widoków żądań użytkownika i przekazanie ich do dalszej części aplikacji.

Kiedyś kontrolery były sercem aplikacji. To w nich znajdowała się cała logika. Łączyły się one bezpośrednio z bazą danych, odbierały i przerabiały dane i wysyłały je do widoków.

Dzisiaj, ponieważ zaczęto dzielić aplikację na więcej warstw, więc rola kontrolerów nieco się zmieniła. Dalej są one odpowiedzialne za przyjmowanie wszystkich żądań użytkownika. Kiedy np. użytkownik naciśnie jakiś przycisk, akcja ta jest prawdopodobnie spięta z jakąś akcją kontrolera, która zostanie wywołana.

## Budowa kontrolera
Kontroler jest to klasa dziedzicząca po klasie `Controller`. W zależności od tego jak go nazwiemy, tak będziemy się do niego odwoływać. Załóżmy, że mamy kontroler:
```csharp =
namespace WarehouseMVC.Web.Controllers
{
    public class HomeController : Controller
    {
        private readonly ILogger<HomeController> _logger;

        public HomeController(ILogger<HomeController> logger)
        {
            _logger = logger;
        }
        // implementacja kontrolera
        
        // przykladowa metoda:
        public IActionResult Index()
        {
            return View();
        }
    }
```

## [Kody statusów HTTP](https://www.restapitutorial.com/httpstatuscodes.html#)
Definiują one odpowiedzi jakie może wysłać serwer w odpowiedzi na otrzymane żądanie. Kody podzielono na klasy kodów, zawierające odpowiedzi o podobnym zastosowaniu. W poniższej tabeli przedstawiono kody z podziałem na klasy. Pogrubiono numery kodów, które są najczęściej używane.

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
<tr><th style="text-align:center">1xx</th><td style="text-align:center"><i>Informational</i></td><td style="text-align:center">Informacyjny</td><td>Klasa kodów statusów wskazująca prowizoryczną odpowiedź.<br />Odpowiedź składa się tylko ze Status-Line i opcjonalnego nagłówka i jest zakończona pustą linią. Ponieważ żaden z kodów tej klasy nie został zdefiniowany przez HTTP/1.0, serwery nie muszą wysyłać odpowiedzi tej klasy do klienta (aplikacji webowej). Klient musi być jednak przygotowany na przyjęcie jednej lub więcej odpowiedzi tej klasy, przed otrzymaniem odpowiedzi. User agent (aplikacja kliencka - przeglądarka) może zignorować nieoczekiwaną odpowiedź tej klasy.</td></tr>
<tr><td colspan=5><details>
<summary style="text-align:center">
Rozwiń kody klasy 1xx
</summary>

| Kod | Nazwa EN | Nazwa PL | Opis |
| ---: | :---: | :---: | --- |
| 100 | _Continue_ | Kontynuuj | Wskazuje, że klient powinien kontynuować swoje zapytanie.<br /> Taka odpowiedź może zostać wysłana, aby poinformować klienta, że otrzymano początkową część zapytania i nie została ona jeszcze odrzucona przez serwer. Klient powinien kontynuować wysyłanie reszty zapytania, lub zignorować tą odpowiedź, jeżeli całe zapytanie zostało już wysłane. Po zakończeniu obsługi zapytania, serwer musi wysłać ostateczną odpowiedź. |
| 101 | _Switching Protocols_ | Przełącz protokoły |Oznacza, że serwer zrozumiał i jest gotowy zrealizować wysłaną prośbę o zmianę protokołu.<br />Serwer zmieni protokół na ten o który proszono, zaraz za pustą linią znajdującą się na końcu tej odpowiedzi.<br />Protokół powinien zostać zamieniony, gdy jest to korzystne, np. gdy poproszono o nowszą wersję HTTP. |
| 102 | _Processing (WebDAV)_ | Przetwarzanie | Jest to tymczasowa odpowiedź używana do poinformowania klienta, że serwer zaakceptował całe żądanie, ale jeszcze go nie ukończył.<br />Ten kod stanu powinien zostać wysłany tylko wtedy, gdy serwer ma uzasadnione oczekiwania, że wykonanie żądania zajmie dużo czasu. Jako wskazówka, jeśli wykonanie metody trwa dłużej niż 20 sekund, serwer powinien zwrócić tą odpowiedź. Serwer musi wysłać ostateczną odpowiedź po zakończeniu przetwarzania żądania. Jest ona np. wysyłana aby zapobiec automatycznemu wylogowaniu użytkownika z powodu przekroczenia czasu, gdy czeka on na odpowiedź. |
</details></td></tr>
<tr><th style="text-align:center">2xx</th><td style="text-align:center"><i>Success</i></td><td style="text-align:center">Sukces</td><td>Kody tej klasy oznaczają, że żądanie klienta zostało pomyślnie przyjęte, zrozumiane i przetworzone.</td></tr>
<tr><td colspan=5><details>
<summary style="text-align:center">Rozwiń kody klasy 2xx</summary>

| Kod | Nazwa EN | Nazwa PL | Opis |
| ---: | :---: | :---: | --- |
| **200** | _OK_ | OK |Oznacza, że żądanie zakończyło się sukcesem. Informacja zwrócona razem z odpowiedzią zależy od metody użytej podczas żądania. Przykładowo:<ul style="list-style-type: none;"><li>GET entity odpowiadające żądanemu zasobowi jest wysyłane w odpowiedzi.</li><li>HEAD w odpowiedzi zostają wysłane tylko pola nagłówka entity, odpowiadającego żądanemu zasobowi, bez body wiadomości.</li><li>POST przesłanie entity opisującego lub zawierającego rezultat operacji</li><li>TRACE wysłanie entity zawierającego treść żądania w formie, w jakiej otrzymał je serwer końcowy</li></ul> |
| **201** | _Created_ | Utworzony | Oznacza, że żądanie zostało wykonane i zaskutkowało utworzeniem nowego zasobu.<br />Do nowo utworzonego zasobu można się odwołać używając URI zwróconego w odpowiedzi. |
| 202 | _Accepted_ | Zaakceptowany | Żądanie zostało zaakceptowane do przetworzenia, ale przetworzenie jeszcze się nie zakończyło.<br />Tą odpowiedź stosuje się, aby pozwolić serwerowi na przyjęcie żądania dla innego procesu (być może takiego, który odbywa się raz na dobę), bez konieczności utrzymania połączenia między serwerem, a przeglądarką do czasu zakończenia przetwarzania. Odpowiedź powinna zawierać aktualny status żądania i wskaźnik do monitora statusu lub oszacowanie, kiedy użytkownik może się spodziewać odpowiedzi. |
| 203 | _Non-Authoritative Information_ | Informacja nieautorytatywna | Serwer pomyślnie przetworzył żądanie, ale zwraca informacje, które mogą pochodzić z innego źródła.<br />Użycie tego kodu nie jest wymagane i jest właściwe tylko wówczas, gdy w innym przypadku odpowiedź byłaby 200 (OK).<br />Ten kod nie jest dostępny w HTTP/1.0 (dostępny od HTTP/1.1). |
| **204** | _No Content_ | Brak zawartości | Serwer spełnił żądanie, ale nie musi zwracać entity-body i może chcieć zwrócić zaktualizowane metainformacje.<br />Odpowiedź może zawierać nowe lub zaktualizowane metainformacje w postaci nagłówków encji, które, jeśli są obecne, powinny być powiązane z żądanym wariantem.<br />Jeśli klient jest przeglądarką, nie powinien zmieniać widoku swojego dokumentu z tego, który spowodował wysłanie żądania. Ta odpowiedź ma przede wszystkim na celu umożliwienie wykonania akcji, bez powodowania zmian w aktywnym widoku dokumentu, chociaż wszelkie nowe lub zaktualizowane metainformacje powinny zostać do niego zastosowane.<br />Odpowiedź nie może zawierać message-body, dlatego jest zawsze zakończona pierwszą pustą linią po polach nagłówka. |
| 205 | _Reset Content_ | Resetuj zawartość | Serwer spełnił żądanie, a agent użytkownika powinien zresetować widok dokumentu, który spowodował wysłanie żądania.<br />Tą odpowiedź stosuje się, aby przeprowadzić akcję za pośrednictwem danych wprowadzonych przez użytkownika, po której następuje wyczyszczenie inputów na dane wejściowe, aby użytkownik mógł łatwo wprowadzić kolejne dane.<br />Odpowiedź nie może zawierać entity. |
| 206 | _Partial Content_ | Częściowa zawartość | Serwer wykonał żądanie GET o część zasobu. |
| 207 | _Multi-Status (WebDAV)_ | Multi-Status | Zapewnia status dla wielu niezależnych operacji.<br />Treść wiadomości, która następuje po niej, jest wiadomością XML i może zawierać kilka oddzielnych kodów odpowiedzi, w zależności od liczby złożonych żądań podrzędnych. |
| 208 | _Already Reported (WebDAV)_ | Już zgłoszone | Członkowie powiązania DAV zostali już wymienieni w poprzedniej odpowiedzi na to żądanie i nie są uwzględniani ponownie.<br />Kod używany wewnątrz elementu odpowiedzi DAV: propstat. |
| 226 | _IM Used_| Użyto manipulacji instancji | Serwer wypełnił żądanie GET o zasób, a odpowiedź jest reprezentacją rezultatu jednej lub więcej manipulacji instancji, wykonanych na obecnej instancji.<br />Rzeczywista aktualna instancja może nie być dostępna, chyba, że przez połączenie odpowiedzi z innymi poprzednimi lub przyszłymi odpowiedziami. |
</details></td></tr>
<tr><th style="text-align:center">3xx</th><td style="text-align:center"><i>Redirection</i></td><td style="text-align:center">Przekierowanie</td><td>Ta klasa kodów stanu wskazuje, że agent użytkownika musi podjąć dalsze działania, aby spełnić żądanie.<br/>Wymagane działanie może zostać przeprowadzone przez agenta użytkownika bez interakcji z użytkownikiem wtedy i tylko wtedy, gdy metodą użytą w drugim żądaniu jest GET lub HEAD. Klient powinien wykrywać nieskończone pętle przekierowań, ponieważ takie pętle generują ruch sieciowy dla każdego przekierowania.<br /><b>Uwaga!</b> Poprzednie wersje tej specyfikacji zalecały maksymalnie pięć przekierowań. Twórcy treści powinni mieć świadomość, że mogą istnieć klienci, którzy wprowadzają takie stałe ograniczenie.</td></tr>
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



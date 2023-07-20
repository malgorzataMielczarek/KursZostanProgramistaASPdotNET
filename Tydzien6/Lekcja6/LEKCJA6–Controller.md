# [LEKCJA 6 – Controller](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-6-aplikacje-webowe-w-asp-net-core/lekcja-6-controller/)
Kontrolery odpowiadają za odebranie z widoków żądań użytkownika i przekazanie ich do dalszej części aplikacji.

Kiedyś kontrolery były sercem aplikacji. To w nich znajdowała się cała logika. Łączyły się one bezpośrednio z bazą danych, odbierały i przerabiały dane i wysyłały je do widoków. Metody kontrolerów były więc bardzo obszerne. Tworzyliśmy w nich modele, pobieraliśmy je z bazy danych i przekazywaliśmy modele do widoków. Oczywiście w razie potrzeby umieszczaliśmy tam również dodatkową logikę.

Dzisiaj, ponieważ zaczęto dzielić aplikację na więcej warstw, więc rola kontrolerów nieco się zmieniła. Dalej są one odpowiedzialne za przyjmowanie wszystkich żądań użytkownika. Kiedy np. użytkownik naciśnie jakiś przycisk, akcja ta jest prawdopodobnie spięta z jakąś akcją kontrolera, która zostanie wywołana. Obecnie panuje trend na tzw. cienkie kontrolery. Oznacza to, że akcje kontrolerów powinny być jak najmniejsze (nie więcej niż 5-10 linii) i przyjmować jak najmniejszą odpowiedzialność. Tak na prawdę to powinny jedynie przyjmować żądania od użytkownika i przesłać je bezpośrednio do serwisów warstwy aplikacji oraz przesłać uzyskaną odpowiedź. Jeżeli więc metody w naszych aplikacjach zaczynają za bardzo się rozrastać, należy się zastanowić, czy nie powinniśmy przenieść części kodu do serwisów.

## Budowa kontrolera
Kontroler jest to klasa dziedzicząca po klasie `Controller`. W zależności od tego jak go nazwiemy, tak będziemy się do niego odwoływać. W pasku adresu, po adresie naszej strony internetowej (na razie będzie to np. _https://localhost:44395_), po backslashach podajemy nazwę kontrolera (bez przyrostka `Controller`), backslash, nazwa metody, którą chcemy wywołać. Załóżmy, że mamy kontroler:
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
        
        // przykladowe metody:

        // view
        public IActionResult Index()
        {
            return View();
        }

        // partial view
        public IActionResult TestPartial()
        {
            return PartialView();
        }

        // status http
        public IActionResult OkTest()
        {
            return Ok("Everything went fine");
        }

        // przekazywanie informacji prze JSON
        public IActionResult JsonResult()
        {
            return new JsonResult("JSON"); // zamiast "JSON" wstawiamy obiekt do serializacji
        }

        // zadania http
        [HttpGet]
        public IActionResult Index(int id)
        {
            return PartialView();
        }

        // implementacja kontrolera
    }
}
```
Jeżeli chcielibyśmy wywołać jego metodę `Index`, w pasku adresu w przeglądarce mielibyśmy np. _https://localhost:44395/Home/Index_.

Przyjrzyjmy się bliżej mechanizmom, z których możemy korzystać w kontrolerze.

### Konstruktor
#### `ILogger`
Do konstruktora naszego kontrolera podajemy obiekt loggera. Logger jest to mechanizm do logowania informacji na temat błędów, interakcji z użytkownikiem i wszystkiego, co chcemy zachować na później, aby np. mieć możliwość powrotu do jakiejś akcji, którą wykonał użytkownik i prześledzenia informacji, aby ustalić co mogło spowodować błąd.
#### Serwisy
W przyszłości, poza loggerem, będziemy musieli do naszego kontrolera przekazywać jeszcze interfejsy naszych serwisów z warstwy aplikacji, abyśmy mogli z nich korzystać.

### Akcje kontrolera
Akcje, czyli po prostu metody naszej klasy. Są one odpowiedzialne za wyświetlanie różnych widoków lub zwrócenie jakiejś informacji. Zwracają obiekt `IActionResult`. Mamy do dyspozycji kilka obiektów, które możemy zwrócić.
#### View
Metoda `View()` kontrolera, zwraca cały widok, czyli cały kontent naszej strony.
#### PartialView
Metoda `PartialView` kontrolera, zwraca tylko część widoku. Często, bardziej złożone widoki, składają się z kilku niezależnych części. Są to właśnie PartialViews, czyli widoki częściowe. Każdy z nich ma swoją niewielką odpowiedzialność.
#### Statusy HTTP
Czasami nie będziemy chcieli zwracać pełnego widoku, a jedynie jakąś informację. Np. mamy listę elementów i koło każdego z nich przycisk do usuwania. Chcemy usunąć jeden z elementów, więc wciskamy odpowiedni przycisk. Kontroler wywołuje więc akcję usuwania. Po usunięciu odpowiedniego elementu z bazy nie chcemy jednak na nowo ładować całego widoku, a jedynie uzyskać informację, czy akcja się udała. Do tego typu celów służą właśnie statusy HTTP. Zdefiniowano powszechnie używane statusy i przypisano im kody. Do poinformowania o pomyślnym przetworzeniu żądania, czyli w naszym przypadku, o tym, że element został usunięty może posłużyć status 200 (OK). Zwracamy go wywołując po prostu metodę `Ok()` kontrolera, z opcjonalnym parametrem `object?`, zawierającym np. opis.

Innym kodem z którym często się stykamy jest kod 400 (Bad Request), informujący, że serwer nie zrozumiał przesłanego przez użytkownika żądania. Zobaczymy go np. gdy próbujemy przejść na stronę, która nie istniej, wywołać nieistniejący w aplikacji kontroler itd. Taką odpowiedź zwróci nam metoda `BadRequest()` kontrolera. Występuje ona w trzech wersjach, bezparametrowej, z parametrem `ModelStateDictionary` (zawierającym błędy do zwrócenia klientowi) i z parametrem `object?` (zawierający obiekt błędu, np. opis, do zwrócenia klientowi).

##### [Kody statusów HTTP](https://www.restapitutorial.com/httpstatuscodes.html#)
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
| 300 | _Multiple Choices_ | Wiele wyborów | Serwer wskazuje wiele opcji na uzyskanie zażądanego zasobu.<br />Zażądany zasób odpowiada dowolnemu spośród zestawu. W odpowiedzi serwer wskazuje więc wiele opcji na uzyskanie zasobu, którymi klient może podążyć. Ten status można np. wykorzystać do przedstawienia różnych opcji formatu wideo, listy plików z różnymi rozszerzeniami lub ujednoznacznienia znaczenia słów. |
| 301 | _Moved Permanently_ | Przeniesiony na stałe | To i wszystkie przyszłe żądania o zażądany zasób powinny być kierowane na podane URI.<br />Zażądany zasób został przypisany do nowego stałego URI i wszystkie przyszłe referencje do tego zasobu powinny używać jednego ze zwróconych URI. Klienci z możliwością edycji linków, powinni automatycznie przełączyć referencję URI żądania na jedną lub więcej referencję zwróconą przez serwer, jeżeli jest to możliwe. Ta odpowiedź jest zapisywana w pamięci podręcznej, chyba że wskazano inaczej.<br />Nowy stały URI powinien być podany w polu _Location_ odpowiedzi. O ile metodą żądania nie była HEAD, entity odpowiedzi powinna zawierać krótką notatkę hipertekstową z hiperłączem do nowych identyfikatorów URI.<br />Jeśli kod zostanie odebrany w odpowiedzi na żądanie inne niż GET lub HEAD, agent użytkownika nie może automatycznie przekierować żądania, chyba że użytkownik może to potwierdzić, ponieważ może to zmienić warunki, na jakich żądanie zostało wysłane.<br /><b>Uwaga!</b> Podczas automatycznego przekierowywania żądania POST po otrzymaniu tego kodu niektóre istniejące aplikacje klienckie HTTP/1.0 błędnie zmienią je w żądanie GET. |
| 302 | _Found_ | Znaleziono | Zażądany zasób znajduje się czasowo pod innym URI.<br />Ponieważ przekierowanie może być okazjonalnie zmieniane, klient powinien nadal używać żądanego URI w przyszłych żądaniach. Ta odpowiedź jest zapisywana w pamięci podręcznej tylko wtedy, gdy jest to wskazane przez pole nagłówka _Cache-Control_ lub _Expires_.<br />Tymczasowe URI powinno zostać podane w polu _Location_ odpowiedzi. O ile metodą żądania nie była HEAD, entity odpowiedzi powinna zawierać krótką notatkę hipertekstową z hiperłączem do nowych identyfikatorów URI.<br />Jeśli kod zostanie odebrany w odpowiedzi na żądanie inne niż GET lub HEAD, agent użytkownika nie może automatycznie przekierować żądania, chyba że użytkownik może to potwierdzić, ponieważ może to zmienić warunki, na jakich żądanie zostało wysłane.<br /><b>Uwaga!</b> RFC 1945 i RFC 2068 określają, że klient nie może zmieniać metody w przekierowanym żądaniu. Jednak większość istniejących implementacji agenta użytkownika traktuje 302 tak, jakby była to odpowiedź 303, wykonując GET na wartości pola _Location_ niezależnie od pierwotnej metody żądania. Dodano kody statusu 303 i 307 dla serwerów, które chcą jednoznacznie określić, jakiej reakcji oczekuje klient. |
| 303 | _See Other_ | Zobacz inne | Odpowiedź na żądanie można znaleźć pod innym identyfikatorem URI i powinna zostać pobrana przy użyciu metody GET dla tego zasobu.<br />Ta metoda istnieje przede wszystkim po to, aby umożliwić przekierowanie agenta użytkownika na wybrany zasób, przez wyjście skryptu aktywowanego przez POST. Nowe URI nie jest zamiennikiem referencji do oryginalnie żądanego zasobu. Odpowiedź nie może zostać zapisana w pamięci podręcznej, ale można zapisać odpowiedź na drugie (przekierowane) żądanie.<br />Inne URI powinno zostać podane w polu _Location_ odpowiedzi. O ile metodą żądania nie była HEAD, entity odpowiedzi powinna zawierać krótką notatkę hipertekstową z hiperłączem do nowych identyfikatorów URI.<br /><b>Uwaga!</b> Wiele programów klienckich starszych niż HTTP/1.1 nie rozumie stanu 303. Gdy istnieje możliwość pracy z takimi klientami, zamiast tego można użyć kodu stanu 302, ponieważ większość agentów użytkownika reaguje na odpowiedź 302, jak opisano tutaj dla 303. |
| **304** | _Not Modified_ | Niezmienione |  |
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

#### Modele w JSONie
Ostatnim elementem który możemy zwrócić standardowo z naszej akcji jest model w JSONie. Jeżeli staramy się robić dużo rzeczy bez konieczności przeładowania strony to za modyfikacje w naszych widokach często będą odpowiadać skrypty JavaScript. Najlepiej komunikować się z nimi właśnie przy pomocy JSONów. Nasze modele serializujemy do formatu JSON i zwracamy. Następnie skrypt odbiera go i przy pomocy odpowiednich metod konwertuje do obiektu, który podmienia w naszym widoku. Do przesłania modelu w postaci JSON, w ASP.NET Core, możemy posłużyć się metodą `Json(object?)` kontrolera,
```csharp =
[Microsoft.AspNetCore.Mvc.NonAction]
public virtual Microsoft.AspNetCore.Mvc.JsonResult Json (object? data);
```
```csharp =
[Microsoft.AspNetCore.Mvc.NonAction]
public virtual Microsoft.AspNetCore.Mvc.JsonResult Json (object? data, object? serializerSettings);
```
gdzie `data` to obiekt do serializacji (nasz model), a `serializerSettings` ustawienia serializatora. Gdy używamy `System.Text.Json`, ustawienia te powinny być instancją klasy `JsonSerializerOptions`, natomiast dla biblioteki `Newtonsoft.Json`, `JsonSerializerSettings`. Metoda zwraca obiekt `JsonResult`, który serializuje podane dane do odpowiedzi jako JSON.

Zamiast użycia metody kontrolera, możemy również bezpośrednio stworzyć nowy obiekt klasy `JsonResult`. Konstruktor tej klasy przyjmuje takie same parametry jak powyższa metoda.

#### Żądania HTTP
Standardowe żądania, jakie możemy odebrać od użytkownika. Typ żądania, jakiego oczekuje dana metoda podajemy nad nią jako atrybut (w nawiasach kwadratowych). Najczęściej używamy żądań:
##### HttpGet
Najbardziej standardowy typ zapytania od użytkownika, o konkretny model. Używa ono jawnego przesyłania parametrów w adresie. Gdybyśmy więc chcieli wywołać metodę `Indeks` naszego kontrolera z parametrem `5`, to w pasku adresu mielibyśmy coś takiego: _https://localhost:44395/Home/Index/5_.
##### HttpPost
Ten typ używa niejawnego przesyłania wartości parametrów przy pomocy formularza HTML. Więcej na temat formularzy dowiemy się w dalszej części kursu.
##### HttpDelete
Używamy gdy chcemy usunąć jakiś element. Przesyłamy wtedy np. samo id. Moglibyśmy do tego użyć również żądania HttpGet lub HttpPost, ale tu dostajemy bezpośrednio informację, że powinniśmy wywołać akcję usunięcia danego rekordu z bazy danych. Jest to dodatkowe zabezpieczenie, że akcją którą chcemy wywołać ma związek z usuwaniem.
##### HttpPut
Żądanie zbliżone z HttpPost, związane jednak z tworzeniem nowego obiektu. Przy pomocy HttpPost możemy np. modyfikować istniejący obiekt, natomiast HttpPut będzie zazwyczaj oznaczał dodanie nowego rekordu do bazy danych.

Nie musimy stosować żądań HttpDelete i HttpPut, gdyż ich funkcję mogą z powodzeniem wykonać żądania HttpGet iHttpPut. Są one jedynie dodatkowym potwierdzeniem.
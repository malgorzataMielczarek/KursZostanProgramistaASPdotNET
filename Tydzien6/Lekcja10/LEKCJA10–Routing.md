# [LEKCJA 10 – Routing](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-6-aplikacje-webowe-w-asp-net-core/lekcja-10-routing/)
Routing, czyli po polsku trasowanie - sposób zamiany adresu na wywołanie konkretnej akcji, konkretnego kontrolera. Podczas trasowania nie jest uwzględniana wielkość liter (ani w tekście dosłownym, ani np. w nazwach kontrolerów, czy akcji).

Standardowo routing konfigurujemy w klasie `Startup` projektu MVC (od ASP.NET Core 6 kod z klasy `Startup` został dodany do klasy `Program`). W utworzonym domyślnie szablonie na końcu tej klasy mamy:
```csharp =
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
    endpoints.MapRazorPages();
});
```
Metoda `UseEndpoints()` hosta naszej aplikacji służy właśnie do skonfigurowania tzw. endpointów do rozpoznawania adresów. Dzięki temu nasza aplikacja rozpoznaje jaka akcja jakiego kontrolera powinna zostać wywołana, w zależności od tego co zostanie wpisane (jaki adres). Najbardziej standardowy i najczęściej stosowany route został już dla nas automatycznie utworzony. Jest to ten fragment: `endpoints.MapControllerRoute( name: "default", pattern: "{controller=Home}/{action=Index}/{id?}");` z kodu powyżej. Mówi nam on, że pierwszy człon, po adresie naszej strony, jest nazwą kontrolera, który chcemy użyć, kolejny to nazwa akcji do wywołania, a następnie w razie potrzeby może zostać podany parametr, który zostanie przekazany do akcji jako `id` (dokładnie taka nazwa musi zostać użyta dla parametru w definicji akcji). Dopiszmy jeszcze jeden przykładowy route, a potem omówimy dokładniej metodę `MapControllerRoute`:
```csharp =
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
        name: "blog",
        pattern: "blog/{*article}",
        defaults: new  { controller = "Blog", action = "Article" });
    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
    endpoints.MapRazorPages();
});
```
## `MapControllerRoute`
Metoda dodaje endpoint dla akcji kontrolera do buildera routingu `IEndpointRouteBilder`. Precyzuje sposób w jaki ma zostać przeprowadzone trasowanie przy pomocy następujących parametrów:
### `name`
`string name` - nazwa tej trasy. Może być to dowolna wymyślona przez nas nazwa. W utworzonym kodzie użyto nazwy `"default"`, jest to jednak tylko nazwa i nie niesie z sobą żadnego dodatkowego znaczenia. Nazwy tras muszą być jednak unikatowe w obrębie całej aplikacji. Jest to parametr obowiązkowy.
### `pattern`
`string pattern` - wzór URL tej trasy. Jest to parametr obowiązkowy.<br />
Wzór może składać się z kilku elementów:
* dosłowny tekst - możemy wpisać tekst, który będzie musiał znaleźć się w URL np. tekst `blog`, z naszego przykładu. Wielkość liter w adresie URL niema wpływu na dopasowanie.
* parametry - w URL możemy zaszyć dowolną ilość parametrów. We wzorze zapisujemy je w nawiasach klamrowych, podając nazwę parametru. W naszych przykładach mamy więc zdefiniowane 4 parametry: `article` (`{*article}`), `controller` (`{controller=Home}`), `action` (`{action=Index}`) i `id` (`{id?}`). Podczas definiowania parametrów poza podaniem ich nazwy, możemy zrobić jeszcze kilka rzeczy:
    * parametr _catch-all_ (`*`)<br />
    W przykładzie, przed nazwą parametru `article`, znajduje się symbol `*`, oznacza to że jest to tzw. _catch-all parameter_, czyli wszystko co wpiszemy w adresie URL po _blog/_ zostanie przypisane do parametru `article` (nawet jeśli będą się tam znajdywać np. backslashe). Tylko ostatni parametr we wzorze może być tego typu.
    * parametr z wartością domyślną<br />
    Do parametru możemy przypisać wartość domyślną, jak to zrobiliśmy w przypadku parametrów `controller` i `action`. Po nazwie parametru wstawiamy znak `=` i podajemy wartość, którą przyjmie nasz parametr, jeżeli nie zostanie ona podana w URL. To znaczy, że wszystkie ścieżki _/Home/Index/8_, _/Home/Index_, _/Home_, _/_, będą prowadzić do akcji `Index` kontrolera `Home`.
    * parametr z ograniczeniami<br />
    Oznacza to, że URL zostanie dopasowany do danego wzoru, jeżeli wartość parametru będzie spełniać określone warunki. W naszych przykładach nie mamy akurat takich parametrów, ale przykłady będzie można zobaczyć przy okazji omówienia parametru [`constraints`](#constraints).
    * parametr opcjonalny (`?`)<br />
    Wstawiając `?` po nazwie parametru, jak w przypadku parametru `id`, oznaczamy go jako opcjonalny. Oznacza to, że może, ale nie musi zostać on uwzględniony w adresie URL. Oczywiście, opcjonalne mogą być tylko końcowe (dowolna ilość) parametry. W przypadku stosowania parametrów opcjonalnych, należy pamiętać, aby w naszych akcjach sprawdzić czy nie mają one wartości `null`, albo nadać parametrom akcji wartości domyślne.
* backslashe - parametry oddzielamy od siebie lub od tekstu dosłownego backslashami (`/`).
#### Zarezerwowane nazwy trasowania
Routing system ma zarezerwowane pewne nazwy, które stosuje do określonych celów. Oznacza to, że nie możemy ich używać np. jako nazw naszych parametrów, używanych np. do przekazania wartości jako parametru akcji. W naszym przykładzie użyliśmy dwóch takich specjalnych parametrów: `controller` i `action`. Jak wiem, pozwalają one naszej aplikacji wywołać odpowiedni kontroler i jego akcję. Takimi zarezerwowanymi nazwami, gdy używamy `Controllers` lub _Razor Pages_, są:
* `action`,
* `area`,
* `controller`,
* `handler`,
* `page`,

a w kontekście _Razor view_ lub _Razor Page_:
* `page`,
* `using`,
* `namespace`,
* `inject`,
* `section`,
* `inherits`,
* `model`,
* `addTagHelper`,
* `removeTagHelper`.

Te słowa kluczowe nie powinny być używane przy generowaniu linków, do parametrów związanych z modelami i właściwości _top level_.
### `defaults`
`[object? defaults = default]` - obiekt zawierający domyślne wartości dla parametrów route. Właściwości obiektu reprezentują nazwy i domyślne wartości. Jest to parametr z wartością domyślną `null`. Zamiast podawać wartości domyśle w osobnym parametrze, można to również zrobić bezpośrednio we wzorze (w parametrze `pattern`), tak jak w naszym wygenerowanym kodzie. Możemy tu również wpisać wartości domyślne parametrów nie będących częścią `pattern`, jak to zrobiliśmy w dopisanym przez nas przykładzie. Oznacza on, że wszystkie adresy URL rozpoczynające się od członu _blog_ będą wywoływać akcję `Article`, kontrolera `Blog`.
### `constraints`
`[object? constraints = default]` - obiekt zawierający ograniczenia dla tego route. Właściwości obiektu reprezentują nazwy i wartości ograniczeń. Jest to parametr z wartością domyślną `null`.<br />Możemy używać tego parametru np. do określenia typu danych jakiego powinien być dany parametr trasy. Używamy tu określonych słów kluczowych. Podobnie jak w przypadku wartości domyślnych, ograniczenia możemy zawrzeć w osobnym parametrze lub wewnątrz wzoru (w parametrze `pattern`).
Ograniczenie     |Przykład               |Przykładowe dopasowania             |Uwagi
----------------:|:---------------------:|:----------------------------------:|:----
`int`              |`{id:int}`               |`123456789`, `-123456789`               |Dopasowuje dowolną liczbę całkowitą.
`bool`             |`{active:bool}`          |`true`, `FALSE`                         |Dopasowuje _true_ lub _false_. Wielkość liter niema znaczenia.
`datetime`         |`{dob:datetime}`         |`2016-12-31`, `2016-12-31 7:32pm`       |Dopasowuje prawidłową wartość `DateTime` niezależną od kultury (`InvariantCulture`).
`decimal`          |`{price:decimal}`        |`49.99`, `-1,000.01`                    |Dopasowuje prawidłową wartość `decimal` niezależną od kultury (`InvariantCulture`).
`double`           |`{weight:double}`        |`1.234`, `-1,001.01e8`                  |Dopasowuje prawidłową wartość `double` niezależną od kultury (`InvariantCulture`).
`float`            |`{weight:float}`         |`1.234`, `-1,001.01e8`                  |Dopasowuje prawidłową wartość `float` niezależną od kultury (`InvariantCulture`).
`guid`             |`{id:guid}`              |`CD2C1638-1638-72D5-1638-DEADBEEF1638`|Dopasowuje prawidłową wartość `Guid` (_globally unique identifier_ - globalnie unikatowy identyfikator).
`long`             |`{ticks:long}`           |`123456789`, `-123456789`               |Dopasowuje prawidłową wartość `long`.
`minlength(value)` |`{username:minlength(4)}`|`Rick`                                |Ciąg musi składać się z przynajmniej 4 znaków.
`maxlength(value)` |`{filename:maxlength(8)}`|`MyFile`                              |Ciąg musi składać się z nie więcej niż 8. znaków.
`length(length)`   |`{filename:length(12)}`  |`somefile.txt`                        |Ciąg musi składać się z dokładnie 12. znaków.
`length(min,max)`  |`{filename:length(8,16)}`|`somefile.txt`                        |Ciąg musi mieć przynajmniej 8 i nie więcej niż 16 znaków.
`min(value)`       |`{age:min(18)}`          |`19`                                  |Liczba całkowita nie mniejsza niż 18.
`max(value)`       |`{age:max(120)}`         |`91`                                  |Liczba całkowita nie większa niż 120.
`range(min,max)`   |`{age:range(18,120)}`    |`91`                                  |Liczba całkowita o wartości przynajmniej 18, ale nie większej niż 120.
`alpha`            |`{name:alpha}`           |`Rick`                                |Ciąg musi składać się z co najmniej jednego znaku alfabetycznego, a-z, bez uwzględniania wielkości liter.
`regex(expression)`|`{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}`|`123-45-6789`       |Ciąg musi pasować do wyrażenia regularnego. Używając `System.Text.RegularExpressions` do przetwarzania niezaufanych danych wejściowych, przekaż limit czasu. Złośliwy użytkownik może wprowadzić dane wejściowe do wyrażeń regularnych, powodując atak typu „odmowa usługi”. Interfejsy API platformy ASP.NET Core korzystające z wyrażeń regularnych przekazują limit czasu.
`required`         |`{name:required}`        |`Rick`                                |Służy do wymuszenia obecności wartości niebędącej parametrem podczas generowania adresu URL.

Do jednego parametru można zastosować kilka ograniczeń, np.`{id:int:min(1)}`.
| :warning:**UWAGA!** |
| :---: |
Nie należy używać ograniczeń do walidacji danych wejściowych. Jeżeli to zrobimy, nieprawidłowe dane spowodują odpowiedź 404(_Not Found_ - nie znaleziono), a powinny powodować odpowiedź 400(_Bad Request_ - złe żądanie) z odpowiednią informacją o błędzie. Ograniczenia route służą do ujednoznaczniania podobnych tras, a nie do sprawdzania poprawności danych wejściowych dla określonego route.
### `dataTokens`
`[object? dataTokens = default]` - obiekt zawierający tokeny danych dla tego route. Właściwości obiektu reprezentują nazwy i wartości tokenów danych. Jest to parametr z wartością domyślną `null`. W odróżnieniu od dwóch poprzednich parametrów, `dataTokens` nie da się dołączyć do parametru `pattern`, jeżeli więc potrzebujemy ich użyć, musimy przypisać wartość do tego parametru.<br />
_Data tokens_ są to dodatkowe wartości (dodatkowy obiekt) powiązane z tą konkretną trasą. Nie mają one wpływu na sam proces routingu. Zasadniczo zostały one zaprojektowane, aby powiązać dane stanu z konkretną trasą. Wartości nie są dynamiczne, nie zmieniają się więc w zależności od URL. Zamiast tego są ustalone dla danego route. To znaczy, że możemy ich użyć do ustalenia, która trasa została wybrana podczas routingu. Może to być użyteczne, gdy mamy kilka możliwości, które mapują tą samą akcję i musimy wiedzieć, która z nich została wybrana (np. akcja różni się w zależności od wybranej trasy). Tego typu akcje nie powinny jednak występować. `dataTokens` raczej się więc nie używa.
## Hierarchia trasowania
Podczas trasowania, aplikacja stara się dopasować podany adres URL do zdefiniowanych tras, w kolejności ich dodawania. Oznacza to, że w naszym przykładzie, route `blog` jest nadrzędny w stosunku do trasy `default`. Czyli otrzymany adres URL najpierw zostanie porównany z trasą `blog`. Jeżeli do niej pasuje, to zostaje wywołana akcja `Article` kontrolera `Blog`. Jeżeli nie pasuje, to zostaje porównany z kolejną trasą (`default`). Jeśli jest zgodny, zostaje wywołana odpowiednia akcja, odpowiedniego kontrolera. Jeżeli nie jest zgodny, to zostaje zwrócony błąd 404, gdyż nie mamy zdefiniowanych więcej tras.

| :warning:**UWAGA!** |
| :---: |
|Dopisując własne trasy, zwróć uwagę na kolejność w jakiej dodajesz je do buildera. Jeżeli jakiś adres URL będzie pasował do kilku tras, będzie to miało wpływ na to, który route zostanie wybrany, a w efekcie na trasowanie.|

## `MapRazorPages`
Na koniec routingu dodajemy do buildera endpointy dla naszych _Razor Pages_, czyli naszych stron, które utworzyliśmy przy pomocy silnika Razor. Robimy to przy pomocy metody `MapRazorPages()`, wywołanej na naszym builderze.

## Trasowanie przez atrybuty
Sytuacja, że potrzebujemy dodawać do routingu własne trasy, zdarza się tylko w specyficznych sytuacjach (jak np. podany w przykładzie blog). Chociaż oczywiście możemy to robić, taka konieczność zdarza się raczej rzadko. Jeżeli potrzebujemy stworzyć bardziej zaawansowane trasowanie, częściej będziemy korzystać z atrybutów przypisanych bezpośrednio do kontrolerów i akcji. Służą do tego klasy implementujące interfejs `IRouteTemplateProvider`. Najbardziej podstawową jest `RouteAttribute`. Jak wszystkie atrybuty, zapisujemy je w nawiasach kwadratowych nad nazwą akcji/kontrolera której/ego dotyczą. W jednym atrybucie możemy umieścić jedną trasę, podając jako argument konstruktora wzór, taki jak zapisywaliśmy [w parametrze `pattern` metody `MapControllerRoute`](#pattern).
| :warning:**UWAGA!** |
| :---: |
| Trasy zdefiniowane przez atrybuty nadpisują te zdefiniowane w konwencjonalny sposób (opisany powyżej).|
### Przykład
```csharp =
[Route("[controller]/[action]")]
public class HomeController : Controller
{
    [Route("~/")]
    [Route("/Home")]
    [Route("~/Home/Index")]
    public IActionResult Index()
    {
        return View();
    }

    [Route("{id?}")]
    public IActionResult About(int? id)
    {
        return View(id);
    }
}
```
* Atrybuty możemy przypisywać zarówno do kontrolera, jak i do akcji (ale możemy też przypisać np. tylko do akcji).
* Jeżeli do kontrolera jest przypisana trasa, to route akcji jest dopisywany na jej końcu, chyba że rozpoczyna się od `/` lub `~/` (jak w trasach do akcji `Index` w przykładzie). Wówczas route akcji stanowi kompletną trasę.
* Do jednego elementu (kontrolera, akcji) możemy przypisać dowolną ilość tras.
* We wzorach możemy stosować parametry (również _catch-all_, z ograniczeniami, z wartościami domyślnymi, opcjonalne), tekst dosłowny i podstawienie tokenów (_token replacement_).
* _Token replacement_ pozwala wstawić w template trasy tokeny `[action]`, `[area]` i `[controller]`, które zostają zastąpione odpowiednio przez nazwę akcji, nazwę obszaru i nazwę kontrolera.
* Jeżeli ustalimy trasę dla kontrolera (nie używającą tokenu `[action]`), to musimy również ustalić trasy dla jego akcji. Inaczej trasa kontrolera będzie prowadzić do wielu endpointów (do wszystkich akcji, dla których nie ustalono trasy i ewentualnie tych, które mają taki sam route jak kontroler), więc aplikacja wyrzuci błąd.
* Jeżeli nie ustalimy trasy dla kontrolera to możemy np. ustalić trasy tylko dla niektórych akcji. Wówczas reszta będzie trasowana według naszego standardowego routingu zdefiniowanego dla naszej aplikacji (w pliku _Startup.cs_ lub _Program.cs_).

### Atrybuty metod HTTP
Jeżeli któraś z akcji ma odpowiadać tylko na konkretne żądanie HTTP, to zamiast używać `RouteAttribute`, trasę możemy podać jako argument konstruktora odpowiedniej klasy pochodnej `HttpMethodAttribute`. Np. `HttpGetAttribute`:
```csharp =
[Route("api/[controller]")]
[ApiController]
public class Test2Controller : ControllerBase
{
    [HttpGet]   // GET /api/test2
    public IActionResult ListProducts()
    {
        return ControllerContext.MyDisplayRouteInfo();
    }

    [HttpGet("{id}")]   // GET /api/test2/xyz
    public IActionResult GetProduct(string id)
    {
       return ControllerContext.MyDisplayRouteInfo(id);
    }

    [HttpGet("int/{id:int}")] // GET /api/test2/int/3
    public IActionResult GetIntProduct(int id)
    {
        return ControllerContext.MyDisplayRouteInfo(id);
    }

    [HttpGet("int2/{id}")]  // GET /api/test2/int2/3
    public IActionResult GetInt2Product(int id)
    {
        return ControllerContext.MyDisplayRouteInfo(id);
    }
}
```
## Jawne przekazywanie wartości parametrów przez URL
Wiemy już, że możemy w ścieżce podać wartości parametrów, które zdefiniowaliśmy w trasie. Istnieje jednak jeszcze jeden sposób. URL poza wywołaniem odpowiedniej akcji może również w sposób całkowicie jawny przypisać wartości jej parametrom. Wówczas po ścieżce wpisujemy znak _?_, a następnie nazwę parametru, równa się i jego wartość. Np. adres _https://learn.microsoft.com/en-us/aspnet/core/mvc/controllers/routing?view=aspnetcore-7.0_, przypisze parametrowi `view`, odpowiedniej akcji, wartość `"aspnetcore-7.0"` (lub jej odpowiednik, po przekonwertowaniu na odpowiedni typ danych). Jeżeli chcemy przekazać wartości dla kilku parametrów, to wstawiamy pomiędzy znak _&_. Np. _https://localhost:7131/Home/User?name=JanKowalski&admin=true_ wywoła nam jakąś akcję `User` kontrolera `Home` z dwoma parametrami: `name` (równym `"JanKowalski"`) i `admin` (równym `true`). Jest to przekazywanie parametrów przez żądanie HTTP GET.
| :warning:**UWAGA!** |
| :---: |
| Jeżeli podana w adresie URL wartość parametru nie będzie dała się przekonwertować na typ jaki ma odpowiadający mu parametr akcji, to parametr akcji będzie miał wartość domyślną dla swojego typu. Dzieje się tak niezależnie czy przekazano wartość parametru w postaci jawnej, czy w postaci uwzględnionej w trasie. |
| Jeżeli w trasie zdefiniowaliśmy dodatkowe parametry (nie opcjonalne) i nie podaliśmy ich w adresie URL to dostaniemy błąd 404. |
| Jeżeli wywołano akcję przyjmującą parametry, ale nie podano wartości tych parametrów lub podano wartości niezgodne z typem, to parametry akcji będą miały wartości domyślne dla swojego typu. |

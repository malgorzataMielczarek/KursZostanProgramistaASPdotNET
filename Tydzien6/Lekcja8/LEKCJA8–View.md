# [LEKCJA 8 – View](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-6-aplikacje-webowe-w-asp-net-core/lekcja-8-view/)
Widoki w modelu MVC znajdują się w folderze _Views_ naszej aplikacji webowej. Tworzymy w nim osobny katalog dla każdego kontrolera, o nazwie zgodnej z nazwą kontrolera. Będziemy w nim umieszczać wszystkie widoki związane z akcjami tego kontrolera. Poza folderami poszczególnych kontrolerów w _Views_ znajduje się jeszcze jeden katalog, _Shared_. W nim umieszczamy wszystkie widoki współdzielone pomiędzy różnymi akcjami i kontrolerami. Znajduje się tu m.in plik _\_Layout.cshtml_.

## _\_Layout.cshtml_
Jest to plik zawierający szablon do stworzenia strony internetowej (pełny dokument html). W nim będą w odpowiednim miejscu wstawiane nasze widoki. Ponieważ jest to plik .cshtml, a nie .html, więc możemy w nim również korzystać z mechanizmów silnika Razor.

## Wyszukiwanie widoku
Aby przejść do pliku z widokiem, który zostanie wywołany przez wybraną akcję naszego kontrolera, możemy w folderze _Views_ w podfolderze naszego kontrolera wyszukać plik .cshtml o tej samej nazwie co nazwa akcji lub kliknąć prawym przyciskiem myszki na nazwę metody i wybrać _Go To View_ (skrót _Ctrl+M, Ctrl+G_).

## Znaczniki Razor (ang. _Razor markup_)
Widoki .cshtml używają silnika Razor, umożliwiającego tworzenie dynamicznych stron internetowych (zmieniających się w zależności od warunków). Umożliwia nam on m.in używania składni C# wewnątrz naszych widoków np. pętli, warunków itp.

### Pomocnicze atrybuty `<a>`
Wprowadza też dodatkowe atrybuty do elementu `<a>` html (hiperłącze, link), np.:
* `asp-page` - ustawia URL na podaną stronę. Jeżeli przed nazwą strony wstawimy `/`, to zostanie stworzony URL pasującej strony począwszy od roota aplikacji. Jeżeli strona o podanym linku nie istnieje, to link będzie prowadził do aktualnej strony.
* `asp-area` - ustawia nazwę obszaru użytego do ustawienia URL
* `asp-controller` - przypisuje kontroler użyty przy generowaniu URL
* `asp-action` - przypisuje akcję kontrolera, która zostanie załączona w generowanym URL

Jeżeli `asp-controller` jest określony, a `asp-action` nie, domyślną akcją jest akcja związana z aktualnie wykonywanym widokiem. W odwrotnej sytuacji, gdy mamy określone `asp-action`, a `asp-controller` nie, użyty zostaje domyślny kontroler wywołujący aktualnie wykonywany widok. Atrybut `asp-page` nie może zostać użyty równocześnie z atrybutami `asp-route`, `asp-controller` i `asp-action`.

### Pomocnicze znaczniki
Składnia Razor wprowadza również dodatkowe znaczniki, które nie występują w czystym html-u. Jednym z nich jest `<partial>`. Jest on używany do renderowania _partial views_. Ma on jeden wymagany atrybut, `name`. Wskazuje on nazwę lub ścieżkę widoku częściowego, który ma zostać wyrenderowany (w to miejsce zostanie wstawiony nasz widok częściowy). Poza tym mamy do dyspozycji atrybuty:
* `for` - przypisuje `ModelExpression`, czyli wyrażenie wykonywane na obecnym modelu. Domyślnie przypisane wyrażenie zostaje przy wykonywaniu poprzedzone `@Model.`, czyli np. `for="Id"` oznacza to samo co `for="@Model.Id"`. Domyślne zachowanie nadpisuje się przez użycie symbolu `@` do zdefiniowania wyrażenia.
* `model` - definiuje instancję modelu, która zostanie przekazana do _partial view_. Możemy tu np. stworzyć nowy obiekt z przypisanymi danymi, np. `model='new Item() { Id = 1, Name = "Apple", TypeName = "Grocery" }'`. Atrybut `model` nie może zostać użyty równocześnie z atrybutem `for`.
* `view-data` - przypisuje `ViewDataDictionary`, który zostanie przekazany do widoku częściowego.

### Przekazywanie danych do widoku
Do widoku możemy przekazać dane silnie lub słabo typowane.
#### Dane silnie typowane (ang. _Strongly-typed data_) - _viewmodel_
Jak już wspominaliśmy do widoku możemy przekazywać modele, tzw. _viewmodel_ (modele widoków). Podajemy je jako argument metody `View` kontrolera. Są to właśnie silnie typowane dane. Jeżeli chcemy, aby nasz widok przyjmował dane w tej formie, to w pierwszej linii umieszczamy dyrektywę `@model`, po której podajemy typ jaki ma nasz model. Do widoku będziemy mogli w ten sposób przekazać tylko i wyłącznie dane sprecyzowanego typu. Np. w pierwszej linii stworzonego przez nas w poprzedniej lekcji widoku mieliśmy `@model IEnumerable<WarehouseMVC.Web.Models.Item>`, co oznaczało, że do tego widoku możemy przekazywać tylko i wyłącznie kolekcję obiektów `Item`, implementującą interfejs `IEnumerable`, np. listę. Dzięki temu w widoku mogliśmy się odwoływać do przekazanego obiektu, znając jego właściwości. Wiedzieliśmy więc, że model jest `IEnumerable`, a co za tym idzie mogliśmy zrobić po nim pętlę `foreach`. Określiliśmy również, że zawiera on obiekty typu `Item`, dzięki czemu mogliśmy się odwoływać do właściwości każdego z nich. Model w widoku niema żadnej nazwy. Odwołujemy się do niego przez `@Model`. `Model`, jest to po prostu właściwość readonly, która zostaje nam przekazana z kontrolera i zawsze tak samo się nazywa.
#### Dane słabo typowane (_Weakly typed data_) - `ViewData`, atrybut `[ViewData]` i `ViewBag`
Poza modelami, możemy przekazywać dane do widoku używając kolekcji `ViewData`. Nie musimy się martwić przesyłaniem tej kolekcji do widoku (jak to się dzieje w przypadku modeli), gdyż odbywa się to automatycznie. Przekazywane w ten sposób dane są słabo typowane, co oznacza, że nie deklarujemy wyraźnie jakiego typu danych będziemy używać. Możemy go używać do przekazywania niewielkiej ilości danych z i do kontrolera i widoku.
Przekazywanie danych pomiędzy |Przykład
:-----------------------------|:-------
Kontrolerem a widokiem        |Wypełnienie listy rozwijanej danymi
Widokiem a widokiem \_Layout  |Ustawienie zawartości elementu `<title>` w _layout view_ z pliku widoku
Widokiem częściowym i widokiem|Widget wyświetlający dane zależnie od strony web, o którą poprosił użytkownik

Słabo typowane mechanizmy są rozwiązywane dynamicznie w czasie wykonywania. W związku z tym nie oferują one sprawdzania typów podczas kompilacji. Co za tym idzie są one bardziej podatne na błędy niż viewmodele. Z tego powodu niektórzy deweloperzy wolą ograniczyć lub wykluczyć ich użycie.
##### `ViewData`
Jest to obiekt `ViewDataDictionary`. Jak sama nazwa wskazuje jest to obiekt typu `Dictionary`, czyli słownik. Jego klucze są typu `string` i nie jest w nich rozróżniana wielkość liter. Jako wartość możemy wpisać obiekt dowolnego typu, jednak zostanie on zapisany jako `string`. Jeżeli więc przechowujemy w `ViewData` obiekty inne niż `string`, aby je potem użyć musimy dokonać jawnego rzutowania.
###### Przykład:
Akcja kontrolera:
```csharp =
public IActionResult SomeAction()
{
    ViewData["Greeting"] = "Hello";
    ViewData["Address"]  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```
Widok:
```csharp =
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```
##### Atrybut `ViewData`
`ViewDataDictionary` można również użyć przez `ViewDataAttribute`. Właściwości kontrolera lub modelu oznaczone atrybutem `[ViewData]`, mają swoje wartości przechowywane i ładowane ze słownika `ViewData`.
###### Przykład:
Fragment kontrolera:
```csharp =
public class HomeController : Controller
{
    [ViewData]
    public string Title { get; set; }

    public IActionResult About()
    {
        Title = "About Us";
        ViewData["Message"] = "Your application description page.";

        return View();
    }
}
```
Fragment _\_Layout.cshtml_:
```html =
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```
##### `ViewBag`
| :warning:**UWAGA!** |
| :---: |
|`ViewBag` nie jest domyślnie dostępny w klasach `PageModel` _Razor Pages_. |

`ViewBag` jest obiektem `Microsoft.AspNetCore.Mvc.ViewFeatures.Internal.DynamicViewData`, zapewniającym dynamiczny dostęp do obiektów przechowywanych w `ViewData`. Może on być nieco wygodniejszy w użyciu, gdyż nie wymaga rzutowania obiektów. Tworzy on jakby nowe właściwości o nazwach tożsamych z kluczami słownika. Do przechowywanych obiektów nie odwołuje się więc przez notację słownikową, ale przez notację z kropką, tak jakbyśmy odwoływali się do właściwości. Użyte nazwy, podobnie jak klucze w `ViewData`, nie rozpoznają wielkości liter.
###### Przykład:
Akcja kontrolera:
```csharp =
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```
Widok:
```html =
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

Ponieważ `ViewData` i `ViewBag` odwołują się do tego samego obiektu, można więc je stosować zamiennie, np. zapisać dane używając `ViewData` i odczytać używając `ViewBag` lub na odwrót. To którego podejścia użyć zależy wyłącznie od indywidualnych preferencji. Wprawdzie oba podejścia można dowolnie mieszać, jednak wybór jednego zwiększa czytelność kodu i ułatwia jego utrzymanie.

##### Różnice między `ViewData` i `ViewBag`
Cecha|`ViewData`|`ViewBag`
----:|:--------:|:------:
Klasa bazowa|`ViewDataDictionary`|`Microsoft.AspNetCore.Mvc.ViewFeatures.Internal.DynamicViewData`
Dodatkowe zalety|Ma dostęp do metod takich jak `ContainsKey`, `Add`, `Remove`i `Clear`. Ponieważ klucz jest typu `string`, więc pozwala na stosowanie białych znaków.|Nie wymaga rzutowania. Składnia może być szybsza do dodania do kontrolerów i widoków. Łatwiej sprawdzić wartości `null`, np.: `@ViewBag.Person?.Name`
Sposób odwołania|`ViewData["Some Key With Whitespace"]`|`@ViewBag.SomeKey = <value or object>`
Rzutowanie|Każdy obiekt inny niż `string` wymaga rzutowania.|Nie wymaga rzutowania.
### Pomocnicze metody
Silnik Razor umożliwia nam również korzystanie z pomocniczych metod, np. `@RenderBody` i `@RenderSection` oraz metody klasy `HtmlHelper<TModel>` np. `@Html.DisplayFor()` i `@Html.DisplayNameFor()`.
#### `@RenderBody()`
Jest to jakby _placeholder_ dla naszych widoków. W środku tej metody zostaną wyrenderowane wszystkie elementy aktualnego widoku.
#### `@RenderSection(string name, [bool required])`
Powoduje wyrenderowanie sekcji o podanej nazwie. Dodatkowy parametr `bool` wskazuje, czy sekcja musi zostać wyrenderowana, np. czy widok musi obowiązkowo posiadać taką sekcję. Jest to więc _placeholder_ na sekcje. Sekcje zapewniają sposób na zorganizowanie, gdzie pewne elementy strony powinny zostać umieszczone. Sekcje zdefiniowane na stronie lub w widoku, są dostępne tylko w ich bezpośredniej stronie _layout_. Nie można się do nich odnosić z widoków częściowych, komponentów itd.
##### Przykład
Definicja sekcji, np. w widoku
```html =
@section Scripts {
     <script type="text/javascript" src="~/scripts/main.js"></script>
}
```
Umiejscowienie sekcji, fragment _\_Layout.cshtml_
```csharp =
<script type="text/javascript" src="~/scripts/global.js"></script>

@RenderSection("Scripts", required: false)
```
#### `@Html.DisplayFor()`
Jako parametr podajemy wyrażenie, wskazujące na to co chcemy zaprezentować. Metoda w zależności od tego jakiego typu dane wskazaliśmy generuje odpowiednie elementy html prezentujące wskazane dane. W wygenerowanym w poprzedniej lekcji widoku użyto tej metody, do wypełnienia tabeli danymi:
```html =
<tbody>
@foreach (var item in Model){
        <tr>
            <td>
                @Html.DisplayFor(modelItem => item.Id)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Name)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.TypeName)
            </td>
            <td>
                @Html.ActionLink("Edit", "Edit", new { /* id=item.PrimaryKey */ }) |
                @Html.ActionLink("Details", "Details", new { /* id=item.PrimaryKey */ }) |
                @Html.ActionLink("Delete", "Delete", new { /* id=item.PrimaryKey */ })
            </td>
        </tr>
}
    </tbody>
```
#### `@Html.DisplayNameFor()`
Podobnie jak w poprzedniej metodzie, jako parametr podajemy wyrażenie, wskazujące na element. Tym razem nie zostanie jednak zaprezentowana wartość tego elementu, a jego nazwa, np. nazwa właściwości. Została ona również użyta w wygenerowanym w poprzedniej lekcji widoku, przy tworzeniu nagłówków tabeli:
```html =
<thead>
        <tr>
            <th>
                @Html.DisplayNameFor(model => model.Id)
            </th>
            
            <th>
                @Html.DisplayNameFor(model => model.Name)
            </th>
            
            <th>
                @Html.DisplayNameFor(model => model.TypeName)
            </th>
            <th></th>
        </tr>
    </thead>
```
Prezentowana nazwa nie musi być jednak bezpośrednio nazwą wybranej właściwości. Istnieje coś takiego jak _data annotations_. Pozwala nam ona na większą personalizację poprzez użycie odpowiednich atrybutów w naszych modelach. Jeżeli np. chcielibyśmy aby powyższa metoda zamiast nazwy właściwości zwróciła jakiś inny tekst (np. w innym języku), to wystarczy, że w _viewmodel_ dodamy do właściwości atrybut `[DisplayName("Nasz tekst")]`, w którym zdefiniujemy nasz własny tekst, np.:
```csharp =
namespace WarehouseMVC.Web.Models
{
    public class Item
    {
        [DisplayName("Identyfikator")]
        public int Id { get; set; }
        [DisplayName("Nazwa")]
        public string Name { get; set; }
        [DisplayName("Typ")]
        public string TypeName { get; set; }
    }
}
```
DisplayName właściwości którym nie nadamy atrybutu `[DisplayName]`, dalej będzie mieć domyślną wartość nazwy parametru.
### Elementy C#
Jak już wspomnieliśmy Razor pozwala nam na użycie pewnych elementów składni C# w naszych widokach. W widoku wygenerowanym w poprzedniej lekcji użyliśmy np. pętli `@foreach`, aby przeiterować po wszystkich elementach kolekcji:
```csharp =
@foreach (var item in Model){
        <tr>
            <td>
                @Html.DisplayFor(modelItem => item.Id)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Name)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.TypeName)
            </td>
            <td>
                @Html.ActionLink("Edit", "Edit", new { /* id=item.PrimaryKey */ }) |
                @Html.ActionLink("Details", "Details", new { /* id=item.PrimaryKey */ }) |
                @Html.ActionLink("Delete", "Delete", new { /* id=item.PrimaryKey */ })
            </td>
        </tr>
}
```
Możemy też chociażby użyć warunków (`@if`, `else if`, `else` i `@switch`), np. do uzależnienia co ma zostać wyrenderowane od wartości jakiegoś parametru:
```html =
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```
Lub innych rodzajów pętli (`@for`, `@foreach`, `@while` i `@do while`). Możemy również używać deklaracji `@using`, aby upewnić się, że jakiś element zostanie usunięty z pamięci, jak również dyrektywę `@using`, która pozwoli nam odwoływać się do innych namespace'ów, albo bloków `@try`, `catch`, `finally`, żeby wyświetlić komunikat o błędzie itd. Możemy też w końcu tworzyć całe bloki kodu C#, gdzie będziemy chociażby tworzyć zmienne:
```html =
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```
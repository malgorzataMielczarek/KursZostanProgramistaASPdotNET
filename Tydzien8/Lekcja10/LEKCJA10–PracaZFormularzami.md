# [LEKCJA 10 – Praca z formularzami](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-8-od-widoku-do-modelu/lekcja-10-praca-z-formularzami/)
O formularzach mówiliśmy już trochę w poprzedniej lekcji. Formularze służą do przesyłania danych od klienta na serwer. Najczęściej używa się ich w połączeniu z metodą POST żądań http. Oznacza to, że dane nie są przesyłane jawnie w pasku adresu (jak to ma miejsce w przypadku żądań GET), ale są spakowane w obiekt i wysyłane jako tzw. **data content**. Formularze będziemy wykorzystywać zarówno do tworzenia nowych obiektów (rekordów) w naszej bazie danych, edycji już istniejących, ale również do czynności nie związanych z wprowadzaniem jakichkolwiek zmian w bazie danych, jak chociażby przedstawione w poprzedniej lekcji filtrowanie, wyszukiwanie i paginacja.

Zobaczmy więc użycie formularza na kolejnym przykładzie. Tym razem zajmiemy się dodawaniem nowego obiektu do listy. Wykorzystajmy ponownie użyty w poprzedniej lekcji przykład listy książek.
## Przykład
Uprościmy na razie nasz przypadek i podczas dodawania nowych książek pominiemy dodawanie autorów, gatunków i przynależności do serii książek, które są przechowywane w osobnych tabelach i są obiektami powiązanymi.
### 1. Viewmodel
Nasz uproszczony viewmodel dla widoku dodawania nowych książek będzie wyglądał następująco:
```csharp
namespace TitlesOrganizer.Application.ViewModels.BookVMs
{
    public class BookVM
    {
        public int Id { get; set; }
        public required string Title { get; set; }
        public string? OriginalTitle { get; set; }
        public string? OriginalLanguageCode { get; set; }
        public int? Year { get; set; }
        public string? Edition { get; set; }
        public string? Description { get; set; }
    }
}
```
### 2. Kontroler
W naszym kontrolerze ponownie będziemy potrzebować dwóch akcji.
1. Będzie odpowiadać na żądania typu GET. Zostanie ona wywołana w pierwszej kolejności, gdy użytkownik będzie chciał utworzyć nową książkę.
2. Będzie odpowiadać na żądania POST. Będzie ona wywoływana przez widok wywołany w pierwszej akcji. Widok prześle przez formularz wstępnie zwalidowane dane wprowadzone przez użytkownika.

Podobnie jak to miało miejsce w przypadku akcji `Index` z poprzedniej lekcji obie akcje będą miały taką samą nazwę, a różnić się będą argumentami (pierwsza będzie bezargumentowa, druga będzie przyjmować viewmodel `BookVM`) i atrybutami określającymi typ żądania http na jakie odpowiada (pierwsza GET, druga POST). Nazwijmy te metody `AddBook`. Będziemy więc mieli coś takiego:
```csharp
 [HttpGet]
public ActionResult AddBook()
{
    return View(new BookVM() { Title = string.Empty });
}

[HttpPost]
public ActionResult AddBook(BookVM book)
{
    int id = _bookService.AddBook(book);

    return View();
}
```
W `BookVM` mamy właściwość `OriginalLanguageCode`, która jest kluczem głównym z tabeli ze wszystkimi językami. Podczas dodawania nowej książki będziemy więc chcieli mieć rozwijaną listę z językami do wyboru. Musimy więc również przesłać do naszego widoku listę z językami (nazwa i kod języka). Ponieważ listy będziemy używać w elemencie `<select>`, więc chcemy aby była ona w postaci `IEnumerable<SelectListItem>`. Musimy jeszcze dokonać odpowiedniego rzutowania. Zrobimy to przy pomocy metody `Select` LINQ. Zrobimy to w tym wypadku w kontrolerze a nie w serwisie, gdyż klasa `SelectListItem` należy do biblioteki `Microsoft.AspNetCore.Mvc`, która jest zainstalowana tylko dla warstwy UI naszej aplikacji. Otrzymaną listę prześlemy do widoku przy pomocy ViewBag. Dopiszmy więc do pierwszej metody odpowiedni kod.
```csharp
[HttpGet]
public ActionResult AddBook()
{
    var languages = _languageService.GetAllLanguagesForList();
    ViewBag.Languages = languages.Select(lang => new SelectListItem(lang.Name, lang.Code));

    return View(new BookVM() { Title = string.Empty });
}
```
Dokładną zawartością drugiej metody na razie nie będziemy się zajmować.
### 3. Widok
Utwórzmy widok dla akcji GET. Klikamy prawym przyciskiem myszki na nazwie akcji i wybieramy _Add View..._. W nowo otwartym oknie wybieramy _Razor View_ i klikamy _Add_. _Template_ ustawiamy tym razem na _Create_ i wyszukujemy nasz viewmodel `BookVM`. Po kliknięciu _Add_ rozpocznie się generowanie nowego widoku. Będzie on wyglądał następująco:
```cshtml
@model TitlesOrganizer.Application.ViewModels.BookVMs.BookVM

@{
    ViewData["Title"] = "AddBook";
}

<h1>AddBook</h1>

<h4>BookVM</h4>
<hr />
<div class="row">
    <div class="col-md-4">
        <form asp-action="AddBook">
            <div asp-validation-summary="ModelOnly" class="text-danger"></div>
            <div class="form-group">
                <label asp-for="Id" class="control-label"></label>
                <input asp-for="Id" class="form-control" />
                <span asp-validation-for="Id" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="Title" class="control-label"></label>
                <input asp-for="Title" class="form-control" />
                <span asp-validation-for="Title" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="OriginalTitle" class="control-label"></label>
                <input asp-for="OriginalTitle" class="form-control" />
                <span asp-validation-for="OriginalTitle" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="OriginalLanguageCode" class="control-label"></label>
                <input asp-for="OriginalLanguageCode" class="form-control" />
                <span asp-validation-for="OriginalLanguageCode" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="Year" class="control-label"></label>
                <input asp-for="Year" class="form-control" />
                <span asp-validation-for="Year" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="Edition" class="control-label"></label>
                <input asp-for="Edition" class="form-control" />
                <span asp-validation-for="Edition" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="Description" class="control-label"></label>
                <input asp-for="Description" class="form-control" />
                <span asp-validation-for="Description" class="text-danger"></span>
            </div>
            <div class="form-group">
                <input type="submit" value="Create" class="btn btn-primary" />
            </div>
        </form>
    </div>
</div>

<div>
    <a asp-action="Index">Back to List</a>
</div>

@section Scripts {
    @{await Html.RenderPartialAsync("_ValidationScriptsPartial");}
}
```
Co po wyświetleniu strony da nam następujący obraz:

![Wygląd automatycznie wygenerowanego widoku w przeglądarce](Ilustracje/AutomatycznieWygenerowanyWidok.png)


```cshtml
@model TitlesOrganizer.Application.ViewModels.BookVMs.BookVM

@{
    ViewData["Title"] = "Create New Book";
}

<h1>Create New Book</h1>
<div class="row">
    <div class="col-md-4">
        <form asp-action="AddBook">
            <div asp-validation-summary="ModelOnly" class="text-danger"></div>
            <div class="form-group">
                <label asp-for="Title" class="control-label"></label>
                <input asp-for="Title" class="form-control" />
                <span asp-validation-for="Title" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="OriginalTitle" class="control-label">Original title</label>
                <input asp-for="OriginalTitle" class="form-control" />
                <span asp-validation-for="OriginalTitle" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="OriginalLanguageCode" class="control-label">Original language</label>
                <select asp-for="OriginalLanguageCode" class="form-control" asp-items="@ViewBag.Languages">
                    <option value="">Select language...</option>
                </select>
                <span asp-validation-for="OriginalLanguageCode" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="Year" class="control-label"></label>
                <input type="number" max="@DateTime.Now.Year" min="800" asp-for="Year" class="form-control" />
                <span asp-validation-for="Year" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="Edition" class="control-label"></label>
                <input asp-for="Edition" class="form-control" />
                <span asp-validation-for="Edition" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="Description" class="control-label"></label>
                <textarea asp-for="Description" class="form-control"></textarea>
                <span asp-validation-for="Description" class="text-danger"></span>
            </div>
            <div class="form-group">
                <input type="submit" value="Create" class="btn btn-primary" />
            </div>
        </form>
    </div>
</div>

<div>
    <a asp-action="Index">Back to List</a>
</div>

@section Scripts {
    <script type="text/javascript">
        var year = document.getElementById("Year");
        year.addEventListener("click", (event) => YearFirstClicked());
        year.addEventListener("focus", (event) => YearFirstClicked());

        function YearFirstClicked()
        {
            if (!year.value)
            {
                year.value = year.max;
            }
        }
    </script>
    @{
        await Html.RenderPartialAsync("_ValidationScriptsPartial");
    }
}
```
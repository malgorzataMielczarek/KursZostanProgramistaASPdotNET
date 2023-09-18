# [LEKCJA 9 – Filtrowanie, wyszukiwanie, paginacja](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-8-od-widoku-do-modelu/lekcja-9-filtrowanie-wyszukiwanie-paginacja/)
**Paginacja** - inaczej stronicowanie, a po ang. _paging_. Jest to często stosowany mechanizm dzielący duże (długie) tabele na strony. Dzięki temu na stronę musimy na raz przesłać mniejszą ilość danych. Użytkownik natomiast, żeby zobaczyć kolejne dane, zamiast używać scrolla, zmienia "stronę tabeli".

Jeżeli mamy do wyświetlenia dużą ilość danych (np. lista wszystkich klientów, produktów, książek, filmów itp.), warto, jeśli jest to możliwe, użyć mechanizmów filtrowania, paginacji i wyszukiwania. Zarówno zmniejszy to ilość danych którą musi przesłać do klienta nasz serwer, jak i korzystnie wpłynie na przejrzystość aplikacji i umożliwi użytkownikowi szybsze dotarcie do szukanych informacji.

## Przykład
Pokażmy więc na przykładzie jak zaimplementować te dodatkowe mechanizmy. Załóżmy, że dodamy filtrowanie, wyszukiwanie i paginację do podstawowej akcji `Index` naszego kontrolera, która wyświetla listę książek (tytułów).
1. Przesłanie do kontrolera dodatkowych danych.
    - Utworzenie w kontrolerze dodatkowej metody `Index`, przyjmującej trzy parametry: wielkość strony, numer aktualnie wyświetlanej strony i string do wyszukania w tytule. Parametry będziemy przesyłać przez formularz.
    - Zaznaczmy, że pierwotna (bezparametrowa) metoda `Index` będzie odpowiadała na żądania GET, a ta nowo utworzona, na żądania POST.
    - Obie metody będą musiały przekazać te dodatkowe dane do serwisu. Metoda bezparametrowa przekazuje wartości domyślne, czyli jakąś wybraną, domyślną wielkość strony (np. 10), numer strony ustawia na `1`, a wyszukiwany string na `string.Empty`.
2. Obróbka danych przez serwis.
    - Odebranie odpowiednich danych.
    - Przygotowanie ograniczonej listy (przefiltrowanej i zawierającej tylko wyniki dla odpowiedniej strony)
    - Przesłanie odpowiednich danych do viewmodelu.
3. Dodanie dodatkowych danych (wielkość i numer strony, wyszukiwany string) do viewmodelu z listą książek.
    - Teraz lista będzie zawierać tylko max. wielkość strony elementów zawierających wyszukiwany ciąg (może być mniej, jeżeli znaleziono mniej elementów lub np. jest to ostatnia strona).
    - Parametr `Count` będzie natomiast zawierał liczbę wszystkich elementów w bazie spełniających kryteria wyszukiwania. Będzie nam to potrzebne do obliczenia liczby stron tabeli.
4. Modyfikacja widoku.
    - Dodanie formularza do przesyłania danych przez POST (tag `<form></form>`).
    - Można dodać możliwość wyboru wielkości strony (liczby wyświetlanych na stronie wyników) np. w formie rozwijanej listy, tzw. dropdown listy (tag `<select></select>`, a wewnątrz niego tagi `<option></option>` z możliwymi do wyboru opcjami).
    - Dodanie pola do wpisywania wyszukiwanego stringu (tag `<input type="text" />`).
    - Dodanie przycisku uruchamiającego wyszukiwanie, czyli przesyłającego formularz do kontrolera (tag `<input type="submit" />`).
    - Dodanie linków do zmiany strony tabeli (np. tag `<a></a>`), wywołującego metodę JavaScript, którą zaraz dopiszemy.
    - Dodanie ukrytego pola do zapisywania wybranego numeru strony (tag `<input type="hidden" />`).
    - Dodanie skryptu JavaScript implementującego metodę, przypisującą wartość wybranego numeru strony do ukrytego pola i przesyłającą formularz.

Zacznijmy od początku.
### 1. Kontroler
Załóżmy, że w kontrolerze mamy metodę:
```csharp =
public ActionResult Index()
{
    ListBookForListVM list = _bookService.GetAllBooksForList();
    return View(list);
}
```
Dodajmy więc jeszcze drugą metodę o tej samej nazwie, która będzie przyjmować dane potrzebne do implementacji filtrowania i paginacji: `public ActionResult Index(int pageSize, int? pageNo, string searchString){//...}`. Dopiszmy atrybuty wskazujące jakie żądania Http ma obsługiwać dana metoda i przekażmy odpowiednie dane do serwisu. Np.:
```csharp =
[HttpGet]
public ActionResult Index()
{
    // ustawiamy wielkosc strony np. na 10, numer strony na 1, a wyszukiwany string na pusty string
    ListBookForListVM list = _bookService.GetAllBooksForList(10, 1, "");
    return View(list);
}

[HttpPost]
public ActionResult Index(int pageSize, int? pageNo, string searchString)
{
    if (!pageNo.HasValue) // sprawdzamy czy przeslano numer strony (pageNo nie jest null)
    {
        pageNo = 1; // jezeli nie przeslano, to ustawiamy sie na pierwsza strone
    }               // wrocimy do tego pozniej

    ListBookForListVM list = _bookService.GetAllBooksForList(pageSize, (int)pageNo, searchString);
    return View(list);
}
```
Możemy dodatkowo wrzucić domyślną wielkość strony do osobnej zmiennej, aby ułatwić sobie późniejsze zmiany, gdybyśmy kiedyś chcieli tą wielkość zmienić. Np. dopisujemy do klasy kontrolera dodatkowe pole `const`:
```csharp = 
private const int PAGE_SIZE = 10;

[HttpGet]
public ActionResult Index()
{
    ListBookForListVM list = _bookService.GetAllBooksForList(PAGE_SIZE, 1, "");
    return View(list);
}
```
jeżeli w danym kontrolerze będziemy wyświetlać kilka różnych takich list, np. książki, autorów, gatunki itp., ale nie chcemy mieć wspólnej wartości dla wszystkich kontrolerów. Gdybyśmy chcieli mieć stałą domyślną wielkość strony dla wszystkich kontrolerów, wówczas musielibyśmy taką zmienną wyrzucić poza klasę kontrolera, np. do osobnej klasy/struktury pomocniczej lub utworzyć nadrzędny kontroler bazowy, jeśli nasze kontrolery będą mieć więcej punktów wspólnych.
### 2. Serwis
Do metody `GetAllBooksForList` serwisu i jego interfejsu dopiszmy odpowiednie parametry (`int pageSize, int pageNo, string searchString`). Moglibyśmy teraz przesłać te dane do repozytorium i bezpośrednio tam wyszukać i pobrać odpowiednie elementy. Chcemy jednak dostać całą listę, aby wiedzieć ile jest na niej elementów (do parametru `Count` viewmodelu i określenia liczby stron tabeli). Tak na prawdę to moglibyśmy zrobić wyszukiwanie po stronie repozytorium, ale zostawmy już wszystko w serwisie. Załóżmy, że nasza metoda wyglądała wcześniej tak:
```csharp =
public ListBookForListVM GetAllBooksForList()
{
    var books = _bookRepository.GetAllBooks()
        .OrderBy(b => b.Title)
        .ProjectTo<BookForListVM>(_mapper.ConfigurationProvider)
        .ToList();

    return new ListBookForListVM()
    {
        List = books,
        Count = books.Count
    };
}
```
Dodajmy do niej implementację związaną z filtrowaniem i stronicowaniem.
1. Najpierw wyszukamy tylko te książki, których tytuły zawierają wskazany ciąg znaków.
     - Użyjemy do tego metody `Where` LINQ i metody `StartsWith`, jeśli chcemy wyszukiwać tytułów zaczynających się od wskazanego tekstu lub `Contains`, jeżeli wpisany przez użytkownika tekst ma się znajdować w dowolnym miejscu tytułu (niekoniecznie na początku).
2. Liczebność otrzymanej listy będziemy zapisywać w parametrze `Count` viewmodelu.
3. Następnie z otrzymanej listy wybieramy tylko te elementy, które mają znaleźć się na wskazanej stronie.
    - Używamy do tego metod `Skip`, do pominięcia elementów z wcześniejszych stron i `Take` do pobrania odpowiedniej liczby elementów.
4. Na koniec tworzymy obiekt viewmodelu naszej listy i umieszczamy w nim otrzymaną w poprzednim punkcie listę, wielkość listy otrzymanej w punkcie pierwszym i wszystkie informacje związane z filtrowaniem i stronicowaniem.

Po tych modyfikacjach metoda serwisu może więc wyglądać tak:
```csharp =
public ListBookForListVM GetAllBooksForList(int pageSize, int pageNo, string searchString)
{
    searchString ??= string.Empty; // zabezpieczenie przed proba wyszukiwania w tytule wartosci null

    var books = _bookRepository.GetAllBooks()
        .Where(b => b.Title.Contains(searchString)) // pobiez tylko ksiazki, ktorych tytuly zawieraja wyszukiwany string
        .OrderBy(b => b.Title)
        .ProjectTo<BookForListVM>(_mapper.ConfigurationProvider)
        .ToList();
    var limitedList = books
        .Skip(pageSize * (pageNo - 1)) // pomijamy wszystkie elementy z kazdej wczesniejszej strony,
        // czyli dla pierwszej strony nie pomijamy nic, dla drugiej tyle elementow ile miesci sie na stronie itd.
        .Take(pageSize) // biezemy kolejne pageSize elementow lub tyle ile jest, jezeli zostalo mniej
        .ToList();

    return new ListBookForListVM()
    {
        List = limitedList, // lista ksiazek do wyswietlenia na tej stronie
        Count = books.Count, // liczba wszystkich ksiazek spelniajacych podane kryterium wyszukiwania, na wszystkich stronach
        PageSize = pageSize, // wielkosc strony, liczba elementow (ksiazek) wyswietlanych na stronie
        PageNo = pageNo, // numer aktualnie wyswietlanej strony
        SearchString = searchString // parametr filtrowania, string wyszukiwany w tytulach ksiazek
    };
}
```
Nie zapomnijmy również poprawić deklaracji w interfejsie serwisu. Czyli 
```csharp
ListBookForListVM GetAllBooksForList();
```
zmieniamy na
```csharp
ListBookForListVM GetAllBooksForList(int pageSize, int pageNo, string searchString);
```
### 3. ViewModel
Dopasujmy implementację viewmodela związanego z tą akcją kontrolera, tak, aby znajdowały się w nim odpowiednie właściwości, do których przypisywaliśmy wartości w metodzie serwisu. Czyli coś takiego:
```csharp
public class ListBookForListVM
{
    public int Count { get; set; }
    public List<BookForListVM> List { get; set; }
}
```
zamieniamy na coś takiego:
```csharp
public class ListBookForListVM
{
    public int Count { get; set; }
    public List<BookForListVM> List { get; set; }
    public int PageNo { get; set; }
    public int PageSize { get; set; }
    public string SearchString { get; set; }
}
```
### 4. View
Załóżmy, że mieliśmy taki widok:
```csharp
@model TitlesOrganizer.Application.ViewModels.BookVMs.ListBookForListVM

@{
    ViewData["Title"] = "Books";
}

<h1>Books</h1>

<p class="row"><a asp-action="Create" class="col">Create New</a></p>
<div class="row">
    <table class="table">
        <thead>
            <tr>
                <th>No</th>
                <th>Title</th>
                <th></th>
            </tr>
        </thead>
        <tbody>
        @foreach (var (book, index) in Model.List.Select((value, index) => (value, index + 1)))
        {
            <tr>
                <td>@Html.DisplayFor(modelItem => index)</td>
                <td>@Html.DisplayFor(modelItem => book.Title)</td>
                <td>
                    @Html.ActionLink("Edit", "Edit", new { id = book.Id }) |
                    @Html.ActionLink("Details", "Details", new { id = book.Id }) |
                    @Html.ActionLink("Delete", "Delete", new { id = book.Id })
                </td>
            </tr>
        }
        </tbody>
    </table>
</div>
```
Dołóżmy do niego odpowiednie kontrolki do filtrowania i paginacji. Pomiędzy paragrafem z linkiem "Create New", a divem z tabelką umieścimy dropdown listę do wyboru wielkości strony (ile książek chcemy umieścić na stronie) i kontrolkę do wpisywania wyszukiwanej w tytule frazy. Dodamy tam również przycisk, umożliwiający przesłanie do kontrolera wybranych z dropdown listy i wpisanych w kontrolkę wyszukiwania danych. Poniżej tabeli z książkami umieścimy natomiast linki z numerami stron, umożliwiające nam przechodzenie między stronami tabeli. Umieścimy tam również dodatkową ukrytą kontrolkę, do której przy pomocy JavaScript będziemy wpisywać wybrany przez użytkownika numer strony.

Wszystkie kontrolki zawierające dane, które chcemy przesłać do kontrolera musimy umieścić w formularzu (gdyż jak już wspominaliśmy chcemy je wysyłać metodą POST). Ponieważ te kontrolki chcemy umieścić za równo na górze jak i na dole widoku, więc poza formularzem w tym przypadku może znaleźć się jedynie nagłówek i pierwszy paragraf widoku.

Na samym dole dokumentu dodamy jeszcze sekcję `Scripts`, w której umieścimy skrypt z naszą funkcją w JavaScript.

Zobaczmy jak mogą wyglądać wszystkie te elementy na naszym przykładzie:
```csharp
@model TitlesOrganizer.Application.ViewModels.BookVMs.ListBookForListVM

@{
    ViewData["Title"] = "Books";
}

<h1>Books</h1>

<p class="row"><a asp-action="Create" class="col">Create New</a></p>
<form asp-action="Index" asp-controller="Books" method="post">
    <div class="row">
        <select asp-for="PageSize">
            <option value="5">5 books</option>
            <option value="10">10 books</option>
            <option value="20">20 books</option>
            <option value="30">30 books</option>
            <option value="50">50 books</option>
            <option value="100">100 books</option>
        </select>
        <input type="text" asp-for="SearchString" name="searchString" id="searchString" />
        <input type="submit" value="Show" />
    </div>
    <div class="row">
        <table class="table">
            <thead>
                <tr>
                    <th>No</th>
                    <th>Title</th>
                    <th></th>
                </tr>
            </thead>
            <tbody>
            @foreach (var (book, index) in Model.List.Select((value, index) => (value, index + 1)))
            {
                <tr>
                    <td>@Html.DisplayFor(modelItem => index)</td>
                    <td>@Html.DisplayFor(modelItem => book.Title)</td>
                    <td>
                        @Html.ActionLink("Edit", "Edit", new { id = book.Id }) |
                        @Html.ActionLink("Details", "Details", new { id = book.Id }) |
                        @Html.ActionLink("Delete", "Delete", new { id = book.Id })
                    </td>
                </tr>
            }
            </tbody>
        </table>
    </div>
    <div class="row">
        @for (int i = 1; i <= Math.Ceiling(Model.Count / (double)Model.PageSize); i++)
        {
            <div class="col">
            @if (i == Model.PageNo)
            {
                <span>@i</span>
            }
            else
            {
                <a href="javascript:PagerClick(@i)">@i</a>
            }
            </div>
        }
        <input type="hidden" name="pageNo" id="pageNo"/>
    </div>
</form>
@section Scripts {
    <script type="text/javascript">
        function PagerClick(index){
            document.getElementById("pageNo").value = index;
            document.forms[0].submit();
        }
    </script>
}
```
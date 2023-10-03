# [LEKCJA 14 – Edycja obiektów](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-8-od-widoku-do-modelu/lekcja-14-edycja-obiektow/)
Edycja obiektów jest bardzo podobna do dodawania obiektów. Często nawet łączy się obie operacje, tworząc dla nich wspólny widok, viewmodel i akcję kontrolera. Jest to wówczas tzw. operacja upsert (Update-Insert).
## Operacja edycji
### Akcja kontrolera
Rozważmy na razie przypadek z osobnymi akcjami do dodawania i edycji elementów. Będą się one różnić głównie akcjami odpowiadającymi na żądania GET. Akcja edycji, w odróżnieniu od akcji dodawania będzie przyjmować jeden parametr, id edytowanego elementu. Możemy mieć więc np. coś takiego:
```csharp
[HttpGet]
public ActionResult AddBook()
{
    var languages = _languageService.GetAllLanguagesForList();
    ViewBag.Languages = languages.Select(lang => new SelectListItem(lang.Name, lang.Code));

    return View(new BookVM());
}

[HttpGet]
public ActionResult EditBook(int id)
{
    BookVM book = _bookService.GetBook(id);

    var languages = _languageService.GetAllLanguagesForList();
    ViewBag.Languages = languages.Select(lang => new SelectListItem(lang.Name, lang.Code));

    return View(book);
}
```
Akcje odpowiadające na żądanie POST będę się różnić zasadniczo wywoływaną metodą serwisu, np.
```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public ActionResult AddBook(BookVM book)
{
    if (ModelState.IsValid)
    {
        int id = _bookService.AddBook(book);

        if (id > 0)
        {
            return RedirectToAction("Details", new { id = id });
        }
    }

    return View(book);
}

[HttpPost]
[ValidateAntiForgeryToken]
public ActionResult EditBook(BookVM book)
{
    if (ModelState.IsValid)
    {
        _bookService.UpdateBook(book);
        return RedirectToAction("Details", new { id = book.Id });
    }

    return View(book);
}
```
### Widok
Obie operacje mają praktycznie taki sam widok, nie będziemy więc go ponownie omawiać (różnią się oczywiście akcją do której wysyłany jest formularz). Podczas tworzenia widoku możemy tym razem skorzystać z szablonu _Edit_, ale wygenerowany widok nie będzie się specjalnie różnić. Pamiętajmy jedynie o zmianie elementów HTML związanych z id, tak aby został jedynie `<input>`, któremu musimy ustawić typ `"hidden"`, aby uniemożliwić modyfikację id.
### Viewmodel
Zakładamy tu, że do edycji używamy tego samego viewmodelu co do dodawania nowych obiektów, `BookVM`.
### Serwis
W serwisie musimy dodać metodę `UpdateBook` i jeżeli jeszcze jaj nie mamy to `GetBook`. Oczywiście te metody musimy również umieścić w odpowiednim interfejsie.

`GetBook` może wyglądać jakoś tak:
```csharp
public BookVM GetBook(int id)
{
    Book? book = _bookRepository.GetBookById(id);
    
    if (book == null)
    {
        return new BookVM();
    }
    else
    {
        return _mapper.Map<BookVM>(book); // zakladajac, ze mamy stworzony odpowiedni mapping przy uzyciu AutoMappera
    }
}
```
`UpdateBook` może natomiast wyglądać jakoś tak:
```csharp
public void UpdateBook(BookVM bookVM)
{
    Book book = _mapper.Map<Book>(bookVM); // zakladajac, ze mamy stworzony odpowiedni mapping przy uzyciu AutoMappera
    _bookRepository.UpdateBook(book);
}

```
### Mapping
Zakładając, że mamy już stworzony mapping z typu `BookVM` na typ `Book` (prawdopodobnie stworzyliśmy go implementując dodawania nowych książek), aby stworzyć mapping w odwrotną stronę (z `Book` do `BookVM`), potrzebny do wyświetlenia aktualnych danych edytowanego elementu, wystarczy użyć metody `ReverseMap` AutoMappera. Czyli jeżeli wcześniej tworzyliśmy taką mapę `profile.CreateMap<BookVM, Book>();` to aby stworzyć mapę w przeciwną stronę, wystarczy, że na końcu tej instrukcji dopiszemy `.ReverseMap()`, czyli: `profile.CreateMap<BookVM, Book>().ReverseMap();`. Spowoduje to utworzenie mapy z typu `BookVM` na typ `Book` i z typu `Book` na `BookVM`.
### Repozytorium
W repozytorium i interfejsie warstwy domenowej muszą znaleźć się użyte powyżej metody, `GetBookById` i `UpdeteBook`.

`GetBookById` może wyglądać jakoś tak
```csharp
public Book? GetBookById(int bookId) => _context.Books
    .Include(b => b.OriginalLanguage)
    .Include(b => b.Genres)
    .Include(b => b.Authors)
    .Include(b => b.BookSeries)
    .FirstOrDefault(b => b.Id == bookId);
```
Metoda `UpdateBook` może wyglądać następująco:
```csharp
public void UpdateBook(Book book)
{
    _context.Attach(book);
    _context.Entry(book).Property(nameof(Book.Title)).IsModified = true;
    _context.Entry(book).Property(nameof(Book.OriginalTitle)).IsModified = true;
    // analogicznie dla pozostalych wlasciwosci uwzglednionych w edycji
}
```
#### Metoda `Attach`
Metoda `Attach` DbContextu powoduje rozpoczęcie śledzenia danej encji i wszystkich związanych z nią wpisów bazodanowych. Domyślnie stan śledzonej encji (`EntityState`) jest ustawiony na `Unchanged`, chyba, że jest to encja z automatycznie generowanym kluczem głównym, w której ten klucz nie został ustawiony. Wówczas ten stan jest ustawiony na `Added`. Pomaga to zagwarantować, że tylko nowe encje zostaną wstawione. Klucz jest uznawany za ustawiony, jeżeli jego wartość jest różna od wartości domyślnej danego typu. Jeżeli typ encji nie posiada generowanego klucza, to stan jest zawsze ustawiany na `Unchanged`. `EntityState` jest enumem i może przyjmować następujące wartości:
* `Added` (4) - encja jest śledzona przez kontekst, ale nie istnieje jeszcze w bazie danych,
* `Deleted` (2) - encja jest śledzona przez kontekst i istnieje w bazie danych, ale została oznaczona do usunięcia z bazy danych,
* `Detached` (0) - encja nie jest śledzona przez kontekst,
* `Modified` (3) - encja jest śledzona przez kontekst i istnieje w bazie danych, a wartości części lub wszystkich jej właściwości zostały zmienione,
* `Unchanged` (1) - encja jest śledzona przez kontekst i istnieje w bazie danych, a wartości jej właściwości nie zmieniły się względem tych, zawartych w bazie danych.

Jak w każdym przypadku, nie nastąpi żadna interakcja z bazą danych, dopóki nie zostanie wywołana metoda `SaveChanges()`.
#### Metoda `Entry`
Metoda `Entry` zwraca `EntityEntry<TEntity>` dla danej encji. Wejście (ang. _entry_) zapewnia dostęp do zmiany śledzonych informacji i operacji dla danej encji. Umożliwi nam to zmianę stanu wybranych właściwości z niezmienionego, na zmodyfikowany.
#### `Property("nazwaWlasciwosci").IsModified`
Metoda `Property` zapewnia dostęp do zmiany śledzonych informacji i operacji danej właściwości tej encji. Pozwala nam to np. ustawić flagę `IsModified` (jest zmieniona, w sensie wartość tej właściwości jest/może być zmieniona i należy tą nową wartość uwzględnić podczas zapisywania zmian w bazie danych) na `true`.
## Operacja upsert
Rozpatrzmy krótko przypadek, gdy połączymy operację dodawania i edycji w jedną.
### Akcja kontrolera
Przedstawione na początku lekcji 4 akcje kontrolera zamienimy na dwie (jedna odpowiadająca na żądania GET i jedna na żądania POST), obsługujące zarówno dodawanie nowych, jaki i modyfikację istniejących elementów.
```csharp
[HttpGet]
public ActionResult EditBook(int? id)
{
    BookVM book;
    if (id.HasValue)
    {
        book = _bookService.GetBook(id);
    }
    else
    {
        book = new BookVM();
    }
    
    var languages = _languageService.GetAllLanguagesForList();
    ViewBag.Languages = languages.Select(lang => new SelectListItem(lang.Name, lang.Code));

    return View(book);
    
}

[HttpPost]
[ValidateAntiForgeryToken]
public ActionResult EditBook(BookVM book)
{
    if (ModelState.IsValid)
    {
        int id = _bookService.UpsertBook(book);
        return RedirectToAction("Details", new { id = id });
    }

    return View(book);
}
```
### Widok
Widok będzie praktycznie ten sam. Oczywiście musimy wstawić odpowiednią nazwę akcji (`EditBook`), do której zostaną wysłane dane z formularza
### Viewmodel
Używamy tego samego viewmodelu co do tej pory (`BookVM`).
### Serwis
Zamiast metod `AddBook` i `UpdateBook` tworzymy jedną metodę `UpsertBook`.
```csharp
public int UpsertBook(BookVM bookVM)
{
    Book book = _mapper.Map<Book>(bookVM); // zakladajac, ze mamy stworzony odpowiedni mapping przy uzyciu AutoMappera
    return _bookRepository.UpsertBook(book);
}

```
### Repozytorium
W repozytorium również zamiast metod `AddBook` i `UpdateBook` tworzymy jedną metodę `UpsertBook`. Ponieważ `Book` ma automatycznie generowany klucz, więc polecenie `_context.Attach(book);`, gdzie `book` jest nową książką (z nieustawionym kluczem, `Id` równe `0`), spowoduje ustawienie stanu `EntityState` na `Added`
```csharp
public int UpsertBook(Book book)
{
    _context.Attach(book);
    if (!(_context.Entry(book).State == EntityState.Added))
    {
        _context.Entry(book).Property(nameof(Book.Title)).IsModified = true;
        _context.Entry(book).Property(nameof(Book.OriginalTitle)).IsModified = true;
        // analogicznie dla pozostalych wlasciwosci uwzglednionych w edycji
    }

    return book.Id;
}
```
# [LEKCJA 14 – Edycja obiektów](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-8-od-widoku-do-modelu/lekcja-14-edycja-obiektow/)
Edycja obiektów jest bardzo podobna do dodawania obiektów. Często nawet łączy się obie operacje, tworząc dla nich wspólny widok, viewmodel i akcję kontrolera. Jest to wówczas tzw. operacja UpSet (Update-Insert).
## Operacja edycji
### Akcja kontrolera
Rozważmy na razie przypadek z osobnymi akcjami do dodawania i edycji elementów. Będą się one różnić głównie akcjami odpowiadającymi na żądania GET. Akcja edycji, w odróżnieniu od akcji dodawania będzie przyjmować jeden parametr, id edytowanego elementu. Możemy mieć więc np. coś takiego:
```csharp
[HttpGet]
public ActionResult AddBook()
{
    return View(new BookVM() { Title = string.Empty });
}

[HttpGet]
public ActionResult EditBook(int id)
{
    BookVM book = _bookService.GetBook(id);
    return View(book);
}
```
Akcje odpowiadające na żądanie POST będę się różnić zasadniczo wywoływaną metodą serwisu, np.
```csharp
[HttpPost]
public ActionResult AddBook(BookVM book)
{
    if (ModelState.IsValid) // sprawdzamy wynik walidacji
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
public ActionResult EditBook(BookVM book)
{
    if (ModelState.IsValid) // sprawdzamy wynik walidacji
    {
        _bookService.UpdateBook(book);
        return RedirectToAction("Details", new { id = id });
    }

    return View(book);
}
```
### Widok
Obie operacje mają praktycznie taki sam widok, nie będziemy więc go ponownie omawiać (różnią się oczywiście akcją do której wysyłany jest formularz).
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
### Repozytorium
W repozytorium i interfejsie warstwy domenowej muszą znaleźć się użyte powyżej metody, `GetBookById` i `UpdeteBook`.

`GetBookById` może wyglądać jakoś tak
```csharp
public Book? GetBookById(int bookId) => _context.Books.Include(b => b.OriginalLanguage).Include(b => b.Genres).Include(b => b.Authors).Include(b => b.BookSeries).FirstOrDefault(b => b.Id == bookId);
```
Metoda `UpdateBook` może wyglądać następująco:
```csharp
public int UpdateBook(Book book)
{
    _context.Attach(book);
    _context.Entry(book).Property(nameof(Book.Title)).IsModified = true;
    _context.Entry(book).Property(nameof(Book.OriginalTitle)).IsModified = true;
    // analogicznie dla pozostalych wlasciwosci uwzglednionych w edycji

    if (_context.SaveChanges() == 1)
    {
        return book.Id;
    }

    return -1;
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

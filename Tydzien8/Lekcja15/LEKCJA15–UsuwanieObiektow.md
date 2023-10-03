# [LEKCJA 15 – Usuwanie obiektów](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-8-od-widoku-do-modelu/lekcja-15-usuwanie-obiektow/)
W widoku związanego z wyświetlaniem listy elementów (prawdopodobnie zawartym w pliku _Index.cshtml_) mieliśmy zapewne element `@Html.ActionLink("Delete", "Delete", new { id = book.Id })`. Silnik Razor tworzył na jego podstawie hiperlink (element `<a>` HTML), którego kliknięcie powodowało wywołanie akcji `Delete` kontrolera. Nie musimy więc tworzyć nowego widoku (już istnieje widok, który ją wywołuje), a jedynie zaimplementować tą akcję.
## Akcja kontrolera
W kontrolerze tworzymy akcję `Delete`. Będzie to akcja odpowiadająca na żądania GET i przyjmująca jeden parametr, id obiektu do skasowania. Musi ona wykonać dwie rzeczy
1. Wywołać odpowiednią metodę serwisu w celu usunięcia elementu o podanym id,
2. Przekierować do akcji `Index`, aby z powrotem wyświetlić listę elementów.

Np.
```csharp
[HttpGet]
public ActionResult Delete(int id)
{
    _bookService.DeleteBook(id);
    return RedirectToAction("Index");
}
```
## Serwis
W serwisie i implementowanym przez niego interfejsie musimy dodać metodę do usuwania obiektów. Musimy w niej właściwie tylko wywołać odpowiednią metodę repozytorium, która usunie element o wskazanym id z bazy danych. Np.
```csharp
public void DeleteBook(int id)
{
    _bookRepository.DeleteBook(id);
}
```
## Repozytorium
W interfejsie warstwy domenowej i implementującym go repozytorium warstwy infrastruktury musimy dodać metodę odpowiedzialną za usuwanie elementu o wskazanym id z bazy danych. Musi ona:
1. Wyszukać element o podanym id,
2. Jeżeli taki element istnieje, to go usunąć.

Np.
```csharp
public void DeleteBook(int bookId)
{
    Book? book = _context.Books.Find(bookId);
    if (book != null)
    {
        _context.Books.Remove(book);
        _context.SaveChanges();
    }
}
```
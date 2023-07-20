# [LEKCJA 10 – Routing](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-6-aplikacje-webowe-w-asp-net-core/lekcja-10-routing/)
Routing, czy li po polsku trasowanie, czyli sposób zamiany adresu na wywołanie konkretnej akcji, konkretnego kontrolera.

Standardowo routing konfigurujemy w klasie `Startup` projektu MVC. W utworzonym domyślnie szablonie na końcu tej klasy mamy:
```csharp =
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
    endpoints.MapRazorPages();
});
```
Metoda `UseEndpoints()` hosta naszej aplikacji służy właśnie do skonfigurowania tzw. endpointów do rozpoznawania adresów. Dzięki temu nasza aplikacja rozpoznaje jaka akcja jakiego kontrolera powinna zostać wywołana, w zależności od tego co zostanie wpisane (jaki adres). Najbardziej standardowy i najczęściej stosowany routing został już dla nas automatycznie utworzony. Jest to ten fragment: `endpoints.MapControllerRoute( name: "default", pattern: "{controller=Home}/{action=Index}/{id?}");` z kodu powyżej. Przyjrzyjmy mu się bliżej.
## `MapControllerRoute`
Metoda dodaje endpoint dla akcji kontrolera do buildera routingu `IEndpointRouteBilder`. Precyzuje sposób w jaki ma zostać przeprowadzone trasowanie przy pomocy następujących parametrów:
### `name`
`string name` - nazwa tego trasowania. Może być to dowolna wymyślona przez nas nazwa. W utworzonym kodzie użyto nazwy `"default"`, jest to jednak tylko nazwa i nie niesie z sobą żadnego dodatkowego znaczenia. Nazwy routingów muszą być jednak unikatowe w obrębie całej aplikacji. Jest to parametr obowiązkowy.
### `pattern`
string pattern` - wzór URL tego trasowania. Jest to parametr obowiązkowy.
### `defaults`
`[object? defaults = default]` - obiekt zawierający domyślne wartości dla parametrów routingu. Właściwości obiektu reprezentują nazwy i wartości domyślnych wartości. Jest to parametr z wartością domyślną `null`. Zamiast podawać wartości domyśle w osobnym parametrze, można to również zrobić bezpośrednio we wzorze (w parametrze `pattern`), tak jak jest to zrobione w naszym wygenerowanym kodzie.
### `constraints`
`[object? constraints = default]` - obiekt zawierający ograniczenia dla tego trasowania. Właściwości obiektu reprezentują nazwy i wartości ograniczeń. Jest to parametr z wartością domyślną `null`.<br />Możemy używać tego parametru np. do określenia typu danych jakiego powinien być dany parametr routingu. Używamy tu określonych słów kluczowych. Podobnie jak w przypadku wartości domyślnych, ograniczenia możemy zawrzeć w osobnym parametrze lub wewnątrz wzoru (w parametrze `pattern`).
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

Do jednego parametru można zastosować kilka ograniczeń, np.`{id:int:min(1)}`. Nie należy używać ograniczeń do walidacji danych wejściowych. Jeżeli to zrobimy, nieprawidłowe dane spowodują odpowiedź 404(_Not Found_ - nie znaleziono), a powinny powodować odpowiedź 400(_Bad Request_ - złe żądanie) z odpowiednią informacją o błędzie. Ograniczenia trasowania służą do ujednoznaczniania podobnych routingów, a nie do sprawdzania poprawności danych wejściowych dla określonego trasowania.
### `dataTokens`
`[object? dataTokens = default]` - obiekt zawierający tokeny danych dla tego routingu. Właściwości obiektu reprezentują nazwy i wartości tokenów danych. Jest to parametr z wartością domyślną `null`. W odróżnieniu od dwóch poprzednich parametrów, _dataTokens_ nie da się dołączyć do parametru `pattern`, jeżeli więc ich użyć, musimy przypisać wartość do tego parametru.<br />
_Data tokens_ są to dodatkowe wartości (dodatkowy obiekt) powiązane z tym konkretnym trasowaniem. Nie mają one wpływu na sam proces routingu. Zasadniczo zostały one zaprojektowane, aby powiązać dane stanu z konkretnym trasowaniem. Wartości nie są dynamiczne, nie zmieniają się więc w zależności od URL. Zamiast tego są ustalone dla danego trasowania. To znaczy, że możemy ich użyć do ustalenia, który routing został wybrany podczas trasowania. Może to być użyteczne, gdy mamy kilka możliwości, które mapują tą samą akcję i musimy wiedzieć, która z nich została wybrana (np. akcja różni się w zależności od wybranego routingu). Tego typu akcje nie powinny jednak występować. `dataTokens` raczej się więc nie używa.

Mówi nam on, że pierwszy człon, po adresie naszej strony, jest nazwą kontrolera, który chcemy użyć, kolejny to nazwa akcji do wywołania, a następnie w razie potrzeby może zostać podany parametr, który zostanie przekazany do akcji jako `id` (dokładnie taka nazwa musi zostać użyta dla parametru w definicji akcji) - ten człon jest opcjonalny (`?`). Poszczególne wartości w adresie mają być oddzielone od siebie backslashami. We wzorze uwzględniono również wartości domyślne dla kontrolera i akcji. Czyli, gdy nic nie wpiszemy, zostanie wywołana akcja `Index` kontrolera `Home`
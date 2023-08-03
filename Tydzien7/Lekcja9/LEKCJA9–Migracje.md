# [LEKCJA 9 – Migracje](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-9-migracje/)
Migracje, jest to mechanizm pozwalający na automatyczne przeniesienie struktury bazy danych stworzonej w podejściu _code first_ do silnika bazodanowego, do naszej instancji serwera, na faktyczną bazę danych.

Moglibyśmy oczywiście ręcznie stworzyć całą naszą bazę danych, przy pomocy silnika bazodanowego i odpowiednich zapytań SQL lub wyklikując odpowiednie opcje np. w Microsoft SQL Server Management Studio. Aby jednak zaoszczędzić programistą czasu i pracy stworzono właśnie migracje.

Komendy związane z migracjami znajdują się w paczce _Microsoft.EntityFrameworkCore.Tools_, którą zainstalowaliśmy w naszym projekcie na początku tego tygodnia. Będziemy ich używać w konsoli _Package Manager Console_ w Visual Studio. Konsolę otworzymy wybierając z menu _Tools_(Alt+T) -> _NuGet Package Manager_(N) -> _Package Manager Console_(O). W konsoli w _Default project:_ musimy wybrać projekt, który będzie związany z migracjami, a co za tym idzie z bazą danych (projekt, w którym znajduje się kontekst). Będzie to oczywiście nasz projekt _.Infrastructure_. Komendy będziemy wywoływać podając po prostu nazwę i po spacji ewentualne parametry. Jeśli będą to parametry obowiązkowe i pozycyjne, to wystarczy, że podamy wartość, którą chcemy im nadać. W przeciwnym razie najpierw musimy podać nazwę parametru poprzedzoną myślnikiem, a po spacji wartość. W tabeli poniżej podano parametry wspólne dla wszystkich komend EF Core.
## Wspólne parametry komend EF Core
Parametr|Opis
:--|:--
`-Context <String>`|Klasa `DbContext`, którą chcemy użyć. Tylko nazwa klasy lub pełna nazwa łącznie z nazwą przestrzeni nazw. Jeśli ten parametr jest pominięty EF Core znajduje klasę kontekstową. Jeżeli istnieje kilka kontekstów, to ten parametr jest wymagany.
`-Project <String>`|Projekt docelowy. Jeżeli ten parametr jest pominięty jako projekt docelowy zostaje użyty domyślny projekt konsoli (_Default project:_, który jest ustawiony w naszej konsoli _Package Manager Console_).
`-StartupProject <String>`|Projekt startowy. Jeśli ten parametr zostanie pominięty, jako projekt docelowy zostanie użyty projekt startowy ustawiony we właściwościach solucji.
`-Args <String>`|Argumenty przekazane do aplikacji.
`-Verbose`|Pokaż szczegółowe dane wyjściowe. Jest to opcja, którą możemy włączyć. Nie podajemy po niej żadnych wartości.

Parametry `Context`, `Project` i `StartupProject` wspierają tzw. _tab-extension_ (rozszerzenie tabulacji), czyli automatyczne wyszukiwanie odpowiedniej wartości parametru przez wciśnięcie klawisza Tab.

Żeby zobaczyć informacje pomocnicze na temat jakiejś komendy możemy użyć komendy `Get-Help` PowerShell'a. W paczce _Tools_ znajdują się takie komendy jak:
## `Add-Migration`
Służy do dodania nowej migracji. Komenda obsługuje wszystkie parametry wspólne wymienione w tabeli powyżej oraz dodatkowo parametry z tabeli poniżej. Posiada jeden obowiązkowy parametr pozycyjny, `Name`. Możemy go więc użyć baz podawania nazwy parametru. Tak więc po nazwie komendy, po spacji, podajemy nazwę jaką chcemy nadać naszej migracji, np. _InitialCreate_. 
Parametr|Opis
:--|:--
`-Name <String>`|Nazwa migracji. Jest to parametr pozycyjny i jest on wymagany.
`-OutputDir <String>`|Katalog, w którym zostanie zapisany plik z migracją. Ścieżki są względne w stosunku do katalogu projektu docelowego. Domyślnie jest to katalog _Migrations_.
`-Namespace <String>`|Przestrzeń nazw, która ma być używana dla wygenerowanych klas. Domyślnie generowane z katalogu wyjściowego (czyli np. _TitlesOrganizer.Infrastructure.Migrations_, jeżeli nie zmienialiśmy domyślnej wartości parametru `OutputDir`).
## `Bundle-Migration`
Tworzy plik wykonywalny do aktualizacji bazy danych. Poza parametrami wspólnymi, posiada jeszcze parametry wymienione w tabeli poniżej.
Parametr|Opis
:--|:--
`-Output <String>`|Ścieżka tworzonego pliku wykonywalnego.
`-Force`|Zastąp istniejące pliki.
`-SelfContained`|Zapakuj również pakiet środowiska uruchomieniowego .NET, żeby nie trzeba go było instalować na maszynie.
`-TargetRuntime <String>`|Docelowe środowisko uruchomieniowe na które ma byś utworzony plik.
`-Framework <String>`|Docelowy framework. Domyślnie, pierwszy w projekcie.
## `Drop-Database`
Usuwa bazę danych. Poza wskazanymi wyżej parametrami wspólnymi dla wszystkich komend EF Core, posiada jeszcze jeden, `WhatIf`. Żaden z parametrów nie jest obowiązkowy, ani pozycyjny.
Parametr|Opis
:--|:--
`-WhatIf`|Pokazuje, która baza danych będzie usunięta, ale bez jej faktycznego usuwania. Jeżeli przed usunięciem bazy danych, chcemy się upewnić, czy odpowiednia baza danych zostanie usunięta możemy wywołać komendę `Drop-Database -WhatIf`.
## `Get-DbContext`
Wyświetla listę i pobiera informacje o dostępnych typach `DbContext`. Posiada tylko parametry wspólne dla wszystkich metod EF Core.
## `Get-Migration`
Wyświetla listę dostępnych migracji. Poza parametrami wspólnymi posiada jeszcze wymienione w tabeli poniżej.
Parametr|Opis
:--|:--
`-Connection <String>`|Connection string do połączenia się z bazą danych. Domyślnie używany string określony w `AddDbContext` lub `OnConfiguring`.
`-NoConnect`|Nie łącz z bazą danych.
## `Optimize-DbContext`
Generuje skompilowaną wersję modelu użytego w `DbContext`. Poza wspólnymi parametrami są jeszcze:
Parametr|Opis
`-OutputDir <String>`|Katalog, w którym mają zostać umieszczone pliki. Ścieżki są relatywne do głównego katalogu projektu.
`-Namespace <String>`|Przestrzeń nazwy, w której mają się znaleźć wygenerowane klasy. Domyślnie nazwa jest generowana przez połączenie nazwy głównej przestrzeni nazw, katalogu określonego przez parametr `OutputDir` i przyrostka `CompiledModels`.
## `Remove-Migration`
Usuwa ostatnią migrację (wycofuje zmiany kodu, które zostały wykonane podczas migracji). Poza wspólnymi parametrami posiada jeszcze parametr `Force`.
Parametr|Opis
:--|:--
`-Force`|Wymuś cofnięcie migracji (wycofaj zmiany, które zostały zastosowane w bazie danych).
## `Scaffold-DbContext`
Generuje kod dla `DbContext` i typy encji dla bazy danych (o ile tabela bazodanowa ma klucz główny). Komenda może oczywiście używać parametrów wspólnych i ponadto parametrów z tabeli poniżej. Ma ona dwa wymagane parametry pozycyjne: `Connection` i `Provider`. Czyli po nazwie komendy po spacji podajemy wartość dla parametru `Connection` i znowu po spacji wartość dla parametru `Provider`.
Parametr|Opis
:--|:--
`-Connection <String>`|Connection string do bazy danych. W przypadku projektów ASP.NET Core 2.x wartość może być `"name=<nazwa connection string>"` (np. `"Name=ConnectionStrings:Blogging"`). W takim przypadku nazwa pochodzi od źródeł konfiguracji skonfigurowanych dla projektu. Jest to parametr pozycyjny i jest wymagany.
`-Provider <String>`|Dostawca do użycia. Zazwyczaj jest to nazwa pakietu NuGet, na przykład: Microsoft.EntityFrameworkCore.SqlServer. Jest to parametr pozycyjny i jest wymagany.
`-OutputDir <String>`|Katalog, w którym mają zostać umieszczone pliki klas entity. Ścieżki są względne w stosunku do katalogu projektu.
`-ContextDir <String>`|Katalog, w którym ma zostać umieszczony plik `DbContext`. Ścieżki są względne w stosunku do katalogu projektu.
`-Namespace <String>`|Przestrzeń nazw, która ma być używana dla wszystkich wygenerowanych klas. Domyślnie generowane z głównej przestrzeni nazw i katalogu wyjściowego.
`-ContextNamespace <String>`|Przestrzeń nazw, która ma być używana dla wygenerowanej klasy `DbContext`. **Uwaga!** Nadpisuje wartość parametru `Namespace`.
`-Context <String>`|Nazwa generowanej klasy `DbContext`.
`-Schemas <String[]>`|Schematy tabel i widoków, dla których mają być generowane typy entity. Jeśli ten parametr zostanie pominięty, uwzględnione zostaną wszystkie schematy. W przypadku użycia tej opcji wszystkie tabele i widoki w schematach zostaną uwzględnione w modelu, nawet jeśli nie zostały one wyraźnie uwzględnione za pomocą parametru `Table`. 
`-Tables <String[]>`|Tabele i widoki, dla których mają zostać wygenerowane typy entity. Tabele i widoki z konkretnych schematów mogą być dodane używając formatu "Schemat.Tabela" lub "Schemat.Widok". Jeżeli ten parametr jest pominięty, wszystkie tabele i widoki zostaną uwzględnione.Jeżeli chcemy wskazać kilka tabel/widoków to ich nazwy (zawarte w cudzysłowach) oddzielamy od siebie przecinkami.
`-DataAnnotations`|Użyj atrybutów do skonfigurowania modelu (tam gdzie jest to możliwe). Jeśli ten parametr jest pominięty, tylko _fluent API_ jest użyte.
`-UseDatabaseNames`|Użyj nazw tabel, widoków, sekwencji i kolumn dokładnie takich, jakie są użyte w bazie danych. Jeśli ten parametr jest pominięty nazwy z bazy danych są zmieniane, tak aby były bardziej zgodne z konwencjami stylów nazw języka C#.
`-Force`|Nadpisz istniejące pliki.
`-NoOnConfiguring`|Nie generuj `DbContext.OnConfiguring`.
`-NoPluralize`|Nie używaj `IPluralizer`. Nie zmieniaj nazw identyfikatorów (klas, parametrów itd.) odpowiednio na liczbę pojedynczą lub mnogą.
## `Script-Migration`
Generuje skrypt SQL, który zastosowuje wszystkie zmiany z jednej wybranej migracji w innej wybranej migracji. Można z nią używać wszystkich wspólnych parametrów oraz parametrów z poniższej tabeli. Komenda posiada dwa parametry pozycyjne: `From` i `To`. Można więc po nazwie komendy i spacji podać wartość dla parametru `From` i, po spacji, wartość dla parametru `To`. Będziemy używać tej komendy, gdy będziemy chcieli zrobić migrację do bazy danych, która z jakichś względów nie wspiera migracji automatycznych.
Parametr|Opis
:--|:--
`-From <String>`|Początkowa migracja. Migracje mogą być identyfikowane według nazwy lub ID. Cyfra 0 to szczególny przypadek, który oznacza przed pierwszą migracją. Domyślnie 0.
`-To <String>`|Kończąca się migracja. Domyślnie ostatnia migracja.
`-Idempotent`|Wygeneruj skrypt, którego można użyć w bazie danych podczas dowolnej migracji.
`-NoTransactions`|Nie generuj instrukcji transakcji SQL.
`-Output <String>`|Plik, w którym ma zostać zapisany wynik. JEŚLI ten parametr zostanie pominięty, plik zostanie utworzony z wygenerowaną nazwą w tym samym folderze, w którym tworzone są pliki wykonawcze aplikacji, np.: _/obj/Debug/netcoreapp2.1/ghbkztfz.sql/_.

Parametry `To`, `From` i `Output` wspierają tzw. _tab-extension_ (rozszerzenie tabulacji), czyli automatyczne wyszukiwanie odpowiedniej wartości parametru przez wciśnięcie klawisza Tab.
## `Update-Database`
Aktualizuje bazę danych do ostatniej lub określonej migracji. Poza wspólnymi parametrami posiada jeszcze dwa, z czego jeden (`Migration`) jest parametrem pozycyjnym. Możemy więc po nazwie komendy i spacji bezpośrednio podać jej wartość.
Parametr|Opis
:--|:--
`-Migration <String>`|Migracja docelowa. Migracje mogą być identyfikowane według nazwy lub ID. Cyfra 0 to szczególny przypadek, który oznacza przed pierwszą migracją i powoduje cofnięcie wszystkich migracji. Jeśli nie zostanie określona żadna migracja, komenda domyślnie przyjmuje ostatnią migrację.
`-Connection <String>`|Connection string do połączenia się z bazą danych. Domyślnie używany string określony w `AddDbContext` lub `OnConfiguring`.

Parametr `Migration` wspieraj tzw. _tab-extension_ (rozszerzenie tabulacji), czyli automatyczne wyszukiwanie odpowiedniej wartości parametru przez wciśnięcie klawisza Tab.
## `Script-DbContext`
Komenda zdefiniowana w paczce _Microsoft.EntityFrameworkCore.Design_, od której zależy paszka _Tools_. Generuje skrypt SQL z DbContext. Pomija wszelkie migracje. Poza parametrami wspólnymi, ma jeszcze parametr `Output`.
Parametr|Opis
:--|:--
`-Output <String>`|Plik, do którego ma zostać zapisany rezultat.
## Tworzenie bazy danych na podstawie kontekstu
### Budowanie migracji
Jeżeli nasz program jest w stanie się zbudować (nie ma w nim żadnych błędów) i zachowaliśmy wszystkie standardy wymagane przez EF Core (przestrzegamy standardów związanych z nazewnictwem, stworzyliśmy odpowiednie relacje itd.) to możemy stworzyć naszą pierwszą migrację, którą nazwiemy _InitialCreate_. W tym celu otwieramy _Package Manager Console_ i wpisujemy w niej komendę `Add-Migration InitialCreate`. Powinno to spowodować zbudowanie migracji (chyba, że jednak mamy błędy). Jeżeli tworzenie migracji powiodło się (w konsoli wyświetlił się komunikat _Build succeeded._), to w projekcie _.Infrastructure_ stworzył nam się nowy folder, _Migrations_. W nim znajduje się plik, ze zbudowaną przed chwilą migracją. Nazwa pliku składa się z dwóch części. Najpierw mamy datę utworzenia migracji, a następnie po podkreślniku podaną przez nas nazwę (_InitialCreate_). Jest to plik .cs. Powinien się on automatycznie otworzyć po zbudowaniu migracji. W nim możemy podejrzeć, co zostanie stworzone w naszej bazie danych. Poza tabelami, które sami zdefiniowaliśmy znajdziemy tam m.in. takie tabele jak `"AspNetRoles"`, czy `"AspNetUsers"`. Zostały one zdefiniowane przez `IdentityDbContext`, którego użyliśmy, gdyż chcemy aby autoryzacja użytkowników odbywała się przy pomocy standardowej struktury zdefiniowanej przez ASP.NET. Ogólnie, stworzona migracja powinna się bez problemu przenieść do bazy danych.
### Aktualizacja bazy danych
Po zbudowaniu migracji, możemy zaktualizować do niej naszą bazę danych. Zanim to jednak zrobimy sprawdźmy jeszcze kilka rzeczy:
1. Upewnijmy się czy migracja została stworzona.
2. Sprawdźmy, czy connection string `"DefaultConnection"` (w _appsettings.json_) wskazuje na właściwą bazę danych.
    1. Sprawdźmy, czy wskazuje na zainstalowaną przez nas w poprzedniej lekcji instancję serwera.
    2. Sprawdźmy, czy wskazuje na właściwą bazę danych. Jeżeli wkleiliśmy skopiowany z podsumowania instalacji connection string, to musimy zmienić jeszcze nazwę bazy danych. Wewnątrz stringa prawdopodobnie mamy wówczas coś takiego `"Database=master"`, czyli nasz connection string wskazuje na bazę danych o nazwie _master_. Jest to baza wewnętrzna naszej instancji serwera. Jest ona nadrzędna nad wszystkimi innymi bazami danych. Tam znajdują się m.in. nazwy baz danych znajdujących się w tej instancji i niektóre logi. Nie powinniśmy więc do niej niczego dopisywać. Musimy więc zmienić nazwę _master_ na inną, którą chcielibyśmy nadać nowotworzonej przez nas bazie danych. Może to być np. nazwa naszej solucji, np. _TitlesOrganizer_ lub jakaś inna nazwa, wskazująca na to, co znajduje się w tej bazie danych.

Po sprawdzeniu, że wszystko się zgadza, możemy wrócić do konsoli ( _Package Manager Console_) i stworzyć bazę danych. Robimy to przy pomocy komendy `Update-Database`. Spowoduje to ponowne zbudowanie naszego projektu (upewnijmy się, że mamy wybrany projekt _.Infrastructure_), aby sprawdzić, czy nie ma żadnych błędów w migracji. Po czym nastąpi aktualizacja (lub, jak w naszym przypadku, utworzenie) bazy danych. Jeżeli wszystko się uda powinniśmy zobaczyć w konsoli komunikat _Done._ Możemy teraz przejść do Microsoft SQL Server Management Studio i podejrzeć naszą bazę danych. W folderze _Databases_ powinna pojawić się nasza baza danych, ze wszystkimi zdefiniowanymi przez nas tabelami (w folderze _Tables_ naszej bazy danych).
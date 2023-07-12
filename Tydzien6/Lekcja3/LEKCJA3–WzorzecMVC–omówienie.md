# [LEKCJA 3 – Wzorzec MVC – omówienie](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-6-aplikacje-webowe-w-asp-net-core/lekcja-3-wzorzec-mvc-omowienie/)
Wzorzec MVC, czyli wzorzec _Model-View-Controller_ (Model-Widok-Kontroler), to wzorzec architektury aplikacji, zgodnie z którym dzielimy ją na trzy warstwy. Każda z nich jest niezależna, a jej działanie nie zależy od budowy pozostałych warstw. Komunikują się one ze sobą, wymieniając dane, jednak jednej warstwy nie interesują szczegóły implementacyjne innej warstwy.

Gdy spojrzymy na utworzony w poprzedniej lekcji projekt, zobaczymy, że, przy pomocy analogicznie nazwanych katalogów, zostały w nim wyszczególnione te trzy warstwy.

## Views
Widoki mają za zadanie wyłącznie zaprezentować w sposób graficzny to, o co poprosił użytkownik. Nie umieszczamy tu żadnej logiki biznesowej, nie robimy żadnych obliczeń, nie zajmujemy się sterowaniem naszą aplikacją, tylko pokazujemy graficzną reprezentację danych. Jeżeli np. użytkownik chce zobaczyć listę wszystkich produktów sprzedawanych w sklepie, to musimy stworzyć widok, który przedstawi takie produkty np. w formie tabeli lub tzw. gridu.

W przypadku ASP.NET Core MVC widoki tworzy się w plikach .cshtml. Używają one składni HTML, jednak ponadto mamy do dyspozycji silnik Razor Engine. Pozwala on nam na dynamiczne pobieranie danych przy pomocy specjalnych atrybutów. Wszystkie elementy silnika Razor, których będziemy używać w naszym kodzie, będą poprzedzone symbolem `@`. Możemy przy jego pomocy pobierać wartości parametrów, wywoływać metody C#, a nawet pisać wewnątrz pliku .cshtml instrukcje warunkowe, tworzyć zmienne lokalne, czy korzystać z innych elementów języka C#.

Widoki we wzorcu MVC charakteryzują się tym, że nie posiadają one żadnej wiedzy na temat tego co robi kontroler i za co odpowiedzialny jest model. Jego rola polega tylko i wyłącznie na prezentacji graficznej informacji o które poprosił użytkownik.
## Controllers
Kontroler jest to można powiedzieć mózg aplikacji. Odbiera on od użytkownika informacje, co ten chce zrobić. Następnie przekazuje on uzyskane informacje do modelu i w zależności od sytuacji pobiera z modelu dane do wyświetlenia i wywołuje odpowiedni widok do ich prezentacji lub przesyła do niego dane, które powinny np. zostać zapisane w bazie danych. W związku z tym kontrolery zawierają część logiki. Pośredniczą one między widokami i modelami. Odpowiadają za wywoływanie kolejnych akcji, w zależności od tego co zrobi użytkownik.

Kontrolery są to po prostu klasy C# dziedziczące po klasie `Controller`. Umieszczamy więc je oczywiście w plikach .cs.
## Models
Modele są odpowiedzialne za reprezentacje danych, z których korzysta nasza aplikacja. Przechowuje on dane i zawiera naszą logikę biznesową. Są one również odpowiedzialne za komunikację z bazą danych.

Modele to zwykłe klasy C#, więc umieszczamy je w plikach .cs.

## Przykład
Załóżmy, że użytkownik chce wyświetlić wszystkie produkty.
1. Użytkownik wciska przycisk</br>
**Widok** będzie zapewne zawierał jakiś przycisk "Wszystkie produkty", czy coś takiego, który wywoła akcję wyświetlenia wszystkich produktów.
2. Przesłanie informacji o wciśnięciu przycisku</br>
**Kontroler** otrzymuje z widoku informację, że użytkownik chce zobaczyć wszystkie produkty.
3. Wysłanie informacji do modelu</br>
**Kontroler** wysyła do modelu informację, że chce uzyskać listę wszystkich produktów.
4. Pobranie danych z bazy danych</br>
**Model** wysyła do bazy danych odpowiednie zapytanie i pobiera z niej odpowiednie dane dotyczące wszystkich produktów.
5. Przekazanie uzyskanych danych</br>
**Kontroler** otrzymuje z modelu listę wszystkich produktów.
6. Wywołanie widoku</br>
**Kontroler** wywołuje odpowiedni widok i przekazuje mu uzyskane z modelu dane.
7. Wyświetlenie danych użytkownikowi</br>
**Widok** prezentujący listę produktów zostaje wyświetlony użytkownikowi.

## Podsumowanie
Obecnie, w nowoczesnych aplikacjach internetowych raczej nie używa się czystej architektury MVC. Dalej korzystamy z zalet tego wzorca, jednak przeplata się go z innymi wzorcami, które są lepsze w zarządzaniu, mają lepszą strukturę organizacyjną dotyczącą logiki biznesowej i prezentacji danych. Należy więc znać podstawy tego wzorca, jednak w praktyce będziemy do niego luźno podchodzić. Pamiętajmy, że wzorce mają nam ułatwić pracę, a nie nas ograniczać.
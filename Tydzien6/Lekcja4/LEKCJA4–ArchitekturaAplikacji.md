# [LEKCJA 4 – Architektura aplikacji](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-6-aplikacje-webowe-w-asp-net-core/lekcja-4-architektura-aplikacji/)
Naszą aplikację webową będziemy tworzyć zgodnie z najnowszymi standardami, czyli zgodnie z tzw. _Clean architecture_ (czystą architekturą). Obecnie uważa się, że najkorzystniejszą budowę dla aplikacji webowych, szczególnie związanych z .NET daje tzw. _Onion architecture_.
## _Onion architecture_
Typ architektury, który obrazowo przedstawia się w formie koncentrycznych warstw. Idąc od środka mamy kolejno: _Domain model_, _Domain service_, _Application service_ i w zewnętrznej warstwie _User interface_, _Infrastructure_ i _Tests_.
Zgodnie z zasadami tej architektury zewnętrzną warstwę można w każdym momencie usunąć, a core aplikacji (wewnętrzne warstwy) dalej powinien działać poprawnie. W praktyce oznacza to, że teoretycznie w każdej chwili można tą warstwę wymienić na inną (chociaż coś takiego zdarza się bardzo rzadko).

![Onion architecture](Ilustracje/OnionArchitecture.png)</br>
_Rysunek 1 Onion architecture_

### _User interface_
Interfejs użytkownika, czyli frontend naszej aplikacji (warstwa prezentacji). Według onion architecture w każdej chwili możemy ją podmienić na inną, bez wpływu na pozostałe warstwy. Np. będziemy mogli podmienić frontend, który będziemy pisać w MVC, na frontend pisany w Angularze. Wymagać to będzie jedynie dopisania odpowiedniego Web API do komunikacji z nim.
### _Infrastructure_
Baza danych naszej aplikacji. Według onion architecture reszta aplikacji jest od niej całkowicie niezależna, więc w każdej chwili możemy ją podmienić na inną np. z MS SQL na PostgreSQL, MySQL lub cokolwiek innego.
### _Tests_
Testy są oczywiście tylko dodatkiem do naszej aplikacji i bez nich nasza aplikacja będzie działać dokładnie tak samo.
### _Application service_
Warstwa spinająca naszą aplikację w całość. Będzie odpowiedzialna za reagowanie na zapytania użytkownika (z _user interface_). Na ich podstawie będzie odpowiedzialna za komunikację z _domain model_ za pośrednictwem _infrastructure_. Na podstawie odpowiedzi z infrastructure będzie odpowiedzialna za wysłanie odpowiedzi do _user interface_. Jest więc łącznikiem między _domain_, a _user interface_.
### _Domain service_
Warstwa zawierająca wszystkie mechanizmy obsługujące wszelkie zasady związane z bazą danych. Będziemy tu tworzyć wszystkie tzw. rule biznesowe, czyli zasady biznesowe do tworzenia naszych modeli. Np. jakiej długości mogą być dane kolumny (ile znaków można do nich wpisać), jakiego typu danych powinniśmy użyć (np. `int`, czy `decimal`), czy dana wartość jest wymagana czy opcjonalna. Tu także zdefiniujemy połączenia pomiędzy tabelami oraz sposoby tworzenia kluczy głównych i obcych.
### _Domain model_
Warstwa opisująca nasze entity, czyli główne modele, odzwierciedlające tabele w naszej bazie danych. Składa się z klas zawierających wyłącznie właściwości. Nazwa klasy będzie odpowiadała nazwie jednej tabeli w bazie danych, a każda właściwość będzie odpowiadała jednej jej kolumnie.

## Tworzenie architektury naszej aplikacji
Zbudujmy więc architekturę naszej aplikacji. Stworzymy odpowiednie foldery i projekty. Bezpośrednio w solucji stwórzmy wymienione niżej foldery i w każdym z nich odpowiedni projekt, z nazwą wskazującą na to za co będzie on odpowiedzialny:
* Folder _Application_ - na _Application Service_
    * Projekt _Class Library (.NET Core)_ - nazwa z dopiskiem _.Application_, np.: _WarehouseMVC.Application_
        * Folder _Interfaces_ - na interfejsy do _Dependency Injection_ (o tym w lekcji 9. tego tygodnia) dla _UI_ (tak aby nasza aplikacja webowa mogła tworzyć odpowiednie zapytania do aplikacji).
        * Folder _Services_ - na serwisy
* Folder _Domain_ - na _Domain service_ i _Domain model_
    * Projekt _Class Library (.NET Core)_ - nazwa z dopiskiem _.Domain_, np.: _WarehouseMVC.Domain_
        * Folder _Interface_ - na interfejsy do naszego _Repository_
        * Folder _Model_ - na _Domain models_
* Folder _Infrastructure_ - na _Infrastructure_
    * Projekt _Class Library (.NET Core)_ - nazwa z dopiskiem _.Infrastructure_, np.: _WarehouseMVC.Infrastructure_. Ten projekt będzie się łączył z naszą bazą danych.
        * Folder _Repositories_ - na implementację interfejsów zawartych w warstwie domenowej
* Folder _Test_ - na _Tests_
    * Projekt _xUnit Test Project (.NET Core)_ - nazwa z dopiskiem _.Tests_, np.: _WarehouseMVC.Tests_
* Folder _UI_ - na _User Interface_
    * Tu przeniesiemy stworzony przez nas projekt aplikacji webowej MVC.

Z utworzonych projektów możemy od razu usunąć przykładowe klasy.
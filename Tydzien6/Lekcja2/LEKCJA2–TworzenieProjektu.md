# [LEKCJA 2 – Tworzenie projektu](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-6-aplikacje-webowe-w-asp-net-core/lekcja-2-tworzenie-projektu/)
Podobnie jak to robiliśmy na początku kursu, tworzenie nowej aplikacji musimy oczywiście rozpocząć od utworzenia nowego projektu. Tym razem nie będzie to jednak projekt typu _Console_, tylko projekt typu _Web_. Visual Studio udostępnia nam wiele szablonów projektów związanych z aplikacjami webowymi. Mamy m.in.:
* _Blazor Server App_ - szablon do utworzenia frontendu do aplikacji webowej, przy pomocy frameworka Blazor,
* _ASP.NET Core Web API_ - do utworzenia czystego backendu aplikacji,
* _ASP.NET Core Empty_ - tworzący zupełnie pusty projekt (bez żadnego szablonu) platformy ASP.NET Core,
* **_ASP.NET Core Web App (Model-View-Controller)_** - tworzący podstawę do aplikacji budowanej zgodnie z architekturą MVC, czyli takiej, jaką chcemy stworzyć,
* _Blazor WebAssembly App_ - czyli kolejny szablon dla frameworka Blazor, do tworzenia aplikacji w nieco innej technologii, niż ten pierwszy,
* _ASP.NET Core with Angular_ - do aplikacji ASP.NET Core używającej Angulara we frontendzie,
* _ASP.NET Core with React.js_ - do aplikacji ASP.NET Core używającej React.js we frotendzie.

1. Uruchamiamy Visual Studio.
2. Wybieramy _Create a new project_.
3. Szukamy projektu _ASP.NET Core Web App (Model-View-Controller)_ (nazwa może się trochę różnić, ważne żeby było MVC).</br>
Aby ułatwić sobie szukanie możemy ustawić filtry kolejno na
    * _C#_,
    * _All platforms_,
    * _Web_.
4. Po wyszukaniu projektu sprawdzamy jeszcze tagi na dole, czy mamy _C#_, _Linux_, _macOS_, _Windows_, _Web_.
5. Po upewnieniu się, że wybraliśmy właściwy projekt klikamy _Next_.
6. Nazywamy projekt.
    * _Project name_ - będzie to nazwa dla tworzonego właśnie projektu aplikacji webowej. Będzie to frontend naszej aplikacji. Warto więc od razu zaznaczyć to w nazwie, np. dodając na końcu _.Web_. Np.: _WarehouseMVC.Web_.
    * _Location_ - to oczywiście lokalizacja naszego projektu.
    * _Solution name_ - nazwa całej solucji, czyli "kontenera" aplikacji w którym umieścimy wszystkie związane z nią projekty. Np. _WarehouseMVC_.
7. Przechodzimy dalej klikając _Next_.
8. Podajemy dodatkowe informacje.
    * _Framework_ - wybieramy wersję frameworka .NET, z której będziemy korzystać tworząc nasz projekt. Tworząc nowy projekt, warto skorzystać z najnowszej dostępnej wersji.
    * _Authentication type_ - wybieramy sposób uwierzytelniania użytkowników, zarządzania nimi.
        * _None_ - standardowo wybrana opcja, oznaczająca brak zarządzania użytkownikami,
        * **_Individual Accounts_** - oznacza, że będziemy tworzyć użytkowników w naszej bazie danych. Będziemy w niej mieć specjalne table _AspNetUserRoles_, _AspNetUsers_ itd. Na nasze potrzeby wybierzemy tą właśnie opcję.
        * _Microsoft identity platform_ - wybieramy, gdy chcemy skorzystać z zewnętrznych źródeł uwierzytelniania, np. mamy skonfigurowane usługi _Azure Active Directory B2C_ (_Azure AD B2C_), _IdentityServer_ itd.
        * _Windows_ - oznacza uwierzytelnienie przy pomocy użytkowników domenowych. Jeżeli tworzymy aplikację do wewnętrznego użytku np. w firmie i mamy skonfigurowaną domenę _Active Directory_, to możemy wybrać uwierzytelnianie Windowsowe i będziemy mogli bez problemu skomunikować się z domeną.
    * _Configure for HTTPS_ - zaznaczamy czy chcemy, aby nasza aplikacja używała połączenia szyfrowanego przez protokół HTTPS. Zawsze powinniśmy zostawiać tą opcję zaznaczoną.
    * Enable Docker - zaznaczamy tą opcję jeśli będziemy chcieli skonteneryzować naszą aplikację (skorzystać z Dockera). Na razie nie będziemy z niego korzystać, więc zostawiamy tą opcję odznaczoną.
    * _Do not use top-level statements_ - omawialiśmy już tą opcję przy okazji tworzenia projektu konsolowego. Jeśli chcemy uprościć wygląd naszego pliku _Program.cs_, to możemy ją wybrać. Jak kto chce.
9. Klikamy _Create_, aby utworzyć nasz projekt.

Utworzony projekt będzie zawierał prostą, działającą stronę internetową. Plik _Program.cs_ zawiera już cały potrzebny kod. Omówimy go sobie w kolejnych lekcjach. Możemy od razu uruchomić nasz nowy projekt.

Podczas pierwszego uruchomienia programu skonfigurowanego do użycia szyfrowanego połączenia HTTPS, zostanie utworzony i podpisany certyfikat ASP.NET Core SSL. Visual Studio zapyta nas, czy chcemy zaufać temu certyfikatowi i tym samym uniknąć ostrzeżeń przeglądarki. Ponieważ tworzymy nasz projekt lokalnie, więc bez zastanowienia możemy wszystkiemu ufać. Zaznaczamy więc opcję _Don't ask me again_, aby uniknąć ponownego wyświetlania się komunikatu i klikamy _Yes_. Później wyświetli nam się ostrzeżenie o instalacji certyfikatu. Oczywiście również potwierdzamy, że chcemy go zainstalować. Ten komunikat również pokaże nam się tylko przy pierwszym uruchomieniu.

Aplikacja uruchomi nam się w przeglądarce. Na razie jest to bardzo prosta strona składająca się ze strony głównej, którą będziemy później rozwijać, strony do umieszczenia naszej polityki prywatności. Mamy również przygotowane szablony do rejestracji i logowania użytkownika.
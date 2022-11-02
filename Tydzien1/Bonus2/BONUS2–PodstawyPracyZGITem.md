# [BONUS 2 – Podstawy pracy z GITem](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-1-plan-gry/bonus-2-podstawy-pracy-z-gitem/)
W Visual Studio możemy nie tylko tworzyć repozytoria, ale również na bieżąco nimi zarządzać. Jeśli nasz projekt jest połączony z repozytorium możemy śledzić jakie zmiany wprowadziliśmy od czasu jego ostatniego zapisania w repozytorium.

 W używanej ostatnio zakładce _Git Changes_ będziemy mieli wypisane wszystkie zmienione pliki. Będą one miały odpowiednie statusy. Status można sprawdzić najeżdżając myszką na nazwę pliku lub patrząc na oznaczenie literowe znajdujące się na końcu linii.
 <ul>
 A - od ang. <i>added</i>, oznacza nowo dodany plik, czyli plik który nie istniał jeszcze podczas ostatniego zapisywania projektu w repozytorium.
 <br/>M - od ang. <i>modified</i>, oznacza zmieniony plik, czyli taki, który istnieje już w repozytorium, jednak od ostatniego zapisu zostały w nim wprowadzone zmiany.
 </ul>
 
 W _Solution Explorer_ przed nazwami plików widzimy teraz dodatkowe ikonki.
<ul>
<img alt="Niebieska zamknięta kłódka" src="https://learn.microsoft.com/en-us/previous-versions/images/ms181372.local_-2077883695_tfsc_checkedin(en-us,vs.80).gif"> oznacza, że plik jest zapisany w repozytorium i nie został zmieniony od czasów ostatniego zapisu.
<br/><img alt="Czerwona ikona zaznaczenia" src="https://learn.microsoft.com/en-us/previous-versions/images/ms181372.local_149304184_tfsc_checkedout(en-us,vs.80).gif"> oznacza, że plik istnieje już w repozytorium, ale został zmieniony od czasu ostatniego zapisu.
<br/><img alt="Zielony plus" src="https://learn.microsoft.com/en-us/visualstudio/extensibility/ux-guidelines/media/vld_c_add.png?view=vs-2017&viewFallbackFrom=vs-2022" height="12px" width="12px"> oznacza, że jest to nowy plik, który został utworzony po ostatnim zapisie projektu w repozytorium.
<br/><img alt="Czerwony minus" src="https://learn.microsoft.com/en-us/previous-versions/images/ms181372.tfsc_exldsourcectrl(en-us,vs.90).gif"> oznacza, że dany plik jest ignorowany przy zapisie projektu w repozytorium (znajduje się w pliku <i>.gitignore</i>).
</ul>

Aby zapisać zmiany w repozytorium musimy wykonać kolejno dwie czynności:
1. _Commit_ - jest to zapisanie zmian w projekcie do repozytorium lokalnego. Tworząc nowy commit możemy wybrać zmiany w jakich plikach chcemy w nim uwzględnić lub zapisać zmiany we wszystkich zmienionych plikach. Musimy również zawrzeć krótki opis wprowadzonych zmian w polu _Enter a message \<Required\>_.
2. _Push_ - jest to zapisanie zmian w repozytorium lokalnym do repozytorium zdalnego. Push zapisuje w repozytorium zdalnym wszystkie nowe (niezapisane jeszcze) commity z repozytorium lokalnego razem z ich opisami.

Możemy to zrobić przyciskając _Commit All_. Przycisk ten ma trzy opcje wykonania commita do wyboru:
* _Commit All_ - zapisuje wszystkie zmiany w śledzonych plikach projektu w repozytorium lokalnym.
* _Commit All and Push_ - zapisuje zmiany w śledzonych plikach projektu w repozytorium lokalnym, a następnie automatycznie przesyła utworzony commit do repozytorium zdalnego. Na początku tej opcji będziemy zapewne używać najczęściej.
* _Commit All and Sync_ - zapisuje wszystkie zmiany w śledzonych plikach projektu w repozytorium lokalnym i pobiera wszystkie niezapisane lokalnie zmiany z repozytorium zdalnego. Opcja ta jest przydatna szczególnie gdy pracujemy nad projektem zespołowo i chcemy sprawdzić czy ktoś nie wprowadził w między czasie żadnych zmian w projekcie. Możemy wówczas sprawdzić, czy w połączonym kodzie niema kolizji i ewentualnie naprawić je, zanim wyślemy zmiany do wspólnego repozytorium zdalnego.

Jeżeli chcemy na razie zapisać w repozytorium tylko część zmienionych plików, wówczas możemy kliknąć znak plusa znajdujący się po prawej stronie wybranych przez nas plików z listy w zakładce _Git Changes_. Spowoduje to dodanie ich do tzw. _Staged Changes_, czyli listy plików których zmiany chcemy zapisać w najbliższym commit'cie. Jeżeli wybierzemy w ten sposób jakieś pliki, wówczas we wszystkich opisanych powyżej opcjach _Commit All_ zostanie zastąpione przez _Commit Staged_ i w efekcie wykonywanego commitu zostaną zapisane tylko wybrane przez nas pliki.

Jeżeli wykonaliśmy sam commit, bez pusha, to w efekcie otrzymamy coś takiego:
![Widok górnego paska _Git Changes_ po wykonaniu 1 commita bez pusha](https://learn.microsoft.com/en-us/visualstudio/version-control/media/vs-2022/git-changes-window-outgoing-incoming.png?view=vs-2017&viewFallbackFrom=vs-2022)

Możemy teraz zrobić push klikając widoczną na rysunku powyżej ikonkę ze strzałką w górę. Oczywiście możemy odrazu zapisać na repozytorium zdalnym kilka commitów.
<br/>Na powyższej ilustracji po lewej od ikonki _push_ widzimy jeszcze dwie przydatne funkcje. Pierwsza z nich (ta bardziej po lewej) to _Fetch_ jej kliknięcie spowoduje sprawdzenie, czy mamy aktualną wersję projektu (czy na repozytorim zdalnym ktoś nie zapisał jakichś commitów, których nie mamy na swoim repozytorium lokalnym). Jeżeli _fetch_ wykryje jakieś różnice należy zrobić _pull_, używając do tego sąsiedniego przycisku. Spowoduje to pobranie z repozytorium zdalnego "dodatkowych" commitów i zapisanie ich w naszym repozytorium lokalnym.

Repozytoria mają tzw. branche (ang. _branch_ - gałąź). Główna gałąź projektu najczęściej nazywana jest _master_, jeżeli tworzymy nowe repozytorium przy pomocy Visual Studio lub _main_, jeżeli robimy to na stronie github.com. Branch jest to można powiedzieć odmiana/wariant naszego projektu. Jeżeli planujemy wprowadzić w projekcie jakieś duże zmiany, albo przetestować jakieś rozwiązanie to dobrą praktyką jest utworzenie nowej gałęzi i przeprowadzanie tych zmian właśnie w niej. Oznacza to, że od pewnego momentu nasz projekt będzie mógł składać się z różnych plików lub te same pliki będą mogły mieć różną zawartość w zależności od tego na który branchu będziemy pracować. Dzięki temu możemy mieć w jednym miejscu działający program, a w drugim swobodnie go modyfikować i ulepszać bez obaw, że coś zepsujemy. Jeżeli przeprowadzone przez nas zmiany uzmamy za sukces, możemy gałęzie połączyć i ponownie mieć tylko jedną wersję programu.

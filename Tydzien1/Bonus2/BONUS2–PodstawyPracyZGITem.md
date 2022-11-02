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
1. _Commit_ - jest to zapisanie zmian w projekcie do repozytorium lokalnego
2. _Push_ - jest to zapisanie zmian w repozytorium lokalnym do repozytorium zdalnego

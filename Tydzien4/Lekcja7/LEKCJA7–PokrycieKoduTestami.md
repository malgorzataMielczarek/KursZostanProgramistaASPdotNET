# [LEKCJA 7 – Pokrycie kodu testami](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-4-testowanie/lekcja-7-pokrycie-kodu-testami/)
## Co powinniśmy testować?
Po tym, jak mówiliśmy jakie ważne są testy, chciałoby się powiedzieć, że wszystko. Nie jest to jednak prawda. Zarówno brak testów, jak i 100% pokrycie kodu testami nie są dobre dla naszej aplikacji. Tak na prawdę powinniśmy przetestować tylko konkretne rzeczy.
<table>
<tr>
<th style="color: green;">Testujemy:</th>
<th style="color: red;">Nie testujemy (zakładamy, że są poprawne, dobrze napisane):</th>
</tr>
<tr>
<td style="color: green;">
<img src="https://www.iconsdb.com/icons/preview/green/checkmark-xxl.png" style="height:1em;" title="-"/><b>Logikę biznesową</b> naszej aplikacji
<ul>
<li>Wszelkie metody, w których manipulujemy obiektami, przekształcamy je według wymagań użytkowników (klienta)</li>
</ul><br/>
<img src="https://www.iconsdb.com/icons/preview/green/checkmark-xxl.png" style="height:1em;" title="-"/>Warstwę aplikacji związaną z <b>interakcją z użytkownikiem</b><br/>
</td>
<td style="color: red;">
  <img src="https://www.citypng.com/public/uploads/preview/transparent-handdrawn-doodle-red-x-close-icon-31631916723qjvbg7xy7u.png" style="height: 1em;" title="-"/><b>Konstruktorów</b> - gdyż:
  <ul>
  <li>Nie powinny one posiadać żadnej logiki.</li>
  <li>Najczęściej znajdują się w nich tylko proste przypisania parametrów do obiektów w danej klasie, więc nie ma tam czego testować.</li>
  <li>Jeżeli już chcielibyśmy przetestować taką metodę i istniej w niej błąd, to istnieje duże prawdopodobieństwo, że powielimy go w teście. Wówczas będziemy mieć nie tylko błędny konstruktor, ale również test.</li>
  <li>Aby zmniejszyć możliwość pomyłki, możemy do ich tworzenia używać <i>snippetów</i> (piszemy <i>ctor</i> i klikamy dwa razy <b>Tab</b>, co tworzy nam podstawowy, bezparametrowy konstruktor), a jeżeli używamy <i>Resharpera</i> to również szablonów.</li>
  </ul><br/>
  <img src="https://www.citypng.com/public/uploads/preview/transparent-handdrawn-doodle-red-x-close-icon-31631916723qjvbg7xy7u.png" style="height:1em; color: red;" title="-"/>Prostych <b>get</b>erów, <b>set</b>erów <ul><li>Z podobnych powodów co konstruktorów</li></ul><br/>
  <img src="https://www.citypng.com/public/uploads/preview/transparent-handdrawn-doodle-red-x-close-icon-31631916723qjvbg7xy7u.png" style="height:1em; display: inline;" title="-"><label style="color: red;">Warstwy aplikacji wygenerowanych przez <b>zewnętrzne biblioteki</b>
  <ul>
  <li>Np. klasy stworzone przez bibliotekę <i>Entity Framework</i> (będziemy z niej korzystać w dalszej części kursu, do połączenia z bazą danych), na podstawie bazy danych.</li>
  <li>Zakładamy, że biblioteki zewnętrzne działają prawidłowo i nie testujemy ich działania. Dla tego ważne jest, aby korzystać z zaufanych bibliotek, przetestowanych przez społeczność programistów.</li>
  </ul></label><br/>
  <img src="https://www.citypng.com/public/uploads/preview/transparent-handdrawn-doodle-red-x-close-icon-31631916723qjvbg7xy7u.png" style="height:1em; color: red;" title="-"/>Warstwy aplikacji związanej bezpośrednio z operacjami na bazie danych (klas <b>CRUD</b>-owych).
  </td>
</tr>
</table>

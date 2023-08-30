# [LEKCJA 4 – DTO](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-8-od-widoku-do-modelu/lekcja-4-dto/)
DTO, czyli _Data Transfer Object_, są to klasy odpowiadające za odwzorowanie modeli. Nie posiadają one żadnych metod, nawet konstruktorów. Zawierają wyłącznie właściwości.

W naszym projekcie możemy spotkać się z kilkoma podobnymi pojęciami. Dla porządku:
* modele (entity) - są to nasze modele domenowe, czyli modele reprezentujące dane z bazy danych
* DTO - czyli modele pośrednie, używane do przekazywania danych między warstwami aplikacji lub procesami. Zawierają wyłącznie dane (właściwości), nie metody.
* viewmodele - tradycyjnie modele dla frontendu, dla widoków naszej aplikacji. Mogą zawierać zarówno właściwości jak i metody. Wywodzą się z wzorca MVVM (_Model-View-Viewmodel_) utworzonego na potrzeby aplikacji desktopowych WPF (_Windows Presentation Foundation_), czy mobilnych Xamarin.

## Viewmodele w aplikacji webowej
Jak już wspomnieliśmy tradycyjnie, w aplikacjach desktopowych i mobilnych, viewmodele mogą zawierać zarówno właściwości (uszczuplone entity), jaki i zachowania (metody).

Aplikacje webowe również zaadaptowały pojęcie viewmodelu, jednak w nieco zmienionej formie. Tutaj są one de facto DTO. Będą służyły do wymiany danych między serwisami, a kontrolerami.

Viewmodele będą to wspomniana w poprzedniej lekcji modele, które będą zwracane przez metody naszych serwisów i wyświetlane w widokach. Będą więc one związane z widokami (stąd _view_), ale będą też w jakiś sposób reprezentować dane z bazy danych (stąd _models_). Viewmodele będziemy tworzyć praktycznie dla każdej akcji naszego kontrolera.
### Architektura
W naszej aplikacji będziemy tworzyć viewmodele w warstwie _application_. W projekcie _.Application_ utwórzmy więc nowy folder i nazwijmy go _ViewModels_.

Ponieważ viewmodeli może być bardzo dużo, warto je uporządkować, tworząc odpowiednie podfoldery. Prawdopodobnie utworzymy oddzielny folder dla każdego modułu (kontrolera), o analogicznej nazwie.
### Nazewnictwo
Nazwy viewmodeli powinny jak najdokładniej opisywać to, co mają one reprezentować. Na końcu będziemy stosować postfiks _Vm_, od _Viewmodel_. Robimy to, aby zaznaczyć, że są to viewmodele, gdyż część nazw mogłaby nam się pokrywać z nazwami modeli domenowych.
### Przykład
Mamy np. entity `Customer`i powiązane z nimi entity `Adress`, do przechowywania adresów klientów, `ContactDetail`, do przechowywania informacji teleadresowych (telefon, email) i `CustomerContactInformation` zawierający dane osoby kontaktowej danego klienta. Na ich podstawie utworzymy zapewne następujące viewmodele (które dla porządku umieścimy w podfolderze _Customer_):
1. 
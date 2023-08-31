# [LEKCJA 4 – DTO](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-8-od-widoku-do-modelu/lekcja-4-dto/)
DTO, czyli _Data Transfer Object_, są to klasy odpowiadające za odwzorowanie modeli. Nie posiadają one żadnych metod, nawet konstruktorów. Zawierają wyłącznie właściwości.

W naszym projekcie możemy spotkać się z kilkoma podobnymi pojęciami. Dla porządku:
* modele (entity) - są to nasze modele domenowe, czyli modele reprezentujące tabele z bazy danych
* DTO - czyli modele pośrednie, używane do przekazywania danych między warstwami aplikacji lub procesami. Zawierają wyłącznie dane (właściwości), nie metody.
* viewmodele - tradycyjnie modele dla frontendu, dla widoków naszej aplikacji. Mogą zawierać zarówno właściwości jak i metody. Wywodzą się z wzorca MVVM (_Model-View-Viewmodel_) utworzonego na potrzeby aplikacji desktopowych WPF (_Windows Presentation Foundation_), czy mobilnych Xamarin.

## Viewmodele w aplikacji webowej
Jak już wspomnieliśmy tradycyjnie, w aplikacjach desktopowych i mobilnych, viewmodele mogą zawierać zarówno właściwości (uszczuplone entity), jaki i zachowania (metody).

Aplikacje webowe również zaadaptowały pojęcie viewmodelu, jednak w nieco zmienionej formie. Tutaj są one de facto DTO. Będą służyły do wymiany danych między serwisami, a kontrolerami.

Viewmodele będą to wspomniana w poprzedniej lekcji modele, które będą zwracane przez metody naszych serwisów i wyświetlane w widokach. Będą więc one związane z widokami (stąd _view_), ale będą też w jakiś sposób reprezentować dane z bazy danych. Viewmodele będziemy tworzyć praktycznie dla każdej akcji naszego kontrolera.
### Architektura
W naszej aplikacji będziemy tworzyć viewmodele w warstwie _application_. W projekcie _.Application_ utwórzmy więc nowy folder i nazwijmy go _ViewModels_.

Ponieważ viewmodeli może być bardzo dużo, warto je uporządkować, tworząc odpowiednie podfoldery. Prawdopodobnie utworzymy oddzielny folder dla każdego modułu (kontrolera lub serwisu), o analogicznej nazwie.
### Nazewnictwo
Nazwy viewmodeli powinny jak najdokładniej opisywać to, co mają one reprezentować (na podstawie jakiego modelu tworzymy i gdzie będziemy ich używać). Na końcu będziemy stosować postfiks _Vm_, od _Viewmodel_. Robimy to, aby zaznaczyć, że są to viewmodele, gdyż część nazw mogłaby nam się pokrywać z nazwami modeli domenowych.
### Przykład
Mamy np. entity `Customer` i powiązane z nimi entity `Adress`, do przechowywania adresów klientów, `ContactDetail`, do przechowywania informacji teleadresowych (telefon, email) i `CustomerContactInformation` zawierający dane osoby kontaktowej danego klienta. Na ich podstawie utworzymy zapewne następujące viewmodele (które dla porządku umieścimy w podfolderze _Customer_):
1. `CustomerForListVm` - będzie zawierał model dla `Customer`a do wyświetlenia na głównej liście z klientami. Umieścimy tu za pewne nazwę, może główny adres albo NIP i na pewno również id. W prawdzie numeru identyfikacyjnego nie będziemy wyświetlać użytkownikowi, ale będzie na później potrzebny, jeśli np. z poziomu tej listy użytkownik będzie mógł wybrać klienta i wyświetlić szczegółowe informacje o nim. Wówczas w wywołaniu kolejnej akcji (do wyświetlenia szczegółowych danych klienta) będziemy musieli podać id wybranego klienta.
2. `ListCustomerForListVm` - będzie przechowywać listę klientów do wyświetlenia. Będzie zawierać tylko dwie właściwości, listę klientów i wielkość listy. Taką klasę tworzymy dla większych list, aby móc oddzielnie przechowywać ich wielkość. Właściwość z liczbą elementów na liście przyda się nam później np. do paginacji (stronicowania - podziału wyświetlanej tabeli z danymi na strony). Dzięki temu nie będziemy musieli za każdym razem odwoływać się do listy obiektów i sprawdzać ile ma elementów, co jest dość kosztowne. Oczywiście jeżeli będziemy tworzyć małe listy, które będziemy wyświetlać w całości, nie trzeba tworzyć takiej dodatkowej klasy.
```csharp =
public class ListCustomerForListVm
{
    public List<CustomerForListVm> Customers { get; set; }
    public int Count { get; set; }
}
```
3. `CustomerDetailsVm` - będzie służył do wyświetlenia szczegółowych informacji o konkretnym kliencie. Umieścimy tu wszystkie dane klienta, które powinny być dostępne dla użytkownika oraz id, np. by dać możliwość późniejszej edycji wybranego klienta. Zawarte dane nie muszą przy tym występować w tej samej formie co w naszym entity. Np. gdy w bazie danych przechowujemy imię i nazwisko prezesa w osobnych kolumnach, tu możemy chcieć je połączyć w jedną właściwość i wyświetlać razem. Możemy również dane z całego obiektu wyciągnąć do pojedynczego stringa. Np. z obiektu `CustomerContactInformation` powiązanego z naszym klientem możemy wyciągnąć stanowisko, imię i nazwisko osoby do kontaktu i skleić je w jeden `string`. Możemy również rozdzielić pewne dane. Np. numery telefonów i e-maile, które trzymamy w jednej tabeli bazodanowej możemy tu rozdzielić na dwie listy, jedną z telefonami i jedną z e-mailami.
4. `AdressForListVm` - będzie zawierał podstawowe dane adresowe do wyświetlenia w głównej tabeli. Ponieważ jeden klient nie będzie miał więcej jak trzech, czterech adresów, a adresy będziemy wyświetlać tylko w odniesieniu do konkretnego klienta, więc w tym wypadku nie musimy tworzyć dodatkowej klasy na listę (nie będziemy przeprowadzać paginacji ani filtrowania, zawsze będziemy wyświetlać wszystkie adresy danego klienta).
5. `ContactDetailListVm` - będzie reprezentować dane teleadresowe do wyświetlenia w tabeli.
6. `NewCustomerVm` - będzie zawierał dane potrzebne do utworzenia nowego klienta oraz id. W prawdzie w momencie tworzenia nowego klienta nie znamy jeszcze jego numeru identyfikacyjnego (zostanie on automatycznie przypisany przez silnik bazodanowy), ale po utworzeniu nowego klienta w bazie danych będziemy chcieli zwrócić z serwisu jego id. Najłatwiej jest to zrobić właśnie przez model.
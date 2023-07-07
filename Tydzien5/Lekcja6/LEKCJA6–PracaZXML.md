# [LEKCJA 6 – Praca z XML](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-5-praca-z-danymi/lekcja-6-praca-z-xml/)
Pliki XML (ang. _E**x**tensible **M**arkup **L**anguage_, rozszerzalny język znaczników) są jednym z najczęściej używanych formatów do przechowywania danych tekstowych. Można w nim zapisywać dane dowolnego typu. Format ten jest uważany za bezpieczny. Jest on powiązany z W3C (World Wide Web Consortium), czyli organizacją zajmującą się ustanawianiem standardów pisania i przesyłu stron WWW. Każdy plik XML powinien spełniać pewne standardy zdefiniowane przez tą organizację. Bardzo często w plikach podajemy więc użyty XSD (_XML Schema Definition_ - schemat XML). Dzięki temu łatwo jest sprawdzić poprawność danego pliku, również przy pomocy walidatorów on-line. Dodatkowo XML to jedyny format wspierany przez natywne elementy platformy .NET. Jest więc on chętnie używany przez programistów webowych, czy to do zapisywania danych, gdy nie mamy dostępu do bazy danych, czy do przekazywania danych pomiędzy różnymi serwisami. Często stosuje się go np. do komunikacji z serwisami rządowymi, czy innymi firmami, do przesyłania danych przez API (_**A**pplication **P**rogramming **I**nterface_ - Interfejs Programowania Aplikacji).

## 1. Dodanie odpowiedniego _usinga_
Klasy do obsługi plików XML znajdują się w przestrzeni nazw `System.Xml.Serialization`. Aby z nich korzystać, musimy więc do sekcji _using_ dołączyć:
```csharp =
using System.Xml.Serialization;
```
## 2. Opatrzenie wszystkich właściwości atrybutami
Aby umożliwić konwersję naszych obiektów do/z plików XML, musimy zaznaczyć w jaki sposób chcemy dokonać serializacji danych. W klasach związanych z danymi (klasach projektu _NazwaAplikacji.Domain_, w których zdefiniowaliśmy nasze _Entities_), które chcemy zapisywać do pliku XML (i/lub odczytywać z niego), każdą publiczną właściwość/pole musimy opatrzyć atrybutem. W nawiasie kwadratowym umieszczamy typ atrybutu i w nawiasie okrągłym w cudzysłowie nazwę, jaką będzie miał dany atrybut. Do wyboru mamy m.in. typy:
### `XmlAttribute`
Wskazuje, że dana publiczna właściwość (pole, parametr, zwracana wartość) jest główna dla danego obiektu i zostanie zserializowana jako atrybut XML. Czyli zostanie umieszczona w znaczniku otwierającym danego obiektu, a nie pomiędzy własnymi znacznikami. Np.:
```csharp =
[XmlAttribute("Id")]
public int Id { get; set; }
```
Co w wygenerowanym pliku XML będzie np. wyglądać
```xml =
<Item Id="1"></Item>
```
### `XmlElement`
Wskazuje, że dana publiczna właściwość (pole, parametr, zwracana wartość) zostanie zserializowana jako normalny element XML, czyli umieszczona pomiędzy własnymi znacznikami. Np.:
```csharp =
[XmlElement("Name")]
public string Name { get; set; }
```
Co w wygenerowanym pliku XML będzie np. wyglądać
```xml =
<Name>TShirt</Name>
```
### `XmlIgnore`
Wskazuje, że dana publiczna właściwość (pole) nie zostanie zserializowana. Używamy jej, gdy nie chcemy zapisywać znajdujących się tam danych w pliku. Np.:
```csharp =
[XmlIgnore]
public int CreatedById { get; set; }
```
## 3. Dodanie bezargumentowego konstruktora
Jeżeli klasa którą chcemy serializować nie posiada bezargumentowego konstruktora, to musimy go dodać. Np.:
```csharp =
public Item() {}
```
## 4. Użycie serializatora
Do serializacji (zamiany danych z obiektów na format XML) i deserializacji (zmiana danych z formatu XML na obiekt) służy klasa `XmlSerializer`. Tworząc instancję tej klasy, podajemy od razu obiekty jakiego typu będziemy za jej pomocą (de)serializować. Np.:
```csharp =
XmlSerializer xmlSerializer = new XmlSerializer(typeof(List<Item>));
```
Gdy zapisujemy w pliku dane o całej kolekcji obiektów, to zostają one umieszczone w jednym elemencie nadrzędnym (_root_). Jeżeli chcemy, to możemy dostosować właściwości tego elementu, np. nadać mu własną nazwę (inaczej znacznik będzie miał domyślną nazwę). Jeżeli serializowany obiekt może mieć wartość `null`, to powinniśmy również to zaznaczyć. Możemy to zrobić tworząc obiekt typu `XmlRootAttribute` i podając go również w konstruktorze serializatora. Np.:
```csharp =
XmlRootAttribute root = new XmlRootAttribute();
root.ElementName = "Items"; // wszystkie zserializowane elementy znajda sie pomiedzy znacznikami <Items></Items>
root.IsNullable = true; // jesli serializowany obiekt jest null, to znacznik bedzie mial atrybut xsi:nil="true"

XmlSerializer xmlSerializer = new XmlSerializer(typeof(List<Item>), root); // serializer z dostosowanym rootem
```
Jeżeli chcemy odczytać dane z pliku w którym zastosowaliśmy własny element nadrzędny, to również musimy to uwzględnić tworząc nasz serializator (musimy podać odpowiedni obiekt `XmlRootAttribute` w konstruktorze). Jeśli tego nie zrobimy wystąpi błąd o znalezieniu nierozpoznanego znacznika.
### Zapis danych
Gdy opatrzyliśmy już odpowiednie elementy klas pożądanymi atrybutami, mamy przygotowaną kolekcję obiektów (lub jeden obiekt) do zapisu i stworzyliśmy serializator, możemy przejść do zapisu danych do pliku. Posłuży nam do tego metoda `Serialize` naszego serializatora. Serializuje ona podany obiekt do dokumentu XML. Przyjmuje ona przynajmniej dwa parametry. Jednym z nich jest oczywiście obiekt (kolekcja) którą chcemy zapisać. Musimy też przekazać jej w jakiejś formie dokument XML, w którym dane chcemy umieścić. Do wyboru mamy kilka możliwości. Możemy przekazać obiekt typu `XmlWriter`, `TextWriter`, `XmlSerializationWriter` lub po prostu `Stream` z dostępem `Write`. Dla przykładu stwórzmy obiekt `StreamWriter` (implementujący klasę `TextWriter`) dla pliku _C:\Temp\items.xml_ i przekażmy go jako parametr metody:
```csharp =
using StreamWriter sw = new StreamWriter(@"C:\Temp\items.xml");
xmlSerializer.Serialize(sw, list);
```
### Odczyt danych
Jeżeli chcemy pobrać dane z istniejącego pliku XML i przekształcić je na obiekty, klasa `XmlSerializer` posiada do tego celu metodę `Deserialize`. Musimy do niej, w jakiejś formie, przekazać plik XML, z którego chcemy odczytać dane. Może być to obiekt klasy `XmlReader`, `TextReader`, `XmlSerializationReader` lub po prostu `Stream` z dostępem `Read`. Metoda zwróci nam obiekt typu `object?`. Aby więc uzyskać obiekt pożądanego typu musimy dokonać jawnej konwersji. Np.:
```csharp =
using StringReader stringReader = new StringReader(xml);
var xmlItems = (List<Item>)xmlSerializer.Deserialize(stringReader);
```
## Przykład
### Stworzenie szablonu
```csharp =
using System.Xml.Serialization;

namespace NazwaAplikacji.Domain.Entity
{
    public class Item
    {
        // glowny atrybut
        [XmlAttribute("Id")]
        public int Id { get; set; }
        
        // zwykłe wlasciwosci
        [XmlElement("Name")]
        public string Name { get; set; }
        [XmlElement("Type")]
        public int TypeId { get; set; }
        [XmlElement("Quantity")]
        public int Quantity { get; set; }

        // ignorowane wlasciwosci
        [XmlIgnore]
        public int CreatedById { get; set; }
        [XmlIgnore]
        public DateTime CreatedDateTime { get; set; }
        [XmlIgnore]
        public int? ModifiedById { get; set; }
        [XmlIgnore]
        public DateTime? ModifiedDateTime { get; set; }

        protected bool isLowInWarehouse;

        // XmlSerializer wymaga bezargumentowego konstruktora
        public Item() {}

        public Item(int id, string name, int typeId)
        {
            Name = name;
            TypeId = typeId;
            Id = id;
        }
    }
}
```
### Zapis danych do pliku
```csharp =
using NazwaAplikacji.Domain.Entity;
using System.Xml.Serialization;

namespace NazwaAplikacji.App.Concrete
{
    public class ListService
    {
        public void Create()
        {
            List<Item> list = new List<Item>();
            list.Add(new Item(1, "TShirt", 3) { Quantity = 200 });
            list.Add(new Item(2, "Jeans", 3) { Quantity = 200 });
            list.Add(new Item(1, "Apple", 2) { Quantity = 500 });
            list.Add(new Item(3, "Strawberry", 2) { Quantity = 1500 });
            list.Add(new Item(2, "Pineapple", 2) { Quantity = 50 });

            XmlRootAttribute root = new XmlRootAttribute();
            root.ElementName = "Items";
            root.IsNullable = true;
            XmlSerializer xmlSerializer = new XmlSerializer(typeof(List<Item>), root);

            using StreamWriter sw = new StreamWriter(@"C:\Temp\items.xml");
            xmlSerializer.Serialize(sw, list);
        }
    }
}
```
### Wygenerowany plik XML (_C:\Temp\items.xml_)
```xml =
<?xml version="1.0" encoding="utf-8"?>
<Items xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <Item Id="1">
    <Name>TShirt</Name>
    <Type>3</Type>
    <Quantity>200</Quantity>
  </Item>
  <Item Id="2">
    <Name>Jeans</Name>
    <Type>3</Type>
    <Quantity>200</Quantity>
  </Item>
  <Item Id="1">
    <Name>Apple</Name>
    <Type>2</Type>
    <Quantity>500</Quantity>
  </Item>
  <Item Id="3">
    <Name>Strawberry</Name>
    <Type>2</Type>
    <Quantity>1500</Quantity>
  </Item>
  <Item Id="2">
    <Name>Pineapple</Name>
    <Type>2</Type>
    <Quantity>50</Quantity>
  </Item>
</Items>
```
Jak widać znacznik `Items` ma jeszcze dodatkowe atrybuty. Są w nich wskazane schematy według których plik został utworzony (schematy W3C).

### Odczyt danych z pliku
```csharp =
using NazwaAplikacji.Domain.Entity;
using System.Xml.Serialization;

namespace NazwaAplikacji.App.Concrete
{
    public class ListService
    {
        public List<Item> Read()
        {
            XmlSerializer xmlSerializer = new XmlSerializer(typeof(List<Item>), new XmlRootAttribute("Items"));

            string xml = File.ReadAllText(@"C:\Temp\items.xml");
            
            using StringReader stringReader = new StringReader(xml);
            var xmlItems = (List<Item>)xmlSerializer.Deserialize(stringReader);

            return xmlItems;
        }
    }
}
```

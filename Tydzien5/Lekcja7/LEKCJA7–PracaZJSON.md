# [LEKCJA 7 – Praca z JSON](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-5-praca-z-danymi/lekcja-7-praca-z-json/)
JSON (_**J**ava**S**cript **O**bject **N**otation_ - notacja obiektów JavaScript) jest jednym z najbardziej popularnych formatów danych występujących w programowaniu internetowym. Jak sama nazwa wskazuje, jest on powszechnie stosowany we frameworkach JavaScriptowych. Ponieważ jest on lekki, jeśli chodzi o zajmowaną pamięć, prosty i przejrzysty w zapisie i odczycie, został więc również chętnie przyjęty przez programistów .NET. Obiekt w formacie JSON zapisuje się w nawiasach klamrowych, w konwencji klucz-wartość. Tzn. W cudzysłowie umieszczamy nazwę właściwości i po dwukropku podajemy jej wartość. Właściwości oddzielamy od siebie przecinkami. Kolekcje obiektów umieszczamy w nawiasach kwadratowych. Poszczególne obiekty w kolekcji również oddzielamy przecinkami.
## Przykład
Weźmy np. dane z poprzedniej lekcji. Zapisane w formacie JSON wyglądałyby one mniej więcej:
```json =
[
  {"Id":1,"Name":"TShirt","Type":3,"Quantity":200},
  {"Id":2,"Name":"Jeans","Type":3,"Quantity":200},
  {"Id":1,"Name":"Apple","Type":2,"Quantity":500},
  {"Id":3,"Name":"Strawberry","Type":2,"Quantity":1500},
  {"Id":2,"Name":"Pineapple","Type":2,"Quantity":50}
]
```
Dane w formacie JSON możemy zapisać w plikach .txt.
## Biblioteka Newtonsoft.Json
W odróżnieniu do formatu XML, JSON nie jest natywnie wspierany przez platformę .NET. Zamiast pisać własną implementację serializacji i deserializacji, możemy jednak skorzystać z jednej z wielu gotowych bibliotek. Najpopularniejszą z nich jest `Newtonsoft.Json`. Aby użyć jej w naszym projekcie, musimy ją najpierw do niego dodać, np. przy pomocy _NuGet Package Manager_, podobnie jak to robiliśmy już wcześniej. Szczegółową dokumentację biblioteki można znaleźć na [stronie twórców](https://www.newtonsoft.com/json/help/html/Introduction.htm). Poniżej omówimy tylko najbardziej podstawowe funkcje.
### Serializacja
Aby przekształcić nasze dane do formatu JSON możemy np. skorzystać z metody `SerializeObject`, statycznej klasy `JsonConvert`.
#### Wersja podstawowa
W najprostszej wersji, używając ustawień domyślnych, przekazujemy do metody tylko obiekt, którego dane chcemy zapisać w formacie JSON. W efekcie działania metody, otrzymamy `string` z zserializowanym obiektem. Np.
```csharp =
List<Item> list = new List<Item>();
list.Add(new Item(1, "TShirt", 3) { Quantity = 200 });
list.Add(new Item(2, "Jeans", 3) { Quantity = 200 });
list.Add(new Item(1, "Apple", 2) { Quantity = 500 });
list.Add(new Item(3, "Strawberry", 2) { Quantity = 1500 });
list.Add(new Item(2, "Pineapple", 2) { Quantity = 50 });

string output = JsonConvert.SerializeObject(list);
```
Bez żadnych dodatkowych działań, nie otrzymamy jednak takiego efektu jak w przykładzie powyżej. Metoda użyje jako wartości kluczy nazw właściwości oraz przypisze nam również domyślne wartości dla niezainicjalizowanych właściwości:
```json =
[{"Id":1,"Name":"TShirt","TypeId":3,"Quantity":200,"CreatedById":0,"CreatedDateTime":"0001-01-01T00:00:00","ModifiedById":null,"ModifiedDateTime":null},{"Id":2,"Name":"Jeans","TypeId":3,"Quantity":200,"CreatedById":0,"CreatedDateTime":"0001-01-01T00:00:00","ModifiedById":null,"ModifiedDateTime":null},{"Id":1,"Name":"Apple","TypeId":2,"Quantity":500,"CreatedById":0,"CreatedDateTime":"0001-01-01T00:00:00","ModifiedById":null,"ModifiedDateTime":null},{"Id":3,"Name":"Strawberry","TypeId":2,"Quantity":1500,"CreatedById":0,"CreatedDateTime":"0001-01-01T00:00:00","ModifiedById":null,"ModifiedDateTime":null},{"Id":2,"Name":"Pineapple","TypeId":2,"Quantity":50,"CreatedById":0,"CreatedDateTime":"0001-01-01T00:00:00","ModifiedById":null,"ModifiedDateTime":null}]
```
#### Zmiana formatowania `string`a wynikowego
Inne przeładowania funkcji umożliwiają np. sformatowanie pliku do bardziej czytelnej formy. Polecenie:
```csharp =
string output = JsonConvert.SerializeObject(list, Formatting.Indented);
```
Zmieni formatowanie `string`u wynikowego na:
```json =
[
  {
    "Id": 1,
    "Name": "TShirt",
    "TypeId": 3,
    "Quantity": 200,
    "CreatedById": 0,
    "CreatedDateTime": "0001-01-01T00:00:00",
    "ModifiedById": null,
    "ModifiedDateTime": null
  },
  {
    "Id": 2,
    "Name": "Jeans",
    "TypeId": 3,
    "Quantity": 200,
    "CreatedById": 0,
    "CreatedDateTime": "0001-01-01T00:00:00",
    "ModifiedById": null,
    "ModifiedDateTime": null
  },
  {
    "Id": 1,
    "Name": "Apple",
    "TypeId": 2,
    "Quantity": 500,
    "CreatedById": 0,
    "CreatedDateTime": "0001-01-01T00:00:00",
    "ModifiedById": null,
    "ModifiedDateTime": null
  },
  {
    "Id": 3,
    "Name": "Strawberry",
    "TypeId": 2,
    "Quantity": 1500,
    "CreatedById": 0,
    "CreatedDateTime": "0001-01-01T00:00:00",
    "ModifiedById": null,
    "ModifiedDateTime": null
  },
  {
    "Id": 2,
    "Name": "Pineapple",
    "TypeId": 2,
    "Quantity": 50,
    "CreatedById": 0,
    "CreatedDateTime": "0001-01-01T00:00:00",
    "ModifiedById": null,
    "ModifiedDateTime": null
  }
]
```
Domyślnie wybrana jest opcja `Formatting.None`, tworząca jednowierszowy wynik.
#### Zmiana sposobu serializacji
Jeżeli chcemy zmienić sposób serializowania np. jakiegoś typu danych, używamy przeładowania przyjmującego obiekt klasy `JsonSerializerSettings` lub kolekcję `JsonConverter[]`, zawierającą obiekty, które zostaną użyte do wykonania konwersji.
#### Użycie atrybutów - wybór właściwości do serializacja
Podobnie jak to było w przypadku biblioteki do serializowania obiektów do XML, biblioteka `Newtonsoft.Json` umożliwia nam również przypisanie atrybutów do poszczególnych właściwości. Jeśli więc chcielibyśmy otrzymać wynik zbliżony do pokazanego w przykładzie na początku tej lekcji, musimy zmodyfikować kod naszej klasy `Item` dodając atrybuty:
```csharp =

```
Wówczas wywołując metodę:
```csharp =
namespace NazwaAplikacji.Domain.Entity
{
    public class Item
    {
        // glowny atrybut - musi posiadac wartosc
        [JsonRequired]
        public int Id { get; set; }

        // zwykle wlasciwosci
        [JsonProperty]
        public string Name { get; set; }

        [JsonProperty("Type")]  // uzywamy w JSON skroconej nazwy
        public int TypeId { get; set; }

        [JsonProperty]
        public int Quantity { get; set; }

        // ignorowane wlasciwosci
        [JsonIgnore]
        public int CreatedById { get; set; }

        [JsonIgnore]
        public DateTime CreatedDateTime { get; set; }

        [JsonIgnore]
        public int? ModifiedById { get; set; }

        [JsonIgnore]
        public DateTime? ModifiedDateTime { get; set; }

        protected bool isLowInWarehouse;

        public Item() { }

        public Item(int id, string name, int typeId)
        {
            Name = name;
            TypeId = typeId;
            Id = id;
        }

        [JsonConstructor]
        public Item(string name)
        {
            Name = name;
            CreatedById = 1;
            CreatedDateTime = DateTime.Now;
        }
    }
}
```
Otrzymamy `string`:
```json =
[
  {
    "Id": 1,
    "Name": "TShirt",
    "Type": 3,
    "Quantity": 200
  },
  {
    "Id": 2,
    "Name": "Jeans",
    "Type": 3,
    "Quantity": 200
  },
  {
    "Id": 1,
    "Name": "Apple",
    "Type": 2,
    "Quantity": 500
  },
  {
    "Id": 3,
    "Name": "Strawberry",
    "Type": 2,
    "Quantity": 1500
  },
  {
    "Id": 2,
    "Name": "Pineapple",
    "Type": 2,
    "Quantity": 50
  }
]
```
### Deserializacja
Do deserializacji `string`u zawierającego dane w formacie JSON z powrotem do obiektu naszego typu możemy użyć metody generycznej `DeserializeObject<T>`, statycznej klasy `JsonConvert`. Jeżeli do serializacji użyliśmy własnych konwerterów lub jakichś innych własnych ustawień serializatora, musimy je podać również jako argumenty tej metody (Jeżeli użyliśmy przeładowania `SerializeObject` z obiektem typu `JsonSerializerSettings` lub `JsonConverter[]`, musimy odpowiedni obiekt przekazać również do tej funkcji). W przeciwnym razie wystarczy podać do metody `string` który chcemy zdeserializować. Wynikiem działania metody jest obiekt podanego typu `T`.

#### Atrybut `JsonConstructor`
Jeżeli przed jednym z konstruktorów klasy do której będziemy deserializować umieścimy atrybut `JsonConstructor`, to właśnie on zostanie użyty do odtworzenia obiektów. Inaczej zostanie użyty konstruktor domyślny: bezargumentowy, a jeżeli nie istnieje to pojedynczy publiczny argumentowy (gdy jest kilka, to ten, którego argumenty najbardziej pasują do posiadanych danych), a gdy również nie istnieje, to niepubliczny. Tylko jeden konstruktor w danej klasie może być opatrzony atrybutem `JsonConstructor`.

#### Przykład
Załóżmy, że chcemy zdeserializować `output` otrzymany w którymś z przykładów powyżej. Napiszemy wówczas:
```csharp =
var list = JsonConvert.DeserializeObject<List<Item>>(output);
```
Otrzymany obiekt `list`, będzie typu `List<Item>?`. Będzie on zawierał listę obiektów `Item`, o wartościach parametrów zgodnych z przechowywanymi danymi. Jeżeli użyjemy zmodyfikowanej klasy `Item` ( z przypisanymi atrybutami JSON), to wówczas wartość `CreatedById` wszystkich utworzonych obiektów wyniesie `1`, a `CreatedDateTime`, czas tworzenia danego obiektu, zgodnie z konstruktorem oznaczonym do użycia. Stanie się tak niezależnie `output` który przykładu użyjemy (nawet jeśli w danych w JSON mieliśmy przypisane inne wartości dla tych parametrów).

### Zapis do pliku
Jeżeli dane zapisane w formacie JSON chcemy zapisać do pliku, a nie używać jedynie wewnętrznie w programie (np. do wymiany danych pomiędzy warstwami aplikacji), możemy do tego użyć metody `Serialize` klasy `JsonSerializer`. Przyjmuje ona strumień do zapisu w postaci obiektu typu `TextWriter` lub `JsonWriter`, obiekt do serializacji i opcjonalnie typ tego obiektu.
* **`JsonWriter`** - Klasa abstrakcyjna biblioteki `Newtonsoft.Json`. Reprezentuje moduł zapisujący dedykowany do danych JSON. Implementuje on interfejs `IDisposable`, więc obiekty tego typu będziemy stosować w połączeniu z instrukcją `using`.
* **`JsonTextWriter`** - Klasa biblioteki `Newtonsoft.Json`, implementująca klasę `JsonWriter`. Posiada ona jednoargumentowy konstruktor przyjmujący obiekt typu `TextWriter`.

Aby dokonać serializacji obiektu do pliku musimy:
1. Utworzyć obiekt typu `TextWriter`, zapisujący dane w naszym pliku docelowym. Ponieważ klasa `TextWriter` jest abstrakcyjna, możemy więc np. utworzyć obiekt implementującej ją klasy `StreamWriter`. Jak już wspominaliśmy dane w formacie JSON zapisujemy w zwykłym pliku tekstowym (pliku z rozszerzeniem .txt).
```csharp =
using StreamWriter sw = File.CreateText(@"C:\Temp\items.txt");
```
2. Moglibyśmy teraz przekazać utworzony w poprzednim punkcie obiekt do serializatora. Skorzystajmy jednak z writera dedykowanego do zapisu danych JSON. Zmieńmy również formatowanie tworzonego JSONa, na bardziej czytelne.
```csharp =
using JsonWriter writer = new JsonTextWriter(sw) { Formatting = Formatting.Indented };
```
3. Mając gotowy writer dla naszego pliku, możemy utworzyć serializer i dokonać serializacji naszego obiektu, zapisując wynik w pliku _C:\Temp\items.txt_.
```csharp =
JsonSerializer serializer = new JsonSerializer();
serializer.Serialize(writer, list); // list to nasz serializowany obiekt, ktory wczesniej stworzylismy
```

I gotowe. Oczywiście możemy również utworzyć obiekt `string` z zapisanymi danymi w formacie JSON, jak to robiliśmy na początku lekcji i potem zapisać jego treść do pliku jednym ze sposobów poznanych w lekcji 5. tego tygodnia.

### Odczyt z pliku
Analogicznie do zapisu możemy odczytywać dane JSON z pliku i deserializować je do obiektu odpowiedniego typu. Tym razem będziemy oczywiście, zamiast writerów, używać readerów. Metoda do deserializacji, podobnie jak ta użyta wcześniej, występuje również w wersji generycznej. Żeby odczytać zapisane przed chwilą dane możemy napisać np.:
```csharp =
using StreamReader sr = File.OpenText(@"C:\Temp\items.txt");
using JsonReader reader = new JsonTextReader(sr);
JsonSerializer serializer = new JsonSerializer();
var list2 = serializer.Deserialize<List<Item>>(reader);
```
Oczywiście możemy również odczytać tekst z pliku tak jak to robiliśmy w lekcji 5. tego tygodnia i później dokonać deserializacji tak jak to robiliśmy wcześniej w tej lekcji.
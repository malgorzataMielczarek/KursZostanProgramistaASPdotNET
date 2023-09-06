# [LEKCJA 6 – Automapper](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-8-od-widoku-do-modelu/lekcja-6-automapper/)
## Mapowanie
Kiedy w poprzedniej lekcji implementowaliśmy serwisy przepisywaliśmy tam m.in.dane z obiektu modelu domenowego na obiekt viewmodelu. Takie przepisywanie danych z właściwości jednego typu na właściwości innego typu nazywamy mapowaniem (lub projekcją) jednego typu na drugi.

Jak mogliśmy zauważyć wymaga to napisania dość dużej ilości na ogół prostego i schematycznego kodu. Napisanie mapowań dla wszystkich viewmodeli zajmuje więc sporo czasu, zwłaszcza dla bardziej rozbudowanych baz danych. Dodatkowo implementując projekcję bezpośrednio w metodach serwisowych zmniejszamy czytelność naszego kodu. Zasadnicza logika biznesowa ginie bowiem w ogromie przypisań typu `vm.Id = model.Id;`.

Z tego powodu powstały biblioteki zdejmujące z programisty konieczność pisania bezmyślnego kodu i automatyzujące proces mapowania. Najbardziej popularną jest _AutoMapper_.
## AutoMapper
[AutoMapper](automapper.org) to niewielka biblioteka służąca do zautomatyzowania procesu mapowania jednego typu na drugi ([dokumentacja](https://docs.automapper.org/en/latest/)). Najlepiej sprawdza się w prostych mapowaniach między modelami domenowymi a viewmodelami, w których przepisujemy wszystkie lub wybrane właściwości jeden do jeden. AutoMapper pozwala również na dodanie dodatkowej logiki. Jeżeli jednak musimy ręcznie dopisywać mapowania zbyt wielu właściwości, to użycie dla tego przypadku automappera mija się z celem. W takim wypadku lepiej napisać własną implementację. Można również połączyć oba podejścia i użyć biblioteki do najprostszych mapowań. Można kazać jej zignorować te właściwości o bardziej skomplikowanej logice projekcji i zaimplementować je osobno, po podstawowym (automatycznym) mapowaniu.
## Instalacja AutoMappera
AutoMapper występuje m.in. w formie paczki NuGet. Otwórzmy więc nasz _NuGet Package Manager_ i zainstalujmy odpowiednie paczki dla projektu _.Application_. Będziemy instalować dwie paczki:
* AutoMapper - czyli podstawowa paczka AutoMappera,
* AutoMapper.Extensions.Microsoft.DependencyInjection - paczka rozszerzająca z metodami do konfiguracji AutoMappera przy pomocy wstrzykiwania zależności (czym zajmiemy się w kolejnej lekcji).
## Użycie AutoMappera
### Organizacja projektu
W naszym projekcie _.Application_ stwórzmy dodatkowy folder, _Mapping_. Umieścimy w nim pliki potrzebna nam do wykonywania mapowań przy pomocy tej biblioteki. Jeden z nich będzie zawierał interfejs generyczny `IMapFrom<T>`, a drugi klasę pomocniczą `MappingProfile`.
### `IMapFrom<T>`
Nasz interfejs będzie wyglądał mniej więcej tak:
```csharp =
using AutoMapper;

namespace TitlesOrganizer.Application.Mapping
{
    public interface IMapFrom<T>
    {
        void Mapping(Profile profile) => profile.CreateMap(typeof(T), GetType());
    }
}
```
Każda klasa implementująca ten interfejs będzie musiała posiadać metodę `Mapping`, która w podstawowej implementacji będzie po prostu wywoływać metodę `CreateMap` obiektu klasy `Profile`. `Profile` jest wewnętrzną klasą AutoMappera.
### `MappingProfile`
`MappingProfile` to nasza klasa, która będzie dziedziczyć po klasie `Profile` AutoMappera. Jej implementacja będzie wyglądać mniej więcej tak:
```csharp =
using AutoMapper;
using System.Reflection;

namespace TitlesOrganizer.Application.Mapping
{
    public class MappingProfile : Profile
    {
        public MappingProfile()
        {
            ApplyMappingsFromAssembly(Assembly.GetExecutingAssembly());
        }

        private void ApplyMappingsFromAssembly(Assembly assembly)
        {
            var types = assembly.GetExportedTypes()
                .Where(t => t.GetInterfaces().Any(i => i.IsGenericType && i.GetGenericTypeDefinition() == typeof(IMapFrom<>)))
                .ToList();

            foreach (var type in types)
            {
                var instance = Activator.CreateInstance(type);
                var methodInfo = type.GetMethod("Mapping");
                methodInfo?.Invoke(instance, new object[] { this });
            }
        }
    }
}
```
W konstruktorze tej klasy będziemy wywoływać jej prywatną metodę `ApplyMappingsFromAssembly`.

**Assembly** jest to tak jakby nasz program. Pozwala nam na odwoływanie się bezpośrednio do kodu programu (tzw. refleksja, np. uzyskiwanie informacji o typie w czasie wykonywania programu), a nie jego skompilowanej wersji. Metoda statyczna `Assembly.GetExecutingAssembly` zwróci nam zestaw zawierający aktualnie wykonywany kod.

W naszej prywatnej metodzie najpierw szukamy interesujących nas typów danych zdefiniowanych w tym zestawie. Metoda `GetExportedTypes` zwróci nam tablicę z typami zdefiniowanymi w tym assembly, które są widoczne poza zestawem. Następnie filtrujemy otrzymaną tablicę w poszukiwaniu typów implementujących lub dziedziczących po interfejsie generycznym `IMapFrom<T>`. Zmieniamy tablicę na listę.<br />
Na każdym typie z utworzonej listy będziemy wykonywać następujące działania:
1. Tworzymy instancję (obiekt) tego typu.
2. Wyszukujemy w tym typie metody `Mapping`. (Interfejs `IMapFrom<T>` gwarantuje nam, że wszystkie implementujące go klasy będą posiadać tą metodę.)
3. Wywołujemy wyszukaną metodę `Mapping` utworzonej instancji. Przekazujemy jako argument metody aktualny obiekt `MappingProfile` (metoda `Mapping` przyjmowała jeden argument typu `Profile`).

Tworząc obiekt `MappingProfile` będziemy więc wywoływać metodę `Mapping` dla wszystkich klas implementujących utworzony przez nas interfejs `IMapFrom<T>`, a tym samym tworzyć wszystkie mappingi.

Stwórzmy więc jakieś przykładowe mapowanie.
### Przykład
Weźmy metodę z poprzedniej lekcji:
```csharp =
public ListCustomerForListVm GetAllCustomersForList()
{
    var customers = _customerRepo.GetAllActiveCustomers();
    ListCustomerForListVm result = new ListCustomerForListVm();
    result.Customers = new List<CustomerForListVm>();
    
    foreach (var customer in customers)
    {
        CustomerForListVm customVm = new CustomerForListVm()
        {
            Id = customer.Id,
            Name = customer.Name,
            NIP = customer.NIP,
            CEO = customer.CEOName + " " + customer.CEOLastName
        };

        customVm.MainCountry = customer.Addresses.FirstOrDefault()?.Country ?? string.Empty;

        result.Customers.Add(customVm);
    }

    result.Count = result.Customers.Count;

    return result;
}
```
Teraz przekształćmy wykonane tu mapowanie na takie, z wykorzystaniem AutoMappera.
1. Chcemy wykonać mapowanie z typu `Customer` na typ `CustomerForListVm`.
2. Przekształćmy klasę `CustomerForListVm` tak, aby implementowała ona interfejs `IMapFrom<T>`.
    1. Dopiszmy informację o implementowanym interfejsie do nagłówka klasy.
        * `T` będzie to typ z jakiego będziemy chcieli wykonać mapowanie, czyli w naszym przypadku model `Customer`.
    2. Dopiszmy implementację metody `Mapping`.
        * Jako parametr będziemy tu przekazywać odpowiedni profil, do którego dopiszemy tworzoną przez nas mapę. Będziemy to robić przy pomocy wspomnianej już wcześniej metody generycznej `CreateMap<TSrc, TDest>` profilu. Podajemy w nawiasach klamrowych dwa typy. Pierwszy jest typem źródłowym (typem z jakiego mapujemy), a drugi docelowym (typem na jaki mapujemy).
        * Mapowanie właściwości `Id`, `Name` i `NIP` z powyższego kodu AutoMapper wykona za nas automatycznie, gdyż są to proste przepisania z `Id` na `Id`, z `Name` na `Name` i z `NIP` na `NIP`.<br />
        Jeżeli w obu typach znajdują się właściwości o takich samych nazwach AutoMapper automatycznie będzie przepisywał pomiędzy nimi wartości, chyba, że zdefiniujemy inaczej.
        * Musimy więc jedynie dopisać informację co chcemy zmapować na właściwości `CEO` i `MainCountry`. Użyjemy do tego metody `ForMember`.
            1. Jako pierwszy jej argument musimy wskazać właściwość typu na który mapujemy, dla którego chcemy wykonać jakąś akcję, czyli nasze `CEO`, a w drugim wywołaniu metody `MainCountry`. Robimy to przy pomocy wyrażenia LINQ, czyli np. `dest => dest.CEO`.
            2. W drugim parametrze wskazujemy jaką akcję chcemy wykonać na wskazanej właściwości.
                1. `CEO` chcemy zmapować. Użyjemy do tego metody `MapFrom` np. `opt => opt.MapFrom(...)`. Jako argument tej metody piszemy wyrażenie mapujące, jako wyrażenie LINQ, czyli np. `src => src.CEOName + " " + src.CEOLastName`.
                2. `MainCountry` załóżmy, że chcemy na razie, aby zostało zignorowane przez AutoMapper, a przypisaniem mu wartości zajmiemy się później (poza mapowaniem). Do tego celu posłuży nam bezargumentowa metoda `Ignore`, czyli np. `opt => opt.Ignore()`
        * Całość implementacji może w naszym przypadku wyglądać mniej więcej tak:
        ```csharp =
        public void Mapping(Profile profile)
        {
            profile.CreateMap<CustomerForListVm, Customer>()
                .ForMember(dest => dest.CEO, opt => opt.MapFrom(src => src.CEOName + " " + src.CEOLastName))
                .ForMember(dest => dest.MainCountry, opt => opt.Ignore());
        }
        ```
3. Użyjmy stworzone mapowanie w metodzie naszej klasy serwisowej.
    1. Aby używać mapowania musimy mieć do dyspozycji obiekt `IMapper`. Stwórzmy więc w naszym serwisie prywatne pole `private readonly IMapper _mapper;`. (W kolejnej lekcji zajmiemy się nadawaniem mu wartości przy pomocy mechanizmu wstrzykiwania zależności.)
    2. Wykonajmy nasze przykładowe mapowanie. Służą do tego dwie metody generyczne `IMappera`. Jako typ będziemy w nich podawać klasę, na jaką ma zostać zmapowany podany obiekt.
        1. `Map<TDest>` służy do mapowania pojedynczego obiektu. Jako parametr tej metody podajemy obiekt do zmapowania. Np.: `CustomerForListVm customVm = _mapper.Map<CustomerForListVm>(customer);`.
        2. `ProjectTo<TDest>` jest to metoda rozszerzająca interfejs `IQueryable` i służy do mapowania elementów kolekcji, np. wewnątrz zapytań linku. Jako parametr tej metody musimy podać obiekt `IConfigurationProvider`. Np. `result.Customers = customers.ProjectTo<CustomerForListVm>(_mapper.ConfigurationProvider).ToList();`
    Nasza metoda serwisowa może więc teraz wyglądać np. tak:
    ```csharp =
    // Z uzyciem metody Map
    public ListCustomerForListVm GetAllCustomersForList()
    {
        var customers = _customerRepo.GetAllActiveCustomers();
        ListCustomerForListVm result = new ListCustomerForListVm();
        result.Customers = new List<CustomerForListVm>();
        
        foreach (var customer in customers)
        {
            CustomerForListVm customVm = _mapper.Map<CustomerForListVm>(customer);
            customVm.MainCountry = customer.Addresses.FirstOrDefault()?.Country ?? string.Empty;

            result.Customers.Add(customVm);
        }

        result.Count = result.Customers.Count;

        return result;
    }
    ```
    lub tak:
    ```csharp =
    // Z uzyciem metody ProjectTo
    public ListCustomerForListVm GetAllCustomersForList()
    {
        var customers = _customerRepo.GetAllActiveCustomers();
        ListCustomerForListVm result = new ListCustomerForListVm();
        result.Customers = customers.ProjectTo<CustomerForListVm>(_mapper.ConfigurationProvider).ToList();
        result.Count = result.Customers.Count;
        
        for (int i = 0; i < result.Count; i++)
        {
            result.Customers[i].MainCountry = customers[i].Addresses.FirstOrDefault()?.Country ?? string.Empty;
        }
        return result;
    }
    ```
    Gdybyśmy do stworzonego w klasie `CustomerForListVm` mapowania dopisali jeszcze mapowanie dla właściwości `MainCountry`, to moglibyśmy całkowicie pozbyć się pętli z powyższego kodu i pomocniczej zmiennej `customers`. Moglibyśmy bowiem napisać bezpośrednio `result.Customers = _customerRepo.GetAllActiveCustomers().ProjectTo<CustomerForListVm>(_mapper.ConfigurationProvider).ToList();`.
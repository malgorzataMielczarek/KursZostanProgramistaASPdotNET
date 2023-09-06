# [LEKCJA 5 – Implementacja serwisu](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-8-od-widoku-do-modelu/lekcja-5-implementacja-serwisu/)
Zaimplementujmy nasze serwisy. W projekcie _.Application_ tworzymy interfejsy serwisów (w folderze _Interfaces_) i ich implementacje (w folderze _Services_). Umieścimy tu metody, które będą wywoływać nasze kontrolery. Klasy serwisowe nazywamy dopisując w postfiksie _Service_, a w przypadku interfejsu jeszcze dodatkowo prefiks _I_ (jak **I**nterface).
## Referencje do repozytoriów
Poza metodami w serwisie musimy jeszcze umieścić odniesienie do odpowiednich repozytoriów, których będziemy używać w tym serwisie. Jak już wspominaliśmy przy okazji omawiania czystej architektury, nie chcemy aby nasze serwisy były zależne od infrastruktury bazy danych. Dlatego, zamiast implementacji repozytoriów, użyjemy ich interfejsów zdefiniowanych w warstwie domenowej. Tworzymy więc pola `private readonly` typu odpowiednich interfejsów. Na razie nie zajmujemy się tym, jak zostaną one przekazane do serwisów. Omówimy to w dalszej części kursu.
## Metody
Metody serwisowe będą zajmować się obróbką danych. Będą przekształcać dane z modeli domenowych na viewmodele i na odwrót. Będą sprawdzać poprawność i kompletność otrzymanych od użytkownika danych, filtrować i sortować dane z bazy danych, wykonywać potrzebne obliczenia itd.
## Kolumna `IsActive` bazy danych
W naszych bazach danych rzadko będziemy na prawdę usuwać raz wpisane tam dane. Zamiast tego nasze tabele często będą zawierać kolumny `IsActive`, `StatusId`, czy coś takiego. Będą to flagi służące do oznaczenia usuniętych przez użytkownika rekordów, których nie chcemy więcej pokazywać użytkownikom. Czyli, jeśli użytkownik chce usunąć jakiś rekord, to oznaczamy jego flagę jako usunięty, zamiast faktycznie kasować dane z bazy danych. Dzięki takiemu podejściu możliwe jest odzyskanie danych, które użytkownik nieumyślnie usunie. Jeżeli zamiast tego będziemy faktycznie kasować dane z bazy danych, to ich odzyskanie będzie niemożliwe (chyba że znajdą się w istniejącej kopii zapasowej). Ponieważ użytkownikom dość często zdarza się coś "niechcący skasować", więc niechętnie usuwamy dane z bazy danych.

Pobierając jakiekolwiek dane z bazy danych możemy od razu odfiltrowywać "usunięte" rekordy. Takie proste filtrowanie możemy wykonywać bezpośrednio w repozytoriach lub później w serwisach.
## Przykład
Pokażmy przykładową implementację metody serwisu, zwracającą listę klientów do wyświetlenia np. w tabelce.
```csharp =
public ListCustomerForListVm GetAllCustomersForList()
{
    // zwrocenie, przez repozytorium, listy klientow, po odfiltrowaniu tych "usunietych"
    var customers = _customerRepo.GetAllActiveCustomers();
    // utworzenie obiektu listy, ktory bedziemy zwracac
    ListCustomerForListVm result = new ListCustomerForListVm();
    result.Customers = new List<CustomerForListVm>();
    // utworzenie odpowiednich obiektow viewmodeli
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

        // dodanie utworzonego obiektu do listy
        result.Customers.Add(customVm);
    }

    // dodanie liczby elementow w liscie do wlasciwosci Count
    result.Count = result.Customers.Count;

    return result;
}
```
Ponieważ z repozytoriów zwracamy typ `IQueryable`, więc dane z bazy danych nie zostaną tak na prawdę zwrócone w linii `var customers = _customerRepo.GetAllActiveCustomers();`, ale dopiero w pętli `foreach (var customer in customers)`, kiedy faktycznie będziemy tych danych używać. Z tego powodu można swobodnie odkładać filtrowanie danych na jak najpóźniej i wykonywać je w serwisach dopiero tuż przed faktycznym użyciem danych.
## Repozytorium vs. serwis
Jak już wspominaliśmy repozytoria nie powinny zawierać żadnej logiki. Możemy jednak umieścić w nich takie proste filtrowanie, jak odrzucenie "usuniętych" rekordów, których na pewno nie będziemy chcieli używać nigdzie w programie. Chodzi głównie o to, aby zwracały one w prost dane z bazy danych, bez żadnych modyfikacji. Wszelkie operacje na danych wykonujemy w serwisie.
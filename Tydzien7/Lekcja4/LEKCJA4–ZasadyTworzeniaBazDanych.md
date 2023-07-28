# [LEKCJA 4 – Zasady tworzenia baz danych](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-7-bazy-danych/lekcja-4-zasady-tworzenia-baz-danych/)
## Klucze
W bazie danych występują tzw. klucze, czyli identyfikatory elementów w bazie danych. Klucze muszą być unikatowe. Tzn. w obrębie jednej tabeli wartość klucza dla każdego wiersz musi być inna. Wyróżniamy dwa podstawowe rodzaje kluczy: główny i obcy.
### Klucz główny
Jest to identyfikator elementów w danej tabeli. Najczęściej będzie to nasze id. Za każdym razem dodając nowy element do bazy danych będziemy sobie taki klucz inkrementować, tak aby różnił się od tych będących już w tabeli. Oczywiście nie musi być to koniecznie `int`. Klucz może być dowolnego typu. Ważne, aby jego wartość była unikatowa. **W tabeli nie mogą znajdować się dwa elementy o takim samym kluczu.**
### Klucz obcy
Używa się go do tworzenia relacji pomiędzy danymi. Klucz obcy jest to klucz główny innego elementu (najczęściej z innej tabeli).
## Relacje
Pomiędzy danymi w bazach danych mogą występować trzy rodzaje relacji: jeden do jednego, jeden do wielu i wiele do wielu.
### Jeden do jednego
Relacja mówiąca, że element z jednej tabeli może mieć relację tylko i wyłącznie z jednym elementem z drugiej tabeli.
#### Przykład
Załóżmy, że klientami naszej aplikacji są firmy. Każda z nich ma wyznaczoną jedną osobę do kontaktu. Np.:
```csharp =
public class Customer
{
    public int Id { get; set; } // klucz glowny
    public string Name { get; set; }
    public string NIP { get; set; }

    // Relacja jeden do jednego
    public CustomerContactInformation CustomerContactInformation { get; set; } // powiązany obiekt
}
```
```csharp =
public class CustomerContactInformation
{
    public int Id { get; set; } // klucz glowny
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Position { get; set; }

    // Relacja jeden do jednego
    public int CustomerRef { get; set; } // referencja do klienta, klucz obcy
    public Customer Customer { get; set; } // powiązany obiekt
}
```
Dodatkowo będziemy musieli stworzyć klasę definiującą na podstawie jakich pól osiągamy relacją. Zajmiemy się tym jednak w kolejnej lekcji, podczas omawiania tworzenia kontekstów.
### Jeden do wielu
Tą relację omówiliśmy już pokrótce w poprzedniej lekcji. 
### Wiele do wielu
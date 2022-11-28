# [LEKCJA 12 – Enum](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-12-enum/)
Enum należy do wyliczeniowych typów wartościowych. Wspominaliśmy już o nim przy okazji omawiania typów wartościowych. Służy do przechowywania określonych danych słownikowych, których ilość się nie zmienia. Przypomnijmy sobie jak tworzy sią typ `enum`:

```csharp =
public enum Nazwa
{
	Slowo1,
	Slowo2,
	Slowo3
	...
}
```

Enum jest typem statycznym. Nie możemy dodawać nowych wartości do zdefiniowanego `enum`a. Każdej wartości słownikowej w tym typie odpowiada określona liczba typu `int`. Tradycyjnie numerowanie rozpoczyna się od zera (pierwszy element ma wartość `0`, a każdy kolejny o jeden większą niż poprzedni), jednak w enumie możemy odpowiadające wartości `int` zdefiniować samodzielnie. Można to zrobić przypisując żądaną wartość do wybranego elementu (wówczas każdy kolejny element będzie mieć wartość o jeden większą niż poprzedni, a każdy poprzedzający go element wartość o jeden mniejszą niż ten następujący po nim). Można również przypisać wartości wszystkim elementom, wówczas nie muszą być to kolejne liczby, ale każdy element musi mieć inną (unikatową) wartość. Dobrym przykładem zastosowania `enum`a jest stosowany kiedyś powszechnie `enum` przechowujący dni tygodnia:

```csharp =
public enum Weekday
{
	Monday = 1,
	Tuesday,
	Wednesday,
	Thursday,
	Friday,
	Saturday,
	Sunday
}
```

Obecnie nie jest już on tak powszechnie definiowany przez programistów, gdyż nowsze wersje C# mają już podobne wyliczenie predefiniowane ([`System.DayOfWeek`](https://learn.microsoft.com/pl-pl/dotnet/api/system.dayofweek?view=net-7.0), niedziela ma tu wartość `0`, poniedziałek `1` itd.). Można również określić dzień tygodnia na podstawie podanej daty, używając w tym celu właściwości `DayOfWeek` struktury `System.DateTime`, reprezentującej moment w czasie wyrażony jako data i godzina (więcej informacji na temat tej struktury można znaleźć w [dokumentacji](https://learn.microsoft.com/en-us/dotnet/api/system.datetime?view=net-7.0)). Enum dni tygodnia jest jednak dobrym przykładem zastosowania wyliczenia, gdyż mamy tu ściśle określone elementy, co do których mamy pewność, że nie zmienią się w czasie. Zostańmy więc przy tym przykładzie i stwórzmy zmienną tego typu:

```csharp =
Weekday day = Weekday.Friday;
Console.WriteLine("Jest {0} dzień tygodnia, czyli: {1}", (int)day, day);
if (day == Weekday.Friday)
	Console.WriteLine("Piątek, piąteczek, piątunio :-D");
```

Jak widać w powyższym przykładzie możemy dokonać rzutowania zmiennej `enum` na typ `int`, aby otrzymać odpowiadającą wartość `int` lub użyć samej nazwy zmiennej, aby otrzymać wartość słownikową. Podajmy jeszcze dla przykładu wyliczenie ze zdefiniowanymi wszystkimi wartościami `int`:

```csharp =
public enum PeriodsOfTime
{
	Minute = 1,
	Quarter = 15,
	Half = 30,
	Hour = 60,
	Day = 1440
}
```

Typy wyliczeniowe są stosunkowo szybkie w użyciu, jednak mają ograniczone zastosowanie. Należy pamiętać, że nie można ich dynamicznie zmieniać (dopisywać kolejnych wartości), więc ich zastosowanie należy dobrze przemyśleć. Późniejsze zmiany warunków lub wymagań w naszej aplikacji, które będą wiązały się z typami enum będą również oznaczały zmiany w innych częściach kodu programu.

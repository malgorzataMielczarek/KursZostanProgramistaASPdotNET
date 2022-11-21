# [LEKCJA 6 – Operatory](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-6-operatory/)

## Arytmetyczne
Zdefiniowanie zmiennych do przykładów z poniższej tabeli:

```csharp =
int a = 5;
int b = 10;
```

| Operator | Znaczenie | Przykład |
| :---: | :--- | :--- |
| + | dodawanie | `int c = a + b; // wynik: 15` |
| - | odejmowanie | `int c = b - a; // wynik: 5` |
| * | mnożenie | `int c = a * b; // wynik: 50` |
| / | dzielenie | `int c = b / a; // wynik: 2` |
| % | modulo - reszta z dzielenia | `int c = b % a; // wynik: 0` |

Jeżeli wynik operacji chcemy przypisać do zmiennej znajdującej się po lewej stronie operatora, zapis operacji możemy skrócić stosując operatory (`+=`, `-=`, `*=`, `/=` i `%=`), np.:

```csharp =
int a = 0;
int b = 10;
a += 2; // to samo co a = a + 2;
// czyli teraz a == 2, b == 10
a -= b; // to samo co a = a - b;
//teraz a == -8, b == 10
a *= -1; // to samo co a = a * (-1);
// a == 8, b == 10
b %= a; // to samo co b = b % a;
// a == 8, b == 2
a /= b; // to samo co a = a / b;
// a == 4, b == 2
```

## Inkrementacja/dekrementacja
1. **Inkrementacja** - zwiększenie wartości zmiennej o jeden. Jest to operacja jednoargumentowa, której operatorem są dwa znaki plus (`++`). Może występować w dwóch wersjach:
	1. postinkrementacja -  operator wstawiamy po argumencie. W tym wypadku wyrażenie ma wartość argumentu (najpierw następuje przypisanie, a potem inkrementacja). Np.:
	
	```csharp =
	int i = 0;
	i++; // to samo co i = i + 1; lub i += 1;
	// wyrażenie ma wartość i
	```
	
	2. preinkrementacja -  operator wstawiamy przed argumentem. W tym wypadku wyrażenie ma wartość argumentu powiększonego o jeden (najpierw następuje inkrementacja, a potem przypisanie). Np.:
	
	```csharp =
	int i = 0;
	++i; // to samo co i = i + 1; lub i += 1;
	// wyrażenie ma wartość i + 1
	```
	
	Przykład pokazujący różnicę między postinkrementacją i preinkrementacją:
	
	```csharp =
	int i = 0, post, pre;
	
	// Postinkrementacja
	post = i++;
	/*
	Inaczej można to zapisać:
	post = i;
	i = i + 1;
	czyli teraz post == 0, a i == 1
	*/
	
	i = 0;
	//Preinkrementacja
	pre = ++i;
	/*
	Inaczej można to zapisać:
	i = i + 1;
	pre = i;
	czyli teraz pre == 1, a i == 1
	*/
	```
	
2. **Dekrementacja** - zmniejszenie wartości zmiennej o jeden. Jest to operacja jednoargumentowa, której operatorem są dwa znaki minus (`--`). Podobnie jak inkrementacja, może występować w dwóch wersjach, o analogicznym działaniu:
	1. postdekrementacja -  operator wstawiamy po argumencie. W tym wypadku wyrażenie ma wartość argumentu (najpierw następuje przypisanie, a potem dekrementacja). Np.:
	
	```csharp =
	int i = 0;
	i--; // to samo co i = i - 1; lub i -= 1;
	// wyrażenie ma wartość i
	```
	
	2. predekrementacja -  operator wstawiamy przed argumentem. W tym wypadku wyrażenie ma wartość argumentu pomniejszonego o jeden (najpierw następuje dekrementacja, a potem przypisanie). Np.:
	
	```csharp =
	int i = 0;
	--i; // to samo co i = i - 1; lub i -= 1;
	// wyrażenie ma wartość i - 1
	```
	
	Przykład pokazujący różnicę między postdekrementacją i predekrementacją:
	
	```csharp =
	int i = 0, post, pre;
	
	// Postdekrementacja
	post = i--;
	/*
	Inaczej można to zapisać:
	post = i;
	i = i - 1;
	czyli teraz post == 0, a i == -1
	*/
	
	i = 0;
	//Predekrementacja
	pre = --i;
	/*
	Inaczej można to zapisać:
	i = i - 1;
	pre = i;
	czyli teraz pre == -1, a i == -1
	*/
	```
	

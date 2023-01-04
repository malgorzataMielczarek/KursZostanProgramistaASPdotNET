# [LEKCJA 10 – Refaktoryzacja](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-3-programowanie-obiektowe/lekcja-10-refaktoryzacja/)
W tej lekcji zajmiemy się refaktoryzacją naszego programu, utworzonego w poprzednim tygodniu, z wykorzystaniem nabytej w tym tygodniu wiedzy. Skupimy się na trzech aspektach:
1. Podział logiczny aplikacji - podzielimy solucję na dodatkowe projekty
2. Wykorzystanie interfejsów - utworzymy interfejsy dla serwisów
3. Wykorzystanie dziedziczenia i klas abstrakcyjnych - utworzenie bazowego serwisu i modelu
## 1. Podzielić solucję na projekty
Aby aplikacja była łatwiejsza w zarządzaniu i dalszym rozwoju, przeniesiemy część jej funkcjonalności do odrębnych projektów. Nie będą to jednak projekty konsolowe, a biblioteki klas (_Class Library (Standard .NET)_). Są to projekty, które nie kompilują się do pliku wykonywalnego (_.exe_) i co za tym idzie nie mogą funkcjonować jako samodzielne programy. Kompilują się natomiast do plików _.dll_, czyli właśnie bibliotek, i służą do implementacji funkcjonalności potrzebnych w programie. Takie biblioteki można później ponownie wykorzystywać w kolejnych aplikacjach. Utworzymy trzy biblioteki i usuniemy przykładową klasę z każdej z nich.
### 1. NazwaAplikacji.App
Po utworzeniu projektu i usunięci przykładowej klasy, jeszcze zanim utworzymy własną klasę, zajmijmy się zależnościami między projektami (ang. _dependencies_). Wiemy na pewno, że nasz główny projekt będzie korzystał z tego projektu. W naszym _Solution Explorer_ kliknijmy więc prawym przyciskiem myszki na _Dependencies_ naszego głównego projektu i wybierzmy _Add Project Reference..._. Otworzy nam się _Reference Manager_. Powinien automatycznie otworzyć się on na zakładce _Projects_ -> _Solution_, czyli liście projektów w naszej solucji (z wyłączeniem projektu, dla którego menadżer otworzyliśmy). Na liście zaznaczamy check box naszego nowo utworzonego projektu, dodając do niego referencję (informację dla kompilatora, że w danym projekcie będziemy korzystać z innego projektu), i klikamy _OK_.

W tym projekcie znajdować się będzie cała logika naszej aplikacji. Umieścimy w niej wszystkie serwisy (klasy serwisowe) służące do sterowania naszą aplikacją.

Podzielmy jeszcze projekt na foldery:
#### 1. Abstract
Tu znajdą się interfejsy dla serwisów.
#### 2. Concrete

### 2. NazwaAplikacji.Domain
W tym projekcie znajdować się będą wszystkie modele (klasy reprezentujące obiekt, który w przyszłości mógłby być obiektem bazodanowym)
## 2. Dodać interfejsy dla serwisów
## 3. Dodać bazowy serwis i bazowy model

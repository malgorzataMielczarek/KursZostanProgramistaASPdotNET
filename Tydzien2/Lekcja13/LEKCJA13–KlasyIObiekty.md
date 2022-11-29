# [LEKCJA 13 – Klasy i obiekty](https://kurs.szkoladotneta.pl/zostan-programista-asp-net/tydzien-2-podstawy-jezyka-c/lekcja-13-klasy-i-obiekty/)
Klasy są podstawą języka C#, ponieważ jest to język obiektowy. Z założenia wszystkie jego elementy są więc obiektami.

**Klasy** - definicje dla obiektów jakiegoś typu (tej klasy).

**Obiekty** - instancje danej klasy.

## Klasa
Każda klasa ma swoje cechy (właściwości - ang. _properties_) i zachowania (metody - ang. _methods_). Klasę tworzymy podając modyfikator dostępu, ewentualne dodatkowe modyfikatory (np. `static`, jeśli ma być to klasa statyczna), słowo kluczowe `class`, nazwę i w klamrach jej właściwości i metody. Klasa będzie nam więc opisywać jakie cechy posiada każdy obiekt danego typu (wartość tej cechy będzie indywidualna dla każdego obiektu, ale fakt jej posiadania pozostaje niezmienny) oraz jakie czynności może wykonywać (lub czasem jakie czynności można na nim wykonywać). Stwórzmy więc przykładową klasę `Dog`, w której postaramy się opisać psa. Każdy pies ma określone cechy, takie jak np. imię, rasa, wiek, płeć, waga, wysokość, długość, umaszczenie, długość sierści itd. Każdy pies wykonuje też pewne, takie same czynności, np.: je, śpi, wychodzi na spacer, bawi się. Spróbujmy więc to zapisać:

```csharp =
public class Dog
{
	// Properties
	public string Name { get; set; }
	public string Breed { get; set; }
	public int Age { get; set; } // in years
	public string Gender { get; set; } // Male, Female
	public decimal Weight { get; set; } // in kg
	public decimal Height { get; set; } // height at the withers in cm
	public decimal Length { get; set; } // in cm
	public string Color { get; set; }
	public CoatLength Fur { get; set; }

	// Methods
	public void Eat(string foodName, decimal quantity) // quantity in grams
	{
		Console.WriteLine($"{this.Name} is eating {quantity} grams of {foodName}.");
		this.Weight += 0.9M * quantity / 1000;
	}
	public void Sleep(int hours)
	{
		Console.WriteLine("{0} is sleeping. It will take {1} {2} hours to wake up.", this.Name, (this.Gender == "Male" ? "him" : "her"), hours);
		this.Weight -= hours * 0.001M;
	}
	public void Walk(int length) // in minutes
	{
		Console.WriteLine("{0} is happy, because {1} went on {2} minutes walk.", this.Name, (this.Gender == "Male" ? "he" : "she"), length);
		this.Weight -= (length * 0.001M + this.Weight * 0.005M);
	}
	public void Play(string playName, int length) //length in minutes
	{
		Console.WriteLine("{0} is happy, because {1} is playing {2} for the next {3} minutes.", this.Name, (this.Gender == "Male" ? "he" : "she"), playName, length);
		this.Weight -= length * 0.002M;
	}
}
public enum CoatLength
{
	Shorthaired,
	Longhaired,
	Hairless,
	Combination
}
```

Stwórzmy teraz psa (konkretny egzemplarz).

## Obiekt
Jak już wiemy klasa, jest to opis całej grupy. Obiekt będzie zaś konkretnym wystąpieniem (egzemplarzem, instancją) tej grupy (klasy). Stwórzmy więc naszego psa. Nasz pies to czarny, pięcioletni kundel, o imieniu Torpeda. Torpeda jest niewielką, energiczną suczką ważącą 5kg i mierzącą 25 cm wysokości w kłębie i 33 cm długości. Jej sierść ma mieszaną długość, gdzieniegdzie jest dłuższa, a w innych miejscach krótsza. Spróbujmy to zapisać:

```csharp =
Dog dog = new Dog();
dog.Color = "black";
dog.Age = 5;
dog.Breed = "mongrel";
dog.Name = "Torpeda";
dog.Gender = "Female";
dog.Weight = 5;
dog.Height = 25;
dog.Length = 33;
dog.Fur = CoatLength.Combination;
```

Pozwólmy też Torpedzie wykonać kilka czynności. Nakarmmy ją, wyprowadźmy na spacer, pobawmy się z nią chwilę i pozwólmy jej odpocząć:

```csharp =
dog.Eat("dry food", 150);
dog.Walk(30);
dog.Play("tug of war", 10);
dog.Sleep(5);

Console.WriteLine("Dog weight after activities: " + dog.Weight);
```

Kiedy będziemy operować na danych z bazy danych, będziemy je na ogół tłumaczyć na obiekty klasy. Później będziemy mogli nimi manipulować przy użyciu metod danej klasy i zmiany właściwości obiektów.

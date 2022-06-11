---
layout: post
title: 56. Builder-designmönstret i Java
---

Builder-designmönstret i Java är ett sätt att bygga oföränderliga objekt (eng. *immutable objects*) med hjälp av en och samma objektsuppbyggnadsprocess [[1](https://howtodoinjava.com/design-patterns/creational/builder-pattern-in-java/)]. Oföränderliga objekt kännetecknas av att deras tillstånd (eng. *state*) inte kan förändras när de en gång skapats, något som ofta är önskvärt av flera anledningar, bl.a. enkelhet i design och användning och möjlighet till [multikörning](https://sv.wikipedia.org/wiki/Multik%C3%B6rning). De har inga set-metoder, bara `final` eller `private`-fält, tillåter inte underklassar att override:a metoder och inte innehåller heller inga metoder för att ändra föränderliga objekt eller referenser till dem [[2](https://docs.oracle.com/javase/tutorial/essential/concurrency/imstrat.html)].

## Frivilliga parametrar
I Java kan man, till skillnad från exempelvis Python, inte sätta frivilliga parametrar i sin konstruktor av en klass. Detta leder till att man måste skriva flera konstuktorer för olika kombinationer av parametrar, något som kan leda till mycket upprepningar av den här typen (där icke-obligatoriska parametrar förses med ett default-värde):

```java
public class Apartment {

    private final String address;
    private final int sqMeter;
    private final boolean hasDishwasher;
    private final List<String> defects;

public Apartment(String address) {
    this.address = address;
    this.sqMeter = 0;
    this.hasDishwasher = false;
    this.defects = new ArrayList<>();
}

public Apartment(String address, int sqMeter) {
    this.address = address;
    this.sqMeter = sqMeter;
    this.hasDishwasher = false;
    this.defects = new ArrayList<>();
}

public Apartment(String address, int sqMeter, boolean hasDishwasher) {
    this.address = address;
    this.sqMeter = sqMeter;
    this.hasDishwasher = hasDishwasher;
    this.defects = new ArrayList<>();
}

// etc.
}
```

## Teleskopisk konstrukor

Upprepningen av kod ovan kan man delvis komma undan med hjälp av en så kallad teleskopisk konstruktor, där varje konstruktor gör ett anrop till ett annan konstruktor med stegvis fler input-parametrar [[3](http://www.javabyexamples.com/telescoping-constructor-in-java)]:

```java
public class Apartment {

    private final String address;
    private final int sqMeter;
    private final boolean hasDishwasher;
    private final List<String> defects;

public Apartment(String address) {
    this(address, 0);
}

public Apartment(String address, int sqMeter) {
    this(address, sqMeter, false);
}

public Apartment(String address, int sqMeter, boolean hasDishwasher) {
    this(address, sqMeter, hasDishwasher, new ArrayList<String>());
}

public Apartment(String address, int sqMeter, boolean hasDishwasher, List<String> defects) {
    this.address = address;
    this.sqMeter = sqMeter;
    this.hasDishwasher = hasDishwasher;
    this.defects = defects;
}

// etc.

}

```
Nackdelen med detta är att det fortfarande blir ganska rörigt med så många konstruktorer. Man måste komma ihåg vilken ordning man ska sätta dem i och riskerar om det finns parametrar av samma typ bredvid varandra att sätta dem fel utan att det upptäcks [[4](https://www.vojtechruzicka.com/avoid-telescoping-constructor-pattern/)]. Ett alternativ till flerkonstruktorslösningar av den här typen kan vara att ha en tom konstruktor för klassen och en massa set-metoder, men då är objekten som skapas inte längre oföränderliga.

## Builder-mönstrets lösning
Builder-mönstret är ett sätt att kringå teleskopiska konstruktor-problemet och samtidigt bevara objektens oföränderlighet. Med Builder-designmönstret skapar man en separat klass för själva byggandet av den egentliga klassen:

```java

public class Apartment {

    private final String address;
    private final int sqMeter;
    private final boolean hasDishwasher;
    private final List<String> defects;

    public Apartment(String address, int sqMeter, boolean hasDishwasher, List<String> defects) {
        this.address = address;
        this.sqMeter = sqMeter;
        this.hasDishwasher = hasDishwasher;
        this.defects = defects;
    }

}

public class ApartmentBuilder {
    
    // obligatorisk
    private final String address;
    
    // frivilliga
    private final int sqMeter;
    private final boolean hasDishwasher;
    private final List<String> defects;

    public ApartmentBuilder(String address) {
        this.address = address;
    }

    
    public ApartmentBuilder sqMeter(int sqMeter) {
		this.sqMeter = sqMeter;
		return this;
    }

    public ApartmentBuilder hasDishwasher(boolean hasDishwasher) {
		this.hasDishwasher = hasDishwasher;
		return this;
    }

    public ApartmentBuilder defects(List<String> defects) {
		this.defects = defects;
		return this;
    }

    public ApartmentBuilder addMold() {
        defects.add("mögel");
        return this;
    }


    public ApartmentBuilder addMoistureDamage() {
        defects.add("fuktskada");
        return this;
    }

    public ApartmentBuilder addPest() {
        defects.add("skadedjur");
        return this;
    }

    public Apartment build(){
        return new Apartment(address, sqMeter, hasDishwasher, defects);
    }

}

```

Eventuella obligatoriska attribut kan sättas i Builder-klassens konstruktur (som görs med addressen i exemplet ovan). Notera att metoderna i Builder-klassen retunerar själva Builder-klassen vilket möjliggör kedjade anrop, se nedan:

```java
public static void main(String[] args) {

	Apartment apartmentA = new ApartmentBuilder("Javagatan 8, 199 50")
	    .sqMeter(60)
	    .hasDishwasher(true)
	    .addMold()
	    .build();

    Apartment apartmentB = new ApartmentBuilder("Javagatan 11, 199 50")
	    .sqMeter(32)
	    .hasDishwasher(false)
	    //inga skador
	    .build();

}

```


## Referenser

[1] [Builder Pattern in Java](https://howtodoinjava.com/design-patterns/creational/builder-pattern-in-java/). HowToDoInJava (03-01-2022).<br>
[2] [A Strategy for Defining Immutable Objects](https://docs.oracle.com/javase/tutorial/essential/concurrency/imstrat.html). Oracle Java Documentation (23-02-2022).<br>
[3] [Telescoping Constructor in Java](http://www.javabyexamples.com/telescoping-constructor-in-java). Java By Examples (05-03-2022).<br>
[4] [Telescoping Constructor Pattern alternatives](https://www.vojtechruzicka.com/avoid-telescoping-constructor-pattern/). Vojtech Ruzicka's Programming Blog (11-06-2022).
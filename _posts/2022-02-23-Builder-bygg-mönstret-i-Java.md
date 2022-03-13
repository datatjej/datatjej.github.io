---
layout: post
title: 56. Builder-designmönstret i Java
---

Builder-designmönstret i Java är ett sätt att bygga oföränderliga objekt (eng. *immutable objects*) med hjälp av en och samma objektsuppbyggnadsprocess [[1](https://howtodoinjava.com/design-patterns/creational/builder-pattern-in-java/)]. Oföränderliga objekt kännetecknas av att deras tillstånd (eng. *state*) inte kan förändras när de en gång skapats, något som ofta är önskvärt av flera anledningar, bl.a. enkelhet i design och användning och möjlighet till [multikörning](https://sv.wikipedia.org/wiki/Multik%C3%B6rning). De har inga set-metoder, bara `final` eller `private`-fält, tillåter inte underklassar att override:a metoder och inte innehåller heller inga metoder för att ändra föränderliga objekt eller referenser till dem [[2](https://docs.oracle.com/javase/tutorial/essential/concurrency/imstrat.html)]. 

## Teleskopiska konstruktor-problemet
Builder-mönstrets existens kan delvis förklaras av teleskopiska konstruktor-problemet (*telescoping constructor problem*) i Java. Eftersom man i Java inte kan skriva konstruktorer med frivilliga parameterar leder det lätt till att man skapar flera konstruktorer med olika kombinationer av parameterar för samma objekt, vilket blir komplicerat ju fler parametrar man har. Den här sidan, [[3](http://www.javabyexamples.com/telescoping-constructor-in-java)], visar dock hur den teleskopiska konstruktorn ändå ska ses som ett bättre alternativ än liknande flerkonstruktorslösningar där en stor del av koden upprepas.

Ett alternativ till flerkonstruktorslösningar kan vara att ha en tom konstruktor för klassen och en massa set-metoder, men då är objekten som skapas inte längre oföränderliga. Builder-mönstret är alltså ett sätt att kringå teleskopiska konstruktor-problemet, och samtidigt bevara objektens oföränderlighet.   

## Builder-mönstrets lösning
Med Builder-designmönstret skapar man en separat klass för själva byggandet av den egentliga klassen. 


## Referenser

[1] [Builder Pattern in Java](https://howtodoinjava.com/design-patterns/creational/builder-pattern-in-java/). HowToDoInJava (03-01-2022).<br>
[2] [A Strategy for Defining Immutable Objects](https://docs.oracle.com/javase/tutorial/essential/concurrency/imstrat.html). Oracle Java Documentation (23-02-2022).<br>
[3] [Telescoping Constructor in Java](http://www.javabyexamples.com/telescoping-constructor-in-java). Java By Examples (05-03-2022).<br>

[4] [Difference Between a Java Keystore and a Truststore](https://www.baeldung.com/java-keystore-truststore-difference). Baeldung.com (30-09-2021).


---
layout: post
title: 56. Builder-designmönstret i Java
---

Builder-designmönstret i Java är ett sätt att bygga oföränderliga objekt (eng. *immutable objects*) med hjälp av samma objektsuppbyggnadsprocess [[1](https://howtodoinjava.com/design-patterns/creational/builder-pattern-in-java/)]. Oföränderliga objekt kännetecknas av att deras tillstånd (eng. *state*) inte kan förändras när de en gång skapats, något som ofta är önskvärt av flera anledningar, bl.a. enkelhet i design och användning och möjlighet till [multikörning](https://sv.wikipedia.org/wiki/Multik%C3%B6rning). De har inga set-metoder, bara `final` eller `private`-fält, tillåter inte underklassar att override:a metoder och inte innehåller heller inga metoder för att ändra föränderliga objekt eller referenser till dem [[2](https://docs.oracle.com/javase/tutorial/essential/concurrency/imstrat.html)]. 

## Telescoping Constructor-problemet

## Lösning
Med Builder-designmönstret 


## Referenser

[1] [Builder Pattern in Java](https://howtodoinjava.com/design-patterns/creational/builder-pattern-in-java/). HowToDoInJava (03-01-2022).<br>
[2] [A Strategy for Defining Immutable Objects](https://docs.oracle.com/javase/tutorial/essential/concurrency/imstrat.html). Oracle Java Documentation (23-02-2022).<br>
[3] [Usage statistics of Default protocol https for websites](https://w3techs.com/technologies/details/ce-httpsdefault). W3techs.com - Web Technology Surveys (20-02-2022).<br>
[4] [Difference Between a Java Keystore and a Truststore](https://www.baeldung.com/java-keystore-truststore-difference). Baeldung.com (30-09-2021).


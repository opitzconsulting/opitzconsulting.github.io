---
title: "Ein Hamcrest Entity Matcher"
categories: software_craftsmanship
author: stefan.lack
---


Der hier vorgestellte Hamcrest Matcher entstand im Rahmen eines Projektes, in dem sehr viele Java Entity Objekte auf Korrektheit ihrer Properties getestet werden mussten.
Mit dem Matcher können mittels eines einzelnen Assert-Statement alle Properties einer Klasse auf Korrektheit geprüft werden. Dabei werden alle fehlgeschlagenen Validierungen am Ende der Prüfung in einer übersichtlichen Fehlermeldung dargestellt.

## Zu Beginn ein Beispiel

{% highlight java %}
  @Test
  public void testing_single_properties_with_matchesAllProperties() {
    final Person expected = new Person( "Maier", "Hans" ).withAge( 42 );
    final Person actual = new Person( "Mayer", "Hans" ).withAge( 7 );

    assertThat( actual, matchesAllProperties( expected ) );
  }
{% endhighlight %}

Nach Ausführung des Tests werden alle Property-Abweichungen angezeigt:

    java.lang.AssertionError:
    Expected: a entity with specified property values
         but: got entity with 2 invalid values [
      -->age		(expected:42, actual:7),
      -->lastName		(expected:Maier, actual:Mayer)]

## Setup

Zur Demonstration des Matchers wird die Klasse Person verwendet:

{% highlight java %}
public class Person {

  String firstName;
  String lastName;
  private String email;
  private int age;

  public Person( String lastName, String firstName ) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  public Person withEmail( String email ) {
    this.email = email;
    return this;
  }

  public Person withAge( int age ) {
    this.age = age;
    return this;
  }

  // getters
}
{% endhighlight %}

## Motivation

Es gibt schon einige Möglichkeiten zum Testen von Properties mit Junit und Hamcrest. Allerdings hat jeder dieser Ansätze Nachteile für unser Einsatzszenario, die wir uns einmal anschauen.
Daher haben wir einen neuen Entity Manager entwickelt.

### Naiver Ansatz zum Testen vieler Properties einer Instanz

Der naive Ansatz für das geschilderte Problem verwendet pro Property ein einzelnes JUnit assertEquals-Statement:
{% highlight java %}
  @Test
  public void testing_single_properties_with_simple_assert_statements() {
    final Person expected = new Person( "Maier", "Hans" ).withAge( 42 );
    final Person actual= new Person( "Mayer", "Hans" ).withAge( 7);

    assertEquals("lastname correct", expected.getLastName(), actual.getLastName());
    assertEquals("firstname correct", expected.getFirstName(), actual.getFirstName());
    assertEquals("age correct", expected.getAge(), actual.getAge());
  }
{% endhighlight %}

Wie zu erwarten schlägt der Test fehl mit der folgenden Meldung:


       org.junit.ComparisonFailure: lastname correct
        Expected :Maier
        Actual   :Mayer
          <Click to see difference>

        	at org.junit.Assert.assertEquals(Assert.java:115)
        	at com.opitzconsulting.entitymatcher.NaiverAnsatz.testing_single_properties_without_hamcrest(NaiverAnsatz.java:13)


Nachteile dieses Ansatzes:

* Es ist sehr aufwändig, für jedes einzelne Property ein eigenes assert-Statement zu Schreiben.

* Werden die gleichen Prüfungen in verschiedenen Test-Methoden benötigt, müssen die Statements immer kopiert werden. Es entsteht also schlecht wartbarer Code.

* Die Überprüfung der assert-Statements hört bei dem ersten Fehlschlag auf. So wird bei Ausführung des Beispiel-Tests nicht angezeigt, dass auch das Alter der Person nicht korrekt ist.

### Verwendung der JUnit ErrorCollector-Rule

In diesem Ansatz verwenden wir nicht einzelne assertEquals-Statements, sondern den [ErrorCollector](http://junit.org/junit4/javadoc/4.12/org/junit/rules/ErrorCollector.html "ErrorCollector (JUnit API)") aus dem JUnit Framework. Dazu muss in der Testklasse eine Instanz der Klasse
`org.junit.ErrorCollector` angelegt und mit der Annotation [@Rule](http://junit.org/junit4/javadoc/4.12/org/junit/Rule.html "Rule (JUnit API)") versehen werden:

{% highlight java %}
  @Rule
  public ErrorCollector errorCollector = new ErrorCollector();

  @Test
  public void testing_single_properties_with_error_collector() {
    final Person expected = new Person( "Maier", "Hans" ).withAge( 42 );
    final Person actual= new Person( "Mayer", "Hans" ).withAge( 7);

    errorCollector.checkThat( "lastname correct", actual.getLastName(), equalTo( expected.getLastName() ) );
    errorCollector.checkThat( "firstname correct", actual.getFirstName(), equalTo( expected.getFirstName() ));
    errorCollector.checkThat( "age correct", actual.getAge(), equalTo( expected.getAge() ));
  }
{% endhighlight %}

Vorteile:

* Im Gegensatz zu dem ersten Ansatz bricht die Testausführung nach dem ersten fehlgeschlagenen Assert-Statement nicht ab. Stattdessen wird der Test bis zum Ende ausgeführt. Anschließend wird der ErrorCollector ausgewertet und die fehlgeschlagen Prüfungen werden protokolliert:

      java.lang.AssertionError: lastname correct
      Expected: "Maier"
           but: was "Mayer"
        at org.hamcrest.MatcherAssert.assertThat(MatcherAssert.java:20)


      java.lang.AssertionError: age correct
      Expected: <42>
           but: was <7>
        at org.hamcrest.MatcherAssert.assertThat(MatcherAssert.java:20)     


Leider sind zwei Probleme des ersten Ansatztes auch hier noch nicht gelöst:

* Der Code ist immer noch sehr aufwändig, weil pro Property ein Statement geschrieben werden muss.

* Die Wartbarkeit ist immer noch nicht gut.

### Verwendung von Hamcrest samePropertyValuesAs

Durch die Verwendung des Matchers [samePropertyValuesAs](http://hamcrest.org/JavaHamcrest/javadoc/1.3/org/hamcrest/beans/SamePropertyValuesAs.html "Hamcrest API") kommen wir schon recht Nahe an unser Ziel, einfache Assert-Statements zu schreiben:

{% highlight java %}
  @Test
  public void testing_single_properties_with_hamcrest_samePropertyValuesAs() {
    final Person expected = new Person( "Maier", "Hans" ).withAge( 42 );
    final Person actual= new Person( "Mayer", "Hans" ).withAge( 7);

    assertThat(actual,samePropertyValuesAs( expected ));
  }
{% endhighlight %}

Der Test schlägt mit der folgenden Meldung fehl:

      java.lang.AssertionError:
      Expected: same property values as Person [age: <42>, email: null, firstName: "Hans", lastName: "Maier"]
       but: age was <7>


Vorteile:

* Die Wartbarkeit ist nun deutlich erhöht, da nur noch ein einzelnes Assert-Statement geschrieben werden muss.

Nachteile:

* Leider stoppt auch der samePropertyValuesAs - Matcher die Ausführung, sobald ein invalides Property gefunden wurde. Die Abweichung in dem Property lastName wurde nicht protokolliert.

## Entity Matcher

Der hier vorgestellte Entity Matcher erfüllt alle drei genannten Anforderungen.
Beispiel-Aufruf:
{% highlight java %}

import static com.opitzconsulting.entitymatcher.EntityMatcher.matchesAllProperties;

public class ExampleEntityMatcher {

  @Test
  public void testing_single_properties_with_matchesAllProperties() {
    final Person expected = new Person( "Maier", "Hans" ).withAge( 42 );
    final Person actual= new Person( "Mayer", "Hans" ).withAge( 7);

    assertThat(actual,matchesAllProperties( expected ));
  }
}
{% endhighlight %}

Nach Ausführung des Tests werden alle Property-Abweichungen angezeigt:

    java.lang.AssertionError:
    Expected: a entity with specified property values
         but: got entity with 2 invalid values [
      -->age		(expected:42, actual:7),
      -->lastName		(expected:Maier, actual:Mayer)]

Vorteile:
- Genau wie beim Hamcrest Matcher "samePropertyValuesAs" wir nur ein einzelnes assert-Statement benötigt.
- Der Entity Matcher überprüft alle Properties, alle Validierungsfehler werden am Ende protokolliert.
- Weiterhin sind mit dem Entity Matcher noch einige andere Dinge möglich, die in den folgenden Abschnitten dargestellt werden.

## Feature: Prüfe nur definierte Properties

Mittels der Methode _matchesSpecifiedProperties_ werden nur die Properties einer Klasse geprüft, die beim Aufruf spezifiziert werden

Im folgenden Test wird zur Demontration im ersten _assertThat_-Statement validiert, dass der Nachname und das Alter gleich sind. In dem zweiten _assertThat_-Statement wird validiert, dass der Vorname nicht gleich ist.

{% highlight java %}
import static com.opitzconsulting.entitymatcher.EntityMatcher.matchesSpecifiedProperties;
...
@Test
public void testMatchesSpecifiedProperties() {
  Person actualPerson = new Person( "Duck", "Donald" )
      .withAge(42)
      .withEmail( "donald.duck@entenhausen.de" );
  Person expectedPerson = new Person( "Duck", "Daisy" )
      .withAge(42)
      .withEmail( "Daisy.duck@entenhausen.de" );

  assertThat( actualPerson,
      matchesSpecifiedProperties( expectedPerson, "lastName", "age" ) );

  assertThat( actualPerson,
      not( matchesSpecifiedProperties( expectedPerson, "firstName" ) ));
}
{% endhighlight %}

## Feature: Prüfe alle mit Ausnahme der definierten Properties

Mittels der Methode _matchesAllPropertiesExcluding_ werden alle Properties einer Klasse mit Ausnahme der spezifizierten Properties geprüft.

Im folgenden Test wird zur Demonstration validiert, dass alle Properties mit Ausnahme von Vorname und E-Mail-Adresse gleich sind:

{% highlight java %}
import static com.opitzconsulting.entitymatcher.EntityMatcher.matchesAllPropertiesExcluding;
...
@Test
public void testMatchesAllPropertiesExcluding() {
  Person actualPerson = new Person( "Duck", "Donald" ).withAge(42).withEmail(
    "donald.duck@entenhausen.de" );
  Person expectedPerson = new Person( "Duck", "Daisy" ).withAge(42 )
    .withEmail( "Daisy.duck@entenhausen.de" );

  assertThat( actualPerson,
    matchesAllPropertiesExcluding( expectedPerson, "firstName", "email" ) );
}
{% endhighlight %}

## Download des Entity-Matchers

Der Entity-Matcher ist auf Github verfügbar: [entity-matcher](https://github.com/opitzconsulting/entity-matcher)

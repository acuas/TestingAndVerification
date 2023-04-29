# Library Service

This XML specification defines a simple Library Service, which enables users to check the availability of a book in the library stock. Let's break down the components of the specification:

## 1. Service Outline Skeleton

The Service Outline Skeleton is the top-level structure of the service, containing elements like Service, Protocol, and Machine. The service is named "LibraryService", and it has a single Protocol and a Machine with the same name.

```xml
<Service name="LibraryService">
  <Protocol name="LibraryService">
    <!-- Protocol contents -->
  </Protocol>
  <Machine name="LibraryService">
    <!-- Machine contents -->
  </Machine>
</Service>
```

## 2. Finite State Machine

The Finite State Machine (FSM) for this service is very simple, as it has only one state called "Initial". This state is marked as the initial state, meaning the service starts in this state. There are two transitions, "checkAvailability/ok" and "checkAvailability/error", both of which have the same source and target state, "Initial". This means the service remains in the same state after performing the checkAvailability operation, regardless of the outcome.

```xml
<Machine name="LibraryService">
  <State name="Initial" initial="true">
    <Transition name="checkAvailability/ok" source="Initial" target="Initial"/>
    <Transition name="checkAvailability/error" source="Initial" target="Initial"/>
  </State>
</Machine>
```

## 3. Protocol Operations

The service has one operation called "checkAvailability". It has an input parameter named "book" of type String, and an output parameter named "available" of type Boolean. This operation is responsible for checking the availability of the specified book in the library stock.

```xml
<Operation name="checkAvailability">
  <Input name="book" type="String"/>
  <Output name="available" type="Boolean"/>
  <!-- Operation scenarios -->
</Operation>
```

## 4. Operation Scenarios

 The checkAvailability operation has two scenarios: "checkAvailability/ok" and "checkAvailability/error". The "checkAvailability/ok" scenario is executed when the specified book is found in the current stock with a quantity greater than zero. In this case, the output "available" is set to true. On the other hand, the "checkAvailability/error" scenario is executed when the specified book has zero or fewer copies in the current stock. In this case, the output "available" is set to false.

```xml
<Scenario name="checkAvailability/ok">
  <Condition>
    <Comparison name="moreThan">
      <Manipulation name="searchAt" type="Integer">
        <Variable name="currentStock"/>
        <Input name="book"/>
      </Manipulation>
      <Constant name="zero" type="Integer"/>
    </Comparison>
  </Condition>
  <Effect>
    <Assignment name="equals">
      <Output name="available"/>
      <Constant name="true" type="Boolean"/>
    </Assignment>
  </Effect>
</Scenario>

<Scenario name="checkAvailability/error">
  <Condition>
    <Comparison name="notMoreThan">
      <Manipulation name="searchAt" type="Integer">
        <Variable name="currentStock"/>
        <Input name="book"/>
      </Manipulation>
      <Constant name="zero" type="Integer"/>
    </Comparison>
  </Condition>
  <Effect>
    <Assignment name="equals">
      <Output name="available"/>
      <Constant name="false" type="Boolean"/>
    </Assignment>
  </Effect>
</Scenario>
```

## 5. Constants

The service has several constants defined. The "available" and "unavailable" constants represent book availability status as Strings, with values "Book1" and "Book2", respectively. The "initialStock" constant is a map containing the initial stock of the library, with book titles as keys and the number of available copies as values. Additionally, the "zero" constant of type "Integer" and "true" and "false" constants of type "Boolean" are used in operation scenarios to compare the current stock quantity and set the output availability value.

```xml
<Constant name="available" type="String">Book1</Constant>
<Constant name="unavailable" type="String">Book2</Constant>
<Constant name="initialStock" type="Map[String, Integer]">
  {Book1=10, Book2=0, Book3=3}
</Constant>

```

## 6. Variables

There is one variable named "currentStock" of type "Map[String, Integer]". It is used to store the current stock of the library, with book titles as keys and the number of available copies as values.

## 7. Conclusion

Together, these components define a simple Library Service that allows users to check the availability of a book in the library stock. The service has a memory to store the initial and current stock, and a single operation with two scenarios to check the book availability. The Finite State Machine captures the service's behavior with a single initial state and self-looping transitions for both operation scenarios.
# Library Service

This XML specification defines a simple Library Service, which enables users to check the availability of a book in the library stock. Let's break down the components of the specification:

## Specification

### 1. Service Outline Skeleton

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

### 2. Finite State Machine

The Finite State Machine (FSM) for this service is very simple, as it has only one state called "Initial". This state is marked as the initial state, meaning the service starts in this state. There are two transitions, "checkAvailability/ok" and "checkAvailability/error", both of which have the same source and target state, "Initial". This means the service remains in the same state after performing the checkAvailability operation, regardless of the outcome.

```xml
<Machine name="LibraryService">
  <State name="Initial" initial="true">
    <Transition name="checkAvailability/ok" source="Initial" target="Initial"/>
    <Transition name="checkAvailability/error" source="Initial" target="Initial"/>
  </State>
</Machine>
```

### 3. Protocol Operations

The service has one operation called "checkAvailability". It has an input parameter named "book" of type String, and an output parameter named "available" of type Boolean. This operation is responsible for checking the availability of the specified book in the library stock.

```xml
<Operation name="checkAvailability">
  <Input name="book" type="String"/>
  <Output name="available" type="Boolean"/>
  <!-- Operation scenarios -->
</Operation>
```

### 4. Operation Scenarios

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

### 5. Constants

The service has several constants defined. The "available" and "unavailable" constants represent book availability status as Strings, with values "Book1" and "Book2", respectively. The "initialStock" constant is a map containing the initial stock of the library, with book titles as keys and the number of available copies as values. Additionally, the "zero" constant of type "Integer" and "true" and "false" constants of type "Boolean" are used in operation scenarios to compare the current stock quantity and set the output availability value.

```xml
<Constant name="available" type="String">Book1</Constant>
<Constant name="unavailable" type="String">Book2</Constant>
<Constant name="initialStock" type="Map[String, Integer]">
  {Book1=10, Book2=0, Book3=3}
</Constant>

```

### 6. Variables

There is one variable named "currentStock" of type "Map[String, Integer]". It is used to store the current stock of the library, with book titles as keys and the number of available copies as values.

### 7. Conclusion

Together, these components define a simple Library Service that allows users to check the availability of a book in the library stock. The service has a memory to store the initial and current stock, and a single operation with two scenarios to check the book availability. The Finite State Machine captures the service's behavior with a single initial state and self-looping transitions for both operation scenarios.

## Validation

The validation report is generated after checking the completeness of the state machine for the LibraryService. It provides information about the state machine, states, and transitions to ensure that every event is explicitly handled by a specified transition in every state. In this case, the validation report confirms that the state machine for LibraryService is complete and all events are appropriately handled.

```xml
<Machine id="0" name="LibraryService">
<Notice id="1" text="Validation report for machine: LibraryService"/>
<State id="2" name="Initial" initial="true">
<Notice id="3" text="Completeness check for state: Initial"/>
<Transition id="4" name="checkAvailability/ok" source="Initial" target="Initial"/>
<Transition id="5" name="checkAvailability/error" source="Initial" target="Initial"/>
</State>
</Machine>
```

Let's break down the generated validation report:

```
<Machine id="0" name="LibraryService">: This line represents the state machine with the name "LibraryService" and assigns an id "0" to it.

<Notice id="1" text="Validation report for machine: LibraryService"/>: This line is a notice that indicates the validation report is for the machine named "LibraryService".

<State id="2" name="Initial" initial="true">: This line represents the state named "Initial", which is the initial state of the machine, and assigns an id "2" to it.

<Notice id="3" text="Completeness check for state: Initial"/>: This line is a notice that indicates the completeness check is being performed for the state named "Initial".

<Transition id="4" name="checkAvailability/ok" source="Initial" target="Initial"/>: This line represents a transition with the name "checkAvailability/ok", having a source and target state "Initial", and assigns an id "4" to it. It means that if the "checkAvailability/ok" event occurs while in the "Initial" state, the state machine will remain in the "Initial" state.

<Transition id="5" name="checkAvailability/error" source="Initial" target="Initial"/>: This line represents a transition with the name "checkAvailability/error", having a source and target state "Initial", and assigns an id "5" to it. It means that if the "checkAvailability/error" event occurs while in the "Initial" state, the state machine will remain in the "Initial" state.

</State> and </Machine>: These lines close the "Initial" state and the "LibraryService" state machine elements, respectively.

The validation report confirms that the state machine is complete because all events are explicitly handled by specified transitions in every state. In this case, both "checkAvailability/ok" and "checkAvailability/error" events have transitions defined in the "Initial" state, ensuring that the state machine handles all possible events.
```

## Verification

The verification process examines the specification for completeness and consistency by analyzing the protocol, memory, and operation scenarios. Here's a step-by-step explanation of the verification process:

Initialization Check: The verification process begins by checking whether the memory is correctly initialized. In this case, it is, and an Analysis node with the text "Memory is correctly initialised" is added to the output.

Input Binding Check: The process then checks if all operation inputs are bound in the scenarios. In this case, there are two warnings generated for the operation "checkAvailability(book)" since the input "book" is not bound in either scenario (checkAvailability/ok and checkAvailability/error).

Determinism Check: Next, the process checks if the operation is deterministic, meaning that for any given input, there is only one possible outcome. The operation "checkAvailability(book)" is found to be deterministic, and an Analysis node is added to the output with the text "Operation is deterministic: checkAvailability(book)".

Input Partitioning: The process then analyzes the input space and determines valid partitions for the input and memory space. In this case, three input partitions are identified:

input 1: moreThan(searchAt(currentStock, book), zero)
input 2: equals(searchAt(currentStock, book), zero)
input 3: lessThan(searchAt(currentStock, book), zero)
Scenario Analysis: The process then analyzes each scenario for the operation "checkAvailability" and determines which input partitions are accepted by each scenario.

Scenario "checkAvailability/ok" accepts input 1 (moreThan(searchAt(currentStock, book), zero))
Scenario "checkAvailability/error" accepts input 2 (equals(searchAt(currentStock, book), zero)) and input 3 (lessThan(searchAt(currentStock, book), zero))
Blocking and Non-determinism Checks: The process checks for any blocking or non-deterministic behavior in the operation. In this case, there are no such issues found.

The verification process concludes with the generation of an XML file containing the analyzed protocol specification, with Notice, Analysis, and Warning nodes as described above. The warnings indicate that the input "book" is not bound in the scenarios, which should be corrected in the specification.

## Test Generation:

Let's break down the XML file step by step:

TestSuite: The root node of the file is the TestSuite, which includes attributes like id, name, testDepth, multiTest, grounding, and metaCheck. It represents the entire test suite generated for the LibraryService specification.

Notice: The Notice node with id="1" contains information about the generated test suite for the LibraryService. It has several child nodes:

a. Analysis: These nodes (id="2" to id="7") provide details about the generated test suite, such as the exploration depth, the number of theoretical, infeasible, redundant, executable, and multi-objective sequences.

b. Warning: Two Warning nodes (id="8" and id="11") indicate that certain transitions were never fired during the test generation process and the specification is not fully covered by the test suite. The analysis in id="12" suggests introducing a new scenario to improve the coverage.

TestSequence: The TestSequence node (id="13") represents a single test sequence within the test suite. It has attributes like state, path, and test. In this case, there's only one test sequence in the suite, with the state set to "Initial," path to "0," and test to "1."

TestStep: Inside the TestSequence node, there is a TestStep node (id="14"), which represents a single step within the test sequence. It has attributes like name, state, and verify. In this case, the TestStep has the name "create/ok," state set to "Initial," and verify set to "true."

a. Operation: The Operation node (id="15") within the TestStep node specifies the operation to be executed in this step, which is "create" in this case.

The generated test suite contains only one test sequence, which consists of a single test step, performing the "create" operation in the "Initial" state. The Notice node provides insights into the test generation process and highlights that the specification is not fully covered, suggesting the need for additional scenarios to improve coverage.

For more info, please visit http://staffwww.dcs.shef.ac.uk/people/A.Simons/broker/index.html.

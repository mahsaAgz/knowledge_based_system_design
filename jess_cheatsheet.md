# Jess is pre-order 
         +
        / \
       3   *
          / \
         2   6

pre-order : + 3 * 2 6  
 in-order : 3 + (2 * 6)


# Basic data types in Jess 

| Jess Type              | Possible Java Types                     |
|------------------------|-----------------------------------------|
| RU.EXTERNAL_ADDRESS    | The wrapped object                      |
| nil                    | A null reference                        |
| TRUE or FALSE          | `String`, `java.lang.Boolean`, or `boolean` |
| RU.ATOM (a symbol), RU.STRING | `String`, `char`, or `java.lang.Character` |
| RU.FLOAT               | `float`, `double`, and their wrappers   |
| RU.INTEGER             | `long`, `short`, `int`, `byte`, `char`, and their wrappers |
| RU.LONG                | `long`, `short`, `int`, `byte`, `char`, and their wrappers |
| RU.LIST                | A Java array                            |



# JESS Syntax
| **Concept**            | **Syntax/Command**                                                                                      | **Description**                                                                                                                                             | **Example**                                                                                                                                  |
|-------------------------|--------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| **Create List**         | `(bind <variable> (create$ <item1> <item2> ...))`                                                      | Creates a plain list.                                                                                                                                       | `(bind ?grocery-list (create$ egg bread milk))`                                                                                              |
| **Access List Element** | `(nth$ <index> <list>)`                                                                                | Retrieves an element from the list (1-based index).                                                                                                         | `(nth$ 2 ?grocery-list)` → `bread`                                                                                                          |
| **Append to List**      | `(bind <new-list> (create$ <old-list> <item1> <item2> ...))`                                           | Combines lists or adds new items.                                                                                                                           | `(bind ?new-list (create$ ?grocery-list salt soap))` → `(egg bread milk salt soap)`                                                          |
| **foreach**             | `(foreach <variable> <list> <expression>+)`                                                           | Loops through each item in the list.                                                                                                                         | `(foreach ?item ?grocery-list (printout t ?item crlf))` → Outputs: `egg bread milk`                                                         |
| **while**               | `(while <condition> do <expression>+)`                                                                | Loops while the condition is true.                                                                                                                          | `(while (> ?count 0) do (bind ?count (- ?count 1)))`                                                                                         |
| **if/then/else**        | `(if <condition> then <expression>+ [else <expression>+])`                                            | Executes different expressions based on a condition.                                                                                                        | `(if (member$ bread ?grocery-list) then (printout t "Buy bread" crlf) else (printout t "Don't buy bread" crlf))`                             |
| **Define Function**     | `(deffunction <name> [<doc>] (<parameters>*) <expressions>* [<return>])`                               | Creates a reusable function.                                                                                                                                | `(deffunction max (?a ?b) (if (> ?a ?b) then (return ?a) else (return ?b)))`                                                                 |
| **Call Function**       | `(<function-name> <arg1> <arg2> ...)`                                                                 | Invokes a defined function.                                                                                                                                 | `(max 10 5)` → `10`                                                                                                                         |
| **Assert Fact**         | `(assert <fact>)`                                                                                     | Adds a fact to working memory.                                                                                                                              | `(assert (car (brand Toyota) (model Corolla)))`                                                                                              |
| **Retract Fact**        | `(retract <fact-id>)`                                                                                 | Removes a fact from working memory.                                                                                                                         | `(retract 1)`                                                                                                                               |
| **Bind Variable**       | `(bind <variable> <value>)`                                                                            | Assigns a value to a variable.                                                                                                                              | `(bind ?x 10)`                                                                                                                              |
| **Reset Working Memory**| `reset`                                                                                               | Initializes the working memory to its original state.                                                                                                       | `reset`                                                                                                                                      |
| **Run Agenda**          | `run`                                                                                                 | Executes all activated rules from the agenda.                                                                                                               | `run`                                                                                                                                        |
| **Facts Display**       | `facts`                                                                                               | Displays the contents of working memory.                                                                                                                    | `facts`                                                                                                                                      |
| **Define Template**     | `(deftemplate <name> <slots>)`                                                                        | Defines a fact structure.                                                                                                                                   | `(deftemplate car (slot brand) (slot model))`                                                                                                |
| **Rule Definition**     | `(defrule <name> <LHS conditions> => <RHS actions>)`                                                  | Creates a rule with conditions and actions.                                                                                                                 | `(defrule example (car (brand ?b)) => (printout t ?b crlf))`                                                                                 |
| **Watch Diagnostics**   | `(watch <event>)`                                                                                     | Enables diagnostics for specific events.                                                                                                                    | `(watch facts)`                                                                                                                              |



### if VS test

| **Aspect**         | **`test`**                                         | **`if`**                                           |
|---------------------|---------------------------------------------------|---------------------------------------------------|
| **Location**        | Used on LHS of rules.                             | Used on RHS of rules or in functions.            |
| **Functionality**   | Filters rule activation based on conditions.      | Executes actions based on conditions.            |
| **Output**          | Does not execute actions; only evaluates.         | Executes specific actions for `then`/`else`.     |
| **Syntax Context**  | `(test <boolean-expression>)`                     | `(if <condition> then <action> [else <action>])` |
| **When to Use**     |When you need to limit rule activation based on custom logic or computed values.| When you need to perform different actions in the RHS or functions depending on conditions|


# Five Primary LHS Conditions
CE := Conditional Element 

| **LHS Condition Type** | **Description**                                                                                      | **Complete Example**                                                                                                      |
|------------------------|----------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| **Pattern CE**         | Matches facts in the working memory based on specific patterns.                                     | **Rule:** Checks if a person is an adult by age. <br> **Jess:** `(defrule is-adult (person (age ?a&:(> ?a 20))) => (printout t "This person is an adult." crlf))`                   |
| **Test CE**            | Evaluates arbitrary expressions that do not directly involve fact pattern matching.                | **Rule:** Adds a bonus to a salary and checks if the total is greater than 100. <br> **Jess:** `(defrule example (person (salary ?salary)) (test (> (+ ?salary 5000) 100)) => (printout t "Salary plus bonus is greater than 100." crlf))` |
| **OR CE**              | Logical disjunction between multiple conditions; rule fires if any condition is true.              | **Rule:** Triggers if a person is either "John" or over 30 years old. <br> **Jess:** `(defrule check-person (or (person (name "John")) (person (age ?age&:(> ?age 30)))) => (printout t "The person is either John or over 30." crlf))` |
| **AND CE**             | Combines multiple conditions that must all be true for the rule to fire.                           | **Rule:** Activates if a person named "Alice" is also an employee. <br> **Jess:** `(defrule employee-alice (and (person (name "Alice")) (employee (name "Alice"))) => (printout t "Alice is an employee." crlf))`                   |
| **NOT CE**             | Specifies that a rule should fire only if a certain pattern does not exist in the working memory.  | **Rule:** Fires if there is no person named "Bob" in the working memory. <br> **Jess:** `(defrule no-bob (not (person (name "Bob"))) => (printout t "There is no person named Bob." crlf))`                                     |

# inference engine types 
Here's the updated table without the example column, focusing on the approach, process description, and typical use of forward and backward chaining in Jess:

| **Chaining Type** | **Approach**   | **Process Description**                                                                                             | **Typical Use**                                                                                                                                                              |
|-------------------|----------------|---------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Forward Chaining** | Data-driven    | Starts with known facts and applies rules to infer new facts iteratively until no more rules apply or a goal is reached. | Ideal for scenarios where all possible conclusions need to be explored as data is added or updated, often used in real-time decision-making systems.                          |
| **Backward Chaining** | Goal-driven    | Begins with a hypothesis or goal and works backwards to find supporting facts or conditions.                           | Useful for verifying specific hypotheses against available data, simulated in Jess through carefully crafted rules that seek specific goals.                                 |
In rule-based systems like Jess (Java Expert System Shell), two principal reasoning strategies can be implemented: **forward chaining** and **backward chaining**. Both are methods for deducing conclusions from a set of facts and rules, but they operate in fundamentally different ways.

### Forward Chaining

**Example:**
1. **Initial Facts:** You start with facts about the weather:
   ```jess
   (assert (weather sunny))
   (assert (temperature high))
   ```
2. **Rules:** You have a rule that suggests activities based on the weather:
   ```jess
   (defrule suggest-activity
     (weather sunny)
     (temperature high)
     =>
     (assert (activity "Go to the beach")))
   ```
3. **Execution:** Given the initial facts, the `suggest-activity` rule would be triggered, leading to the assertion of a new fact `(activity "Go to the beach")`.


### Backward Chaining

**Example:**
1. **Goal:** You want to determine if going hiking is advisable.
2. **Rules:** You might have rules that establish when hiking is advisable based on weather conditions:
   ```jess
   (defrule hiking-advisable
     ?f <- (goal (activity hiking))
     (weather sunny)
     (not (weather-condition severe))
     =>
     (retract ?f)
     (assert (activity-advisable hiking)))
   ```
3. **Execution:** When a query is made if hiking is advisable, Jess checks if the conditions for `hiking-advisable` are met by looking for facts that match the rule's prerequisites.

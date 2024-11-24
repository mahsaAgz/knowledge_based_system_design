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

---

# if VS test

| **Aspect**         | **`test`**                                         | **`if`**                                           |
|---------------------|---------------------------------------------------|---------------------------------------------------|
| **Location**        | Used on LHS of rules.                             | Used on RHS of rules or in functions.            |
| **Functionality**   | Filters rule activation based on conditions.      | Executes actions based on conditions.            |
| **Output**          | Does not execute actions; only evaluates.         | Executes specific actions for `then`/`else`.     |
| **Syntax Context**  | `(test <boolean-expression>)`                     | `(if <condition> then <action> [else <action>])` |
| **When to Use**     |When you need to limit rule activation based on custom logic or computed values.| When you need to perform different actions in the RHS or functions depending on conditions|

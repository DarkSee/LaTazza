# Requirements Document Template

Authors: ["Hip hip"] team 

Date: 26th March 2019

Version: 0.0.1

# Contents

- [Stakeholders](#stakeholders)
- [Context Diagram and interfaces](#context-diagram-and-interfaces)
	+ [Context Diagram](#context-diagram)
	+ [Interfaces](#interfaces) 
	
- [Stories and personas](#stories-and-personas)
- [Functional and non functional requirements](#functional-and-non-functional-requirements)
	+ [Functional Requirements](#functional-requirements)
	+ [Non functional requirements](#non-functional-requirements)
- [Use case diagram and use cases](#use-case-diagram-and-use-cases)
	+ [Use case diagram](#use-case-diagram)
	+ [Use cases](#use-cases)
	+ [Relevant scenarios](#relevant-scenarios)
- [Glossary](#glossary)
- [System design](#system-design)

# Stakeholders


| Stakeholder name  | Description | 
| ----------------- |:-----------:|
|         Employee          |       Person who works in a workplace where LaTazza is used so that he/she can buy capsules     | 
|         Visitor          |       Person who does not work in the company where LaTazza is used but that can buy coffee as well as the employees      |
|         Manager          |       A specialized employee, that can handle the use of LaTazza for supplying and selling capsules      | 
|        Inventory        |        Archive that records all the current amount of capsules available | 
|         Others          |       Persons that are not allowed to use the capsule vending machine      | 
|        Warehouse        |        Place where all capsules are stores and ready to be delivered | 
|        Banking System        |        The system for managing the payment method between manager and warehouse |
|        Capsules           |         Product to be selt/bought  |




# Context Diagram and interfaces

## Context Diagram
```plantuml
left to right direction
skinparam packageStyle rectangle

actor Employee as E
actor Visitor as V
actor Manager as M
actor Warehouse as W
actor Inventory as I
actor "Banking System" as BS
actor "Capsules" as C

rectangle system {
  (LaTazza) as LT
   M -- LT
   E -- LT
   V -- LT
   LT -- I
   LT -- W
   LT -- BS
   C -- LT  
}
```


## Interfaces
| Actor | Logical Interface | Physical Interface  |
| ------------- |:-------------:| -----:|
|       |  |  |

# Stories and personas
\<A Persona is a realistic impersonation of an actor. Define here a few personas and describe in plain text how a persona interacts with the system>

\<Persona is-an-instance-of actor>  \<stories will be formalized later as use cases>


# Functional and non functional requirements

## Functional Requirements

\<In the form DO SOMETHING, or VERB NOUN, describe high level capabilities of the system> <will match to high level use cases>

| ID        | Description  |
| ------------- |:-------------:| 
|  FR1     |  |  
|  FR2     |  |
|  ...     |  |

## Non Functional Requirements

\<Describe constraints on functional requirements>

| ID        | Type (efficiency, reliability, ..)           | Description  | Refers to |
| ------------- |:-------------:| :-----:| -----:|
|  NFR1     |  |  | FR\<x>|
|  NFR2     |  |  | FR\<y>|
|  ...     |  |  | FR\<x>|


# Use case diagram and use cases


## Use case diagram
\<define here UML Use case diagram UCD summarizing all use cases, and their relationships>

## Use Cases
\<describe here each use case in the UCD>

### Use case 1, UC1
| Actors Involved        |  |
| ------------- |:-------------:| 
|  Precondition     | \<Boolean expression, must evaluate to true before the UC can start> |  
|  Post condition     | \<Boolean expression, must evaluate to true after UC is finished> |
|  Nominal Scenario     | \<Textual description of actions executed by the UC> |
|  Variants     | \<other executions, ex in case of errors> |

### Use case 2, UC2

### Use case \<n>


# Relevant scenarios
State at which UC the scenario refers to
\<a scenario is a sequence of steps that corresponds to a particular execution of one use case>
\<a scenario is more formal description of a story>
\<only relevant scenarios should be described>

## Scenario 1

| Scenario ID: SC1        | Corresponds to UC:  |
| ------------- |:-------------:| 
| Step#        | Description  |
|  1     |  |  
|  2     |  |
|  ...     |  |

## Scenario 2

...

# Glossary

\<use UML class diagram to define important concepts in the domain of the system, and their relationships>  <concepts are used consistently all over the document, ex in use cases, requirements etc>
```plantuml
class Manager {
}

class Visitor {
  +name
  +surname
}

class Capsule {
  +IDType
  +pricePerUnit
  +Brand
}

class Box {
  +ID
  +numberOfBoxes
}

class Order{
  +ID
  +status
  +trackingNumber
}

class Wallet {
  +ID
  +balance
}

class Inventory {
   +cashAccount
   +numberOfCapsulePerType
}

Manager --|> Employee
Box "1" o-- "50" Capsule : contains
Order "1..*" o-- "1" Box : made up of
Manager "1" -- "1..*" Order : sends
Employee "1" <|-- "1" Wallet : has
Manager "1" -- "1..*" Visitor : sells to
Manager "1" -- "1..*" Employee : sells to
Manager "1" -- "1" Inventory : manage
```
# System Design
\<describe here system design> <must be consistent with Context diagram>

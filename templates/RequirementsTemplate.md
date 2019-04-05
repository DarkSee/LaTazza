# Requirements Document Template

Authors: ["Hip hip"] team 

Date: 26th March 2019

Version: 0.0.2

# Contents
- [Notes](#notes)
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

# Notes


We assumed that:
- The manager is the only one who interacts with the LaTazza application.
- Clients refer manager in order to required new bevarages.
- The warehouse receives order requests by email, so he doesn't interact with the application.


# Stakeholders


| Stakeholder name  | Description | 
| ----------------- |:-----------:|
|         Employee          |       Person who works in a workplace where LaTazza is used so that he/she can buy capsules     | 
|         Visitor          |       Person who does not work in the company where LaTazza is used but that can buy coffee as well as the employees      |
|         Manager          |       A specialized employee, that can handle the use of LaTazza for supplying and selling capsules      | 
|        Summary       |        Archive that records all the current amount of capsules available | 
|         Others          |       Persons that are not allowed to use the capsule vending machine      | 
|        Warehouse        |        Place where all capsules are stores and ready to be shipped | 
|        Banking System        |        The system for managing the payment method between manager and warehouse |
|        Capsules           |         Product to be sold/bought  |
|        Mail System       |    The system for granted communication between manager and warehouse|



# Context Diagram and interfaces

## Context Diagram
```plantuml
left to right direction
skinparam packageStyle rectangle

actor Manager as M
actor "Mail System/Warehouse ?" as W
actor Summary as S
actor "Banking System" as BS


rectangle system {
  (LaTazza) as LT
   M -- LT
   LT -- S
   LT -- W
   LT -- BS
    
}

note left 
        We haven't included in the Context Diagram "Visitors" and "Employees" 
        as we have assumed that the two entities do not interact with the system.
        We assumed that the manager is the only one who interacts with the LaTazza application.
end note

```


## Interfaces


| Actor | Logical Interface | Physical Interface  |
| ------------- |:-------------:| -----:|
|   Manager     |     GUI       |    Screen , keyboard|
|  Mail System    |     APIs       |Internet connection|
| Banking System | Web service, APIs |Internet connection |

# Stories and personas

**PERSONA1** : THE MANAGER

John Martin
42, manager of a company, divorced

As a manager, John is responsible for managing the sale of bevarages in the company
and for the payments of the capsules.
Currently when employees or visitors want to purchase new capsules, the manager handles the orders. 
Moreover, when the employees want to charge their local account, John updates their balances. 

Goals:
- Manage the summary of capsules available
- Make easier the management of economic transactions with clients and warehouse


It's monday morning.
John arrives in the office and he opens the application. 
He notices that the number of capsules available is not enough for the entire week. 
For this reason he decides to make an order to the warehouse.
The first client of the day is Greg, an employee, that he wants to order 25 capsules of different types. 
Since his local account is empty, he asks to John to charge his account.
So John selects the capsules requested and makes the order.
In this particular day, there are also a lot of visitors interested in the conference held in the company. 
One of them goes to John's office asking for a capsule of coffee.
John manages the order using the application and the visitor pays cash.



# Functional and non functional requirements

## Functional Requirements


| ID        | Description  |
| ------------- |:-------------:| 
|  FR1      | Make an order for capsules                     | 
|  FR1.1    | Manage payments                                |
|  FR1.2    | Send the order to warehouse                    | 
|  FR2      | Sell capsules to clients                       |
|  FR3      | Manage local accounts                          |
|  F3.1     | Add new local account                          |
|  FR3.2    | Add cash to wallet                             |
|  FR3.3    | Decrease wallet balance by selt capsules'price |
|  FR3.4    | Delete local account                           |
|  FR4      | Show the current capsules available            |
|  FR5      | Confirm order delivery for updating the summary|

## Non Functional Requirements


| ID        | Type (efficiency, reliability, ..)           | Description  | Refers to |
| ------------- |:-------------:| :-----:| -----:|
|  NFR1     | Reliability | System downtime should be less than 1 hour per day               | FR1           |
|  NFR2     | Efficiency  | Payment should be managed in less than 1 min                     | FR2, FR3      | 
|  NFR3     | Efficiency  | Order should be comnunicated to warehous in less than half a day | FR1           |
|  NFR4     | Privacy     | Sensitive datas should be preserved                              | FR1, FR2      |
|  NFR5     | Domain      | Payment should be made in euros                                  | FR1, FR2, FR3 |
|  NFR6     | Reliability | Show the correct amount of capsules available at the moment, refresh automatically after each sale | FR4|
|  NFR7     | Efficiency  | Payment should be managed in less than 1 min | FR2|
|  NFR8     | Safeguard   | Debt should be kept above 50€ | FR3 |



# Use case diagram and use cases


## Use case diagram


```plantuml
left to right direction
skinparam packageStyle rectangular

actor warehouse as w
actor manager as m
actor "Banking System" as bs


m -- (sell capsules)
m -- (make an order)
m -- (confirm order delivery)
m -- (show summary)
m -- (add credit to local account)
m -- (add new local account)
m -- (remove local account)

(make an order) -- bs
(make an order) -- w
(make an order) .> (update summary) : include


(sell capsules) .> (accept payment): include
(accept payment) .> (use account balance for employee): include
(accept payment) .> (use cash for visitors) : include
(sell capsules) .> (update summary) : include
(add credit to local account) .> (update wallet) : include

```

## Use Cases

### Use case 1, Make an order

| Actors Involved        |  |
| ------------- |:-------------:| 
|  Precondition     | There are not enough capsules|  
|  Post condition     | Now there are enough capsules|
|  Nominal Scenario     | The manager selects the number of boxes needed, sends an order to the warehouse paying with a credit card.If the operation is successful, an order number is automatically generated and then it will be used by the Manager to confirm that the order is delivered.  The application updates the summary after receiving the boxes  |
|  Variants     | If the delivered order is incorrect, the corresponding value will be refund to the manager. If the payment is unsuccessful, order will be cancelled. |

### Use case 2, Sell capsules

| Actors Involved        |  |
| ------------- |:-------------:| 
|  Precondition     | Client asks for capsules|  
|  Post condition     | Client has got capsules to use|
|  Nominal Scenario     | If the client is an employee, the manager selects the corresponding local account, he selects the payment method, the number of capsules requested. Then he commits the request. The application updates the summary.  |
|  Variants     | If the capsules in the summary are not enough, the application prints an error message. If the debt is higher than threshold, sale will not be performed. |

### Use case 3, Add credit to local account's wallet

| Actors Involved        |  |
| ------------- |:-------------:| 
|  Precondition     | Employees ask for adding credit to their account|  
|  Post condition     | The wallet balance is increased |
|  Nominal Scenario     | Manager takes money from an employee and then add credit to the wallet of the account related to that employee.  |
|  Variants     | The balance can be increased or decreased by a different amount than desired, due to typos. The increased wallet balance can belong to the wrong account. Possibility to undo should be implemented. |

### Use case 4, Add new local account

| Actors Involved        |  |
| ------------- |:-------------:| 
|  Precondition     | A new employee is hired|  
|  Post condition     | The new employee has got his local account|
|  Nominal Scenario     | The manager adds a new account linked to the new hired employee, setting his balance to zero.  |
|  Variants     | If the new emloyee wants to recharge immediately his wallet, the balance will not be set to zero but to the amount charged. |

### Use case 5, Remove local account

| Actors Involved        |  |
| ------------- |:-------------:| 
|  Precondition     | An employee leaves the company|  
|  Post condition     | The local account of the employee is no longer available|
|  Nominal Scenario     | The manager removes the account of the employee.  |
|  Variants     | If the manager selects the wrong ID, he/she has the possibility to revert the operation before doing other actions. |

### Use case 6, Show summary

| Actors Involved        |  |
| ------------- |:-------------:| 
|  Precondition       | Manager doesn't remember how many capsules are left and how much money there is in the cash account|  
|  Post condition     | The manager takes note of the number of capsules avaiable per type and the cash account |
|  Nominal Scenario   | The manager wants to know the number of capsules avaiable per type and the cash account so he press the "Show summary" button in the main Menu of the LaTazza application|
|  Variants           | - |

### Use case 7, Confirm order delivery
| Actors Involved        |  |
| ------------- |:-------------:| 
|  Precondition       | An order is just been delivered to the company|  
|  Post condition     | Summary has been updated with the new capsules amount |
|  Nominal Scenario   | After order delivery, manager confirms the delivery of the order on the application, using the dedicated interface. Then the summary is updated according to the number of capsules received. |
|  Variants           | The number/types of capsules delivered does not correspond to the capsules ordered, so there's a mismatch with the summary infos and the real supplies.|


# Relevant scenarios


## Scenario 1

| Scenario ID: SC1        | Corresponds to UC:  Make an order|
| ------------- |:-------------:| 
| Step#        | Description  |
|  1     | The manager selects the number of boxes needed | 
|  2     | The manager selects the type of capsules|
|  3     | The manager selects the circuit used for the payment with credit card|
|  4     | The manager inserts his name and surname in the corresponfing fields|
|  5     | The manager inserts the credit card number used to pay|
|  6     | The manager inserts the CVV number|
|  7     | The manager enters the expiry month and the expiry year of the credit card used|
|  8     | The manager confirms the order|
|  9     | An order number is automatically generated |
|  10    | The application sends an email with order details to the warehouse|
|  11    | The application update the summary after receiving the boxes|


## Scenario 2

| Scenario ID: SC2        | Corresponds to UC:  Sell capsules to employee|
| ------------- |:-------------:| 
| Step#        | Description  |
|  1     | Manager selects the local account corresponding to the employee | 
|  2     | Maanger selects the number and type of capsules requested by the client |
|  3    |  Manager selects the payment method chosen by the client|
|  4    |  Manger confirms the sale|
|  5     |  The application update the summary |
| 6      | If the employee pays with his wallet, the application updates his balance|


## Scenario 3

| Scenario ID: SC3        | Corresponds to UC:  Add credit to local account's wallet|
| ------------- |:-------------:| 
| Step#        | Description  |
|  1     | Manager selects the local account corresponding to the employee | 
|  2     | Maanger types how much credit the employee wants to add to its account wallet balance |
|  3     |  Manager confirms the action |
|  4     | The application shows a popup that confirms that the action had succes |
|  5     | The application updates the balance related to that account|

## Scenario 4

| Scenario ID: SC4        | Corresponds to UC:  Add new local account|
| ------------- |:-------------:| 
| Step#        | Description  |
|  1     | Manager creates a new local account for the employee just hired|
|  2     | Manager sets the ID of the account with the employee's ID|
|  3     | Manager sets to 0€ the local account balance |

## Scenario 5

| Scenario ID: SC5        | Corresponds to UC:  Remove the local account of an employee  |
| ------------- |:-------------:| 
| Step#        | Description  |
|  1     | Manager selects the corresponding ID of the local account|
|  2     | Manager provides to delete the corresponding local account|

## Scenario 6

| Scenario ID: SC6        | Corresponds to UC:  Show summary  |
| ------------- |:-------------:| 
| Step#        | Description  |
|  1     | In order to know the number of capsules per type available to be sold directly, the Manager selects the "Show Summary" button on the main page of the LaTazza|
|  2     | In order to know the amount of money present in the cash account, the Manager selects the "Show Summary" button on the main page of the LaTazza|


## Scenario 7

| Scenario ID: SC6        | Corresponds to UC:  Confirm order delivery  |
| ------------- |:-------------:| 
| Step#        | Description  |
|  1     | Manager clicks on the interface dedicated to the confirm of the order delivery|
|  2     | Manager writes the order number on the interface, then confirms. |
|  3     | LaTazza adds the number of capsules contained in that order to the summary. |
|  4     | Manager closes the windows and return to the home page. | 

# Glossary


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
}

class Wallet {
  +ID
  +balance
}

class Summary {
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
Manager "1" -- "1" Summary : updates
```
# System Design
```plantuml
top to bottom direction

class "Application-main" {
 +makeOrder()
 +sellCapsules()
 +showTotalBalance()
 +addCreditToWallet()

}

class "Buying interface" {
 +selectTypeOfCapsules()
 +selectNumberOfBoxes()
 +commitOrder()
 +showOrderStatus()
 +updateSummaryAfterDelivery()

}

class "Selling interface" {
 +selectCapsuleToSell()
 +confirmSale&Update()

}

class "Summary interface" {
 +showTotalBalance()
 +showCurrentInventory()

}

class "Account management interface" {
 +createNewAccount()
 +deleteAccount()
 +addCredit()
 +decreaseCredit()
 +showWalletBalance()

} 


"Selling interface" -- "Application-main"
"Summary interface" -- "Application-main"
"Buying interface" -- "Application-main"
"Account management interface" -- "Application-main"
"Account management interface" -- "Selling interface"
```

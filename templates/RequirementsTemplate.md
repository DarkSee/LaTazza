# Requirements Document Template

Authors: ["Hip hip"] team 

Date: 26th March 2019

Version: 0.0.3

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



# Context Diagram and interfaces

## Context Diagram
```plantuml
left to right direction
skinparam packageStyle rectangle

actor Manager as M
note left
        We haven't included in the Context
        Diagram "Visitors" and "Employees" 
        as we have assumed that the two entities
        do not interact with the system.
        We assumed that the manager is the only one
        who interacts with the LaTazza application.
        
        There are no vending machines:
        Manger can handle the distirbution of capsules himself 
end note

actor "Mail System" as MS
actor "Banking System" as BS


rectangle system {
  (LaTazza) as LT
   M -- LT
   LT -- MS
   LT -- BS
    
}


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
|  FR2.1    | Sell capsules to visitors                      |
|  FR2.2    | Sell capsules to employee                      |
|  FR2.2.1  | Sell capsules to employee by cash              |
|  FR2.2.2  | Sell capsules to employee by local account     |
|  FR3      | Manage local accounts                          |
|  F3.1     | Add new local account                          |
|  FR3.2    | Add credit to local account                    |
|  FR3.3    | Decrease wallet balance by selt capsules'price |
|  FR3.4    | Delete local account                           |
|  FR4      | Show summary                                   |
|  FR4.1    | Show the current capsules available            |
|  FR4.2    | Show cash account                              |
|  FR5      | Confirm order delivery for updating the summary|

## Non Functional Requirements


| ID        | Type (efficiency, reliability, ..)           | Description  | Refers to |
| ------------- |:-------------:| :-----:| -----:|
|  NFR1     | Reliability | System downtime should be less than 1 hour per day                                                 | FR1           |
|  NFR2     | Efficiency  | Payment should be managed in less than 1 min                                                       | FR2, FR3      | 
|  NFR3     | Efficiency  | Order should be comnunicated to warehouse in less than half a day                                   | FR1           |
|  NFR4     | Privacy     | Sensitive datas should be preserved                                                      | FR1, FR2      |
|  NFR5     | Domain      | Payment should be made in any currencies (defined while installing)                                | FR1, FR2, FR3 |
|  NFR6     | Reliability | Show the correct amount of capsules available at the moment, refresh automatically after each sale | FR4           |
|  NFR7     | Safeguard   | Debt should be kept below 10€ | FR3 |



# Use case diagram and use cases


## Use case diagram


```plantuml
left to right direction

actor "Mail System" as ms
actor manager as m
note left 
        Clients orally ask to manager
        in order to get new capsules 
end note

actor "Banking System" as bs

m -- (sell capsules)
m -- (make an order)
m -- (confirm order delivery)
m -- (show summary)
m -- (add credit to local account)
m -- (add new local account)
m -- (remove local account)

(make an order) -- bs
(make an order) -- ms
(make an order) .> (update summary) : include
(sell capsules) .> (sell capsules to visitors)
(sell capsules) .> (sell capsules to employee)
(sell capsules) .> (accept payment): include
(accept payment) .> (use account balance for employee): include
(accept payment) .> (use cash for visitors) : include
(sell capsules) .> (update summary) : include
(add credit to local account) .> (update wallet) : include

```

## Use Cases

### Use case 1, Make an order

| Actors Involved        | Manager, Mail System, Banking System |
| ------------- |:-------------:| 
|  Precondition     | |  
|  Post condition     | An order of capsules is done|
|  Nominal Scenario     | The manager selects the number of boxes needed, sends an order to the warehouse paying with a credit card.If the operation is successful, an order number is automatically generated and then it will be used by the Manager to confirm that the order is delivered.  The application updates the summary after receiving the boxes  |
|  Variants     | If the delivered order is incorrect, the corresponding value will be refund to the manager. If the payment is unsuccessful, order will be cancelled. |

### Use case 2, Sell capsules to visitors

| Actors Involved        | Manager |
| ------------- |:-------------:| 
|  Precondition     | Visitor asks for capsules|  
|  Post condition     | Visitor has got capsules to use|
|  Nominal Scenario     | The manager selects the "Visitor" checkbox,the number of capsules requested and the type of capsules. The manager receives the payament by cash,then he commits the request. The application updates the summary.  |
|  Variants     | If the capsules in the summary are not enough, the application prints an error message.  |

### Use case 3, Sell capsules to employee

| Actors Involved        | Manager |
| ------------- |:-------------:| 
|  Precondition     | Employee asks for capsules|  
|  Post condition     | Employee has got capsules to use|
|  Nominal Scenario     | The manager selects the corresponding local account, he selects the payment method (cash or local account), the number of capsules requested. Then he commits the request. The application updates the summary.  |
|  Variants     | If the capsules in the summary are not enough, the application prints an error message. If the debt is higher than threshold, sale will not be performed. |


### Use case 4, Add credit to local account

| Actors Involved        | Manager |
| ------------- |:-------------:| 
|  Precondition     | Employees ask for adding credit to their account|  
|  Post condition     | The wallet balance is increased |
|  Nominal Scenario     | Manager takes money from an employee and then add credit to the wallet of the account related to that employee.  |
|  Variants     | The balance can be increased by a different amount than desired, due to typos. The increased wallet balance can belong to the wrong account. |

### Use case 5, Add new local account

| Actors Involved        | Manager |
| ------------- |:-------------:| 
|  Precondition     | A new employee is hired|  
|  Post condition     | The new employee has got his local account|
|  Nominal Scenario     | The manager adds a new account linked to the new hired employee, setting his balance to zero.  |
|  Variants     | If the new emloyee wants to recharge immediately his wallet, the balance will not be set to zero but to the amount charged. ID with a wrong format, operation refused. |

### Use case 6, Remove local account

| Actors Involved        | Manager |
| ------------- |:-------------:| 
|  Precondition     | An employee leaves the company|  
|  Post condition     | The local account of the employee is no longer available|
|  Nominal Scenario     | The manager removes the account of the employee.  |
|  Variants     | If the manager selects the wrong ID, he/she has the possibility to revert the operation before doing other actions. |

### Use case 7, Show summary

| Actors Involved        | Manager |
| ------------- |:-------------:| 
|  Precondition       | Manager doesn't remember how many capsules are left and how much money there is in the cash account|  
|  Post condition     | The manager takes note of the number of capsules avaiable per type and the cash account |
|  Nominal Scenario   | The manager wants to know the number of capsules avaiable per type and the cash account so he press the "Show summary" button in the main Menu of the LaTazza application|
|  Variants           | - |

### Use case 8, Confirm order delivery
| Actors Involved        | Manager|
| ------------- |:-------------:| 
|  Precondition       | An order is just been delivered to the company|  
|  Post condition     | Summary has been updated with the new capsules amount |
|  Nominal Scenario   | After order delivery, manager confirms the delivery of the order on the application, using the dedicated interface. Then the summary is updated according to the number of capsules received. |
|  Variants           | The number/types of capsules delivered does not correspond to the capsules ordered, so there's a mismatch with the summary infos and the real supplies.|


# Relevant scenarios


## Scenario 1 - Order successfully submitted
 Precondition:   
 Post condition: An order of capsules is done

| Scenario ID: SC1        | Corresponds to UC:  Make an order|
| ------------- |:-------------:| 
| Step#        | Description  |
|  1     | The manager selects the number of boxes needed | 
|  2     | The manager selects the type of capsules|
|  3     | The manager selects the circuit used for the payment with credit card|
|  4     | The manager inserts his name and surname in the corresponfing fields|
|  5     | The manager inserts the credit card number used to pay|
|  6     | The manager inserts the CVV number|
|  7     | The manager enters the expiring month and the expiring year of the credit card used|
|  8     | The manager confirms the order|
|  9     | An order number is automatically generated |
|  10    | The application sends an email with order details to the warehouse|
|  11    | The application updates the summary after receiving the boxes|


## Scenario 2 - Capsules successfully sold to visitors
 Precondition: Visitor asks for capsules  
 Post condition: Visitor has got capsules to use

| Scenario ID: SC2        | Corresponds to UC:  Sell capsules to visitor|
| ------------- |:-------------:| 
| Step#        | Description  |
|  1    | Manager selects the "Vistor" checkbox in the "Sell Capsules" window|
|  2    | Manager selects the number and type of capsules requested by the visitor |
|  3    |  Manager receives the payment from visitor by cash|
|  4    |  Manger confirms the sale|
|  5    |  The application updates the summary |



## Scenario 3 - Capsules successfully sold to employee
 Precondition: Employee asks for capsules  
 Post condition: Employee has got capsules to use

| Scenario ID: SC3        | Corresponds to UC:  Sell capsules to employee|
| ------------- |:-------------:| 
| Step#        | Description  |
|  1     | Manager selects the local account corresponding to the employee | 
|  2     | Maanger selects the number and type of capsules requested by the employee |
|  3    |  Manager selects the payment method chosen by the employee|
|  4    |  Manger confirms the sale|
|  5     |  The application updates the summary |
| 6      | If the employee pays with his wallet, the application updates his balance|



## Scenario 4 - Local account successfully recharged
 Precondition: Employees ask for adding credit to their account  
 Post condition: The wallet balance is increased

| Scenario ID: SC4        | Corresponds to UC:  Add credit to local account|
| ------------- |:-------------:| 
| Step#        | Description  |
|  1     | Manager selects the local account corresponding to the employee | 
|  2     | Maanger types how much credit the employee wants to add to its account wallet balance |
|  3     |  Manager confirms the action |
|  4     | The application shows a popup that confirms that the action had succes |
|  5     | The application updates the balance related to that account|

## Scenario 5 - New local account is created
 Precondition: A new employee is hired  
 Post condition: The new employee has got his local account

| Scenario ID: SC5        | Corresponds to UC:  Add new local account|
| ------------- |:-------------:| 
| Step#        | Description  |
|  1     | Manager creates a new local account for the employee just hired|
|  2     | Manager sets the ID of the account with the employee's ID|
|  3     | Manager sets to 0€ the local account balance |

## Scenario 6 - Local account is removed
 Precondition: An employee leaves the company  
 Post condition: The local account of the employee is no longer available

| Scenario ID: SC6       | Corresponds to UC:  Remove the local account of an employee  |
| ------------- |:-------------:| 
| Step#        | Description  |
|  1     | Manager selects the corresponding ID of the local account|
|  2     | Manager provides to delete the corresponding local account|

## Scenario 7 - Summary is successfully printed
 Precondition: Manager doesn't remember how many capsules are left and how much money there is in the cash account  
 Post condition: The manager takes note of the number of capsules avaiable per type and the cash account

| Scenario ID: SC7        | Corresponds to UC:  Show summary  |
| ------------- |:-------------:| 
| Step#        | Description  |
|  1     | In order to know the number of capsules per type available to be sold directly, the Manager selects the "Show Summary" button on the main page of the LaTazza|
|  2     | In order to know the amount of money present in the cash account, the Manager selects the "Show Summary" button on the main page of the LaTazza|


## Scenario 8 - Summary successfully updated after delivery
 Precondition: An order is just been delivered to the company  
 Post condition: Summary has been updated with the new capsules amount

| Scenario ID: SC8        | Corresponds to UC:  Confirm order delivery  |
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

class Employee {
  +ID
  +balance
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

class Summary {
   +cashAccount
   +numberOfCapsulePerType
}

Manager --|> Employee
Box "1" o-- "50" Capsule : contains
Order "1" o-- "1..*" Box : made up of
Manager "1" -- "1..*" Order : sends
Manager "1" -- "1..*" Visitor : sells to
Manager "1" -- "1..*" Employee : sells to
Manager "1" -- "1" Summary : updates
```
# System Design
```plantuml
top to bottom direction


class LaTazzaSystem {
}

class Computer {
     +makeOrder()
 +sellCapsules()
 +showTotalBalance()
 +addCreditToWallet()
 +selectTypeOfCapsules()
 +selectNumberOfBoxes()
 +commitOrder()
 +showOrderStatus()
 +updateSummaryAfterDelivery()
  +selectCapsuleToSell()
 +confirmSale&Update()
  +showTotalBalance()
 +showCurrentInventory()
 +createNewAccount()
 +deleteAccount()
 +addCredit()
 +decreaseCredit()
 +showWalletBalance(

}
LaTazzaSystem o-- Computer


class MailGateway {
  +sendOrder()
}

Computer -- MailGateway
```

## System Diagram

Ex. 
Database to store messages
App/ browser
web server
API gateway

```mermaid 
flowchart

browser --> web_server
API --> account_service
API --> message_service
API --> feedback_service
mobile --> API
web_server --> API

message_service --> message_DB
account_service --> account_DB
DB:

```

## Sequence diagram
![[Pasted image 20231010145559.png]]

made of loops and alt

alt - like if else branches 
loop - repeat blocks

SD edit, functions specify transitions


## Flow Chart

```mermaid
flowchart TB
	start([terminal])
	step1[processstep]
	start --> step1
	branch{?}
	step1 --> branch
	input1 --> branch
	branch --> yes --> step2
	branch --> no --> step3
	stop([terminal])
	step2[process step] --> stop
	step3[process step] --> stop
	input1[/input/]
```



## Model View Controller (MVC)
1. well defined boundaries, separation of concerns
2. easy to change
3. multiple presentation styles, easy to support

```mermaid
flowchart TB
	controller <--> view
	model <--> controller
	data <--> model
	data[(data)]
	view["view \nhow data is presented"]
	model["model \n how data is stored"]
	User((User))
	controller <--> User
```


``` mermaid
sequenceDiagram
    actor user
	user->>controller: HTTP GET student.html/students/123
	controller->>model: getStudent(id=123)
	activate view
	model->>+data: SELECT *from STUDENTS where id=123
	data-->>-model: student rows or none
	model-->>controller: student or null
	alt student found
		controller->>+view: showstudent(student)
		view-->>-controller: student.html
	else 
		controller->>+view: studentnotfound(student)
		view-->>-controller: error.html
	end
	controller->>user: HTTP response, student.html, error.html
```

## UML Class Diagrams

``` mermaid
---
title: Student
---
classDiagram
    class Student
    Student : +String name
    Student : +double gpa
    Student : +Courses courses[0...*]
    Student : +addClass(amount)
    Student : +dropClass(amount)
```

```mermaid
classDiagram
classA --|> classB : Inheritance
classC --* classD : Composition
classE --o classF : Aggregation
classG --> classH : Association
classM ..|> classN : Realization
classK ..> classL : Dependency

classI -- classJ : Link(Solid)
classO .. classP : Link(Dashed)
```

```mermaid
classDiagram
class University
class Departments
class Employees
University--*Departments
Employee--o Departments
```



###### Multiplicity
	1
	1...3
	0...*
	1...*

###### Access Modifiers
	+ public
	- private
	# protect
	~ package
	Class [0...*]

Inheritance (Is a type of)
	Causes coupling 
Composition (Is made of)
Aggregation (Contains, has)
	**composition**: university is composed of departments, but departments cannot exists without a university
	**aggregation**: department is aggregation of employees, but employees can exist outside of a department
Association (can invoke a behavior, can call a method)

# Design Principles
1. Favor composition "has a" over "is a", composition over inheritance
2. Program to an interface not an implementation
3. Identify parts of your code that stay the same and those that are likely to change and isolate them from one another


```mermaid
classDiagram
class FlightBehaviour
	FlightBehaviour: +fly()
class FlightByFlapping
	FlightByFlapping: +fly()
class FlightByTossing
	FlightByTossing: +fly()
class FlightByEngine
	FlightByEngine: +fly()

FlightByFlapping ..|> FlightBehaviour
FlightByTossing ..|> FlightBehaviour
FlightByEngine ..|> FlightBehaviour


class QuackBehavior
	QuackBehavior: +quack()
class Quack
	Quack: +quack()
class Squeak
	Squeak: +quack()
class Thump
	Thump: +quack()

Quack ..|> QuackBehavior
Squeak ..|> QuackBehavior
Thump ..|> QuackBehavior

class Duck
	Duck: FlightBehaviour
	Duck: +fly()
	Duck: +quack()
Duck --> FlightBehaviour: Association
Duck --> QuackBehaviour: Association

class Mallard
	Mallard: flightBehaviour
class Mandarin
	Mandarin: flightBehaviour
class Common
	Common: flightBehaviour
class Rubber
	Rubber: flightBehaviour
Mallard --|> Duck
Common --|> Duck
Mandarin --|> Duck
Rubber --|> Duck

```


# Observer
Publisher and subscribers of data
Subject and Observer 

subject
	allow me to notify observer when there's change
observer 
	detailed implementation on what type of data to ask for
	list of observers

```mermaid 
classDiagram
class WeatherData
	WeatherData: getTemp()
	WeatherData: getPressure()
	WeatherData: Humidity()
	WeatherData: newDataAvailable()
WeatherData --|> CurrentConditionDisplay: Updates
WeatherData --|> ForecastDisplay: Updates
WeatherData --|> StatisticsDisplay: Updates
class CurrentConditionDisplay
	CurrentConditionDisplay: update()
class StatisticsDisplay
	StatisticsDisplay: update()
class ForecastDisplay
	ForecastDisplay: update()
```

``` java
public class WeatherData(){
	private Display currentConditions //concrete classes
	private Display statistics 
	private Display forcast
	
	public void newDataAvailable(){
		float temp = getTemperature()
		float humidity = getHumidity()
		currentCondition.update(temp, humidity)
		statistics.update(temp, humidity)
		forecast.update(temp, humidity)
	}
} 
```

```java

```
# Decorator

have to change boolean for everything, then cost and description needs to be changed everytime
``` java
class Beverage{
	Boolean nonfat
	Boolean decaf
	Boolean extrafoam
	public Double getCost(){
	
	}
	public String getDescription(){
	
	}
	
}
```


``` mermaid
classDiagram
class Beverage
class Houseblendfatnondairdecaf
class Houseblendfatnondairy
class Houseblendfatdecaf
class nondairdecafextrafoam
```

```mermaid
flowchart 
subgraph blend
subgraph dairy
subgraph foam

end
end
end
```

```mermaid
classDiagram 
class Beverage
	Beverage: cost()
class HouseBlend
	HouseBlend: cost()
class AddOnDecorator
	AddOnDecorator: cost()
AddOnDecorator--|>Beverage
HouseBlend--|>Beverage
class Vanilla
	Vanilla: cost()
class Sugarfree
	Sugarfree: cost()
Vanilla --|> AddOnDecorator
Sugarfree --|> AddOnDecorator
```

```java
Beverage hb = new HouseBlend()
Beverage hbsugarfree = new Sugarfree(hb)

```


``` mermaid
sequenceDiagram
    actor user
	user->>controller: HTTP POST keywords.html/keywords/id
	controller->>model: getKeywords(query.data.text)
	activate view
	model->>+data: model.eval(data.text)
	data-->>-model: keyWordList
	model-->>controller: response.json = keyWordList
	controller->>+view: boldKeywords(keyWordList, paragraph)
	view-->>-controller: paragraph
	controller->>user: paragraph
```

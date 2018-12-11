







Movie Store Inventory Manager
Design Document





Team #7: Taco
Melroy Ranon D'Souza
Allen Fulmer
Ginger A Rabun
Daniel L Yan
Table of Contents
1. Overview	3
2. Design Requirements	3
2.1 Input Files	3
‘data4movies.txt’	3
‘data4customers.txt’	4
‘data4commands.txt’	4
2.2 Movie Genres	5
Comedy (‘F’ for funny)	5
Drama (‘D’)	5
Classics (‘C’)	5
2.3 System Actions	5
Borrow (‘B’)	5
Return (‘R’)	6
Inventory (‘I’)	6
History (‘H’)	6
3. UML Diagram	7
3.1 Class Diagram	7
3.2 UML Alternative Design	8
4. Class Interactions	9
4.1 Class Descriptions	10
MovieStore?	10
CommandParse	10
Customer	11
Operation	11
Return	11
Borrow	11
History	11
Inventory	12
MovieFactory	12
ComedyFactory	12
DramaFactory	12
ClassicFactory	12
Movie	12
ComedyMovie	12
DramaMovie	12
ClassicMovie	13
4.2 Use Cases	13
Check Inventory	13
Access Customer History	13
Borrow a Movie	13
Return a Movie	13
4.3 Program Flow	13
5. Error Handling	15
6. Main Description	15
7. Header Files	15
8. Future Extensions	17

1. Overview
A local movie rental store wants an automated system that tracks inventory and customer transactions. We have been hired to design this system to meet all of the requirements outlined by the owners. 
Currently, only three genres of movies will be tracked: Comedy, Drama, and Classic. Movies from each genre will be identified by different attributes: Comedies by their Title and Release Year; Dramas by their Director and Title; Classics by their Release Date and Major Actor. 
Four types of actions will be supported in the system: Borrow a movie that is in stock, Return a movie that was borrowed, display current Inventory, and display customer transaction History (borrows and returns). 
The data for the system will be supplied by three text files: data4movies.txt, containing the movies in stock; data4customers.txt, containing the list of registered customers; and data4commands.txt, containing the sequence of commands to be performed.
2. Design Requirements
2.1 Input Files
‘data4movies.txt’
Purpose
	Used to populate the collection of movies before commands are processed.
Format
	Genre, NumInStock, Director, Title, ReleaseYear
	Genre, NumInStock, Director, Title, MajorActor ReleaseMonth&Year
Examples
F, 10, Nora Ephron, You've Got Mail, 1998
D, 10, Steven Spielberg, Schindler's List, 1993
C, 10, George Cukor, Holiday, Katherine Hepburn 9 1938
‘data4customers.txt’
Purpose
	Used to populate the collection of customers before commands are processed.
Format
	IDNumber LastName FirstName
Examples
	1111 Mouse Mickey
	1000 Mouse Minnie
‘data4commands.txt’
Purpose
	Provides the sequence of commands to process.
Format
	I
	H IDNumber
	B IDNumber MediaType Genre Director Title
	B IDNumber MediaType Genre ReleaseMonth&Year MajorActor
	B IDNumber MediaType Genre Title ReleaseYear
R IDNumber MediaType Genre Director Title
	R IDNumber MediaType Genre ReleaseMonth&Year MajorActor
	R IDNumber MediaType Genre Title ReleaseYear
Examples
	I
	H 1000
	H 8000
	B 8000 D F You've Got Mail, 1998
	B 1000 D D Barry Levinson, Good Morning Vietnam,
	R 1000 D C 5 1940 Katherine Hepburn
2.2 Movie Genres
Comedy (‘F’ for funny)
Sorting Attributes
Title
Release Year
Example Commands
	B 9999 D F You've Got Mail, 1998
	R 8888 D F Annie Hall, 1977
Drama (‘D’)
Sorting Attributes
Director
Title
Example Commands
B 1000 D D Gus Van Sant, Good Will Hunting,
R 7777 D D Steven Spielberg, Schindler's List,
Classics (‘C’)
Sorting Attributes
Release Month & Year
Major Actor
Example Commands
B 1000 D C 3 1971 Ruth Gordon
R 4444 D C 2 1971 Malcolm McDowell
2.3 System Actions
Borrow (‘B’)
Description
If successful, the Movie Store stock is decreased by 1 and the transaction is recorded in the customer’s history. To be successful, the customer ID# must exist, the movie must exist and it must have a positive stock.
Command Format
	B CustomerID MediaType MovieType [movie sorting attributes]
Return (‘R’)
Description
If successful, the Movie Store stock is increased by 1 and the transaction is recorded in the customer’s history. To be successful, the customer ID# must exist, the movie must exist and it must be currently borrowed by the customer.
Command Format
	R CustomerID MediaType MovieType [movie sorting attributes]
Inventory (‘I’)
Description
Prints a list of every movie and how many copies are currently in stock. Comedy movies are printed first, sorted by Title, then Release Year; Drama movies are printed second, sorted by Director, then Title; Classic movies are printed last, sorted by Release Month and Year, then Major Actor.
Command Format
	I
History (‘H’)
 Description
If successful, prints a list of every transaction (borrows and returns) made by a customer. To be successful, the customer ID# must exist.
Command Format
	H CustomerID
3. UML Diagram

4. Class Interactions
MovieStore
Contains all classes below
Customer
Single class, used by HashTable, Operation

HashTable
	Single class, used by Customer

Movie
	Is implemented by MovieFactory, used by Operation

MovieFactory
	Is implemented by ComedyFactory
	Is implemented by DramaFactory
	Is implemented by ClassicFactory

Operation
	Is implemented by Borrow
	Is implemented by Return
	Is implemented by Inventory
	Is implemented by History

Hierarchy view:

Customer
HashTable
Movie
	MovieFactory
		ComedyMovieFactory
			ComedyMovie
		DramaMovieFactory
			DramaMovie
		ClassicMovieFactory
			ClassicMovie
Operations
	Return
	Borrow
	History
	Inventory

4.1 Class Descriptions
Customer
HashTable has been initialized and contains Customer objects are created through the read in string from readLine. Error handling is performed through readLine.
Movie
Movie objects have been created and a movie map has been initialized. Movie objects are created through the read in string from readLine through self-registering factories and polymorphism. Error handling is performed through readLine.
Operation
Reads in the command file for all commands and checks if a hashtable of Customer exists and a map of Movie exists. Runs all system actions based on hashtable of Customer and map of Movie. Error handling is performed through readLine.
Return
Takes in all necessary parameters for changing each movie type based on Movie object (Comedy, Drama, and Classic), and modifies object as necessary (primarily stock).
Borrow
Takes in all necessary parameters for changing each movie type based on Movie object (Comedy, Drama, and Classic), and modifies object as necessary (primarily stock).
Inventory
Takes in a static map of Movie objects and sorts it based on Movie type through custom sorting. This is used for accessing and printing out all objects.
History
Takes in a transaction string from the read commands and appends it as a string var if it’s valid. 

4.2 Program Flow
Program Flow
ass4.cpp - creates a MovieStore object globally
readLine() occurs in Customer, which then HashTable is populated by Customer*
readLine() occurs in Movie, which then creates factories by passed in line
readLine() occurs in Operations, which then also has a static map passed from Movie
All actions are then read from readLine() from Operations and actions are performed
Printing all outputs necessary

Reading Order
1. data4movies.txt
2. data4customers.txt
3. data4commands.txt 
5. Error Handling
- Create enums to specify movie types and possible actions (set can be an alternative)
- Ensure formatting has to match up (i.e. must be string, int, string. string…)
- Store incorrect files in an error map<string, string> (could be vector<string>)
- readLine() functions all contain error handling
- Print out error messages regarding incorrect inputs
6. Main Description
Call readLines function in Customer, Movie, and Operations. Refer to Program Flow for additional function calls.
7. Header Files
MovieStore
Super class holding all classes below
Customer
Members
	Int ID
	String name
Methods
getHistory():bool
addHistory(string, int): void
validTransaction(string): bool
readLine(string): void
Operation
Members
Movie * movieType
Methods
getMovieType(map<string, Movie*>): Movie *
create(Movie *): Operation *
readLine(string): void
validCommand(string): bool
Return
Members
Int stock
String title
String director
Int month
Int year
Movie * movieType
Methods
changeMovie(Movie *): void
Borrow
Members
Int stock
String title
String director
Int month
Int year
Movie * movieType
Methods
changeMovie(Movie *): void
History
Members
	String transaction
Bool valid
Methods
checkIfValid(string): void
printHistory() : void
Inventory
Methods
printInventory() //goes to each movieFactory and calls their printInventory methods.
MovieFactory
Methods
create(): Movie * // each Factory subclass implements
registerType(): Movie // each Factory subclass implements
ComedyFactory
Members
map<string, Comedy *>
Methods
create(): Movie *
registerType(): Movie
DramaFactory
Members
map<string, Drama *>
Methods
create(): Movie *
registerType(): Movie
ClassicFactory
Members
map<string, Classic *>
Methods
create(): Movie *
registerType(): Movie
Movie
Members
String command
Int stock
String title
String director
Int year
InventoryItem * movieType
Methods
readLine(string):void
Static movieMap(string): map<string, Movie *>
Comedy

Methods
+ getTitle(): string
+ getYear(): int
+ canBorrow(): bool
+ canReturn(): bool
Drama

Methods
+ getTitle(): string
+ getYear(): int
+ canBorrow(): bool
+ canReturn(): bool
Classic
Methods
+ getTitle(): string
+ getYear(): int
+ canBorrow(): bool
+ canReturn(): bool
8. Future Extensions
Design Questions
1. Can your design be extended beyond the specifications given here?
2. Could you easily add new videos or DVDs to your design?
3. Can you easily add other categories of videos or DVDs?
4. Could you easily add new categories of media to your design, for example, music?
5. Could you expand to check out other kinds of items, for example VCRs or DVD players?
6. Could you easily add new operations to your design?
7. Could you incorporate time, for example, a due date for borrowed items?
8. Could you easily add an additional store, or handle a chain of stores?

Future Design:
Design can easily be extended by specifications given here. We would change MovieFactory into MediaFactory and Movie into Media. Where Media would be a generic interface that has very limited member items for all media types (as DVD, VCR, CD, etc. do not use the same member parameters). 
New videos or DVDs could easily be added, they would be simply be added under Media and have appropriate factories created for each object.
Each new category would be added through factory creation interfaces.
New categories of Media are added through an additional MediaFactory types.
Checkouts could easily be added through Media, as it encompasses any media types which then have respective factories made and objects created for it.
New operations are easily added by another class in the Operations interface
Time would be incorporated as another parameter in Borrow/Return operations. Alternatively we would abstract Borrow/Return further down into Media types (such as DVDBorrow) which would have different parameters.
Another store could easily be added as MovieStore is the parent interface

Example of a future design flow

Customer
HashTable
Operations
	Return
	Borrow
	History
	Inventory
Movie
	Factory & Object Creation
DVD
	Factory & Object Creation
Music
	Factory & Object Creation

Example of Factory & Object Creation
	MusicFactory
		PopFactory
		LiveConcertFactory
		OperaFactory
Music
		Pop
		LiveConcert
		Opera

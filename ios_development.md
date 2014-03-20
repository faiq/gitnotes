#iOS development
##Lecture 1
***
- Properties are our instance variables we have auto setters and getters for them.

- Every class has a header (.h) and implementaiton file (.m)

- (.h) - public API 

- (.m) - private API and implementaion 


- in (.h) you must show super class

		@interface Card : NSObject
		
 	in form @interface <name> :  < what we inheret from>
 
 
 - all private implementation wrapper around this guy 
 
 		@implementation Card
 	
 - in implementation, import your header. 
 
 - we could have private interface in our (.m) files. - we use it mostly for properties. 
 

- in our (.h) file 

		@property (strong) NSString *contents
	whats on the card. 
	
- all objects live on the heap (where you allocate the memory)

- strong property - keep the memory in the heap as long as there is a pointer to it (refrence counting)

- weak property - keep it as long as there is a strong pointer to it, if theres not it will point it to nil 



 - nonatomic - means not thread safe. 
 
 - the @property makes the getters and setters for you 
 
 - the _property is the storage space for the property  
 
##Lecture 2 
***


- Method names in objective C (multiple arguments)

		- (void) addCard:(Card *)card atTop:(BOOL)atTop
		
	name is addCard atTop 
	
	if we dont want this guy to have the atTop we need to declare a new method (can be the same name)
#Using Jquery
*** 
##What does JQuery allow you to do?
- allows elements inside the DOM (Document Object Model) to be manipulated once the webpage is done loading 

##How does it accomplish such a feat? 
- Jquery searches through your html document and looks for what element you are trying to manipulate
		
		$('li).text("Faiq"); 
	This changes the text of every li object to Faiq

- You can also use CSS selectors to selec which elements to manipulate 

		$('#container').text(); 
	Allows you to get the text from the element by the id name of container 
	
		$('.article').text(); 
	Allows you to get the text from **all** elements by the class name of article
	
###Other selectors  

- Uses same selectors as CSS 

- Allows you to grab descendants directly from a html tag/id/selector 

		$('#destnations li')
		
	This selects all li elements that are descendants of the id destinations. Note that this will grab direct children and indirect children as well. 
	
- You can use CSS selectors here as well (to select direct children)

		$('#destinations > li')
		
	Will grab only the direct descendants of the id destination.  

- You can also grab multiple things at once! 

		$(".promo, #france"); 
	Grab items with the class promo and the id france 

- You can grab elements using pseudo selectors as well 

		$("#destnations li:first"); 
	Goes to the class destinations and picks out the first li element for you 

###Traversing to find what you need

- You can traverse through the DOM to find what you're looking for 
		
		$("#destinations").find("li"); 
		
	Select something and then find what you're looking for with the find method 
	
- Traversing up the DOM
		
		$('li').first.parent();
	Gets you the first lists items parent 

- Traversing down the DOM 

		$('ul').children('li');
	Gets you only the direct children that are li elements.

##Manipulating the DOM
We want to add thigns to our web page: How do we do this 

1. Make a new DOM node	 
	
		var price = $('<p>From $399.99</p>);
	Creates a Node but doesn't add it to the DOM   

2. Use a jQuery method to add it to the DOM 
		
		.append(<element>);
		.prepend(<element>);
		.after(<element>);
		.before(<element>);
		
3. You can manipulate based on interaction as well.
	- listen fo an event and run some code 
	
		```
			$(SELECTOR).on(EVENT, function (){})
		```
###Specifing Where to Manipulate the DOM
- Use the "this" keyword to specify the item triggering the event

```
	$('button').on('click', function(){
		$('this).remove();
	});
	
```
On the button click only that button you click willbe removed rather than all buttons with the class button 


##Some jQuery Methods to help you find what you're looking for 
- closeset method

		.closest( selector )
	will traverse up the dom and find the css selector that is closest to the jquery object you call it on 
	

- filtering method 
	
		$('').filter('')
		
	will filter through the html elements on the left and give you the ones that also have that property on the right 

-add Class method
	
	$('').addClass('.ClassName'); 
	


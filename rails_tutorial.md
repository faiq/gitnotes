#Rails tutorial Notes/Outline

#Chapter 2 
***
##Modeling data
- When creating any application we want to figure out how we're going to organize the data in our app. (users, posts, etc)
		
		$rails generate scaffold User name:string email: string 
	This command creates a new "User" resource, and it allows us to think of users as objects that can be read, updated, and deleted. The "name:string" and "email:string" are parameters of the resource. 
	

- Next we need to update our database with our new "User" model 

		$bundle exec rake db:migrate
		
##MVC
- Model View Controller is the schematic rails will be using to get us our data and display it on the webpage. 
	- How it works: 
		- browser makes a request to the page 
		- router figures out what controller to pass you along to and sends you to the index of that controller 
		- the controller then pulls out information from the model 
		- the controller then sends the users to be rendered in a view 
		- this view is passed back to the controller which sends it back to the browser  

#Chapter 3
***
##Mostly static pages 
- We're going to first make a page with actions and views (right now with static html)
- rails actions come bundled together inside controllers, which contains actions that serve a common purpose. A controller is a container for a group of webpages
- So lets generate a controller
		
		$rails generate controller StaticPages home help --no-test-framework
	
- this will update the routes file in the config directory
	to look like the following 
		
		SampleApp::Application.routes.draw do
  			get "static_pages/home"
  			get "static_pages/help"
  			.
  			.
  			.
		end
- whenever the client makes a get request the route file will map requests for the static_pages/home to the home action in the static_pages controller 

- taking a look inside the controller 

		class StaticPagesController < ApplicationController
  		
  			def home
  			end

  			def help
  			end
		end
	- what a controller usually does is take things from a model, stores them, passes them along to a view to be rendered, gets the result, and brings it back to the browser.
	- in this instance it has no model so it just gets a rendered view and returns it to the browser 
	
##Test driven development 

###Idea: Write the test first, then implement code to get it to pass 
- To write the tests in rails we will be using rspec 

		$rails generate integration_test static_pages
	Creates our integration test (request spec) for our static pages
	
- Taking a look at this file location: (/spec/requests/static_pages.rb)

- Note: this is not generated from the computer 

```
require 'spec_helper'

describe "Static pages" do

  describe "Home page" do

    it "should have the content 'Sample App'" do
      visit '/static_pages/home'
      expect(page).to have_content('Sample App')
    end
  end
end
```

- this code is written in domain specific language. It is designed for human readibiliy. 
- the describe home page line tells other programmers/you what it is going to be testing. 
- "    it "should have the content 'Sample App'" do
" simply tells what the text expects. This is also done for human readibility.
- The lines 
		 
		 visit '/static_pages/home'
      	 expect(page).to have_content('Sample App')
    - simply use Capybara functions to visit that webpage and check to see if they actually have the content 'Sample App'

- To test it out use the line 

		bundle exec rspec spec/requests/static_pages_spec.rb
- Made test for home, make test for help, make test for about. 

##Dynamic webpages
- We could add ruby script to our webpages to make them more dynamic.

- To execute ruby we use <% ... %> like such

		 <% provide(:title, 'About Us') %>
	this code executes the provide function and associates the string title with the string 'About us'

- To execute and insert ruby into the template we use <%= ... %>

		<%= yeild(:title) %> 

#Chapter 4
***

- Rails allows for the creation of helper methods just place them in app/helpers/application_helper.rb

###Strings in ruby

1. String interpolation - we could use interlopation to quickly concatinate strings. 

	- Example
		
			first = "Faiq"
			last = "Raza"
			"#{first} #{last}"
		this returns the string "Faiq Raza"  

###Arrays in Ruby

1. Arrays in Ruby are similar to other languages. use [ ] for array access

2. Array Methods - Ruby has some built in methods for adding to an array.

	- Split method - splits a string into an array by the argument you give it.
	
			"fooxbarxbazx".split('x') => ["foo", "bar", "baz"] 

	- .first, .second, .last - for access in an array 
	
	- .sort
	
	- .push() - used to add to the end of an array 
	

###Blocks in Ruby 

- Example 
			
			(1..5).each do
				puts 2 * i
			end
	this will return (2, 4, 6, 8, 10) 
	
- We could also use the .each method to itterate across a range or array.

###Hashes 

- A hash is an associative collection of information of information. Kind of like a JSON object  

- Example 

```
flash = { success: "It worked!", error: "It failed." }

flash.each do |key, value|
   puts "Key #{key.inspect} has value #{value.inspect}"
end

Returned Results
Key :success has value "It worked!"
Key :error has value "It failed."


```
- CRUD methods - Create, Update, Read, Destroy

###CSS with Rails 

- inside the file application.html.erb, the file that helps us build the html for our views we had the line 

		<%= stylesheet_link_tag "application", media: "all",
                                           "data-turbolinks-track" => true %>
                                           
    What this line ends up doing is calling the method stylesheet_link_tag and passes the arguments application, which is the path to the stylesheet, and a hash with two elements. The first key value pair tells to include all media and to use the turbo links feature 

###Ruby classes 

- Everything in Ruby is an instance of a class 
- We can write to main ruby classes like string. 

####Our own user class 




			
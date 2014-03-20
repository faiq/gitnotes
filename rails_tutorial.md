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
example_user.rb
//completely useless 

#Chapter 5
***

##Adding in some css to your page 

- The tutorial first goes into adding a general nav-bar to the top of the page. 

- They accomplish this by editing the application.html.erb file in app/view/layouts

```
<!DOCTYPE html>
<html>
  <head>
    <title><%= full_title(yield(:title)) %></title>
    <%= stylesheet_link_tag "application", media: "all",
                                           "data-turbolinks-track" => true %>
    <%= javascript_include_tag "application", "data-turbolinks-track" => true %>
    <%= csrf_meta_tags %>
    <!--[if lt IE 9]>
    <script src="http://html5shim.googlecode.com/svn/trunk/html5.js"></script>
    <![endif]-->
  </head>
  <body>
    <header class="navbar navbar-fixed-top navbar-inverse">
      <div class="navbar-inner">
        <div class="container">
          <%= link_to "sample app", '#', id: "logo" %>
          <nav>
            <ul class="nav pull-right">
              <li><%= link_to "Home",    '#' %></li>
              <li><%= link_to "Help",    '#' %></li>
              <li><%= link_to "Sign in", '#' %></li>
            </ul>
          </nav>
        </div>
      </div>
    </header>
    <div class="container">
      <%= yield %>
    </div>
  </body>
</html>
```

- in this file we added some html related to bootstrap. (formatted normally)
- we used some embeded ruby to add in the links to the Home, Help, and sign in. 

- Then we added in some html in our home page 
(This is all from the home page, all the other stuff is basic bootstrap/css/html)
- Something interesting to note:

		  <%= link_to "Sign up now!", '#', class: "btn btn-large btn-primary" %>
here we used the link to add a link to a button... so link_to works for more than hyperlinks 

		<%= link_to image_tag("rails.png", alt: "Rails"), 'http://rubyonrails.org/' %>
here we used it to display the rails website to the rails image. 
 


- Gemfile stuff 
		
		gem 'bootstrap-sass', '2.3.2.0'
add that badboy to your Gemfile and bundle install 

- Assset pipeline 

		    config.assets.precompile += %w(*.png *.jpg *.jpeg *.gif)
add that to your config/application.rb file 

- Now we're ready to add in some bootstrap to our files go to app/assets and vim a new file called 
called custom.css.scss

		@import bootstrap; 
	brings in the bootstrap gem to all the pages. 

- We will use this custom.css.scss file to make universal changes across the application 

##The asset pipeline 

- Three principles to understand asset directories, manifest files, preprocessor engines 

###Manifest files

- tells rails how to combine your assets into single files 

- located inside app/assets/stylesheets/application.css

		*= require_tree .
		*= require_self
tells the application to include all the css files app/assests/stylesheets directory and itself 

###Preprocessor engines 

- Takes the erb, coffee, and scss and renders them into regular 

###Assets Libraries

- app/assets: assets specific to the present application

- lib/assets: assets for libraries written by your dev team

- vendor/assets: assets from third-party vendors

##Sass stuff

- like css, only a little better, 


##Links 

- just add a route to the link_to mesage 

		<%= link_to "About", about_path %>
		
#Chapter 6
*** 
*Overall idea: Create a working model for the user*

- Rails does a lot of the initial set up (creating boiler plate code for the model, creating databases for us, and the testing suite) all we have to do use a simple command 

		rails generate model User param1:type_of_param param2:type_of_param

- This inherently runs a migration, which provides a way to alter the structure of the database. The migrations help keep a valid table of our model.

```
		located in db/migrate/[timestamp]_create_users.rb
		
			class CreateUsers < ActiveRecord::Migration
  				def change
    				create_table :users do |t|
      				t.string :name
      				t.string :email
      				t.timestamps
    			end
  			end
		end 
 
	
```
- To run the migration that creates the database in db/development.sqlite3

		 bundle exec rake db:migrate
 
##CRUD

- Create, Read (find), Update, Destroy
	- these methods do exactly what the say they do with items in your database
	
	Some examples: 
	
	**Creating**
	
	```	
	user = User.create(name:"faiq", email:"faiqrazarizvi@yahoo.com")
	```
	
	**Finding** 
	
	```
	User.find_by_email("mhartl@example.com")
	
	User.find_by(email:"faiqrazarizvi@yahoo.com")
	
	User.all
	
	```
	
	**Updating** 
	
	```
	user.update_attributes(name: "The Dude", email: "dude@abides.org")
	
	```
			
	**Destroy**
	
	```
	user.destroy
	```
	
##Validation

###Checking if the fields are empty or not

##### ***Some notes about the validates method ***

- validates takes in an attribute of an object,

```
validates  :name
```

- then you can add restrictions on that attribute 

```
validates :name, presence:true, length: { maximum: 16 }
``` 
- Here the restrictions are to check if its present, and to check the length of the attribute 




In my app/models/user.rb just simply add the line! 

	validates :name, presence: true
	
###Checking if the length is good
Same file. Quick validation. 

	validates :name,  presence: true, length: { maximum: 50 }
	
###Checking if the email is actually an email


- Use regular expressions. 

		
		VALID_EMAIL_REGEX = /\A[\w+\-.]+@[a-z\d\-.]+\.[a-z]+\z/i
  		validates :email, presence: true, format: { with: VALID_EMAIL_REGEX }
  		
- /\A[\w+\-.]+@[a-z\d\-.]+\.[a-z]+\z/i	     full regex
- /											start of regex
- \A											match start of a string
- [\w+\-.]+									at least one word character, plus, hyphen, or dot
- @											literal “at sign”
- [a-z\d\-.]+									at least one letter, digit, hyphen, or dot
- \.											literal dot
- [a-z]+										at least one letter
- \z											match end of a string
- /											end of regex
- i											case insensitive

###Uniqueness validation 

- To do this we'll be adding the :unique option to the validates method

```
validates :email, presence: true, format: { with: VALID_EMAIL_REGEX },
                    uniqueness: { case_sensitive: false }
```

- We still need to validate uniqueness on the database level, not just the local memory level
- To achieve this we add a unique index on the emails. We accomplish a change in database structure through migrations. 

		rails generate migration add_index_to_users_email

- We add the following lines in the migration file 

		add_index(:users, :email, {:unique=>true})

- add_index(:table, :column, {:other_options=>true})

- lastly, we're going to make all the emails lowercase so when looking them up with active records in different databases its consistant 

###Adding a Password 

- Again we're adding an attribute to the database so we need to run another migration. 
- We also need to add in a hashing library 

		rails generate migration add_password_digest_to_users password_digest:string


###Having a Password and confirming it

- LITERALLY 1 LINE OF CODE IN YOUR USER MODEL

		has_secure_password

- this line will add a password and a password confirmation field and make sure they are the same

###User Authentication

- We want to find a user by his email, then test whether or not its him with his password, again all taken care of with has_secure_password



 



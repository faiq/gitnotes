#Git Tutorial/Notes 

##Overview
***
- Git is a source version control system - allows you to track changes in your project 
	- allows for multiple people to edit the project any time 

##Setting up Git 
*** 
- Use these Commands for line ending prefrences 
		
		git config --global core.autocrlf input
		git config --global core.safecrlf true

##Creating the Project
*** 
1. Make a directory in which you are going to put your project in 

2. go into that directory and initialize the repository (the place you're going to keep your program)
	
	- This could all be done in your terminal using the following commands
			
			mkdir NAMEOFDIRECTORY 
		-Makes the directory you are going to be using to hold all your files. This will become your Repository later. 
		
			cd NAMEOFDIR 
		
		-Goes inside the new directory you made 
	
			git init
		
		-Initializes the repository inside the directory

##Adding files/Changing files  
***

Now that we have our repository all setup we could start adding some changes to it 

1. Create a file. For example hello.rb - (Simple Hello World Program)
2. Add this file to the repository. 
		
		git add hello.rb 
		
	This notifies git that hello.rb has been added/changed in the repository, but it doesn't **commit** the changes to the repository yet. 
	
3. If you're happy with the change you made to the file you can now store it in the repository with the commit command 
		
		git commit -m "COMMENT"
		
	Whenever you commit a file you're usually changing something in a file, or adding something new so add a comment so you know what you changed. You don't want to confuse yourself! 
	
4. Checking if you did it right
		
		git status
		
	This command tells you what you have yet to commit. Meaning any files you added or changed and have not used the "add" command to notify the repository of change. 
	
###Understanding Git (READ THIS)

- While these commands will get you by 90% of the time, you may find yourself frustrated when you make some changes to a file, add them to the repos, commit them, but the changes you made have not showed up.
####So what gives?
	- the reason for this behavior is simple git tracks changes in files, not the files themselves. So **everytime** you make a change to the file add and commit it to the repository! 

##Seeing your history 
***
- So you want to track your changes on the repository. You want to know who changed what when 
you can use this simple command
			
			git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short
			
	--pretty="..." defines the format of the output.
	
	%h is the abbreviated hash of the commit
	
	%d are any decorations on that commit (e.g. branch heads or tags)
	
	%ad is the author date
	
	%s is the comment
	
	%an is the author name
	
	--graph informs git to display the commit tree in an ASCII graph 	layout
	
	--date=short keeps the date format nice and short

##Making things shorter (Aliases)
***
- To add some aliases you need to change your config file to do this you use the following command 

		vim ~/.gitconfig
	
	Vim is just a file editor, and the file you will be editing here is your gitconfig file
	
- Some aliases to use 

```		
[alias]
  co = checkout
  ci = commit
  st = status
  br = branch
  hist = log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short
  type = cat-file -t
  dump = cat-file -p
```
just copy and paste that to your git config 

##Branches 
***
**Overview**

- Branches are you how deploy expriemental changes to your program from a given point 

		git checkout -b <branchname>
	creates a new branch/ checkout also changes what branch you're on w/ -d flag
	
		git merge <branchname>
	can merge <branchname> and current branch together
	
##Push
***
- Add your project on github first with the menu there
- tell the computer where you repository is 

		git remote add origin <location>

- then push 
	
		git push



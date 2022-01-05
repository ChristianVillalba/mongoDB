# MongoDB
Introduction to MongoDB

Notes from MongoDB module.     
[The Complete 2022 Web Development Bootcamp](https://www.udemy.com/course/the-complete-web-development-bootcamp/)  
Instructor: Dr. Angela Yu   

## Set Up MongoDB
#### Install MongoDB
Link [Dowload MongoDB (Community Server)](https://www.mongodb.com/try/download/community)            
Install MongoDB with the Installation Wizard

#### Setup Alias Shortcuts for Mongo and Mongod
Open up your Hyper terminal running Git Bash.      
```
cd ~
touch .bash_profile
vim .bash_profile
```
In vim, hit the I key on the keyboard to enter insert mode:    
```
alias mongod="/c/Program\ files/MongoDB/Server/5.0/bin/mongod.exe"
alias mongo="/c/Program\ Files/MongoDB/Server/5.0/bin/mongo.exe"
```
The number 5.0 is the version. Change to current version if neccessary.       
Hit the Escape key on your keyboard to exit the insert mode. Type to save and exit Vim:
```
:wq!
```

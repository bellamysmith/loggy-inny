# Lab: Loggy-Inny

###Objective.
Build a basic application which allows a user to only sign up, log in, and log out.

###Overview.

One model is used -- the **User** class represents a user in MongoDB:

* **User**
	* **authenticate(password)**. Tests whether a plain-text password is equal to the password hash stored in database.
	* **password=(password)**. Allows setting of the password (along with attr_reader :password), without requiring direct manipulation of password_digest.


Two controllers are used for authentication. Each handles the following HTTP requests:

* **UsersController**
	* **\#new**. GET - Form for creating a new user.
	* **\#create**. POST - Saves a new user in MongoDB.
* **SessionsController**
	* **\#new**. GET - Displays a log in form.
	* **\#create**. POST - Logs a user in (via SessionsHelper).
	* **\#destroy**. DELETE - Logs a user out (via SessionsHelper).

A helper module contains support methods for the controllers. They can also be used throughout our controllers and views:

* **SessionsHelper**. 
	* **log_in(user)**. Sets a cookie or session variable.
	* **log_out(user)**. Removes a cookie or session variable.
	* **logged_in?**. Is a user logged in? (i.e. does the cookie/session variable exist?)
	* **current_user**. Method that returns a User object of the currently logged in user.
	* **current_user=(user)**. Allows us to set the current user to a user object.
	

###Further Reading.

**"Ruby On Rails Tutorial", Chapters 6-8 [(Amazon.com)](http://www.amazon.com/Ruby-Rails-Tutorial-Addison-Wesley-Professional/dp/0321832051/)**

* [Chapter 6, User Model](http://draft.railstutorial.org/book/modeling_users)
* [Chapter 7, Sign Up](http://draft.railstutorial.org/book/sign_up)
* [Chapter 8, Log In/Log Out](http://draft.railstutorial.org/book/log_in_log_out#cha-log_in_log_out)

---

##Loggy-Inny Lab.

This lab is located in 05-week/loggy-inny-lab.

#### ADDING ROUTES ###

1. In config/routes.rb, make the website root direct to the home page (sessions#index)! Test the path in your browser by visiting 'http://localhost:3000'.

2. In config/routes.rb, add get/post/delete routes to log in and log out. Test the GET path in your browser by navigating to the URL you set up. 


#### USER MODEL ###

3. The user password is not hashed in the database! We installed the gem bcrypt for you -- change app/models/user.rb to hash the user's password using BCrypt. 

	Change the "password" field to "password_digest", then add "attr_reader :password" to allow a plaintext password to be passed to the class.

4. Verify by using genghisapp that when a new user is created, the user's password is not plaintext!


#### SESSIONS CONTROLLER ###

5. In sessions#new, the user's email and password are POST'd to sessions#create. Write code for the sessions#create which authenticates the user (i.e. tests whether the test password matches the BCrypt'd hash). Pseudocode is provided.

6. Add error messages to the sessions#new view should the login fail.


#### SESSIONS HELPER MODULE ###

7. In app/helpers/sessions_helper.rb, write the log_in, log_out, and logged_in? methods. log_in adds a cookie, log_out removes a cookie, and logged_in? tests whether the cookie exists. You may name your cookie anything -- make sure the name is specific and makes sense! The cookie should contain the ID of the logged in user.

8. Now write the current_user method. This method should return "@current_user", the User object of the currently logged in user, or nil if a user is not logged in.

	If the user is logged in, find the User object by ID using Mongoid, and store it in "@current_user". Return nil if the user is not found or not logged in.

9. Now write the setter method for "current_user" (current_user=). This single line allows us to assign a passed-in User object to "@current_user".


#### SESSIONS INDEX VIEW ###

10. In app/views/sessions/index.html.erb, write an if statement which displays the current user's name and email if logged in. Only display the "Log in!" link if the user is logged out; otherwise, display the "Log out!" link.

11. Replace '#' in the "Log in!" and "Log out!" links with the proper *_path names.


#### VICTORY ###

12. Log in and relish your victory!

---
layout: post
title:      "The Bookshelf App"
date:       2021-01-02 21:58:48 +0000
permalink:  the_bookshelf_app
---


Being a reader is just as much about buying books as it is about actually reading them. I have shelves of books sitting in my apartment unread, and still I find myself searching through bookstore.org to purchase even more. I needed a way to track my unread books, to be able to decide which one to take off my self next, and to keep myself organized. 

When I read the specs for our Sinatra project, I knew this was my time for all my unread books to shine. The Bookshelf app allows users to add, edit, and remove books from their bookshelf. Users can read all the details of each of their books in one tidy place so they are able to pick their perfect next read. But how does it work?

## Basic Set Up

### Application Controller

A Sinatra application uses a MVC framework. MVC stands for Models-Views-Controllers. Models are responsible for the data, Views are responsible for the display the user sees, and the Controllers are how the Models and Views communicate. Most of the code is written in the controllers. Each model as it's own controller, as well as the Application Controller, the hub of the application. The application controller is where the homepage of the application exists, as well as any set up required. 

```
require './config/environment'

class ApplicationController < Sinatra::Base

  configure do
    set :public_folder, 'public'
    set :views, 'app/views'
    enable :sessions
    set :session_secret, 'top_session'
  end

  get "/" do
    erb :welcome
  end
	
end
```

The helper methods provide support for your application. In the case of the Bookshelf App, I used helper methods in the Application Controller to ensure that a user was logged in properly while navigating my app. The helper methods also allowed me to ensure that the user that was logged in could only see their books from their homepage, and not a collection of all the books in the database for all users. 

```
helpers do 

    def logged_in?
      !!session[:user_id]
    end

    def current_user
      @current_user ||= User.find_by(id: session[:user_id])
    end


 end 
```

### Models and their Controllers

As previously stated, each Model has its own Controller. A Model is simply a ruby class that inherits from ActiveRecord::Base. In the Models, I established the has many/belongs to relationship between my User and Book models. The user has many books, and a book belongs to a user. In these models, I also ensured that when a User is singing up, they have to have both a username and password filled out. The User model also ensures that each username is unique.

```
class User < ActiveRecord::Base
    has_many :books

    has_secure_password
    validates :username, presence: true
    validates :username, uniqueness: true
end 
```

I also wanted to create some validations in the Book class. When adding a book to their bookshelf, a user has the option to fill out a title, author, genre, and brief description. While I think that the genre and description can be left out, I wanted to ensure that each book at least had a title and an author. 

```
class Book < ActiveRecord::Base
    belongs_to :user

    validates :title, presence: true
    validates :author, presence: true
end 
```

Now that my models are complete, I was able to set up the database and migrations.

### Database

The quickest way to create a database table and migration is to type this command in the terminal: 

```
rake db:create_migration NAME="create_ books"
```

This will give us the barebones of the migration class, as well as a timestamp on the file. When the migration is first created it looks like this:

```
class CreateBooks < ActiveRecord::Migration
  def change

  end
end

```

The change method is where we build out our table. As previously mentioned, I wanted each of my books to have a title, an author, a genre, and a description. The books table will also need to continue the has many/belongs to relationship by providing a foreign key for the user the book belongs to. The final product is below:

```
class CreateBooks < ActiveRecord::Migration
  def change
    create_table :books do |t|
      t.string :title
      t.string :author
      t.string :genre
      t.string :description
      t.integer :user_id
      t.timestamps null: false
    end
  end
end
```

Now I did essentially the same for the users table. However, only the books table will keep track of the has many/belongs to realtionship, so the users table will not require a foreign key. The user, I decided, needed to just have a username and password. For security purposes, I used the column name password_digest, so when the password is saved to the session, it is salted. This makes hacking an account harder.

```
class CreateUsers < ActiveRecord::Migration
  def change
    create_table :users do |t|
      t.string :username
      t.string :password_digest
      t.timestamps null: false
    end
  end
end
```

Once both of these tables are complete, we run a rake db:migrate to connect the database to the models. Now I was able to really start working on the Bookshelf app, creating views and building out the controllers for both User and the Book classes.

## Giving the Bookshelf Functionality!

This is where things got exciting. Using the corneal gem, I had basic layout design and I was able to start really getting into my project.

When building out the controller and the view, I needed to keep in mind a few things. First, I needed full CRUD functionality for the Book model. I also needed to keep [restful routes](http://https://medium.com/adventures-in-code/snow-white-the-7-restful-routes-afcf87bbe5bd) in mind.

### Create

To create a new book, I need to have a get route to bring up the new book form view, and a post route to process that form. In the Books Controller, I first created the get '/books/new' route. The /books part of that route is the restful routes in action. 

```
    get '/books/new' do 
        erb :"books/new"
    end
```

The erb :"books/new" will bring up the view with the form to create a new book. When the user submits the information, granted they have at least a title and an author, a new book will be created in the post route:

```
post '/books' do
        book = Book.new(params[:book])
        if book.save
            current_user.books.create(params[:book])
            redirect "/books"
        else
            @errors = book.errors.full_messages.join(" - ")
            erb :'/books/new'
        end
 end 
```

### Read 

Read allows the user to see all of the items in the book model. When first building out my app, I noticed I was seeing all the books I had in my seed data, and not just the ones for the specific user that was signed in. Thanks to the helper methods that I created in the Application Controller, I was able to show only the books specific to the user signed in. The get route looks like this:

```
get '/books' do
        @books = current_user.books
        erb :"books/index"
end 
```

This brings the user to the index show page. Because of the variable, @books, that I created from all of the current_users's books, I am able to display certain things in the index page. In my view, I iterated through the @books to pull the title and the author of each book, displayed in list form. I also made the book title a link to the individual book show page.

The show page for an individual book, also a Read method, is accessed by the get route:

```
  get '/books/:id' do
        @book = Book.find_by(id: params[:id])
        erb :"books/show"
  end
	```
	
The :id is a dynamic route, meaning it processes whatever is put into that spot. In this case, we want to use the book's id number. This is a number that is attached to the book when it is created and does not change. It is the most efficient way to find each book.

I again made the book in question a variable, @book, so its details could be accessed in the show page. The show page displays all the information the user has submitted for the book and includes the option to........

## Update

I am *very* specific when it comes to book genres. My undergraduate degree is in English Literature, so I am a snob. So if I'm putting a book on my shelf and I categorize as as fantasy when it is more specifically *high fantasy,* I'm definitly going to need to change that which is where the update function comes in.

My update, or edit, function once again has to use the dynamic :id route:

```
get '/books/:id/edit' do
        @book = Book.find_by(id: params[:id])
        if @book.user == current_user
            erb :"books/edit"
        else
            redirect "/books"
        end
end 
```

Only the user with that book should be able to change a book. I would be appalled to sign onto my Bookshelf and see that someone had changed something of mine. The @book.user == current_user validation will make sure there is protection in place.

This will bring us to the edit form where the user is able to change whatever they need. This action will be saved in the **patch** route of the controller:

```
    patch '/books/:id' do
        @book = Book.find_by(id: params[:id])
        if @book.user == current_user
            @book.update(params[:book])
            redirect "/books/#{@book.id}"
        else 
            redirect "/books"
        end
    end
```

Now, what happens when you finish a book and your list finally shrinks (or you find an excuse to buy another book) - this is where the final CRUD function comes in...

## Delete
My 2021 resolution is simple; read all my unread books and then some. I spent too much time in 2020 binge watching TV and I think I've finished all of Netflix and Hulu's libraries. Its time for me to work on a different library.

Delete is a form that I put on the individual book's show page. When that button is clicked, the **delete** route in the books controller will be processed:

```
    delete '/books/:id' do
        @book = Book.find_by(id: params[:id])
        if @book.user == current_user
            @book.destroy
            redirect '/books'
        else
            redirect '/books'
        end 
    end 
```

## So, what about the User?

The Users Controller will have a bit less to do, as I didn't give it full CRUD functionality. I very well could have given the user the ability to edit their username or password, or delete their account, but instead I just made sure that they had their own homepage (show), and the ability to signup and login and logout.

The signup function is similar to the book's create function. The get "/signup" route brings a user to the users/new view, which is a form to create a username and password. If they forget to fill something in, or if the username is already taken, an error message will pop up. Otherwise, the post "/signup" route will create a new user and save. 

```
get "/signup" do
        erb :"users/new"
    end 

post "/signup" do
        user = User.new(params[:user])
        if user.save
            session[:user_id] = user.id 
            redirect "/users/#{user.id}"
        else 
            @errors = user.errors.full_messages.join(" - ")
            erb :'/users/new'
     end
end
```

The login function requires authentication to make sure the password entered is the same as the password saved for that user. 

```
    post "/login" do
        user = User.find_by(username: params[:user][:username])
        if user && user.authenticate(params[:user][:password])
            session[:user_id] = user.id 
            redirect "/users/#{user.id}"
        else 
            redirect "/login"
        end
    end
```

The user's show page again uses the dynmaic route with an id number. This is bascially just their homepage that can direct them to all the CRUD functions from the Books Controller.

```
    get "/users/:id" do
        @user = User.find_by(id: params[:id])
        erb :"/users/show"
    end 
```

And finally, the logout function clears the session and logs the user out.

## Wrapping up

This project was very exciting. I felt like the smartest person in the world when I finished it. There is something so satisfying about combinging my love of creativity and reading with coding. Bringing something to life on my screen is so rewarding. I'm excited to continue learning and building!


---
layout: post
title:      "Building a Rails cookbook app"
date:       2021-02-27 21:08:32 +0000
permalink:  building_a_rails_cookbook_app
---


If you’re starting your Rails project and thinking, “Oh my gosh, this is a mountain,” you are not alone! When starting projects in the past, I felt ready and prepared, but with Rails I felt overwhelmed. I absolutely let that get to me in the beginning stages and the doubt forced me to spend a lot of time on things I didn’t need, I confused myself on issues I knew how to solve. So if that is you, stop what you’re doing, take a deep breath in, and a deep breath out. Do that four more times. Okay, let’s get started.

## The Project

Build a complete Ruby on Rails application. This application must:
-	Use complex forms
-	RESTful routes
-	Models must include at least one has_many, one belongs_to, and two has_many :through relationships.
-	Join table must include a user-submittable attribute.
-	Models must include validations.
-	Must include at least one class level ActiveRecord scope method, and it must be chainable.
-	Standard user authentication, including signup, login, logout, and passwords.
-	Must also allow login from some other service like Facebook, Twitter, Google, etc.
-	Must include a nested resource with appropriate RESTful URLs. This must include a nested new route with a form that relates to the parent resource, and a nested index or show route.
-	Display correct validation errors; fields_with_errors class, errors must be present within the view.
-	Must be DRY
-	And finally, do not use scaffolding to build your project.

Wow, that’s why we were all overwhelmed. Seems like a lot, but let’s just take it step by step, and keep breathing!

## The Idea

One of the hardest parts of this project for me was just coming up with an idea that fit the has_many, has_many through relationship requirements. I had already used a Bookshelf app for the Sinatra project, and didn’t want to repeat myself. Though it was tempting because I could do so much more with it for Rails. Alas, after a late night viewing of Julie and Julia, I decided to go with a Cookbook app. It would go like this:

	A user has many recipes
	A country of origin has many recipes
	Recipes belong to a user and a country

But what about the has_many through?! Well…

A user has many countries, through recipes
A country has many users, through recipes.

Alright! Now we’re cooking with gas.

## Beginning steps

To get started, you have to set up your application and the best way to do this is to run:

```
> 	rails new your-project-name
> 	cd your-project-name
> 	bundle install
```

Then set up your repo on github. We’ve done this before! Let’s move on.

To set up your models, controllers, and migrations, you can use a Rails Generator. How fun are these? You can find all the information you could want on rails generators here (add link).

I decided to use resources. Resources set up a model, a controller, a migration table, and a folder for the views for all three of my models. Double check that your migrations have everything you need, including that special password_digest column to protect your passwords and migrate, migrate, migrate! Actually, just migrate it once, but I added extra for dramatic effect.

## Models

Don’t forget to set up those associations! You should have columns in your join table with the ids for the other models, but don’t forget to set those relationships in your models as well! 

What else do we need in models? Validations! Some helpful validations might be:

1.	Presence validation
2.	Uniqueness validation

You might require some others, which can be found in the [ActiveRecord documentation for validations](https://guides.rubyonrails.org/active_record_validations.html).

Don’t forget to use [has_secure_password](https://api.rubyonrails.org/classes/ActiveModel/SecurePassword/ClassMethods.html) in your User model! We want to make sure we keep those passwords protected at all costs.

## Let’s Talk About Forms

Alright, let’s talk about those new and edit forms! For my project, I had a new forms for users and recipes, but I only had an edit form for recipes. 

The big question; should we use form_for, form_with, or form_tag for our forms? For me, I chose to use form_for so I could use instance variables in my view to populate the fields on the edit form. I also used form_for so the field_with_errors field is automatically generated. You can find more information on the different forms [here](https://guides.rubyonrails.org/form_helpers.html). But the biggest difference is this: if you have a model for the form, you would use form_for, if you do not have a model, just a path, you would use form_tag. Form_with, however, can be used with both.

### Partials

Don’t forget you can use partials! Since my edit form and my new form for my Recipe model is almost exact, I can move that form into a partial titled _form. When I want to use that form in a view, I simply write

`<%= render 'form' %>`

Rails is smart enough to know when the form is being called from edit or new. This helps keep our application DRY!

## So, whats the deal with nested routes?

Nested routes allow you to display your has_many and belongs_to relationship in routes. In my project, I nested recipes under country of origin. This way, I could allow users to see all the recipes for a certain country, a nested index route, as well as allow a user to add a new recipe to a country.

So how do you write out nested routes? Like this!

```
  resources :country_of_origins, only: [:index] do
      resources :recipes, only: [:index, :new, :create]
  end
```

Outside of the nested route, you would list the other recipe routes you want for the entire app. So above this, I have written out:
 
 ```
 resources :recipes
 ```
 
 Now I will have full CRUD routes for my recipes, while also having routes associated with the nested recipes.
 
 You'll also have to adjust your controller actions for the nested routes. Adding an instance variable for the parent will let the controller actions know if you're coming from that action from a nested route or not. For example, I adjusted the index in my RecipesController to look like this:
 
 ```
 def index
        if params[:country_of_origin_id] && @country = CountryOfOrigin.find_by_id(params[:country_of_origin_id])
            @recipes = @country.recipes.ordered_by_skill_level
        else 
            @recipes = Recipe.all.ordered_by_skill_level
        end
end
```

Nested routes can be confusing, so be sure to check out the [API Dock](https://guides.rubyonrails.org/routing.html#nested-resources) for more information.

## Final Thoughts

This project can be overwhelming. As long as you take it step by step, using the resources you have within Flatiron as well as our friend Google, you will be able to build a fully functioning Rails app in no time. Don't forget to ultilize stylesheets to make this project your own! Have fun with it and reach out for help when you need it. Your cohert mates are there for you, as well as your instructor. And don't forget to get that extra confidence boost from your Education Coach when you need it! Happy coding!





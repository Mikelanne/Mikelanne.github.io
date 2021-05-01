---
layout: post
title:      "Building a JavaScript project with a Rails API"
date:       2021-05-01 23:09:34 +0000
permalink:  building_a_javascript_project_with_a_rails_api
---


And we thought the Rails project was overwhelming. We sure have egg on our face. Remember, just take a deep breath. The first thing I reccomend doing is getting an old school pen and paper and writing down your thoughts. Organize your vision for this project; what do you want to be able to do? What information do you want to access from the API? What relationships can you create with this idea? Get all your thoughts and plan together and then make yourself a big cup of coffee and get ready.

## The API

Right when I found out we would be creating our own API for this project, I knew I wanted to make a companion for the fantasy series I'm reading. If you're a fan of fantasy, then you know its easy to lose track of all the characters and locations that you encounter. And Google can be your WORST enemy. You type in a character name hoping for a brief description and you see 1000 spoilers. Not ideal! With this project, you can keep track of the characters on your own time, writing your own descriptions or using the glossary in the back of your current book. 

Okay, I got my idea, now what? The API was not intimidating for me at all, I felt it was a nice safe spot in this otherwise scary project. So, I created my models, my controllers, and something new! A serializer for each class. A serializer is a way to change your Rails data into JavaScript friendly data. Its not scary at all. I used a serializer made by Netflix. I found it on GitHub [here](http://https://github.com/jsonapi-serializer/jsonapi-serializer). That GitHub will tell you exactly how to install it! 

Don't forget! You can run this command to create the right structure for your backend API:

```
rails new my_api --api
```

## The Frontend

With my API complete, seed data loaded, I was ready to move on to the JavaScript part of this project. Let me give you a brief overview of my classes.

**Character**
belongs_to a group
Attributes: name, title, ta'veren, home, abilities, and group_id

**Group**
has_many characters
Attributes: name, description

**Location**
Attributes: name, leader, description

When I first started this project, I had a lot of ideas. I was going to have edit, create, and delete functions for *everything.* But, it turns out, that was a little ambitious. It's important when doing these projects to not put so much pressure on yourself that you completely lose your mind. Know yourself, know how much you can handle, and don't lose sleep. It will be way harder to complete this project if you're stressed out and tired. Take care of yourself! Even though I gave up on some of this CRUD functionality, I'm sure I'll have to do it in the live code! So, remember to review how you might do these after you've submitted your project, when you have some more time to review and study.

This project needs to be Object Oriented. So, you will have files for each of your models and their APIs. Also include a index.js to call functions and any other miscellaneous code you might need. Instead of views like in Ruby, your single page JS appilcation will run from your index.html file. The HTML structure for your app lives here. So whats all of this look like together? I'll show you!

```
WHEEL_OF_JAVASCRIPT
   src
	    character.js
			characterApi.js
			group.js
			groupApi.js
			location.js
			locationApi.js
	index.html
	index.js
	README.md
	style.css
```

What a difference from all the files you had to keep track of in Ruby, right?? So what goes where? Your API classes are going to be where your fetch requests live. For example:

```
Class CharacterApi {

static baseURL = 'http://localhost:3000/characters'

    static getCharacters() {
        fetch(this.baseURL)
        .then(response => response.json())
        .then(data => {
            data["data"].forEach(character => {
                const char = new Character({id: character.id, ...character.attributes})
            })
        })
    }
	}
```

This fetch request handles the Read CRUD action. It communicates with my backend to display the index of Characters. Remember, you'll need seperate fetch requests for each CRUD action. If you want a little help with fetch, you can find that [here](http://https://devhints.io/js-fetch).

Okay, so I have my Read fetch request, now I need to display that information. Thats where my Character class comes in! In the Character class, we set the constructor function. W3 Schools explains constructor functions like [this](http://https://www.w3schools.com/js/js_object_constructors.asp): "Sometimes we need a "blueprint" for creating many objects of the same "type". The way to create an "object type", is to use an object constructor function."

We need to make sure the information we have in our front end can communiate properly with our backend API. So, our constructor function in our Character class will look like this:

```
class Character {

    static all = []

    constructor({id, name, ta_veren, abilities, title, home, group_id, image}){
        this.id = id
        this.name = name
        this.ta_veren = ta_veren
        this.abilities = abilities
        this.title = title
        this.home = home
        this.group_id = group_id
        this.image = image

        this.element = document.createElement("div")
        this.element.className = "card"
        this.element.id = `character-${id}`
        this.element.dataset.id = id

        Character.all.push(this)

        this.renderCards()
        }
		}
```

This function will create the blueprint I need to create the Items from my backend. In my constructor function, I'm also able to create the element I will use to display these characters. For my project, I displayed them as trading cards. These books were written in the 1990's, the era of trading cards. Or, at least, thats what I think of it as. I created these cards using that `renderCards()` method. Another important thing to note from this constructor function is the use of `this`. Think of the `this` keyword as you would the `self` keyword in Ruby and you'll be setting up your app in no time.

Now, we weren't done with our backend just yet! We need to make sure the API knows how to communicate the index method with the frontend. How do we do that? Well, its very similar to writing Ruby index methods, except we want to use our fancy serializer to help our Ruby data transform into the JavaScript data we need.

```
class CharactersController < ApplicationController

    def index
        characters = Character.all
        render json: CharacterSerializer.new(characters)
    end
		
end
```

## Conclusion

This project is hefty! It requires a lot of work so make sure you're managing your time! And don't forget, you need sleep and you need balanced meals. Take care of yourself and remember, you have all the tools you need to build this project. Good luck and happy coding!


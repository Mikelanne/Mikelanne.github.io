---
layout: post
title:      "The Final Project"
date:       2021-07-04 16:23:30 +0000
permalink:  the_final_project
---


Wow, I cannot believe we made it. When I first started Flatiron, I was still furloughed from my job as a waitress. I had been working in resturaunts for a decade and when the pandemic hit, I was lost. I was unemployed for six months. During that time, I knew I had to finally figure out exactly what I wanted to do with the rest of my life. I have an English degree, which has helped me immensely when it comes to creative and critical thinking. I dabbled in the idea of teaching, then in the idea of getting my Master's degree and becoming a professor. But neither of those really felt right to me. Then, in March 2020, when we were let go from our jobs, my fiance introduced me to programming. I started on a few websites just as something to do, didn't think anything would come of it, but I quickly fell in love. Getting accepted to Flatiron was huge for me, I couldn't wait to start this journey.

Over the last few months I've felt like both the smartest person in the world, and the dumbest. Software development really brings you on a rollercoaster of emotions, and this final project was no different. I started the first project week off telling myself I couldn't do it, thinking about transferring, but with encouragement from my instructor, I decided to get to work. I had some big ideas for this project. I wanted to create a program that could be utilized in retaurants. A Point of Sale system, a reservation system, and a system for the host stand all in one. But that was a tad ambitious for a two week gap of time, so I forced myself to scale way down and decided to go for an app that would help me keep my plants alive because I have to be honest, I'm not doing great in that department.

## The Beginning

The first thing I needed to do to get this project going was relax. I couldn't build an app with the self loathing mindset I had. I started small, creating the Rails API. I am basically a pro at that by now. That boosted my confidence enough to tackle the front-end. The key for completing this project is simple: you need to believe in and trust yourself. We've been at this for months now, some of us working full time, caring for family members, living through a pandemic. The first thing we need to do is give ourselves a break, cut yourself some slack. There's a lot going on right now, be patient with yourself, and listen to your body. If you need to rest, you need to rest. If I was still as stressed and anxious as I was on the first few days of project week, I never would have been able to finish this project on time. But by allowing myself to take a minute to rest, to think, to step away, I was able to complete this project with a little time to spare.

Using React and Redux was a huge challenge for me. I didn't feel as prepared as I had with previous projects. So, I rewatched some lectures, I watched videos on YouTube and I was able to pull it off. Lets start at the very beginning, (it is a very good place to start.)

## Store? Does this mean I can shop?

I'm a blonde woman nearing my 30s and I love to shop. It's cliche, but it's true. Sadly, this isn't that kind of store. But maybe when you finish this project, you can buy yourself something nice (I know I am). The store in Redux is what allows your front-end to communicate with your back-end. The store holds on to your state. The store is an object, and in order to change it, you must dispatch an action to it. So let's start there. How do we create the store? Well, with Redux, it's just a few lines of code in your index.js file.

First, you'll need to import the method createStore from redux. Redux is great because it simplifies our code, it gives us far less work to do as programmers. So after you import it, you gotta use it! 

```
const store = createStore(yourReducer)
```

You want to pass this store along to the rest of your app though, so that you can use and change state through props. How do we do this? Well, you need a Provider of course. Another awesome feature from React and Redux. The Provider wraps the entire App component, taking the store in as a prop, allowing every component to access the store if need be. Don't forget to import Provider from react-redux or it won't work!

```
<Provider store={store}>
        <App />
</Provider>
```

You did it! Now take a break and buy yourself something nice.

## Reducers, Actions, State, oh my!

Okay, so now we need to access the store and display some information. Let's talk about the reducer. Our reducer was passed to `createStore` as an argument. This reducer needs to do a few things and who better to list those things than the [Redux documentation](https://redux.js.org/usage/structuring-reducers/basic-reducer-structure): 

> * The first time the reducer is called, the state value will be undefined. The reducer needs to handle this case by  supplying a default state value before handling the incoming action.
> * It needs to look at the previous state and the dispatched action, and determine what kind of work needs to be done
> * Assuming actual changes need to occur, it needs to create new objects and arrays with the updated data and return those
> * If no changes are needed, it should return the existing state as-is.

Okay, so step by step what does our reducer need? First, it's going to need to exist. 

```
export default function yourReducer(state, action){

}
```

Okay, now according to the documentation, we need to make sure the state has a default value.

```
export default function yourReducer(state = [], action)....
```

Okay, so now it needs to look at the action, the previous state, and decide what needs to be done. Lets say it just needs to get the items from your backend and add them to the state. And if there's nothing that needs to be done? Just return the state as is.

```
export default function yourReducer(state = {items: []}, action) {
     switch (action.type) {
		 
		 case "GET_ITEMS":
		        return {...state, items: action.payload}
						
		default:
		       return state
	  }
}
```


Alright we suddenly added a lot of stuff. Let's talk about it. We should know from previous lessons that `...state`	spreads the current state and creates a copy of it, allowing as to add new items to state. But what about `action.payload`? The payload has additional information about what happened in the action, in this case it would be the items we are adding to the state. Let's take a look at that action in the itemActions.js file.

## Go on boy, fetch!

We meet again, old friend. Our action is going to require a fetch request to get that information from the back-end. By this point, we all know how fetch requests work but with this one, we're going to need to add a dispatch. The dispatch comes from the `mapDispatchToProps` method in the container where you're going to display your items. A lot goes into this, including the need to import connect from react-redux. More information about using connect can be found [HERE](https://react-redux.js.org/api/connect). But basically, it connects your React component to your Redux store. 

```
const mapDispatchToProps = dispatch => {
    return {
        fetchYourItems: () => dispatch(fetchYourItems())
    }
}
```

I would like to go back to our action, so if you would like more information about mapDispatchToProps, please take a peek at the documentation found [HERE](https://react-redux.js.org/using-react-redux/connect-mapdispatch).

So what do we do with this dispatch? We send it to our action along with our url to our back-end. Let's see what that looks like.

```
export const fetchYourItems = () => {
    return(dispatch) => {
		    fetch(yourURL)
				.then(resp => resp.json())
				.then(yourItems => {
				      dispatch({type: "GOT_ITEMS", payload: yourItems})
				})
		}
}
```

Now we have incorporated our dispatch, our action, and our fetch request and we should be able to take this data now and display it on our front-end. That's the start of this project, the rest is entirely up to you. How will you display this information? How will you incorporate routes, forms, and post requests? The best thing about programming is you get to be as creative as you want, you get to create apps that you would want to use.

## Wrapping up

This is the very basics of what this project entails, and the work doesn't end once you submit. Study! Study study study. Practice code, prepare for that assessment. Live coding, walking through your project, these are skills you'll need when you start interviewing for jobs. And that's what this was all for, so we could start a career we enjoy instead of settling for any career we can figure out. The React documentation that I've linked to several times in this blog is a major source of information. But before you start studying, enjoy the moment. You are submitting your final project, all those tears, all those doubts are gone. You've proved you can do this. Now go out there and secure that job.












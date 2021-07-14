---
layout: post
title:      "What's the deal with Props and State in React?"
date:       2021-07-14 13:20:00 -0400
permalink:  whats_the_deal_with_props_and_state_in_react
---


Starting to learn React might have you thinking to yourself, "why do we have 2 names for the same thing?" when working with props vs. state. Props and state are very similar, in React, it's true. Both Props and State can hold on to attributes for a component, they seem to function very similarly. So what's the diff? 

## Props

Props, short for properties, allow a developer (that's you!) to pass values down to a component. These values can be any data type and they allow us to create dynamic components. We pass props down to a child component from the parent. Let's say we have an App that lets us keep track of our plants. Our plants have the properties of a name, how much water they need, how much light they need, and how difficult they are to maintain. Let's see how we could send this information to a child component using props.

```
class PlantsContainer extends Component {

const name = "Golden Pothos"
const water = "Water every 1-2 weeks"
const light = "Bright, indirect light"
const difficulty = "Beginner"

   render(){
		 return(
			<div>
			 <Plant
				name={this.name}
				water={this.water}
				light={this.light}
				difficulty={this.difficulty}
			/>
		 </div>
		)
	}
```

Props come from the Parent Component, in this case the PlantsContainer, and are sent to the Child Component, in this case the Plant component. Once they're in the Child Component, they cannot be changed. Don't touch 'em! The Props can be used to display data in the child component, but that's it. How would you display them? Let's see.

```
const Plant = (props) => {

  return(
	 <div>
	 <h1>{this.props.name}</h1>
	 <p>
		Water: {this.props.water}
		Light: {this.props.light}
		Difficulty: {this.props.difficulty} 
	 </p>
 )	
}
```

So that covers props, but what if we *want* to change the properties of your component? That's where state comes in. 

## State

State is data that you can mutate in your component. State is controlled by the component itself, and it doesn't exist outside of that component. State is the components own little imaginary friend, except it's real and we can all see it. The Parent Component doesn't have to send any more data to the Child, the Child can just mutate it's own state on it's own.

Let's say we want to be able to click a button next to our plant so the user knows if the plant is alive or if it's dead. Of course, we want this plant to start out alive, and we really want it to stay alive but we know ourselves and we know our skills and sometimes, the plant doesn't make it.

So, how do we set our component up to start out with a plant that is marked alive? We set an initial state.

```
const Plant = (props) => {

 constructor() {
  super()
  this.state = {
  health: "alive"
  }
}

 return(
	<div>
	<h1>{this.props.name}</h1>
	<p>
		Water: {this.props.water}
		Light: {this.props.light}
		Difficulty: {this.props.difficulty} 
	</p>
	<button>I've Given Up</button>
	)
}
```

Okay, so now we have our initial state which adds the key value pair representing that the plant is alive. We've also added a button that we can click to change this state but it doesn't do anything right now! It's just a button. So, let's change that.

Well, how do we make that button trigger something? An event listener! We know these. We're pros at these by now. We also need a function to handle that event listener. Let's use an onClick event listener and name the fuction handleOnClick.

So how do we *change* the state? Well, in that handleOnClick function, we're going to use setState to mutate the state.

```
const Plant = (props) => {

 ......
 
 function handleOnClick = () => {
 
 this.setState({
	health: "dead"
	})
 }
 
  return(
	 <div>
	 <h1>{this.props.name}</h1>
	 <p>
		Water: {this.props.water}
		Light: {this.props.light}
		Difficulty: {this.props.difficulty} 
	 </p>
	 <button onClick={this.handleOnClick}>I've Given Up</button>
  )
 }

```

setState() simply exists to update or mutate the state. seState() triggers the DOM to rerender, which is why it's use is so important. Without setState(), we would just be changing the state, but not alerting the DOM of that change. One thing to remember is that setState() sets the state asynchronously. Meaning, it will finish the handleOnClick function before updating the state. 

## Conclusion

So that's it! That is the difference between state and props. Props are sent down from the Parent Componenet, they are not to be manipulated in the Child in any way. State is controlled by the component in which it lives, meaning it can be updated and manipulated by that component in any way necessary. Happy coding!





# Case study: a reactive state object

Intrigued by the possibilities in Javascript I wanted to explore some more functionalities the programming language has to offer. How does React from Facebook™ create that [State object](https://reactjs.org/docs/faq-state.html). More importantly how can I create an object that knows when it’s value changed and tie a listener to it. Time to create an object that listens to its own changes.

*Little disclaimer, what I’m going to do does not need to be a class, but I thought it would be interesting to create a little microlibrary, for reusing purpose.*

### But first, how do objects work?
The simple answer: its a variable where you can store data as properties. Here’s an example of a real life object:

```javascript
Object car = {
  color: "red",
  doors: 4,
  wheels: 4
}
```

The car (object) is red, has 4 doors and 4 wheels (properties). Easy. You organize it like this to make it clearer for you who’s coding and the one after you who has to work with it. (I’m basically going to assume you have at least some minor knowledge in coding).

**But there’s more to it.** Objects can actually be very advanced stuff, no wonder there’s a term in programming called ‘Object Oriented Programming’. Objects can also hold functions, arrays and nested objects, even setting or getting a property can be handled inside the object better, more on that later.

### Getters & Setters
Who? [Getters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get) and [Setters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/set) are what their name implies (like everything in programming). They get properties values from objects, or set property values. To get a value of a property you would type in “car.doors” and to set a value you would type “car.doors = 4”.

So here comes a little magic, what if we wanted to set the number of doors of the car to always have + 1 than the number you typed in (because people always forget the door of the trunk, for example). Typing in car.doors = 4 wouldn’t magically make it 5. We need a function to handle that. Insert the standard object methods (object functions): get & set.

#### Setter
```javascript
const car = {
  color: 'red',
  doors: 4,
  wheels: 4,
  // v This is an object method v
  set doors(value) {
    return this.doors = value + 1
  }
}
```

If we pass “car.doors = 4” the setter method will be activated and will add 1 to 4 and save it in the object, **this** is referring to the object it’s in (checkout
[this](https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/scope-closures/ch1.md) out if you want to learn more about scopes, pun intended).

#### Getter

```javascript
const car = {
  color: 'red',
  doors: 4,
  wheels: 4,
  set doors(value) {
    return this.doors = value + 1
  },

  get color() {
    return 'orange'
  }
}
```

Take a look at the above object. If I call car.color I won’t get back the value ‘red’ , instead it will return ‘orange’ because that’s what we told the object to return on car.color. We can also call our property with car[‘color’] it does the same but you can insert strings in between the brackets for dynamic programming.

Now this is not a perfect example of how you would use getters but hopefully it is an easily understandable example.

### So how do we set listeners on value change?
Alright that’s the basics, but what if we wanted to receive an alert every time we change the cars amount of doors.

```javascript
const car = {
  color: 'red',
  doors: 4,
  wheels: 4,

  set doors(value) {
    this.doors = value + 1
    return alert('The car now has ' + this.doors + ' doors.')
  }
}
```
Now if we type somewhere in our document car.doors = 5, we will get an alert saying: “The car now has 6 doors.”, 6 because we told it to add 1.

Pretty neat huh, how we can manipulate our data with getters and setters. This already explains a bit more how React handles their State object.

But I’m not done yet, although this works it will get tedious to add a setter method for every property you add. Is there not another way that we can just say that we want to tie a listener to our new property.

### Back to school, uh no Class.
So insert [classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/class). Classes are used as javascript object constructors that you can see as a small factory. As a rule of thumb: class variables are Capitalized. So factories eh, that sounds promising so can we automate the creation of our property listeners? Yes.

So let’s create our factory called ‘ReactiveObject’. 'ReactiveObject' because it responds/reacts to changes made to it's properties. 
But like every factory it needs workers in this case a [constructor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/constructor) to tell the new object on creation what it needs to do. But you can also add your own
[methods](https://developer.mozilla.org/en-US/docs/Glossary/Method), in short: they’re functions saved as properties in objects.

#### Creating our new class
```javascript
class ReactiveObject {
  constructor() {}

  addProperty(key, value) {
    this[key].value = value // Remember how we can use '[property]' ;).
  }
}
```

Right now it does nothing useful, we need to add getters/setters for each property we add. We do this with a build-in method of objects in Javascript:
[Object.defineProperty()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty).

```javascript
class ReactiveObject {
  constructor() {}

  addProperty(key, value, listener) {
    // We have to create an internal value for the property, that's why I added the + 'Internal'
    // The get & set methods below will make sure that you can still use object.key instead of object.keyInternal.
    this[key + 'Internal'] = {}
    this[key + 'Internal'].value = value
    this[key + 'Internal'].listener = listener && typeof listener === 'function' ? listener : function () { }

    // Define getter & setter
    Object.defineProperty(this, key, {
      set(x) {
        this[key + 'Internal'].value = x
        this[key + 'Internal'].listener()
      },
      get() { return this[key + 'Internal'].value }
    })
  }
}
```

So how do we use this? First we have to create our new object, let’s continue with my car example.

```javascript
// Create a new reactive object.
const car = new ReactiveObject()

// Add the new property color
car.addProperty('color', 'red', function() { alert('Changed car color to: ' + car.color) })

console.log(car.color) // output: 'red'

car.color = 'blue' // Will alert: "Changed car color to: blue" !
```

And that’s how you can make objects listen to their own changes. Long story short: add a listening function to each property that gets ‘set’ in the set() method.

So we created a little factory that automates that process and reduces it down to one method with 3 parameters. What’s next? I’ll leave that up to you, but here are some ideas on how to expand on this:

- Expand the constructor so you can add an object in the creation of the object, like so: new ReactiveObject({color: ‘red’}) to add a property to the created object from the inserted object. (if unclear send me a message).
- Create a method to change a property’s listener.
- Create a method to remove a property.
- Add defensive coding, throw error messages when the user is not using it right.
- Block the original setter from being used, if you so please. (Because those can’t have listeners if you don’t use the created method from the tutorial).

I hope you learned something, if not and think I did this wrong then please send me a mail or text.

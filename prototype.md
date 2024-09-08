### What Is Prototyping in JavaScript?

In JavaScript, when your code runs, the language sets up a built-in capital **`Object`** function in memory. Along with this function, an important unnamed object is created. This unnamed object isn't empty and contains essential JavaScript methods such as `toString()`, `isPrototypeOf()`, and `valueOf()`. We can access this unnamed object through **`Object.prototype`**.

```javascript
console.log(Object.prototype);

// Output:
// {}
// constructor: ƒ Object()
// isPrototypeOf: ƒ isPrototypeOf()
// propertyIsEnumerable: ƒ propertyIsEnumerable()
// toString: ƒ toString()
// valueOf: ƒ valueOf()
// __proto__: null
// toLocaleString: ƒ toLocaleString()
// [[Prototype]]: null
```

This object also contains a special **`constructor()`** function, which points back to the capital **`Object`** function.

```javascript
Object.prototype.constructor
// Output: ƒ Object() { [native code] }
```

When we define a **function constructor** or a **class**, JavaScript creates another unnamed object. You can access this object via **`{class/functionName}.prototype`**. Methods defined inside the class are stored in this object, and it also includes a `constructor()` function pointing back to the class or function.

```javascript
class Product {
  display() {
    console.log("Hello");
  }
}

console.log("1st Output:", Product.prototype);
console.log("2nd Output:", Product.prototype.constructor);

// Output:
// 1st Output: { display: ƒ }
// 2nd Output: class Product {
//   display() {
//     console.log("Hello");
//   }
// }
```

Finally, JavaScript links the class or function's prototype to **`Object.prototype`**.

![](https://github.com/soumadip-dev/js-learning-notes/blob/main/Images/prototype.jpg)

#### Linking Objects to Prototypes

When we create an object from a class using the `new` keyword, an empty object is created, and the class's constructor is called. This constructor modifies the new object, and afterwards, a hidden link is established between the object and the prototype of the class.

Thus, the object is linked to the class's prototype, which in turn is linked to **`Object.prototype`**. This allows us to access methods from both prototypes. When you call a method from an object, JavaScript first checks if the method is defined on the object itself (via the constructor). If it isn't found, JavaScript looks for the method in the class's prototype, and if it's still not found, it searches in **`Object.prototype`**. 
**This process of sharing methods between objects is known as prototyping.**

```javascript
class Product {
  display() {
    return "Hello";
  }
}

let p = new Product();
console.log(p.display()); // Access method from the Product class prototype
console.log(p.toString()); // Access method from Object.prototype
```

#### `__proto__` Property

In JavaScript, every object has an internal property called **`[[Prototype]]`**, which can be accessed using the **`__proto__`** property. The **`__proto__`** property refers to the object's prototype, allowing it to inherit properties and methods from its prototype chain. This chain links the object to its class's prototype and eventually to **`Object.prototype`**.

```javascript
let p = new Product();
console.log(p.__proto__); // Outputs Product.prototype
console.log(p.__proto__.__proto__); // Outputs Object.prototype
```

In this example, `p.__proto__` points to `Product.prototype`, and `p.__proto__.__proto__` points to `Object.prototype`. This property is also called `dunder proto`.

| **Note**: 
In function constructors, member functions are not directly added, as everything is inside the constructor. To solve this, we use `objectName.prototype.functionName = function(){...}`.

```JavaScript
function Product(a,b){
	this.a = a;
	this.b = b;
}
Product.prototype.display = function (){
	console.log(this);
}
let p = new Product(2,5);
p.display();
```

### How JavaScript OOP Differs from Languages like Java and C++

In languages like Java and C++, creating an object from a class means that any changes made to the class after the object is created do not affect the existing object. However, in JavaScript, modifying the prototype after object creation will reflect those changes in the already created object.

```JavaScript
function Product(a, b) {
    this.a = a;
    this.b = b;
}

Product.prototype.display = function() {
    console.log(this);
}

let p = new Product(2, 5);
p.display();  // Displays the current object properties

// Modify the prototype method
Product.prototype.display = function() {
    console.log("Modified", this);
}

p.display();  // Displays the modified output
```

In this example, after creating the object `p`, we modify the `display` function in `Product.prototype`. When `p.display()` is called again, it uses the modified method because JavaScript objects reference prototypes dynamically, allowing real-time changes.

This is why JavaScript is not considered a purely object-oriented language but rather **object-linked-to-other-object** language.

### Prototype Inheritance

In JavaScript, prototype inheritance is a method by which objects inherit properties and methods from other objects. Here are the key points:
Objects can inherit from other objects through a prototype chain. For example, a child object can inherit properties from a parent object via the prototype.
When a child class extends another parent class, the child class's prototype is connected to the prototype of the parent class. This allows the child class objects to access both the member functions of the parent class and the properties initialized in the parent class constructor using the `super()` keyword.

Here's an example demonstrating prototype inheritance in JavaScript:

```js
// Define the Event Class
class Event {
  // Constructor for the Event class
  constructor(type) {
    this.type = type; // Initialize the type of event
  }

  // Method to get event information (present in Event.prototype)
  getInfo() {
    return `This is an event of type: ${this.type}`; // Return a string describing the event type
  }
}

// Define the Movie Class inheriting from Event
class Movie extends Event {
  // Constructor for the Movie class
  constructor(type) {
    super(type); // Call the parent constructor to initialize type
  }
}

// Define the Comedy Class inheriting from Event
class Comedy extends Event {
  // Constructor for the Comedy class
  constructor(type) {
    super(type); // Call the parent constructor to initialize type
  }
}

// Create instances of Movie and Comedy
let m = new Movie("Movie"); // Create a Movie instance with type "Movie"
let c = new Comedy("Comedy"); // Create a Comedy instance with type "Comedy"

// Output the event information for Movie and Comedy instances
console.log(m.getInfo()); // The object of Movie class (m) can access getInfo because it extends the Event class
console.log(c.getInfo()); // The object of Comedy class (c) can access getInfo because it extends the Event class
```

In this example:
- Both `Movie` and `Comedy` classes inherit from the `Event` class.
- They both have access to the `getInfo` method defined in `Event`.
- The `super()` keyword is used to call the parent class's constructor and initialize the type property.

![](https://github.com/soumadip-dev/js-learning-notes/blob/main/Images/protoInheritance.jpg)


### `Call`, `Apply`, and `Bind` Methods in JavaScript

In JavaScript, `call`, `apply`, and `bind` are three very useful methods that allow you to control the value of the `this` keyword within a function.

#### `Call`:
The `call` method allows you to explicitly set the value of `this` inside a function. It invokes the function with a specific `this` context and passes arguments individually.

```javascript
const myObj = {
    name: "Ram",
    greet: function(welcomeMessage, location) {
        console.log(`God ${this.name} ${welcomeMessage} to ${location}`);
    }
}
```

If you call `myObj.greet()`, `this` refers to `myObj`, so the name will be "Ram". To set `this` to another object, you can use the `call` method.

```javascript
const newObj = {
    name: "Krishna"
}

// Using call to set `this` to newObj and passing two arguments
myObj.greet.call(newObj, "Welcome", "Vrindavan"); 
// Output: God Krishna Welcome to Vrindavan
```

The `call` method allows us to pass the first argument as the new `this` context. If no object is passed, `this` refers to the global object. You can pass function parameters after the `this` reference.

#### `Apply`:
The `apply` method is similar to `call`, but it takes two arguments:
1. The object to which `this` will refer.
2. An array of parameters to be passed to the function.

```javascript
// Using apply to set `this` to newObj and passing arguments as an array
myObj.greet.apply(newObj, ["Welcome", "Vrindavan"]);
// Output: God Krishna Welcome to Vrindavan
```

#### `Bind`:
The `bind` method is like `call` but does not invoke the function immediately. Instead, it creates a new function with `this` permanently bound to the specified value. You can call this new function later.

```javascript
// Create a new function with `this` bound to newObj and arguments pre-set
const boundGreet = myObj.greet.bind(newObj, "Welcome", "Vrindavan");

// Call the bound function
boundGreet();
// Output: God Krishna Welcome to Vrindavan
```

#### **Note**: 
Arrow functions do not work with `call`, `apply`, and `bind` as they do not have their own `this`. The `this` context of arrow functions is lexically scoped based on where the arrow function is defined.

### Prototype Inheritance (Function)

We can achieve prototype inheritance using functions by linking the prototype of a child class to the prototype of a parent class. This can be done using:

- `child.prototype.__proto__ = parent.prototype;`  
- Alternatively, `Object.create()` can be used to achieve the same.

In classes, we use the `super` keyword to call the constructor of the parent class. However, in functions, there is no `super` keyword. Instead, we can use the `call()` function to achieve the same functionality by explicitly calling the parent constructor inside the child function.

#### Example:

```JavaScript
// Parent constructor function: Event
function Event(name, date) {
  this.name = name;
  this.date = date;
}

// Adding a method to the parent prototype to describe the event
Event.prototype.getDetails = function () {
  return `Event: ${this.name} on ${this.date}`;
};

// Child constructor function: MovieEvent
function MovieEvent(movieName, date) {
  // Inherit properties from Event
  Event.call(this, movieName, date);
}

// Link the child's prototype to the parent's prototype
MovieEvent.prototype = Object.create(Event.prototype);
// Another way
// MovieEvent.prototype.__proto__ = Event.prototype;

// Create an instance of MovieEvent
let movieEvent = new MovieEvent("Inception", "25th September 2024");

console.log(movieEvent.getDetails());
// Output: Event: Inception on 25th September 2024
```

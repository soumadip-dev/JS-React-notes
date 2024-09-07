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


### Classes
Classes serve as blueprints for real-life entities, defining their structure and behavior. When we talk about how entities "look," we're referring to their properties. When we discuss how they "behave," we mean the actions or methods that can be performed on them.

```javascript
class NameOfTheClass {
  // Details like member functions and data members go here
}
```

### Constructor
Every class in JavaScript includes a special method called a constructor. This method is automatically called when an object of the class is created. The constructor's default version is known as the default constructor, but you can provide your custom implementation.

```javascript
class Product {
  constructor() {
    // This is your custom constructor
  }
}
```

### Objects
Objects are instances of classes and are created using the `new` keyword in JavaScript. This process is distinct from other languages.

```javascript
let iphone = new Product();
```

### How `new` Works
When you use `new`, it follows a four-step process:

1. Creates a new, empty object.
2. Calls the class's constructor, passing the new object as `this`. This allows the constructor to use `this` to refer to the new object.
3. Handles the necessary setup for prototypes (discussed later).
4. If the constructor explicitly returns an object, this object is assigned to the variable. If nothing or something other than an object is returned, the constructor ignores it.

### The `this` Keyword in JavaScript
In most cases, `this` refers to the context in which it was called, known as the "call site." The call site can be an object, a position in the code, or the `new` keyword. However, there is an exception when using arrow functions. In arrow functions, `this` is lexically bound, meaning it refers to the scope in which the arrow function was defined, not the call site.

```javascript
const obj = {
  x: 10,
  y: 20,
  outerFn: function () {
    const innerFn = () => {
      console.log(this.x, this.y);
    };
    innerFn();
  },
};
obj.outerFn(); 
```

In this code, `this` inside the arrow function `innerFn` is not determined by the function itself. Instead, it inherits `this` from its surrounding scope, which is the `outerFn` method. Since `outerFn` is a regular function and `this` within it refers to the `obj` object, `innerFn` also uses `this` from the `obj` object. As a result, `console.log(this.x, this.y);` outputs `10 20`.

### Encapsulation and Data Security in JavaScript Classes
In JavaScript, classes do not inherently protect data members from being accessed or modified outside the class, which can violate the principle of encapsulation in Object-Oriented Programming (OOP). To enforce data security and encapsulation, data members can be made private by prefixing them with a `#`, restricting their access to within the class.

```javascript
class Product {
  #name;
  #price;
  #description;

  displayProduct() {
    console.log(this.#name, this.#price, this.#description); // Accessible inside the class
  }
}
```

### `Getter` and `Setter` Methods
To manage access to these private members, getter and setter methods can be defined. These methods allow controlled read and write access, enabling validation and ensuring the integrity of the data.

```javascript
class Product {
  #price;

  getPrice() {
    return this.#price;
  }

  setPrice(p) {
    if (p > 0) {
      this.#price = p;
    } else {
      console.log("Invalid price");
    }
  }
}

let p = new Product();
p.setPrice(0); // Prints "Invalid price"
p.setPrice(500); // Sets the value of #price to 500
console.log(p.getPrice()); // Prints 500
```

Alternatively, the `get` and `set` keywords can be used to define getters and setters, making the syntax more concise and intuitive.

```javascript
class Product {
  #description;

  get description() {
    return this.#description;
  }

  set description(d) {
    if (d.length === 0) {
      console.log("Invalid description");
      return;
    }
    this.#description = d;
  }
}

const iphone = new Product();
iphone.description = "Something"; // Setter
console.log(iphone.description); // Getter --> Prints "Something"
```

### Functions as Class Constructors
In JavaScript, classes are essentially syntactic sugar over function constructors. Before the introduction of the `class` keyword, functions were commonly used to create object blueprints.

```javascript
function Product(n, p, d) {
  this.name = n;
  this.price = p;
  this.description = d;
  this.displayProduct = function () {
    console.log(
      "Name:",
      this.name,
      "Price:",
      this.price,
      "Description:",
      this.description
    );
  };
}

let iphone = new Product("iPhone 11", 900, "Apple iPhone 11");
iphone.displayProduct();
// Output: Name: iPhone 11 Price: 900 Description: Apple iPhone 11
```

In the example above, the `Product` function acts as a **function constructor.** The `this` keyword is used to assign properties and methods to the newly created object. When the `new` keyword is used, it triggers a similar four-step process as in class-based constructors, ensuring that the object is properly constructed and linked to its prototype.

### `Static` Keyword in JavaScript
The `static` keyword in JavaScript is used to define methods or properties on a class that belong to the class itself, rather than instances of the class. This means you can call a static method or access a static property without creating an instance of the class.

```javascript
class Product {
  // Static method
  static getProductCategory() {
    return "Electronics";
  }

  // Static property
  static discountRate = 0.1;

  constructor(name, price) {
    this.name = name;
    this.price = price;
    console.log(`Accessing the static property: ${Product.discountRate}`);
  }
}

// Accessing static method directly from the class
console.log(Product.getProductCategory()); // Output: "Electronics"

// Creating an instance of the Product class
const product1 = new Product("Laptop", 1500);
```

### Builder Design Pattern
Consider a class like this:

```javascript
class Product {
  #name;
  #price;
  #category;
  // ...
  constructor(name, description, price, category, discount) {
    this.name = name;
    this.price = price;
    this.description = description;
    // ...
  }
}
```

Here, the constructor has too many parameters, making it difficult to remember the order and value of each parameter. JavaScript does not support multiple constructors, so one approach is to use default values and getter/setter methods.

```javascript
class Product {
  #name;
  #price;
  #category;
  // ...
  get name() {
    /*...*/
  }
  set name(name) {
    /*...*/
  }
  get price() {
    /*...*/
  }
  set price(p) {
    /*...*/
  }
}
```

Using getters and setters can be limiting because we cannot validate the object before its creation. To address this, we can use a custom constructor and pass a single object (builder) that contains all the necessary properties.

```javascript
class Product {
  #name;
  #price;
  #category;
  // ...
  constructor(builder) {
    this.name = builder.name;
    if (builder.price > 0) this.price = builder.price;
    this.description = builder.description;
    // ...
  }
  get name() {
    /*...*/
  }
  set name(name) {
    /*...*/
  }
  get price() {
    /*...*/
  }
  set price(p) {
    /*...*/
  }
}
const p = new Product({
  name: "iPhone 11",
  price: 8700,
  description: "Apple iPhone",
});
```

This is a basic implementation of the Builder pattern. We can enhance it by introducing a static `Builder` class to create and configure product objects.

```javascript
class Product {
  #name;
  #price;
  #category;
  // ...
  constructor(builder) {
    this.name = builder.name;
    if (builder.price > 0) this.price = builder.price;
    this.description = builder.description;
    // ...
  }

  static get Builder() {
    class Builder {
      constructor() {
        this.name = ""; // Default values
        this.price = 0;
      }
      setName(name) {
        this.name = name;
        return this; // Allows method chaining
      }
      setPrice(price) {
        this.price = price;
        return this; // Allows method chaining
      }
      // Add more setters if needed
      build() {
        return new Product(this);
      }
    }
    return new Builder(); // Returns a new Builder instance
  }
  get name() {
    /*...*/
  }
  set name(name) {
    /*...*/
  }
  get price() {
    /*...*/
  }
  set price(p) {
    /*...*/
  }
}
```

### To use the Builder pattern with method chaining:

```javascript
const p = Product.Builder.setName("iPhone 11")
  .setPrice(1100)
  .setCategory("Smartphone")
  .build();
```

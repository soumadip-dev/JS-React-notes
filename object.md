# Object in JavaScript

## Important Object Methods

Consider the following object:

```javascript
let product = {
    name: "iPhone 14 Pro",
    company: "Apple",
    price: 125000,
    warranty: "1 Year",
    color: "Black",
};
```

### `Object.keys()`: Fetches all unique keys

```javascript
Object.keys(product); // ['name', 'company', 'price', 'warranty', 'color']
```

### `Object.values()`: Fetches all values

```javascript
Object.values(product); // ['iPhone 14 Pro', 'Apple', 125000, '1 Year', 'Black']
```

### `Object.entries()`: Fetches key-value pairs

Returns an array where each index contains a 2-length array: the 0th index is the key and the 1st index is the value.

```javascript
Object.entries(product);
// [['name', 'iPhone 14 Pro'], ['company', 'Apple'], ['price', 125000], ['warranty', '1 Year'], ['color', 'Black']]
```

### Count Key-Value Pairs

```javascript
Object.keys(product).length; // Number of key-value pairs
```

## Immutability in Objects

### `const` and Object Properties:
Using `const` prevents reassignment of the variable, but it does not make the object's properties immutable. When you create an object, the reference is stored in the variable, while the object itself is stored in heap memory.

```javascript
const obj = { x: 10, y: 20 };
obj.x = 99; // Allowed
obj.z = 98; // Allowed
// Reassigning a new object to the `const` variable is not allowed:
obj = { a: 10 }; // This will throw an error
```

The above code throws the error:
**TypeError: Assignment to constant variable.**

To make objects immutable, use `Object.seal()`, `Object.freeze()`, or `Object.preventExtensions()`.

### `Object.seal()`:
This method prevents adding or deleting properties but allows modification of existing ones:

```javascript
const product = { name: "iPhone 14 Pro", price: 125000 };
Object.seal(product);

product.company = "Apple"; // Not allowed, new properties cannot be added
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

delete product.price; // Not allowed, properties cannot be deleted
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

product.name = "iPhone 14 Pro Max";
console.log(product); // { name: "iPhone 14 Pro Max", price: 125000 } // Existing properties can be modified
```

### `Object.freeze()`:
This method prevents adding, deleting, or modifying properties:

```javascript
const product = { name: "iPhone 14 Pro", price: 125000 };
Object.freeze(product);

product.company = "Apple"; // Not allowed
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

delete product.price; // Not allowed
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

product.name = "iPhone 14 Pro Max"; // Not allowed
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }
```

### `Object.preventExtensions()`:
This method prevents adding properties but allows deletion and modification of existing ones:

```javascript
const product = { name: "iPhone 14 Pro", price: 125000 };
Object.preventExtensions(product);

product.company = "Apple"; // Not allowed
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

delete product.price; // Allowed
console.log(product); // { name: "iPhone 14 Pro" }

product.name = "iPhone 14 Pro Max"; // Allowed
console.log(product); // { name: "iPhone 14 Pro Max" }
```

### `Object.isFrozen()` and `Object.isSealed()`
These methods check if an object is frozen or sealed:

```javascript
console.log(Object.isFrozen(product)); // true
console.log(Object.isSealed(product)); // true
```

If an object is sealed but not frozen:

```javascript
const x = { a: 10 };
Object.seal(x);

console.log(Object.isFrozen(x)); // false
console.log(Object.isSealed(x)); // true
```

If an object is neither sealed nor frozen, both methods return false.

### `Object.defineProperty()`
This function allows fine control over object properties, making them writable or configurable:

- **Writable**: Can the property value be updated?
- **Configurable**: Can the property be modified or deleted?

**Syntax**:
```javascript
Object.defineProperty(object, "key", {
    configurable: false,
    writable: false,
});
```

### Can we make our own `Object.freeze()`? 
Yes, we can combine `Object.preventExtensions` and `Object.defineProperty` to make our own custom seal method.

```javascript
function customFreeze(obj) {
    let keys = Object.keys(obj);
    for (let i = 0; i < keys.length; i++) {
        Object.defineProperty(obj, keys[i], {
            configurable: false,
            writable: false,
        }); // this will stop deletion and updation of key value pair
    }
    Object.preventExtensions(obj); // this will stop addition of new key value pairs
}
```




Here's the properly formatted note:

---

# Object in JavaScript

## Important Object Methods

Consider the following object:

```javascript
let product = {
    name: "iPhone 14 Pro",
    company: "Apple",
    price: 125000,
    warranty: "1 Year",
    color: "Black",
};
```

### `Object.keys()`: Fetches all unique keys

```javascript
Object.keys(product); // ['name', 'company', 'price', 'warranty', 'color']
```

### `Object.values()`: Fetches all values

```javascript
Object.values(product); // ['iPhone 14 Pro', 'Apple', 125000, '1 Year', 'Black']
```

### `Object.entries()`: Fetches key-value pairs

Returns an array where each index contains a 2-length array: the 0th index is the key and the 1st index is the value.

```javascript
Object.entries(product);
// [['name', 'iPhone 14 Pro'], ['company', 'Apple'], ['price', 125000], ['warranty', '1 Year'], ['color', 'Black']]
```

### Count Key-Value Pairs

```javascript
Object.keys(product).length; // Number of key-value pairs
```

## Immutability in Objects

### `const` and Object Properties

Using `const` prevents reassignment of the variable but does not make the object's properties immutable. When you create an object, the reference is stored in the variable, while the object itself is stored in heap memory.

```javascript
const obj = { x: 10, y: 20 };
obj.x = 99; // Allowed
obj.z = 98; // Allowed
// Reassigning a new object to the `const` variable is not allowed:
obj = { a: 10 }; // This will throw an error
```

This code throws the error:
**TypeError: Assignment to constant variable.**

To make objects immutable, use `Object.seal()`, `Object.freeze()`, or `Object.preventExtensions()`.

### `Object.seal()`

This method prevents adding or deleting properties but allows modification of existing ones:

```javascript
const product = { name: "iPhone 14 Pro", price: 125000 };
Object.seal(product);

product.company = "Apple"; // Not allowed, new properties cannot be added
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

delete product.price; // Not allowed, properties cannot be deleted
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

product.name = "iPhone 14 Pro Max";
console.log(product); // { name: "iPhone 14 Pro Max", price: 125000 } // Existing properties can be modified
```

### `Object.freeze()`

This method prevents adding, deleting, or modifying properties:

```javascript
const product = { name: "iPhone 14 Pro", price: 125000 };
Object.freeze(product);

product.company = "Apple"; // Not allowed
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

delete product.price; // Not allowed
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

product.name = "iPhone 14 Pro Max"; // Not allowed
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }
```

### `Object.preventExtensions()`

This method prevents adding properties but allows deletion and modification of existing ones:

```javascript
const product = { name: "iPhone 14 Pro", price: 125000 };
Object.preventExtensions(product);

product.company = "Apple"; // Not allowed
console.log(product); // { name: "iPhone 14 Pro", price: 125000 }

delete product.price; // Allowed
console.log(product); // { name: "iPhone 14 Pro" }

product.name = "iPhone 14 Pro Max"; // Allowed
console.log(product); // { name: "iPhone 14 Pro Max" }
```

### `Object.isFrozen()` and `Object.isSealed()`

These methods check if an object is frozen or sealed:

```javascript
console.log(Object.isFrozen(product)); // true
console.log(Object.isSealed(product)); // true
```

If an object is sealed but not frozen:

```javascript
const x = { a: 10 };
Object.seal(x);

console.log(Object.isFrozen(x)); // false
console.log(Object.isSealed(x)); // true
```

If an object is neither sealed nor frozen, both methods return `false`.

### `Object.defineProperty()`

This function allows fine control over object properties, making them writable or configurable:

- **Writable**: Can the property value be updated?
- **Configurable**: Can the property be modified or deleted?

**Syntax**:
```javascript
Object.defineProperty(object, "key", {
    configurable: false,
    writable: false,
});
```

### Can We Make Our Own `Object.freeze()`?

Yes, we can combine `Object.preventExtensions` and `Object.defineProperty` to create a custom freeze method:

```javascript
function customFreeze(obj) {
    let keys = Object.keys(obj);
    for (let i = 0; i < keys.length; i++) {
        Object.defineProperty(obj, keys[i], {
            configurable: false,
            writable: false,
        }); // This will prevent deletion and updating of key-value pairs
    }
    Object.preventExtensions(obj); // This will prevent adding new key-value pairs
}
```

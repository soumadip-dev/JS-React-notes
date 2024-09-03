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

### `Object.keys()`: Fetches All Unique Keys
This method returns an array of the object's property names (keys).

```javascript
Object.keys(product); 
// Output: ['name', 'company', 'price', 'warranty', 'color']
```

### `Object.values()`: Fetches All Values
This method returns an array of the values corresponding to the object's keys.

```javascript
Object.values(product); 
// Output: ['iPhone 14 Pro', 'Apple', 125000, '1 Year', 'Black']
```

### `Object.entries()`: Fetches Key-Value Pairs
This method returns an array of key-value pairs, where each pair is an array itself.

```javascript
Object.entries(product); 
// Output: [['name', 'iPhone 14 Pro'], ['company', 'Apple'], ['price', 125000], ['warranty', '1 Year'], ['color', 'Black']]
```

### Counting Key-Value Pairs

```javascript
Object.keys(product).length; 
// Output: 5
```

## Immutability in Objects

### `const` and Object Properties
Using `const` prevents reassignment of the variable, but the object's properties can still be modified, added, or deleted. When we create an object, the reference is stored in the variable, while the object itself is stored in heap memory. The `const` keyword only makes the reference to the object immutable, not the object itself.

```javascript
const obj = { x: 10, y: 20 };
obj.x = 99;          // Allowed: Updating a property
obj.z = 98;          // Allowed: Adding a new property
delete obj.z;        // Allowed: Deleting a property

// Reassigning a new object to the `const` variable will throw an error:
obj = { a: 10 };     // Error: Assignment to constant variable.
```

### Making Objects Immutable

#### `Object.seal()`
This method prevents adding or deleting properties but allows modification of existing ones.

```javascript
const product = { name: "iPhone 14 Pro", price: 125000 };
Object.seal(product);

product.company = "Apple";       // Not allowed: Adding new property
delete product.price;            // Not allowed: Deleting property

product.name = "iPhone 14 Pro Max"; 
console.log(product);            // Allowed: Modifying existing property
// Output: { name: "iPhone 14 Pro Max", price: 125000 }
```

#### `Object.freeze()`
This method prevents adding, deleting, or modifying properties.

```javascript
const product = { name: "iPhone 14 Pro", price: 125000 };
Object.freeze(product);

product.company = "Apple";       // Not allowed: Adding new property
delete product.price;            // Not allowed: Deleting property
product.name = "iPhone 14 Pro Max"; 
// Not allowed: Modifying existing property
console.log(product);
// Output: { name: "iPhone 14 Pro", price: 125000 }
```

#### `Object.preventExtensions()`
This method prevents adding new properties, but allows modification and deletion of existing properties.

```javascript
const product = { name: "iPhone 14 Pro", price: 125000 };
Object.preventExtensions(product);

product.company = "Apple";       // Not allowed: Adding new property
delete product.price;            // Allowed: Deleting property
product.name = "iPhone 14 Pro Max"; 
console.log(product);            // Allowed: Modifying existing property
// Output: { name: "iPhone 14 Pro Max" }
```

### `Object.isFrozen()` and `Object.isSealed()`
These methods check if an object is frozen or sealed, respectively.

```javascript
console.log(Object.isFrozen(product));  // true
console.log(Object.isSealed(product));  // true
```

If an object is sealed but not frozen:

```javascript
const x = { a: 10 };
Object.seal(x);

console.log(Object.isFrozen(x));  // false
console.log(Object.isSealed(x));  // true
```

If an object is neither sealed nor frozen, both methods return `false`.

### `Object.defineProperty()`
This method allows fine control over object properties, making them writable or configurable.

- **Writable**: Can the property value be updated?
- **Configurable**: Can the property be modified or deleted?

**Syntax**:

```javascript
Object.defineProperty(object, "key", {
    configurable: false,
    writable: false,
});
```

### Creating a Custom `Object.freeze()`
You can create your own version of `Object.freeze()` using `Object.preventExtensions()` and `Object.defineProperty()`.

```javascript
function customFreeze(obj) {
    let keys = Object.keys(obj);
    for (let i = 0; i < keys.length; i++) {
        Object.defineProperty(obj, keys[i], {
            configurable: false,
            writable: false,
        }); 
    }
    Object.preventExtensions(obj); // Prevents adding new properties
}
```

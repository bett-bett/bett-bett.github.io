---
layout: post
titleL: Functions vs Procedures
date: 10-11-2024
categories: [javascript]
tag: [Funtions-vs-Procedures]
toc: false
comments: false
pin: true
---


# Functions vs. Procedures in JavaScript and React


## [TL;DR](#core-concepts)

---
---
### 1. **What are Functions and Procedures?**

- **Functions**:
  - Functions are blocks of code that **return a value** and don’t modify outside state if they’re pure.
  - **Usage**: calculations, transforming data, returning values based on inputs.
  - 

- **Procedures**:
  - Procedures are also functions, but they trigger changes or side effects.
  - **Usage**: logging, fetching data, modifying global variables, working with the DOM, updating the UI, updating state, making API calls, performing actions based on events.

---

### 2. **Examples of Functions vs. Procedures**

#### **Example of a Pure Function**: 
This function only relies on its inputs and has no side effects.
  ```javascript
  function add(a, b) {
      return a + b;
  }
  // Example usage:
  add(2, 3); // returns 5
  ```
   In React: Pure Function for Data Transformation
  ```javascript
  function capitalize(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
  }
  ```

 #### **Example of a Procedure**
 This function performs a side effect (logging to the console).
  ```javascript
  function logMessage(message) {
      console.log(message);
  }
  // Example usage:
  logMessage("Hello, World!"); // prints "Hello, World!" to console
  ```


Procedure for Updating State:
  ```javascript
  const [count, setCount] = useState(0);

  function incrementCount() {
      setCount(count + 1);
  }
  ```

---

### 3. **Why Differentiate Functions from Procedures?**

- **Cleaner, Testable Code**: Separating logic (functions) from actions (procedures) allows for easier testing and debugging.
- **Better Reusability**: Pure functions can be reused across components or even projects.
- **More Predictable**: Pure functions return the same output for the same input, making behavior easier to predict and test.

---

### 4. **Pure Functions in JavaScript and React**

- **Characteristics of Pure Functions**:
  - Always return the same output for the same input.
  - Do not modify external state or produce side effects.

- **Example of a Pure Function**:
  ```javascript
  function getDiscountedPrice(price, discountRate) {
      return price - (price * discountRate);
  }
  ```

#### In React:

- **Pure Functional Component**:
  ```javascript
  function Greeting({ name }) {
      return <h1>Hello, {name}!</h1>;
  }
  ```
  - This component is pure because it does not modify any outside state and only relies on its `name` prop.

---

### 5. **Procedural Code in JavaScript and React**

- **Common Uses**: Found where we need to perform actions or update application state without needing to return values.
- **Best Practice**: Encapsulate code that produces side effects in functions with clear purposes, and isolate them as much as possible.

#### In JavaScript:

- **Example of a Procedure for Updating the DOM**:
  ```javascript
  function updateTitle(newTitle) {
      document.title = newTitle;
  }
  // No return value; purely an action
  ```

#### In React:

- **State Updater Procedure in useEffect**:
  ```javascript
  useEffect(() => {
      document.title = `
      Count Clicker
      Count = ++${count}
      `;
  }, [count]); // React watches count and performs a side effect when it changes
  ```

- **Procedure for Fetching Data**:
  ```javascript
  function fetchData(url) {
      fetch(url)
          .then(response => response.json())
          .then(data => console.log(data))
          .catch(error => console.error("Error:", error));
  }
  ```

---

### 6. **Functional Programming in JavaScript**

- **Pure Functions**: Functions with no side effects and consistent return values.
- **Example of a Pure Function**:
  ```javascript
  function double(x) {
      return x * 2;
  }
  ```
  - **Benefit**: Easy to test and debug.

- **Procedural Example in React**: When interacting with APIs or updating the DOM
  ```javascript
  useEffect(() => {
      async function fetchData() {
          const response = await fetch("https://api.example.com/data");
          const data = await response.json();
          setData(data); // updates state (side effect)
      }
      fetchData();
  }, []); // dependency array ensures it only runs once
  ```

---
---



## [Core Concepts](## "Flash Cards")

### 1. Understanding Functions vs Procedures

#### Pure Functions
- Return a value based solely on inputs
- Have no side effects
- Same input always produces same output
- Are predictable and testable
```javascript
// Pure function example
function calculateTotal(price, tax) {
    return price + (price * tax);
}
```

#### Procedures
- Perform actions or side effects
- May not return a value
- Change external state
- Often handle UI updates, API calls, or state management
```javascript
// Procedure example
function saveUserData(userData) {
    localStorage.setItem('user', JSON.stringify(userData));
    console.log('User data saved');
}
```

### 2. Side Effects in JavaScript

Common side effects include:
- Modifying DOM elements
- Making API calls
- Writing to localStorage
- Updating global variables
- Writing to databases
- Logging

```javascript
// Side effect examples
function updateUIElement(id, value) {
    document.getElementById(id).textContent = value; // DOM modification
}

async function fetchUserData() {
    const response = await fetch('/api/user'); // API call
    return response.json();
}
```

### 3. React-Specific Patterns

#### Pure Components
```javascript
// Pure component
function PriceDisplay({ price, currency }) {
    const formattedPrice = new Intl.NumberFormat('en-US', {
        style: 'currency',
        currency
    }).format(price);
    
    return <div>{formattedPrice}</div>;
}
```

#### Components with Side Effects
```javascript
// Component with side effects
function UserProfile({ userId }) {
    const [user, setUser] = useState(null);
    
    useEffect(() => {
        // Procedure wrapped in useEffect
        async function loadUser() {
            const data = await fetchUserData(userId);
            setUser(data);
        }
        loadUser();
    }, [userId]);
    
    return <div>{user?.name}</div>;
}
```

### 4. Best Practices

#### Function Organization
```javascript
// Separate pure functions from procedures
const userUtils = {
    // Pure function for validation
    isValidEmail: (email) => {
        return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
    },
    
    // Procedure for API interaction
    saveUser: async (userData) => {
        await fetch('/api/users', {
            method: 'POST',
            body: JSON.stringify(userData)
        });
    }
};
```

#### Error Handling
```javascript
// Pure function with error handling
function divideNumbers(a, b) {
    if (b === 0) {
        throw new Error('Division by zero');
    }
    return a / b;
}

// Procedure with error handling
async function saveWithLinearRetry(data, maxRetries = 3, delay = 1000) {
    for (let i = 0; i < maxRetries; i++) {
        try {
            const result = await saveData(data);
            return result;
        } catch (error) {
            // If this is the last retry, throw the error
            if (i === maxRetries - 1) {
                throw new Error(`Failed after ${maxRetries} attempts: ${error.message}`);
            }
            
            // Wait for the specified delay before next retry
            await new Promise(resolve => setTimeout(resolve, delay));
            console.log(`Attempt ${i + 1} failed, retrying...`);
        }
    }
}
```

### 5. Testing Considerations

#### Testing Pure Functions
```javascript
// pure function
describe('calculateTotal', () => {
    test('calculates correct total with tax', () => {
        expect(calculateTotal(100, 0.1)).toBe(110);
    });
});
```

#### Testing Procedures
```javascript
// requires mocking
describe('saveUserData', () => {
    beforeEach(() => {
        localStorage.clear();
    });
    
    test('saves user data to localStorage', () => {
        const userData = { name: 'John' };
        saveUserData(userData);
        expect(JSON.parse(localStorage.getItem('user'))).toEqual(userData);
    });
});
```

## 1 second advice

1. **Composition Over Procedures**
   - Try to compose multiple pure functions instead of creating complex procedures
   - Makes code more maintainable and testable

2. **State Management**
   - Keep state changes isolated and predictable
   - Use React's useState for local state
   - Consider [Redux]()/Context for global state

3. **Debugging**
   - Pure functions are easier to debug (input → output)
   - Log procedures carefully to track side effects
   - Use browser dev tools to monitor network calls and state changes

4. **Performance**
   - Pure functions can be memoized (React.memo, useMemo)
   - Procedures often need careful optimization (useCallback)
   - Consider the cost of side effects in your application

5. **Documentation**
   - Document side effects clearly
   - Specify return types and parameters
   - Include examples for complex functions


---

<!-- # Expand Knowledgbase | Contribute | Rabit Holes | ToDo: -->

# DOM Manipulation in JavaScript - Complete Guide

## Table of Contents
1. [Introduction to DOM](#introduction-to-dom)
2. [DOM Structure](#dom-structure)
3. [Selecting Elements](#selecting-elements)
4. [Creating Elements](#creating-elements)
5. [Modifying Elements](#modifying-elements)
6. [Working with Attributes](#working-with-attributes)
7. [CSS Class Manipulation](#css-class-manipulation)
8. [Styling Elements](#styling-elements)
9. [DOM Traversal](#dom-traversal)
10. [Event Handling](#event-handling)
11. [Form Manipulation](#form-manipulation)
12. [Advanced DOM Operations](#advanced-dom-operations)
13. [Performance Considerations](#performance-considerations)
14. [Best Practices](#best-practices)
15. [Common Patterns](#common-patterns)

## Introduction to DOM

The Document Object Model (DOM) is a programming interface for HTML and XML documents. It represents the structure of a document as a tree of objects, where each node represents a part of the document (elements, attributes, text, etc.).

The DOM allows JavaScript to:
- Access and modify HTML elements
- Change element content and attributes
- Add or remove elements
- Respond to user interactions
- Dynamically update page content

## DOM Structure

The DOM represents HTML as a hierarchical tree structure:

```
Document
└── html
    ├── head
    │   ├── title
    │   └── meta
    └── body
        ├── header
        ├── main
        │   ├── section
        │   └── div
        └── footer
```

### Key DOM Node Types

- **Document Node**: The root of the DOM tree
- **Element Nodes**: HTML elements (div, p, span, etc.)
- **Text Nodes**: Text content within elements
- **Attribute Nodes**: Element attributes (class, id, src, etc.)
- **Comment Nodes**: HTML comments

## Selecting Elements

### Single Element Selection

#### getElementById()
```javascript
const element = document.getElementById('myId');
// Returns the element with id="myId" or null if not found
```

#### querySelector()
```javascript
const element = document.querySelector('.my-class');
const element = document.querySelector('#myId');
const element = document.querySelector('div[data-value="test"]');
const element = document.querySelector('ul > li:first-child');
// Returns the first matching element or null
```

### Multiple Element Selection

#### getElementsByClassName()
```javascript
const elements = document.getElementsByClassName('my-class');
// Returns a live HTMLCollection
```

#### getElementsByTagName()
```javascript
const elements = document.getElementsByTagName('div');
// Returns a live HTMLCollection of all div elements
```

#### querySelectorAll()
```javascript
const elements = document.querySelectorAll('.my-class');
const elements = document.querySelectorAll('div, p, span');
// Returns a static NodeList
```

### Selection Comparison

| Method | Returns | Live/Static | Performance |
|--------|---------|-------------|-------------|
| getElementById | Element/null | N/A | Fastest |
| querySelector | Element/null | N/A | Fast |
| getElementsByClassName | HTMLCollection | Live | Fast |
| getElementsByTagName | HTMLCollection | Live | Fast |
| querySelectorAll | NodeList | Static | Slower |

## Creating Elements

### createElement()
```javascript
const div = document.createElement('div');
const paragraph = document.createElement('p');
const button = document.createElement('button');
```

### createTextNode()
```javascript
const textNode = document.createTextNode('Hello World');
```

### innerHTML vs createElement
```javascript
// Using innerHTML (less secure, but simpler for complex HTML)
element.innerHTML = '<div class="card"><p>Content</p></div>';

// Using createElement (more secure, better performance for simple elements)
const card = document.createElement('div');
card.className = 'card';
const paragraph = document.createElement('p');
paragraph.textContent = 'Content';
card.appendChild(paragraph);
```

## Modifying Elements

### Text Content

#### textContent
```javascript
element.textContent = 'New text content';
// Sets/gets plain text, ignores HTML tags
```

#### innerText
```javascript
element.innerText = 'New text content';
// Sets/gets visible text, respects styling (display: none)
```

#### innerHTML
```javascript
element.innerHTML = '<strong>Bold text</strong>';
// Sets/gets HTML content - use with caution for security
```

### Adding and Removing Elements

#### appendChild()
```javascript
const parent = document.getElementById('parent');
const child = document.createElement('div');
parent.appendChild(child);
// Adds element as the last child
```

#### insertBefore()
```javascript
const parent = document.getElementById('parent');
const newElement = document.createElement('div');
const referenceElement = document.getElementById('reference');
parent.insertBefore(newElement, referenceElement);
```

#### removeChild()
```javascript
const parent = document.getElementById('parent');
const child = document.getElementById('child');
parent.removeChild(child);
```

#### remove()
```javascript
const element = document.getElementById('myElement');
element.remove();
// Modern method to remove element directly
```

#### replaceChild()
```javascript
const parent = document.getElementById('parent');
const newElement = document.createElement('div');
const oldElement = document.getElementById('old');
parent.replaceChild(newElement, oldElement);
```

### Modern Insertion Methods

#### insertAdjacentHTML()
```javascript
element.insertAdjacentHTML('beforebegin', '<div>Before element</div>');
element.insertAdjacentHTML('afterbegin', '<div>First child</div>');
element.insertAdjacentHTML('beforeend', '<div>Last child</div>');
element.insertAdjacentHTML('afterend', '<div>After element</div>');
```

#### insertAdjacentElement()
```javascript
const newElement = document.createElement('div');
element.insertAdjacentElement('beforebegin', newElement);
```

## Working with Attributes

### Getting Attributes
```javascript
const value = element.getAttribute('data-value');
const id = element.getAttribute('id');
```

### Setting Attributes
```javascript
element.setAttribute('data-value', 'new-value');
element.setAttribute('class', 'my-class');
```

### Removing Attributes
```javascript
element.removeAttribute('data-value');
```

### Checking Attributes
```javascript
if (element.hasAttribute('data-value')) {
    // Attribute exists
}
```

### Data Attributes
```javascript
// HTML: <div data-user-id="123" data-user-name="John"></div>
const userId = element.dataset.userId; // "123"
const userName = element.dataset.userName; // "John"

// Setting data attributes
element.dataset.status = 'active';
```

### Boolean Attributes
```javascript
// For attributes like disabled, checked, selected
element.disabled = true;
element.checked = false;
element.hidden = true;
```

## CSS Class Manipulation

### className Property
```javascript
element.className = 'class1 class2 class3';
const classes = element.className; // Returns string of all classes
```

### classList API (Recommended)

#### add()
```javascript
element.classList.add('new-class');
element.classList.add('class1', 'class2', 'class3');
```

#### remove()
```javascript
element.classList.remove('old-class');
element.classList.remove('class1', 'class2');
```

#### toggle()
```javascript
element.classList.toggle('active');
// Adds class if not present, removes if present

element.classList.toggle('visible', condition);
// Adds if condition is true, removes if false
```

#### contains()
```javascript
if (element.classList.contains('active')) {
    // Element has 'active' class
}
```

#### replace()
```javascript
element.classList.replace('old-class', 'new-class');
```

## Styling Elements

### Inline Styles
```javascript
element.style.color = 'red';
element.style.backgroundColor = 'blue';
element.style.fontSize = '16px';
element.style.marginTop = '10px';

// CSS properties with hyphens become camelCase
// margin-top becomes marginTop
// background-color becomes backgroundColor
```

### Getting Computed Styles
```javascript
const styles = window.getComputedStyle(element);
const color = styles.color;
const fontSize = styles.fontSize;
```

### CSS Custom Properties (Variables)
```javascript
// Setting CSS variables
element.style.setProperty('--main-color', 'blue');
document.documentElement.style.setProperty('--global-color', 'red');

// Getting CSS variables
const mainColor = getComputedStyle(element).getPropertyValue('--main-color');
```

### Style Object Methods
```javascript
// Set multiple styles
Object.assign(element.style, {
    color: 'white',
    backgroundColor: 'black',
    padding: '10px',
    borderRadius: '5px'
});
```

## DOM Traversal

### Parent Navigation
```javascript
const parent = element.parentNode;
const parentElement = element.parentElement;
```

### Child Navigation
```javascript
const children = element.children; // HTMLCollection of child elements
const childNodes = element.childNodes; // NodeList including text nodes
const firstChild = element.firstElementChild;
const lastChild = element.lastElementChild;
```

### Sibling Navigation
```javascript
const nextSibling = element.nextElementSibling;
const previousSibling = element.previousElementSibling;
```

### Ancestor/Descendant Methods
```javascript
// Find closest ancestor matching selector
const ancestor = element.closest('.container');

// Check if element matches selector
const matches = element.matches('.my-class');

// Find descendant elements
const descendants = element.querySelectorAll('.child-class');
```

## Event Handling

### Adding Event Listeners
```javascript
element.addEventListener('click', function(event) {
    console.log('Element clicked!');
});

// Arrow function
element.addEventListener('click', (event) => {
    console.log('Element clicked!');
});

// With options
element.addEventListener('click', handler, {
    once: true,      // Execute only once
    passive: true,   // Never calls preventDefault
    capture: true    // Capture phase
});
```

### Removing Event Listeners
```javascript
function clickHandler(event) {
    console.log('Clicked!');
}

element.addEventListener('click', clickHandler);
element.removeEventListener('click', clickHandler);
```

### Event Object
```javascript
element.addEventListener('click', function(event) {
    event.preventDefault();    // Prevent default behavior
    event.stopPropagation();  // Stop event bubbling
    
    console.log(event.target);        // Element that triggered event
    console.log(event.currentTarget); // Element with event listener
    console.log(event.type);          // Event type ('click')
    console.log(event.clientX);       // Mouse X coordinate
    console.log(event.clientY);       // Mouse Y coordinate
});
```

### Common Events
```javascript
// Mouse events
element.addEventListener('click', handler);
element.addEventListener('dblclick', handler);
element.addEventListener('mouseenter', handler);
element.addEventListener('mouseleave', handler);
element.addEventListener('mouseover', handler);
element.addEventListener('mouseout', handler);

// Keyboard events
element.addEventListener('keydown', handler);
element.addEventListener('keyup', handler);
element.addEventListener('keypress', handler);

// Form events
element.addEventListener('submit', handler);
element.addEventListener('change', handler);
element.addEventListener('input', handler);
element.addEventListener('focus', handler);
element.addEventListener('blur', handler);

// Window events
window.addEventListener('load', handler);
window.addEventListener('resize', handler);
window.addEventListener('scroll', handler);
```

### Event Delegation
```javascript
// Instead of adding listeners to multiple elements
document.getElementById('container').addEventListener('click', function(event) {
    if (event.target.classList.contains('button')) {
        // Handle button click
        console.log('Button clicked:', event.target);
    }
});
```

## Form Manipulation

### Accessing Form Elements
```javascript
const form = document.getElementById('myForm');
const input = form.elements.username;
const input2 = form.elements['email'];
```

### Getting Form Values
```javascript
const textValue = document.getElementById('textInput').value;
const checkboxValue = document.getElementById('checkbox').checked;
const selectValue = document.getElementById('select').value;
const radioValue = document.querySelector('input[name="gender"]:checked').value;
```

### Setting Form Values
```javascript
document.getElementById('textInput').value = 'New value';
document.getElementById('checkbox').checked = true;
document.getElementById('select').selectedIndex = 1;
```

### Form Validation
```javascript
const input = document.getElementById('email');

// HTML5 validation
if (input.validity.valid) {
    console.log('Input is valid');
} else {
    console.log('Validation errors:', input.validity);
}

// Custom validation
input.addEventListener('input', function() {
    if (this.value.includes('@')) {
        this.setCustomValidity('');
    } else {
        this.setCustomValidity('Please enter a valid email');
    }
});
```

### Form Submission
```javascript
form.addEventListener('submit', function(event) {
    event.preventDefault(); // Prevent default submission
    
    const formData = new FormData(this);
    
    // Process form data
    for (let [key, value] of formData.entries()) {
        console.log(key, value);
    }
});
```

## Advanced DOM Operations

### DocumentFragment
```javascript
// Efficient way to add multiple elements
const fragment = document.createDocumentFragment();

for (let i = 0; i < 1000; i++) {
    const div = document.createElement('div');
    div.textContent = `Item ${i}`;
    fragment.appendChild(div);
}

document.getElementById('container').appendChild(fragment);
```

### Cloning Elements
```javascript
const original = document.getElementById('original');
const clone = original.cloneNode(true); // true for deep clone
clone.id = 'cloned'; // Change ID to avoid duplicates
document.body.appendChild(clone);
```

### Working with Templates
```javascript
// HTML: <template id="cardTemplate">...</template>
const template = document.getElementById('cardTemplate');
const clone = template.content.cloneNode(true);
document.body.appendChild(clone);
```

### Mutation Observer
```javascript
const observer = new MutationObserver(function(mutations) {
    mutations.forEach(function(mutation) {
        console.log('Mutation type:', mutation.type);
        console.log('Added nodes:', mutation.addedNodes);
        console.log('Removed nodes:', mutation.removedNodes);
    });
});

observer.observe(document.body, {
    childList: true,     // Watch for child additions/removals
    subtree: true,       // Watch entire subtree
    attributes: true,    // Watch for attribute changes
    attributeOldValue: true,
    characterData: true  // Watch for text changes
});
```

### Intersection Observer
```javascript
const observer = new IntersectionObserver(function(entries) {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            console.log('Element is visible:', entry.target);
        }
    });
});

observer.observe(document.getElementById('target'));
```

## Performance Considerations

### Batch DOM Updates
```javascript
// Bad - causes multiple reflows
element.style.width = '100px';
element.style.height = '100px';
element.style.backgroundColor = 'red';

// Good - batch updates
element.style.cssText = 'width: 100px; height: 100px; background-color: red;';

// Or use classes
element.className = 'new-styles';
```

### Use DocumentFragment for Multiple Insertions
```javascript
// Bad - multiple DOM insertions
for (let i = 0; i < 100; i++) {
    const div = document.createElement('div');
    container.appendChild(div); // Causes reflow each time
}

// Good - single insertion
const fragment = document.createDocumentFragment();
for (let i = 0; i < 100; i++) {
    const div = document.createElement('div');
    fragment.appendChild(div);
}
container.appendChild(fragment); // Single reflow
```

### Cache DOM Queries
```javascript
// Bad - queries DOM multiple times
for (let i = 0; i < 10; i++) {
    document.getElementById('counter').textContent = i;
}

// Good - cache the element
const counter = document.getElementById('counter');
for (let i = 0; i < 10; i++) {
    counter.textContent = i;
}
```

### Minimize Layout Thrashing
```javascript
// Bad - alternating reads and writes
element.style.width = element.offsetWidth + 10 + 'px';
element.style.height = element.offsetHeight + 10 + 'px';

// Good - batch reads, then writes
const width = element.offsetWidth;
const height = element.offsetHeight;
element.style.width = width + 10 + 'px';
element.style.height = height + 10 + 'px';
```

## Best Practices

### 1. Always Check if Elements Exist
```javascript
const element = document.getElementById('myElement');
if (element) {
    element.textContent = 'New content';
}
```

### 2. Use Modern Methods
```javascript
// Prefer modern methods
element.remove(); // Instead of parent.removeChild(element)
element.classList.add('class'); // Instead of className manipulation
```

### 3. Avoid innerHTML for User Content
```javascript
// Dangerous - XSS vulnerability
element.innerHTML = userInput;

// Safe - escapes HTML
element.textContent = userInput;
```

### 4. Use Event Delegation
```javascript
// Instead of adding listeners to many elements
document.addEventListener('click', function(event) {
    if (event.target.matches('.button')) {
        handleButtonClick(event);
    }
});
```

### 5. Debounce Expensive Operations
```javascript
function debounce(func, wait) {
    let timeout;
    return function executedFunction(...args) {
        const later = () => {
            clearTimeout(timeout);
            func(...args);
        };
        clearTimeout(timeout);
        timeout = setTimeout(later, wait);
    };
}

const debouncedHandler = debounce(function() {
    console.log('Expensive operation');
}, 300);

window.addEventListener('scroll', debouncedHandler);
```

## Common Patterns

### Toggle Visibility
```javascript
function toggleElement(element) {
    element.classList.toggle('hidden');
    // Or
    element.style.display = element.style.display === 'none' ? 'block' : 'none';
}
```

### Create Element with Properties
```javascript
function createElement(tag, properties = {}, children = []) {
    const element = document.createElement(tag);
    
    Object.keys(properties).forEach(key => {
        if (key === 'className') {
            element.className = properties[key];
        } else if (key === 'textContent') {
            element.textContent = properties[key];
        } else {
            element.setAttribute(key, properties[key]);
        }
    });
    
    children.forEach(child => {
        if (typeof child === 'string') {
            element.appendChild(document.createTextNode(child));
        } else {
            element.appendChild(child);
        }
    });
    
    return element;
}

// Usage
const div = createElement('div', {
    className: 'container',
    id: 'main',
    'data-value': '123'
}, ['Hello ', createElement('span', {textContent: 'World'})]);
```

### Fade In/Out Animation
```javascript
function fadeIn(element, duration = 300) {
    element.style.opacity = 0;
    element.style.display = 'block';
    
    const start = performance.now();
    
    function animate(currentTime) {
        const elapsed = currentTime - start;
        const progress = elapsed / duration;
        
        if (progress < 1) {
            element.style.opacity = progress;
            requestAnimationFrame(animate);
        } else {
            element.style.opacity = 1;
        }
    }
    
    requestAnimationFrame(animate);
}

function fadeOut(element, duration = 300) {
    const start = performance.now();
    const startOpacity = parseFloat(getComputedStyle(element).opacity);
    
    function animate(currentTime) {
        const elapsed = currentTime - start;
        const progress = elapsed / duration;
        
        if (progress < 1) {
            element.style.opacity = startOpacity * (1 - progress);
            requestAnimationFrame(animate);
        } else {
            element.style.opacity = 0;
            element.style.display = 'none';
        }
    }
    
    requestAnimationFrame(animate);
}
```

### Dynamic Table Creation
```javascript
function createTable(data, headers) {
    const table = document.createElement('table');
    
    // Create header
    if (headers) {
        const thead = document.createElement('thead');
        const headerRow = document.createElement('tr');
        
        headers.forEach(header => {
            const th = document.createElement('th');
            th.textContent = header;
            headerRow.appendChild(th);
        });
        
        thead.appendChild(headerRow);
        table.appendChild(thead);
    }
    
    // Create body
    const tbody = document.createElement('tbody');
    
    data.forEach(row => {
        const tr = document.createElement('tr');
        
        Object.values(row).forEach(value => {
            const td = document.createElement('td');
            td.textContent = value;
            tr.appendChild(td);
        });
        
        tbody.appendChild(tr);
    });
    
    table.appendChild(tbody);
    return table;
}
```

---

## Conclusion

DOM manipulation is a fundamental skill for web development. This guide covers the essential methods and patterns you'll use regularly. Remember to:

- Always check if elements exist before manipulating them
- Use modern methods and APIs when available
- Consider performance implications of your DOM operations
- Implement proper error handling
- Use event delegation for better performance
- Keep security in mind, especially with innerHTML

Practice these concepts and gradually incorporate more advanced techniques as you become comfortable with the basics.
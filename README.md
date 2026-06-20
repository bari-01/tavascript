# Tavascript

This library is free software; you can redistribute it and/or modify it under the terms of the GNU Lesser General Public License as published by the Free Software Foundation; either version 2.1 of the License, or (at your option) any later version. See the GNU Lesser General Public License for more details.

### Router
Client side router using the History API.
- **`Router` class**: Maps URL paths to components.
- **Route Parameters**: Supports dynamic routing (e.g., `/user/:id`).
- **Cleanup Handlers**: Automatically triggers the `_cleanup()` method attached to disappearing components.

**Usage:**
```typescript
import { Router } from './router';

const app = new Router('root-element-id');
app.add('/', () => {
    const el = document.createElement('div');
    el.textContent = 'Home Page';
    return el;
});
app.resolve();
```

### State Management
- **`Store<T>`**: Manages any initial state type.
- **Updates**: Provide either a raw value or an updater function (e.g., `prev => next`).
- **Reactivity**: Subscribing returns an unsubscription function.

**Usage:**
```typescript
import { Store } from './store';

const counter = new Store<number>(0);
const unsubscribe = counter.subscribe((count) => console.log('Count is:', count));

counter.set(c => c + 1); // Notifies subscribers
unsubscribe(); // Clean up listeners
```

### Component Definitions
- **`createComponent()`**: A factory enabling segregation between the DOM mounting implementation and the cleanup hook.

**Usage:**
```typescript
import { createComponent } from './component';

export const MyButtonPage = createComponent((context) => {
    const btn = document.createElement('button');
    btn.textContent = 'Click me';

    const handler = () => context.navigate('/success');
    btn.addEventListener('click', handler);

    return btn;
}, () => {
    // Optional Cleanup Method - Router removes listener automatically
    console.log("Unmounted button");
});
```

### Rendering
- **`h(tag, props, ...children)`**: Builds DOM trees dynamically while attaching inline event listeners (like `onClick`) and properties.
- **`html(string)`**: Parses raw HTML strings directly into elements.

**Usage:**
```typescript
import { h, html } from './render';

const navLinks = h('nav', { className: 'flex gap-4 p-4 bg-gray-100' },
    h('a', { href: '/', onClick: (e) => navigate(e) }, 'Home'),
    h('button', { 
        className: 'bg-blue-500 text-white p-2', 
        onClick: () => store.set('loggedIn') 
    }, 'Login')
);

// For static HTML chunks:
const icon = html(`<svg class="w-6 h-6"><path d="..."/></svg>`);
navLinks.appendChild(icon);
```

# Code Snippets

## Python

## JavaScript

### Vanilla

#### Copy an Array or Object

[Source](https://gomakethings.com/how-to-make-a-deep-copy-of-an-array-or-object-with-vanilla-javascript/)

```javascript
// Shallow copy an array
let wizardsCopy1 = Array.from(wizards);
let wizardsCopy2 = wizards.slice();
let wizardsCopy3 = [...wizards];

// Shallow copy an object
let moviesCopy1 = Object.assign({}, movies);
let moviesCopy2 = {...movies};

// Deep clone array or object
const moviesCopy = structuredClone(movies);
const wizardsCopy = structuredClone(wizards);
```

#### Autogrowing textarea elements

[Source](https://gomakethings.com/how-to-automatically-size-a-textarea-based-on-its-using-vanilla-javascript/) / [Demo](https://codepen.io/cferdinandi/pen/ExvvbaW)

```css
.autogrow {
	display: grid;
}

.autogrow::after {
	content: attr(data-replicated-value) " ";
	white-space: pre-wrap;
	visibility: hidden;
}

.autogrow > textarea {
	resize: none;
}

.autogrow > textarea,
.autogrow::after {
	/* Add textarea styles here so that the textarea and div look the same */
	grid-area: 1 / 1 / 2 / 2;
}
```

```html
<label for="content">Type something</label>
<div class="autogrow">
	<textarea id="content" onInput="this.parentNode.setAttribute('data-replicated-value', this.value)"></textarea>
</div>
```

```javascript
function setAutogrow () {
	let autogrow = document.querySelectorAll('.autogrow');
	for (let field of autogrow) {
		if (!field.value) continue;
		field.setAttribute('data-replicated-value', field.value);
	}
}
```

### React

#### Detect Click outside of Component

[Source](https://www.robinwieruch.de/react-hook-detect-click-outside-component/)

```jsx
const useOutsideClick = (callback) => {
  const ref = React.useRef();

  React.useEffect(() => {
    const handleClick = (event) => {
      if (ref.current && !ref.current.contains(event.target)) {
        callback();
      }
    };

    document.addEventListener('click', handleClick, true);

    return () => {
      document.removeEventListener('click', handleClick, true);
    };
  }, [ref]);

  return ref;
};

// Usage
function App() {
    const [count, setCount] = React.useState(0);

    const handleClickOutside = () => {
        setCount(0);
    };
    const ref = useOutsideClick(handleClickOutside);

    const handleClick = () => {
        setCount((state) => state + 1);
    };

    const handleHeaderClick = (event) => {
        // do something
        event.stopPropagation();
    };

    return (
        <div style={style} onClick={handleHeaderClick}>
            <div>Header</div>
            <button ref={ref} type="button" onClick={handleClick}>
                Count: {count}
            </button>
        </div>
    );
}
```

#### Type-checking useReducer hook

[Source](https://www.benmvp.com/blog/type-checking-react-usereducer-typescript/)

```typescript jsx
type Visibility = 'all' | 'completed' | 'active'
interface State {
  todos: {
    id: number
    text: string
    completed: boolean
  }[]
  visibility: Visibility
}
type Action =
  | { type: 'add_todo'; id: number; text: string }
  | { type: 'toggle_todo'; id: number }
  | { type: 'set_visibility'; visibility: Visibility }

const reducer = (state: State, action: Action): State => {
  switch (action.type) {
    case 'add_todo':
      return {
        ...state,
        todos: [
          ...state.todos,
          { id: action.id, text: action.text, completed: false },
        ],
      }
    case 'toggle_todo':
      return {
        ...state,
        todos: state.todos.map((todo) =>
          todo.id === action.id
            ? { ...todo, completed: !todo.completed }
            : todo,
        ),
      }
    case 'set_visibility':
      return {
        ...state,
        visibility: action.visibility,
      }
  }
}
```

#### Cure to React useState Hell

[Source](https://www.builder.io/blog/use-reducer)

```jsx
import { useState } from 'react'

// Problem
function EditCalendarEvent() {
  const [startDate, setStartDate] = useState()
  const [endDate, setEndDate] = useState()
  const [title, setTitle] = useState('')
  const [description, setDescription] = useState('')
  const [location, setLocation] = useState()
  const [attendees, setAttendees] = useState([])
  
  return <>
    <input value={title} onChange={e => setTitle(e.target.value)} />
    {/* ... */}
  </>
}

// With ...spread
function EditCalendarEvent() {
    const [event, setEvent] = useState({
        title: '', description: '', attendees: []
    })

    return <>
        <input value={event.title}
               onChange={e => setEvent({...event, title: e.target.value }) } />
        {/* ... */}
    </>
}

// With useReducer
function EditCalendarEvent() {
    const [event, updateEvent] = useReducer((prev, next) => {
        // Validate and transform event, to ensure state is always valid
        // in a centralized way
        // ...
        return { ...prev, ...next }
    }, { title: '', description: '', attendees: [] })

    return <>
        <input value={event.title}
               onChange={e => updateEvent({ title: e.target.value }) } />
        {/* ... */}
    </>
}

// With useReducer and Redux-ify
function EditCalendarEvent() {
    const [event, updateEvent] = useReducer((state, action) => {
        const newEvent = {...state}

        switch (action.type) {
            case 'updateTitle':
                newEvent.title = action.title;
                break;
            // More actions...
        }
        return newEvent
    }, { title: '', description: '', attendees: [] })

    return <>
        <input value={event.title}
               onChange={e => updateEvent({ type: 'updateTitle', title: 'Hello' }) } />
        {/* ... */}
    </>
}
```

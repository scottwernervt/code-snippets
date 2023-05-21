# Code Snippets

## Python

### All Unique

[Source](https://morioh.com/p/271bc88c0100)

```python
def all_unique(lst):
    return len(lst) == len(set(lst))


x = [1,1,2,2,3,2,3,4,5,6]
y = [1,2,3,4,5]
all_unique(x) # False
all_unique(y) # True
```

### Difference and Difference By

[Source](https://morioh.com/p/271bc88c0100)

```python
def difference(a, b):
    set_a = set(a)
    set_b = set(b)
    comparison = set_a.difference(set_b)
    return list(comparison)


difference([1,2,3], [1,2,4]) # [3]
```

```python
def difference_by(a, b, fn):
    b = set(map(fn, b))
    return [item for item in a if fn(item) not in b]


from math import floor
difference_by([2.1, 1.2], [2.3, 3.4], floor) # [1.2]
difference_by([{ 'x': 2 }, { 'x': 1 }], [{ 'x': 1 }], lambda v : v['x']) # [ { x: 2 } ]
```

### Duplicates

[Source](https://morioh.com/p/271bc88c0100)

```python
def has_duplicates(lst):
    return len(lst) != len(set(lst))
    
    
x = [1,2,3,4,5,5]
y = [1,2,3,4,5]
has_duplicates(x) # True
has_duplicates(y) # False
```

### Chunk

[Source](https://morioh.com/p/271bc88c0100)

```python
def chunk(list, size):
    return [list[i:i+size] for i in range(0,len(list), size)]
```

### Flatten

[Source](https://morioh.com/p/271bc88c0100)

```python
def spread(arg):
    ret = []
    for i in arg:
        if isinstance(i, list):
            ret.extend(i)
        else:
            ret.append(i)
    return ret

def deep_flatten(xs):
    flat_list = []
    [flat_list.extend(deep_flatten(x)) for x in xs] if isinstance(xs, list) else flat_list.append(xs)
    return flat_list


deep_flatten([1, [2], [[3], 4], 5]) # [1,2,3,4,5]
```

### Spelling Corrector

[Source](https://norvig.com/spell-correct.html)

```python
import re
from collections import Counter

def words(text): return re.findall(r'\w+', text.lower())

WORDS = Counter(words(open('big.txt').read()))


def P(word, N=sum(WORDS.values())): 
    "Probability of `word`."
    return WORDS[word] / N


def correction(word): 
    "Most probable spelling correction for word."
    return max(candidates(word), key=P)


def candidates(word): 
    "Generate possible spelling corrections for word."
    return (known([word]) or known(edits1(word)) or known(edits2(word)) or [word])


def known(words): 
    "The subset of `words` that appear in the dictionary of WORDS."
    return set(w for w in words if w in WORDS)


def edits1(word):
    "All edits that are one edit away from `word`."
    letters    = 'abcdefghijklmnopqrstuvwxyz'
    splits     = [(word[:i], word[i:])    for i in range(len(word) + 1)]
    deletes    = [L + R[1:]               for L, R in splits if R]
    transposes = [L + R[1] + R[0] + R[2:] for L, R in splits if len(R)>1]
    replaces   = [L + c + R[1:]           for L, R in splits if R for c in letters]
    inserts    = [L + c + R               for L, R in splits for c in letters]
    return set(deletes + transposes + replaces + inserts)


def edits2(word): 
    "All edits that are two edits away from `word`."
    return (e2 for e1 in edits1(word) for e2 in edits1(e1))
```

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

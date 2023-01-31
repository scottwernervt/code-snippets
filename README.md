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

### 1) `getElementById` vs `getElementsByClassName` vs `querySelector` / `querySelectorAll`

- **`getElementById(id)`** → returns **one element** by its unique `id`. Very fast, no CSS selector. Example:
  ```js
  const title = document.getElementById('pageTitle');
  ```
- **`getElementsByClassName('cls')`** → returns a **live HTMLCollection** of elements that currently have the class. Not an array. Example:
  ```js
  const cards = document.getElementsByClassName('card');
  cards[0].classList.add('ring');
  ```
- **`querySelector(selector)`** → returns the **first match** using a **CSS selector** (most flexible). Example:
  ```js
  const firstCallBtn = document.querySelector('.card .call-btn');
  ```
- **`querySelectorAll(selector)`** → returns a **static NodeList** of **all matches**. You can loop with `forEach`. Example:
  ```js
  document.querySelectorAll('.copy-btn').forEach(btn => { /* ... */ });
  ```

### 2) How to create and insert a new element into the DOM?

Steps:

1. **Create** the element → `document.createElement('li')`
2. **Fill** it (text/HTML/attributes) → `el.textContent = 'Hello'` or `el.innerHTML = '...'`
3. **Insert** it in the right place → `parent.appendChild(el)` or `parent.prepend(el)` or `parent.insertBefore(el, ref)`

Example:

```js
const li = document.createElement('li');
li.className = 'history-item';
li.innerHTML = '<strong>999</strong> — Fire Service';
document.getElementById('historyList').prepend(li);
```

### 3) What is Event Bubbling and how does it work?

When you click a child element, the event **first runs on the target**, then **bubbles up** through its ancestors (parent → grandparent → document).
This lets parent elements react to events triggered on their children.

### 4) What is Event Delegation and why is it useful?

Instead of adding listeners to **many** child elements, add **one** listener to a **common parent**. Inside the handler, check `event.target` (or `closest`) to decide what was clicked.
Benefits: **fewer listeners**, **works for dynamically added elements**, **better performance**.

Example:

```js
document.getElementById('cardGrid').addEventListener('click', (e) => {
  if (e.target.closest('.copy-btn')) {
    // handle copy for the clicked card
  }
});
```

### 5) `preventDefault()` vs `stopPropagation()`

- **`preventDefault()`** → stops the **default browser action** (e.g., form submit, link navigation, checkbox toggle).
- **`stopPropagation()`** → stops the event from **bubbling** to ancestor elements.
  They solve different problems and can be used together if needed.

---

## Clipboard Challenge — Example

> “Copy specific text from a card to the clipboard and show an alert.”

```html
<button class="copy-btn">Copy</button>
<span class="number">999</span>
<script>
  document.querySelector('.copy-btn').addEventListener('click', async () => {
    const number = document.querySelector('.number').textContent.trim();
    try {
      await navigator.clipboard.writeText(number);
      alert('Copied: ' + number);
    } catch (e) {
      // fallback
      const ta = document.createElement('textarea');
      ta.value = number;
      document.body.appendChild(ta);
      ta.select();
      document.execCommand('copy');
      ta.remove();
      alert('Copied: ' + number);
    }
  });
</script>
```

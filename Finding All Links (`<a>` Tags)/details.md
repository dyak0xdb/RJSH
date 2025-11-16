# Finding All Links (`<a>` Tags) on a Web Page

## `document.links`

`document.links` returns **all `<a>` and `<area>` elements** in the page that have an `href` attribute.

Example:

```javascript
document.links
// HTMLCollection(2) [a, a]
// 0: a
// 1: a
````

> Notice: This shows **the element itself**, not the URL.

---

## Accessing the `href` Value

You can access the `href` attribute (the URL) of a specific link:

```javascript
document.links[0].href
// 'file:///C:/js-quiz-practice/index.js'
```

---

## Looping Over All Links

To get all URLs on the page, you can use a `for...of` loop:

```javascript
for (const link of document.links) {
  console.log(link.href);
}
```

### Output

```text
https://example.com/page1
https://example.com/page2
...
```

> This is simple but powerful! It loops over **each link element** and prints its `href` value.

---

### Notes

* `document.links` is **live**: if you add new `<a>` elements dynamically, it will update automatically.
* You can also filter links, e.g., only external links:

```javascript
for (const link of document.links) {
  if (link.href.startsWith('http')) {
    console.log(link.href);
  }
}
```

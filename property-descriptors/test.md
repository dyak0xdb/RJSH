# ✅ User-Controlled Object → XSS / Abuse Checklist

> **Goal:**  
> When an object receives data from the user, use this checklist to decide  
> **if it is exploitable** and **where to focus your testing**.

---

**I was reading this section of <a href=https://javascript.info/property-descriptors>property-descriptors</a>descriptors when I noticed this.**

## 🔹 1. Source Check
❓ Does the value come from the user?

- [ ] `prompt()`
- [ ] form input / textarea
- [ ] `location.*`
- [ ] `document.cookie`
- [ ] `localStorage` / `sessionStorage`
- [ ] `postMessage`
- [ ] `JSON.parse(response)`
- [ ] API / WebSocket data

✅ If **yes (directly or indirectly)** → continue  
❌ If hardcoded → skip

---

## 🔹 2. Type Check
❓ What is the data type?

- [ ] string 🔥
- [ ] array ⚠️
- [ ] object ⚠️
- [ ] number / boolean ❌

📌 **Strings are the highest XSS priority**

---

## 🔹 3. Object Creation
❓ How is the object created?

- [ ] Object literal `{}`  
- [ ] `new Object()`  
- [ ] `Object.create(null)` ⚠️  
- [ ] class / constructor

⚠️ `Object.create(null)` reduces prototype abuse

---

## 🔹 4. Property Descriptor Check
In DevTools:

```js
Object.getOwnPropertyDescriptor(obj, "key")
````

Check:

* [ ] `writable`
* [ ] `configurable`
* [ ] `enumerable`

📌 Meaning:

* writable → value can be changed
* configurable → property can be redefined
* enumerable → appears in loops / render

---

## 🔹 5. Mutation Opportunity

❓ Can the value change before rendering?

* [ ] reassignment
* [ ] `Object.assign`
* [ ] spread operator `{...obj}`
* [ ] cloning / merging
* [ ] JSON stringify → parse

🔥 **Common validation bypass point**

---

## 🔹 6. Enumeration Abuse

❓ Is the object looped?

* [ ] `for...in`
* [ ] `Object.keys`
* [ ] `Object.values`
* [ ] `Object.entries`

⚠️ Dangerous if combined with:

* [ ] `innerHTML +=`
* [ ] template rendering

---

## 🔹 7. Sink Check (Where is it used?)

❓ Where is the value rendered?

### 🚨 Dangerous sinks

* [ ] `innerHTML`
* [ ] `outerHTML`
* [ ] `insertAdjacentHTML`
* [ ] `document.write`

### ⚠️ Conditional

* [ ] `setAttribute`
* [ ] template literals

### ✅ Safer

* [ ] `innerText`
* [ ] `textContent`

📌 Safe sinks can become unsafe after conversion

---

## 🔹 8. Type Conversion

❓ Is the object or value converted?

* [ ] `"" + obj`
* [ ] `${obj.key}`
* [ ] `String(obj)`
* [ ] `JSON.stringify(obj)`

🔥 Many XSS bugs happen here

---

## 🔹 9. Validation vs Render Gap

❓ Where is validation applied?

* [ ] on input
* [ ] on property
* [ ] length-only check
* [ ] blacklist filtering

⚠️ Validation before mutation = bypass chance

---

## 🔹 10. Prototype / Shadowing Check

❓ Are these protections present?

* [ ] `hasOwnProperty`
* [ ] key whitelist
* [ ] `Object.freeze`
* [ ] `Object.seal`

❌ Missing checks may allow:

* prototype pollution
* shadowed properties
* unexpected rendering

---

## 🔹 11. Persistence Check

❓ Is the data stored?

* [ ] localStorage
* [ ] sessionStorage
* [ ] cookies
* [ ] backend storage

🔥 Stored XSS > Reflected XSS

---

## 🔹 12. Reality Check

❓ Is this worth your time?

* No source → ❌ skip
* No sink → ❌ low value
* No mutation → ⚠️ difficult

🎯 Continue only if **multiple red flags** exist

---

## 🧠 Bug Hunter Mindset

> Property descriptors explain **control**
> Exploits need a **chain**
> XSS is the **final result**

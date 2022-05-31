---
# try also 'default' to start simple
theme: bricks
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: true
colorSchema: 'dark'
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false
---

<div class="cover">

# CSS Houdini
Deep dive into browser APIs
</div>

<p class="author">Negar Jamalifard</p>

<img class="logo" src="assets/jsworld.svg"/>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

<style>
.cover {
   background-color: rgba(0,0,0, 0.5);
   padding: 5px 0;
   position: relative;
 }
.cover h1 + p {
   opacity: 1;
   color: #a4a4a4;
   margin-top: 1px;
 }

.cover .author {
   position: absolute;
   bottom: 0;
   width: 100%;
 }

.cover .logo {
   width: 50px;
    position: absolute;
    top: 0;
    right: 0;
    margin: 20px;
 }
</style>

---
layout: intro
class: text-left
---

# About Me

<div class="about-me">
<img src="/assets/avatar.jpeg" class="avatar rounded-full h-40" />

<div class="text-left w-100 mx-auto mt-8">

- 🪪 ✨ Negar Jamalifard ✨
- 👩🏽‍💻 Frontend Developer @ Lightspeed Commerce
- 📍 Berlin 🛫 Montreal  
- 🐦 Twitter: @NegarDev

</div>

 

</div>

<style>
.avatar {
   position: absolute;
   left: 16.5%;
   top: 41%;
   border: 5px solid #ed5050;
}

</style>

---

# List of Content

<div class="ml-8">

1. Rendering in Browsers
   - Parsing
   - Render tree
   - Layout
   - Paint
1. CSS Houdini
   - Typed OM
   - Properties and Values
   - Paint API
   
</div>
  
---
layout: intro
---

# How do browsers render CSS?

---

# Rendering Engines

<div class="grid grid-cols-3 gap-4 mt-20">

<div class="text-center">
 
#### Blink

   <img src="assets/blink.png" class="w-40 m-auto mt-5" />
</div>
<div class="text-center">

#### Gecko

   <img src="assets/gecko.png" class="w-80 m-auto mt-5" />
</div>

<div class="text-center">

#### Webkit

   <img src="assets/webkit.png" class="w-40 m-auto mt-5" />
</div>

</div>


---

# 1. Parsing

<br/><br/>
<br/><br/>
<br/><br/>

<div class="text-center">

```mermaid  {theme: 'dark'}
flowchart LR;
A[Bytes] -.-> B[Characters] -.-> C[Tokens] -.-> D[Nodes] -.-> E[Object Model];
```

</div>


---
layout: two-cols
---
## DOM

<br/><br/>

```mermaid {theme: 'dark', scale: 0.6}
flowchart TD
    html[html]
    body[body]
    head[head]
    main[main]
    footer[footer]
    div[div]
    p[p]
    html-->body
    html-->head;
    body-->main
    body-->footer
    main-->p
    main-->div
```

::right::

<v-click>

## CSSOM

<br/><br/>

```plantuml {scale: 0.7}
!theme toy
@startuml
skinparam backgroundcolor transparent

object body {
  font-size = "1rem"
}

object p {
  color = "red"
}

object div {
  posotion = "absolute"
  top = "10px"
}

object span {
  display = "none"
}

body --> div
body --> p
p --> span

@enduml
```

</v-click>


---
layout: items
cols: 2
preload: false
---
# 2. Render tree

::items::

<div
   v-motion 
   :initial="{ opacity: 1, x: 0, transition: { duration: 3000 } }"
   :enter="{ opacity: 0, x: 20, transition: transition}"
>

```mermaid {theme: 'neutral', scale: 0.6}
flowchart TD
    html[html]
    body[body]
    head[head]
    main[main]
    footer[footer]
    div[div]
    p[p]
    html-->body
    html-->head;
    body-->main
    body-->footer
    main-->p
    main-->div
```

</div>


<div 
   v-motion 
   :initial="{ opacity: 1, x: 0, transition: { duration: 3000 } }"
   :enter="{ opacity: 0, x: -20, transition: transition}"
>

```plantuml {scale: 0.7}
!theme toy
@startuml
skinparam backgroundcolor transparent

object body {
  font-size = "1rem"
}

object p {
  color = "red"
}

object div {
  posotion = "absolute"
  top = "10px"
}

object span {
  display = "none"
}

body --> div
body --> p
p --> span

@enduml
```

</div>


<div
   style="position: absolute; left: 50%; transform: translateX(-50%)"
   v-motion 
   :initial="{ opacity: 0, transition: { duration: 2000 } }"
   :enter="{ opacity: 1, transition: {...transition, delay: 1000}}"
>

```plantuml {scale: 0.7}
!theme toy
@startuml
skinparam backgroundcolor transparent

object body {
font-size = "1rem"
}

object p {
color = "red"
}

object div {
posotion = "absolute"
top = "10px"
}

body --> div
body --> p

@enduml
```

</div>

<script setup lang="ts">
const transition = {
    type: 'spring',
    damping: 10,
    stiffness: 20,
    mass: 2,
  } 
</script>

---
layout: section
---
# Layout
Reflow

::right::
<img src="https://images.unsplash.com/photo-1608303588026-884930af2559?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=3701&q=80" />

---
layout: section
---
# Paint
Rasterizing

::right::
<img src="https://images.unsplash.com/photo-1525909002-1b05e0c869d8?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2448&q=80" />


---

# Rendering flow

<br/><br/>
<br/><br/>

<div class="text-center">

```mermaid
flowchart LR;
A[HTML/CSS]
B{{Parsing}}
C[DOM]
D[CSSOM]
E[Render Tree]
F{{Layout}}
G{{Paint}}
A -.-> B --> C & D --> E --> F --> G; 
```
</div>

---
layout: intro
---

# Enters Houdini...

---
layout: section
---

# Extensible Web Manifesto

::right::

<v-click>

The main idea of this manifesto is to move the focus from creating high-level APIs to providing low-level APIs and expose the underlying layers.

</v-click>

---
layout: section
---

# CSS Houdini

::right::

<v-clicks>

- An umbrella term for a group of low-level APIs that are coming as a result of Extensible Web Manifesto applied to CSS
- It is going to enable developers to have access to every phase of the rendering process and extend its behaviour

</v-clicks>



---
layout: section
---

# Typed OM


::right::

<v-click>

It is an upgraded enhanced version of the CSSOM.
- It’ll add types to css values in the form of JavaScript objects.
- On top of that it will provide a set of semantic APIs that makes working with CSS in JavaScript more pleasant

</v-click>


---

# Typed OM


<div class="mt-30">


### With CSSOM:
```ts
const fontSize = getComputedStyle(box).getPropertyValue("font-size");

// 16px
```
</div>


---

# Typed OM


### With the new TypedOM

```ts
const fontSize = box.computedStyleMap().get("font-size");
```
<br />

<img v-click src="assets/typedom_cssunitvalue.png" alt="CSSUnitValue in typed OM" style="height: 300px;" class="mx-auto" />

---

# Typed OM
Setting a transform

```js{1|1-2|4|6|6-7|6-9|all}
const x = CSS.percent(-50);
const y = CSS.percent(-50);

const translation = new CSSTranslate(x, y);

box.attributeStyleMap.set(
  "transform",
  new CSSTransformValue([translation])
);
```

---
layout: fact
---

# Demo
[Code Sandbox](https://codesandbox.io/s/naughty-dijkstra-glz1jy?file=/src/typed-om.js)

---

# Properties & Values API

### Javascript

```ts{all|2|3|4|5|all}
CSS.registerProperty({ 
  name: '--my-color',
  syntax: '<color>', 
  inherits: false,
  initialValue: 'pink',
});
```


<br />

### CSS

```css
@property --my-color {
  syntax: '<color>';
  inherits: false;
  initial-value: pink;
}
```

<div class="text-center">

[CodeSandbox](https://codesandbox.io/s/naughty-dijkstra-glz1jy?file=/src/styles.css)

</div>

---
layout: section
---

# Worklets

::right::


<div v-click>

Worklets are independent scripts that run during the rendering process.
- They run by the rendering engine
- They are thread-agnostic

</div>

<div v-click>

Houdini has introduced three worklets:
- Pain Worklet or Paint API
- Animation Worklet or Animation API
- Layout Worklet  or Layout API

</div>

---

#  Paint API

<br/><br/>
<br/><br/>

<div class="text-center">

```mermaid
flowchart LR;
A[HTML/CSS]
B{{Parsing}}
C[DOM]
D[CSSOM]
E[Render Tree]
F{{Layout}}
G{{Paint}}
A -.-> B --> C & D --> E --> F --> G; 
```
</div>

---
layout: two-cols
---

# Paint API

<br/><br/>

<v-clicks>

1. Register the worklet by a name using `registerPaint`
2. Add the script module in HTML or JS file:
   ```js
   CSS.paintWorklet.addModule('worklet.js')
   ```
3. Use it in CSS with
   ```css
   background: paint(worklet_name)
   ```


</v-clicks>


::right::

<v-click>


### A Paint Worklet

```js{all|1,17,19,20|6,7,12-16|8|9|10,2|11,3|all}
class ImagePainter {
  static get inputProperties() { return ["--myVariable"]; }
  static get inputArguments() { return ["<color>"]; }
  static get contextOptions() { return {alpha: true}; }

	// The only mandatory method
  paint(
      ctx, // CanvasRenderingContext2D
      size, // The geometry of the painting area
      properties, // List of custom propertes
      args // List of arguments passed to paint(..)
  ) {
    
      // Painting logic
      
  }
}

// Register our class under a specific name
registerPaint('my-image', ImagePainter);
```

</v-click>

---
layout: fact
---

# Demo
[Code Sandbox](https://codesandbox.io/s/paint-api-demo-5ozbyv?file=/src/components/Checkbox/style.scss)
---

# Some Cool Websites

- [houdini.how](https://houdini.how/)
- [css-houdini.rocks](https://css-houdini.rocks/)
- [Extra CSS](https://extra-css.netlify.app/)

---

# Can I use it?

<img src="assets/caniuse.png" width="550" class="mx-auto"/>

<div class="text-center">

https://ishoudinireadyyet.com/
</div>

---
layout: fact
---
# Thanks

### Let's keep in touch: @NegarDev












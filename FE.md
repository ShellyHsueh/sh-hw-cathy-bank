# FE HW (Not Angular)

## 1. Sort
> There is an array, each item has such format:
{firstName: 'xxx', lastName: 'xxx', customerID: 'xxx', note: 'xxx', profession: ‘xxx’} lastName, note can be empty, customerID can only be a set of digital numbers. profession can only have ‘student’, ‘freelancer’, ‘productOwner’, ‘engineer’ or ‘systemAnalytics’.

### Q1-1. Please follow the principle (‘firstName’ + ‘lastName’ + ‘customerID’) to sort this array and print it out.
```js
/**
 * Ans1. by js-built-in sort method (for cleaner code)
 */
function sortUserName(users) {
  if (!Array.isArray(users) || users.length <= 1) {
    console.log(users);
    return;
  }

  const toCompareFormat = user => `${user.firstName}_${user.lastName}_${user.customerID}`;
  users.sort((a, b) => toCompareFormat(a).localeCompare(toCompareFormat(b)));
  console.log(users);
}

/**
 * Ans2. by quick sort
 */
function sortUserNameByQuickSort(users) {
  if (!Array.isArray(users) || users.length <= 1) {
    console.log(users);
    return;
  }

  quicksort(users, 0, users.length - 1);
  console.log(users);
}

function quicksort(arr, start, end) {
  if (start >= end) {
    return;
  }
  const pIndex = partition(arr, start, end);
  quicksort(arr, start, pIndex - 1);
  quicksort(arr, pIndex + 1, end);
}

function partition(arr, start, end) {
  const pivotIndex = end;
  const pivot = toCompareFormat(arr[pivotIndex]);

  let pIndex = start;
  for (let i = start; i <= end - 1; i++) {
    if (toCompareFormat(arr[i]).localeCompare(pivot) <= -1) {
      swap(arr, i, pIndex);
      pIndex++;
    }
  }
  swap(arr, pIndex, pivotIndex);
  return pIndex;
}

function toCompareFormat(user) {
  return `${user.firstName}_${user.lastName}_${user.customerID}`;
}

function swap(arr, i, j) {
  const temp = {...arr[i]};
  arr[i] = {...arr[j]};
  arr[j] = {...temp};
}
```

### Q1-2. Please sort by ‘profession’ to follow the principle. (‘systemAnalytics’ > ‘engineer’ > ‘productOwner’ > ‘freelancer’ > ‘student’)
```js
function sortByType(users) {
  if (!Array.isArray(users) || users.length <= 1) {
    console.log(users);
    return;
  }

  const professionWeightMap = {
    systemAnalytics: 0,
    engineer: 1,
    productOwner: 2,
    freelancer: 3,
    student: 4,
  }
  const toCompareFormat = user => `${professionWeightMap[user.profession]}_${user.firstName}_${user.lastName}_${user.customerID}`;
  users.sort((a, b) => toCompareFormat(a).localeCompare(toCompareFormat(b)));
  console.log(users);
}
```


## 2. CSS
```html
<div class="container">
<div class="header">5/8 外出確認表</div> <div class="content">
      <ol class="shop-list">
<li class="item">麵包</li>
<li class="item">短袖衣服</li> <li class="item">飲用水</li> <li class="item">帳篷</li>
</ol>
      <ul class="shop-list">
<li class="item">暈車藥</li> <li class="item">感冒藥</li> <li class="item">丹木斯</li> <li class="item">咳嗽糖漿</li>
</ul> </div>
<div class="footer">以上僅共參考</div> </div>
```

```css
.container {
  font-size: 14px;
}
.container .header {
  font-size: 18px;
}
.container .shop-list {
   list-style: none;
   margin-left: -15px;
}
.container .shop-list li.item {
   color: green;
}

/* Q1. Explain why does this color not works, and how to fix make it work on 1st list */ 
.container .shop-list .item {
  color: blue;
}

/* Q2. Write styling make every other line give background color to next one */
```

* Ans 2-1: The specifier the selector is the higher priority it gets - i.e., the selector containing both element tag and class name (`li.item`) has higher priority than the one with class name only (`.item`).

* Ans 2-2: *(not quite sure the requirement of Q2; I set the lines to be in different background colors)*
```css
.container .shop-list li.item:nth-child(odd) {
  background-color: rgb(252, 234, 213);
}

.container .shop-list li.item:nth-child(even) {
  background-color: rgb(156, 208, 210);
}
```


## 3. Please write down a function to console log unique value from this array
> let items = [1, 1, 1, 5, 2, 3, 4, 3, 3, 3, 3, 3, 3, 7, 8, 5, 4, 9, 0, 1, 3, 2, 6, 7, 5, 4, 4, 7, 8, 8, 0, 1, 2, 3, 1];

```js
function getUniqueNumber(items) {
  if (!Array.isArray(items)) {
    console.log([]);
    return;
  }

  const countMap = {};
  items.forEach(v => countMap[v] ? countMap[v]++ : (countMap[v] = 1));

  const uniques = [];
  Object.entries(countMap).forEach(([v, count]) => count === 1 && uniques.push(v));
  console.log(uniques);
}
```


## 4. What is different between `<section>`and `<article>`, can you make an example how you will be using it?

Both are for better readability of the document.

`<section>` wraps elements that are related to each other and for the same purpose.
Take a product page for instance, there might be an eye-catching section with a title and a rotating banner, followed by several feature sections (including a title, a description, a "Learn More" button, and an image), and a specification section at the end of the page.

`<article>` represents a complete and independent block of contents - i.g., a blog post, an essay, a research, privacy policy declaration, etc.


## 5. Please explain about what is CSS boxing model and the layout components that it consists of

Every HTML element is wrapped as a box with padding, border, and margin (from content itself to outside) - i.e., the box we see in browser debug tools. A box is an display unit in css. It can be placed in a normal flow (default), or in an individual layer with top/right/bottom/left offsets (by positioning).


## 6. Can you explain CSS priority, and what principle are your used to writing CSS stylesheet.

Ans 6-1. CSS priority
Different declaration gets different priority points
Overall: (1) style declared with !important > (2) inline style > (3) styles defined by selectors (ID > class/attribute > element tag)
The more specific css (more selector used) tends to have higher priority; the one with highest priority points gets applied.
If still any conflicts (getting same points), the later declared one gets applied.

Ans 6-2. Personal CSS styling principle
Trying my best to avoid others having to read long/complex CSS files to figure out the style while reading the HTML.
1. Avoid duplication: the same css should be shared, regardless of css methodologies (ex. BEM, OOCSS), or css structure (ex. css module, css in js)
2. Keep the declaration as clear in HTML as possible
3. Keep css declaration as brief as possible
4. Personally prefer the structure that relatively unlikely brings naming conflicts (ex. css module, css in js) while the project grows.

## 7. Can you introducing some of Semantic HTML elements that you already know and how you used it ever, please make some example.

- Heading (h1-h6): the heading level; using it to highlight title (h1), subtitle (h2), abstract/description (h3) for the page; for better UX for who needs, also for SEO.
- Interactive elements (ex. button / a / input): `<button>` for js functionality, `<a>` for links, `<input>` for input/slider; avoid using "`<div>` + js" for interaction if possible; especially important when guiding keyboard users clickable/interactive elements in the page -- I've tuned key board navigation & screen reader with some HTML elements / attributes for web service accessibility
- Page-structural elements (ex. header / nav / aside / main / section / p / footer): declare the structure of the page; `<header>` for top banner (the introduction of the page); `<nav>` for the navigation bar; `<aside>` for side menu; `<main>` the main content of the page; `<p>` for paragraph (for browser styles and screen reader to highlight the paragraph); `<footer>` for the bottom of the page (ex. other links, privacy policy)


## 8. Can you explain about Interface and Enum in Typescript, and where will you be using, please make some examples.

- Interface is for declaring an object structure; commonly used in declaring the props object of a React component
- Enum is for declaring some constants - e.g., if we have some string constants and we use them in many places or use them as category identifiers, it's better to use them via Enum instead of to manipulate a same string everywhere (for better maintainability and for issues caused by typo)


## 9. The photo below is a page structural layer, please according to SEO friendly rules, write down HTML base structure. Note. Mobile friendly first.

```html
<body>
  <header>
    <a href="index.html"><img src="logo.png" alt="Demo Website Logo"></a>
    <nav>
      <ul>
        <li><a href="#">Menu Item 1</a></li>
        <li><a href="#">Menu Item 2</a></li>
        <li><a href="#">Menu Item 3</a></li>
      </ul>
    </nav>
  </header>
  <main>
    <section>
      <div class="item-photos-slide">
        <img src="image1.jpg" alt="description-for-image1">
        <img src="image2.jpg" alt="description-for-image2">
      </div>
      <div>
        <div class="item-detail">
          <h2>Item Detail Heading</h2>
          <h3>Probably need a subheading for the item detail</h3>
        </div>
        <div class="item-desc-list">
          <ul>
            <li>Description 1 for the item detail</li>
            <li>Description 2 for the item detail</li>
            <li>Description 3 for the item detail</li>
          </ul>
        </div>
      </div>
    </section>
  </main>
  <footer>
    <p>&copy; 2023 Website Name | <a href="contact.html">Contact Us</a></p>
  </footer>
</body>
```

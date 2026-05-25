# a nasa daily astronomy image site using their public APOD API
hi! in this guide we will be learning how to use javascript to fetch an API. this is beginner friendly but covers some intermediate concepts like setting up vite and deploying with github actions. in this project we will be using a public API safely with vite and deploying it live on github pages.

## what exactly will we build? 
a website that fetches nasa's astronomy picture of the day and displays the title, image (or video), and explanation. ot automatically updates every day with a new image.
> **note:** this is a simple project meant to teach you the concept of working with APIs. you are welcome to use this as a learning reference but please do not submit an exact copy - make it your own by changing the design, adding features, or using a different API.

i have divided this guide into 12 small stages so nothing gets tangled, and have attached some resources below.
## prerequisites
 
- [Node.js](https://nodejs.org) version 20 or higher installed
- a [GitHub](https://github.com) account
- a code editor like [VS Code](https://code.visualstudio.com)
- a free nasa API key from [api.nasa.gov](https://api.nasa.gov)


## create a vite project
open folder in your code editor in which you will be making your project, open terminal and run:
```bash
npm create vite@latest .
```
the dot up here tells terminal to create vite in the existing opened folder.

when it asks questions, pick:
- framework: **Vanilla**
- variant: **JavaScript**

then install dependencies and start the dev server:
 
1. install dependencies
```bash
   npm install
```

2. start the dev server
```bash
   npm run dev
```
 
open the URL shown in your terminal (usually `http://localhost:5173`). you should see the vite demo page. that means everything is working.

press ctrl + c to stop the server for now.
 
> **important:** always use the localhost URL shown in your terminal to view your project. never open `index.html` directly in the browser or use the VS Code live preview extension. those don't understand vite's module system and your API key will not work.
 
## clean out the scaffold files
 
vite gives you demo files you don't need. delete these:
 
- `src/counter.js`
- `src/javascript.svg`
- `public/vite.svg`
---
## replace index.html
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>NASA Daily</title> <!--put your project title here. in my case, im calling it nasa daily.-->
  </head>
  <body>
    <div id="app"></div>
    <script type="module" src="/src/main.js"></script>
  </body>
</html>
```
the `<div id="app">` is an empty box. your JavaScript will fill it with content. the script tag loads your code.

also, notice the `type="module"` in the script tag - this tells the browser to treat your javascript as a modern ES module. without it, `import.meta.env` won't work and your API key will show as `undefined`.

basically, vite uses ES module syntax under the hood. `type="module"` enables features like `import`, `export`, and `import.meta` in the browser. it also automatically defers the script, meaning it waits for the HTML to fully load before running -- which is why your `document.querySelector("#app")` always finds the element.

the reason why i asked you to add this code is because this is the base structure for any vite vanilla js project. the only things you will ever change are the `<title>` and what goes inside `<body>`.

once you have the basics working, you can extend this by adding a search bar, date picker, or styling with css. the core fetch pattern stays the same.

for example if you wanted to add a date picker to view past NASA images, you would add this to `index.html`:

```html
<input type="date" id="datepicker"/>
<div id="app"></div>
```

and update `main.js` to use the selected date:

```javascript
const date = document.querySelector("#date-picker").value;
fetch(`https://api.nasa.gov/planetary/apod?api_key=${API_KEY}&date=${date}`)
```

but for this guide we are keeping it simple and focusing on the core concept. modifications are up to you!

## javascript

the entire logic of this project lives in `src/main.js`. let's build it step by step so you understand what every line does.

### step 1: get your api key

```javascript
const API_KEY = import.meta.env.VITE_NASA_API_KEY;
```

`import.meta.env` is how vite gives your code access to variables stored in `.env`. the `VITE_` prefix is required - vite only exposes variables that start with it. without this your key will be `undefined`.

### step 2: show something while loading

```javascript
document.querySelector("#app").innerHTML = "<p>loading...</p>";
```

`document.querySelector("#app")` finds the element with `id="app"` in your html. `.innerHTML` sets the content inside it. we show "loading..." immediately so the page is never blank while waiting for the api.
![screenshot](./guideAssets/2026-05-25%20(5).png)

### step 3: fetch the data

```javascript
fetch(`https://api.nasa.gov/planetary/apod?api_key=${API_KEY}`)
```

`fetch` makes a network request to the nasa api in the background. notice the backticks instead of quotes - that's because we're embedding `${API_KEY}` inside the url. the `${}` syntax replaces the variable with its actual value at runtime.

for example:
```javascript
const name = "alisha";
console.log(`hello ${name}`) // prints: hello alisha
console.log("hello ${name}") // prints: hello ${name}
```

regular quotes treat `${}` as plain text. backticks treat it as code.

### step 4: handle the response

```javascript
.then(response => response.json())
```

`fetch` returns a promise -- meaning it runs in the background while the rest of the page loads. `.then` runs when it finishes. `response.json()` converts the raw response into a javascript object you can work with.

### step 5: use the data

```javascript
.then(data => {
```

`data` is the object nasa sent back. open your browser console and `console.log(data)` to see everything inside it. you will see fields like `data.title`, `data.url`, `data.explanation`, and `data.media_type`. it should look something like this:

![screenshot](./guideAssets/2026-05-25%20(6).png)

now we will focus on bringing this data on our site page. remove console.log and pause for a second and think - how would you put `data.title` inside `#app`?

i want you to try this yourself before seeing the guide further. 

hint: you already know `document.querySelector("#app")` and `.innerHTML`. try combining them with `data.title`.

i hope you must've tried it yourself, and if you were successful it would have looked something like this:
```javascript
.then(data => {
    document.querySelector("#app").innerHTML = `${data.title}`;
})
```
`document.querySelector("#app")` finds the element with `id="app"` in your html - the empty div we created earlier. `.innerHTML` then fills it with whatever string you give it. since we are using backticks we can embed `${data.title}` directly inside the string, which gets replaced with the actual title nasa sent back. so instead of seeing `${data.title}` on the page, you see whatever today's image is called. "Thackeray's Globules" in my case. `<h1>` is normally a html tag that makes te biggest heading.


![screenshot](./guideAssets/2026-05-25%20(7).png)

**task:** try adding `data.url` as an image and `data.explanation` as a paragraph yourself in the same place where `data.title` exists, before scrolling down. 

**hint:** use the same `${}` syntax and the html tags you already know - `<img>` and `<p>`.

here is the full solution:

```javascript
.then(data => {
    document.querySelector("#app").innerHTML = `
        <h1>${data.title}</h1>
        <img src="${data.url}" />
        <p>${data.explanation}</p>
    `;
})
```
it must be looking like this:
![screenshot](./guideAssets/2026-05-25%20(8).png)
![screenshot](./guideAssets/2026-05-25%20(9).png)

you can include whatever you want depending on your api and idea.
the image seems huge right now but dont worry that is something we will fix eventually with css styling.

### step 6: handle image or video

if you were able to make it here by implementing each task on your own, then you should be proud of yourself for building the whole thing yourself! now one important thing in our project is that nasa sometimes sends a video instead of an image. so `<img src="${data.url}" />` won't usually work. to solve this problem we handle it with an `if` statement. before moving forward, i want you to give it a try yourself. remember `data.media_type` tells you if it's "image" or "video". try writing an if statement to show either an `<img>` or a `<video>` tag. it doesn't really matter if you didn't write the correct code at all, what matters is that you tried :D

here is how you should give it a try yourself:
```javascript
if (data.media_type === "image") {
    // show image
} else {
    // show video
}
```
solution:
```javascript
let media;

if (data.media_type === "image") {
    media = `<img src="${data.url}"/>`;
} else {
    media = `<video src="${data.url}" controls></video>`;
}
```
now, some of you might be wondering where did `document.querySelector("#app").innerHTML` go? or might be thinking why doesn't it look something like:
```javascript
if (data.media_type === "image") {
    document.querySelector("#app").innerHTML = `<img src="${data.url}"/>`;
} else {
    document.querySelector("#app").innerHTML = `<video src="${data.url}" controls></video>`;
}
```
see, this is technically a correct approach, but the problem is that every time you set `innerHTML` it replaces everything. so your entire code would look something like this:
```javascript
fetch(`https://api.nasa.gov/planetary/apod?api_key=${API_KEY}`).then(response => response.json()).then(data => {
    document.querySelector("#app").innerHTML = `<h1>${data.title}</h1>`;
    if (data.media_type === "image") {
        document.querySelector("#app").innerHTML = `<img src="${data.url}"/>`;
    } else {
        document.querySelector("#app").innerHTML = `<video src="${data.url}" controls></video>`;
    }
    document.querySelector("#app").innerHTML = `<p>${data.explanation}</p>`;
})
```
run this and you will only see the explanation - the title and image are completely gone because each `innerHTML` call wiped out the previous one.
![screenshot](./guideAssets/2026-05-25%20(10).png)

the fix is to store the media in a variable first, then set `innerHTML` exactly once with everything inside it:
```javascript
.then(data => {
    let media;

    if (data.media_type === "image") {
        media = `<img src="${data.url}"/>`;
    } else {
        media = `<video src="${data.url}" controls></video>`;
    }
})
```
now we set `innerHTML` exactly once with everything inside it. `${media}` gets replaced with either the image or video tag we built in the previous step.
```javascript
document.querySelector("#app").innerHTML = `
    <h1>${data.title}</h1>
    ${media}
    <p>${data.explanation}</p>
`;
```

![screenshot](./guideAssets/2026-05-25%20(11).png)
see the difference? i made the size of image small by using inline styles: `style="width: 300px; height: 200px;` to show everything at once . so don't get confused :)

### handle errors

```javascript
.catch(err => {
    document.querySelector("#app").innerHTML = `<p>Error: ${err.message}</p>`;
});
```

`.catch` runs if anything goes wrong -- no internet, wrong api key, api is down. without this, errors fail silently and you just see a blank page with no idea why.

### the complete code

```javascript
const API_KEY = import.meta.env.VITE_NASA_API_KEY;

document.querySelector("#app").innerHTML = "<p>loading...</p>";

fetch(`https://api.nasa.gov/planetary/apod?api_key=${API_KEY}`)
  .then(response => response.json())
  .then(data => {
    let media;

    if (data.media_type === "image") {
      media = `<img src="${data.url}"/>`;
    } else {
      media = `<video src="${data.url}" controls></video>`;
    }

    document.querySelector("#app").innerHTML = `
      <h1>${data.title}</h1>
      ${media}
      <p>${data.explanation}</p>
    `;
  })
  .catch(err => {
    document.querySelector("#app").innerHTML = `<p>Error: ${err.message}</p>`;
  });
```
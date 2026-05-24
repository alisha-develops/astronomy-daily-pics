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


## step 1: create a vite project
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
 
## step 2: clean out the scaffold files
 
vite gives you demo files you don't need. delete these:
 
- `src/counter.js`
- `src/javascript.svg`
- `public/vite.svg`
---
## step 3: replace index.html
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
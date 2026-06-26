> note for the reviewer: ~7 straight went entirely into the markdown guide. cuz its intentionally a super beginner project, i had to deliberately write js code that was wrong to show examples of what could go wrong, and then walk through how to fix it using different approaches (this is also why js took 2 hr). pls have a look at  [`guide.md`](./GUIDE.md)! tysm :D**
# my nasa daily astronomy image site (APOD API) 

<table>
  <tr>
    <td valign="top" width="50%">
      <p>hi! welcome to my project. i built a live space dashboard that talks directly to nasa's computers to grab and display the astronomy picture of the day (apod).</p>
      <p>it automatically pulls today's official astronomy title, description, and handles switching between high-res photos and youtube video embeds depending on what nasa uploads that day.</p>
    </td>
    <td valign="top" width="50%">
      <img src="https://cdn.hackclub.com/019e7ab0-8296-7103-b85f-1821f63f7e45/2026-05-26%20(7).png" alt="NASA APOD Dashboard">
    </td>
  </tr>
</table>

this project is as an official guide and exemplar build for my mission of web development over at the hack club stardance mission (https://stardance.hackclub.com/). since this is literally a guided project example for the mission, the code is intentionally stripped down to the absolute essentials.

## what i did & why it's beginner friendly

i designed this repo to be a learning blueprint for everyone else in the stardance program, i intentionally kept the features as simple and clean as possible. i wanted to strip away the noise so the core logic is incredibly easy to read and recreate, which is exactly why it serves as a great beginner project.

here is exactly what i implemented in the project pipeline:
- instead of writing old-school plain html, i initialized a modern vanilla js vite project sandbox so we can run a real local development server.
- to keep things secure for open source, i used vite's .env files and import.meta.env system. this lets us use the nasa api safely without hardcoding secret keys where people can steal them on github.
- i wrote a clean fetch() pipeline using .then() chains to pull raw space data in the background and parse it into working javascript objects.
- i used basic dom manipulation (queryselector and innerhtml) alongside conditional if/else checks to inject the data straight into an empty html shell on the fly.
- i configured an automated deploy.yml workflow using github actions so that every single time code is pushed to the main branch, a cloud runner automatically builds the vite assets and updates the live site on github pages.

prerequisites to build this
if you want to spin up your own workspace based on my project, make sure you have these ready:
1. node.js (version 20 or higher installed)
2. a github account to save your progress
3. vs code (or your favorite code editor)
4. a free api key from api.nasa.gov

ready to build your own?
if you are a track participant looking to learn how to do all of this step-by-step, i wrote a massive, 12-stage breakdown covering every single line of code and folder setup. i even included mini-tasks to help test your brain along the way!

**go check out my full tutorial file here: [`GUIDE.md`](./GUIDE.md)**

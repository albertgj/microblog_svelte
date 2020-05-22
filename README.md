### In order to use correctly the web app read this document. 
___
## Introduction
This Web App was created with [SvelteJS](https://svelte.dev) and it uses the [microblog rest api](https://github.com/albertgj/microblog).

## Known Problems
* THERE IS A PROBLEM WITH THE ROUTING LIBRARY USED IN THIS APP
  * **To avoid any problems DO NOT refresh the page when you navigate between pages. You can refresh only when you are in the homepage, otherwise the router doesn't find the pages.**

## Instructions
First of all pull the microblog rest api, to get the newest features.

Run the microblog rest api and mysql workbench.

**The registration of a new user gives this user a "USER ROLE" by default**

**Also when you REGISTER a new user you have to navigate to the login page to signin.**

**You can use the two admin users that are saved in the db automatically:**
| USERNAME | PASSWORD |
|----------|----------|
| admin1   | root     |  
| admin2   | root     |  

## Notes
The jwt that is sent by the server is stored in a cookie. **This is not recommended**, it should be stored in a **HttpOnly cookie**, but for **semplicity** I used normal client side cookies to store the token.
___

## Configuration

*Looking for a shareable component template? Go here --> [sveltejs/component-template](https://github.com/sveltejs/component-template)*


## Get started

Install the dependencies...

```bash
cd microblog-svelte
npm install
```

...then start [Rollup](https://rollupjs.org):

```bash
npm run dev
```

Navigate to [localhost:5000](http://localhost:5000). You should see your app running. Edit a component file in `src`, save it, and reload the page to see your changes.

By default, the server will only respond to requests from localhost. To allow connections from other computers, edit the `sirv` commands in package.json to include the option `--host 0.0.0.0`.


## Building and running in production mode

To create an optimised version of the app:

```bash
npm run build
```
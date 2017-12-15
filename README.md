# Project Summary
The no-database project is your first major project here at DevMountain. It is your first opportunity to bring together the various skills you've learned and demonstrate how well you can put them together.

It is important to remember that this is first and foremost a learning experience. Your application will not be perfect. Set a high bar for yourself, but don't expect this app to revolutionize an industry or be the best at this or that thing. The major projects at DevMountain (NoDB, personal, group) are more often than not the best learning experience opportunities you'll have during your time spent here.

### This Repo is going to cover these steps in order to get your started:
  * Create-React-App
  * Setting Up A Server File
  * Running Nodemon
  * Writing a basic axios call
  * Building the corresponding Endpoint

## Create React App

By now you should have `create-react-app` installed, but if this isn't the case run `npm install -g create-react-app`.  When that is finished, cd into the folder where you would like to create your new app and run `create-react-app <name>` (replace name in the command with the name that you would like to give your project).  Please note that `create-react-app` will create a folder with the newly created basic react app inside of it. Once create-react-app finishes, cd into the project directory and run `npm start`. You have now created a new react app!

## Setting Up the Server File

The first step in creating your server is setting up the file structure. It is recommended that you create a folder in the root of your project called `server`. Inside the server folder, create a file called `server.js`. Be aware that this file structre is not requrired to create a server, it is just a common and clean set up. Now you are ready to open up your server.js file. You will need to install three dependencies for this file: express, body-parser and cors.  To do this open up your terminal, make sure you are in your project's root, and run either `npm install --save express body-parser cors` or `yarn add express body-parser cors`.  

You will then need to import those dependencies in your server.js file. 
```javascript
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');  
```
   
You will then need to use those imported dependencies. 
 
```javascript
const app = express();
app.use(bodyParser.json());
app.use(cors());
```
 
Now it is time to specify what port you want your server running on and get it listening on that specific port.  To do this create variable `port` and set it equal to an arbitrary number (usually between 3000 and 9000). For this demonstration we will choose `3333`.  Then use the `app.listen()` method to set up the server to listen on that port. It should look something like this.

```javascript
const port = 3333
app.listen(port, () => console.log(`Server listening on port ${port}...`));
```

These are the basic steps to getting your first server built and running!
 
## Running Nodemon

Now in order to make that server functional we need to run NodeJS. Node is a run-time environment for executing JavaScript code server-side. We can just run `node server/server.js` to get it up and running, but we would have to restart our server (Node) each time we made a change. Enter Nodemon. Nodemon is a utility that will monitor for any changes in your sever and automatically restart it for you, which is awesome for development! If you haven't installed nodemon yet, the command is `npm install -g nodemon`. (We are installing nodemon globally because it will be useful for every project you make going forward). 
 
Once nodemon is installed you can run `nodemon` and point it at your server `server/server.js` for a total command of `nodemon server/server.js`. That is a perfectly fine way to do things but we are developers now! The less typing the better! We can rid the need of typing `server/server` after our nodemon call each time very easily. To do this open up your package.json file and underneath the property `private` insert the line:

```
"main":"server/server.js",
```

Be sure to remember the trailing `,` so that you have a valid JSON document! Then in the terminal run `nodemon`. You have now officially created a server and have it up and running.
 
## Making An Axios Call

It's time to get our front end (React side) talking to our back end (Node side)!  First install axios with `npm install --save axios` or `yarn add axios`.  Create-react-app builds a shell App component for you, but for this component we will need state.  Add a constructor, super, and state to your component so that when we make our axios call we can save that information. For this demonstration we will be receiving an image link from the server to display in the React app. Let's go ahead and create a piece of state named `picture` that will be an empty string. The top of your component should look something like this:

```javascript
class App extends Component {
  constructor() {
    super();
    this.state = {
      picture: ''
    };
  }
}
```

Now, let's use a React component lifecycle method to run our axios call as the page loads. The lifecycle method we will want to use for our app is `componentDidMount()`. This will run anything inside the method everytime the page reloads. Create the `componentDidMount` method then make a `axios.get()` call inside of it. We need to direct our axios call to our node server's endpoint, we do that by using `http://localhost:3333/api/test` The port number on `localhost` is the same port that we created to run our server on. `/api/test` is the specific server endpoint we want to hit. We will use `this.setState()` inside of the `.then` to set the image link we received to state so it can be used later. It should look like this: 

```javascript
componentDidMount() {
  axios.get('http://localhost:3333/api/test')
  .then(response => {
    this.setState({
      picture: response.data
    });
  });
}
```

## Building The Correponding Endpoint

Now we need to build an endpoint to receive the axios call that we are making on our front end. This endpoint will take in information sent in the request (`req`) and use it to perform a task, then send it back to the front end in a response (`res`).  In this case, the front end is sending a "get" request because it wants to simply receive information. We must match the type of request, therefore we will be creating an `app.get` endpont. The URL of this endpoint will need to match what we had in our axios call `/api/test`. Inside the callback of the endpoint we need to specify what exactly the get response will send back to the front end, in this case a link. It should look like this: 

```javascript
app.get('/api/test', (req, res)=> {
  res.status(200).send('http://i63.tinypic.com/2j9y8p.png');
});
```

Run the React dev server with `npm start`. When viewing our app at http://localhost:3000, as the page reloads we are making an axios call to our server and it is sending us back some information, in this case, a picture URL. Finally, in App.js, make use of React state by replacing the JSX/HTML line for the logo, like this:

```javascript
<img src={this.state.picture} className="App-logo" alt="logo" />
```

You should now have an image of Kanye West, like this:

![Preview of website showing spinning Kanye](https://github.com/tylercollier-devmtn/no-db-shell/raw/master/assets/preview.png)

Good luck and happy coding!

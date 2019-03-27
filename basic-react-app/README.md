This is how to install ReactJS which is a frontend library.

This will be installed on a linux 64.

First make sure NodeJS is installed, because we need to use npm installer which comes with NodeJS.

OK, create folder for the project.

	mkdir basic-react-app

Then cd into the folder

	cd basic-react-app

Now we need to create the package.json file.

The package.json file is what will manage all the node packages like the dependencies for the project.

We can do this easily by using this command.

	npm init

BTW if you add the 

	-y

command you can skip all the info inputs

Now we need to create the package-lock.json

	npm install

Now we need to install webpack and the webpack cli.

	npm install webpack webpack-cli --save-dev

Webpack does the work for you. It's basically kind of like a compiler that turns the JS into a bundle that can be referenced by the HTML.

We need to create a webpack.config.js file

	 touch webpack.config.js

This will let webpack know where the packages are compiled to.

For react is it in

	dist

folder

Just add this boilerplate code to the webpack.config.js

	const path = require('path');

	module.exports = {
	  entry: './src/index.js',
	  output: {
	    filename: 'main.js',
	    path: path.resolve(__dirname, 'dist')
	  }
	};

Now we need to create a index.js file in a src direct

	mkdir src

	cd src

	touch index.js

This is where we put JavaScript that interacts to the index.html.

Lets do a simple Hello World function.

	function helloWorld() {
	  let element = document.createElement('div');
	  element.innerHTML = 'Hello world!';
	  return element;
	}

	document.body.appendChild(helloWorld());

Now we need a HTML file.

Create this in the dist dir.

	mkdir dist

	cd dist

	touch index.html

Lets do a simple HTML sheet.

	<!doctype html>
	<html>
	  <head>
	    <title>Hello World!</title>
	  </head>
	  <body>
	    <script src="main.js"></script>
	  </body>
	</html>

Ok, run webpack

	npx webpack

Make sure the main.js file is in the dist dir

Lets add a build script in the package.json file

	"build": "webpack"

This will let us run the command

	npm run build

which lets use use npm instead of running webpack

Ok, if we open the file in a browser

	dist/index.html

we should see hello world.

Now its time to add Reactjs and its dependencies.

	npm install react react-dom

Let's change the helloworld function we have to react.

In the src/index.js add this react component

	import React from 'react';
	import ReactDOM from 'react-dom';

	ReactDOM.render(
	React.createElement('h1', null, 'Hello from React!'),
	document.getElementById('root')
	);

Now we have to update the index.html in the dist to render the component.

	<div id="root"></div>

That's pretty much it.

But we can add a react-server if you want to host a local server on your machine.

But we don't actually have to do that.

If you really want to do that then use npm to install the react-server whatever it is now.

Make to check to see what the most recent version is because installing an older version really messes up the installation.

Then add a command in the package.json called build and add the command

then it will be 

	npm build

or something similar and then it will create the server and show up on a localhost server.

But honestly just manually opening the file is fine, if you dont mind refresing it everytime there is a change.

Also you may want to install babel.

But last time I installed it, there were a bunch of errors because I installed an older verison and I dont want to deal with another dependency unless I have to use it.



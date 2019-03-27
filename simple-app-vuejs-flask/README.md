This is a simple full stack setup with Vue.js on the frontend and flask on the backend.

I chose these two frameworks to build an app because of the low amount of boilerplate code.

After looking into the various frameworks for web development, I wish I could stick to just html,css,js but there are limitations on speed, outdated design patterns.

These tools do speed up development time and there are powerful tools built into the frameworks that allow for something to be done with one line of code. Manually this could take hours, etc.

I believe starting an application with Vue.js and flask, for a simple prototype and then rewritting it before it gets too big with a more heavy framework can be a good process.

For something like angularjs, just building a simple webpage is overkill.

The machine is a linux 64.

Firstly install the Vue.js CLI.

A CLI is a command line interface that has the commands that make installing the nessessary dependencies.

To install the Vue CLI we need npm so make sure to have NodeJS installed to use the npm installer that comes with it.

	npm install -g vue-cli

We'll have the client-side aka frontend and the backend in seperate folders.

	vue init webpack frontend

This will create the vue packages in the frontend dir

	cd frontend

	npm install

this will create a package.json file

After everything is installed 

	npm run dev

This is create the localhost server

we have vue installed

Ok, let's make a simple home page and an about page.

put these pages in the 

	frontend/src/components

First create the Home.vue page

	touch Home.vue

Insert this simple page

	// Home.vue

	<template>
		<div>
			<p>Home page</p>
		</div>
	</template>

Now create the About.vue page

	touch About.vue

Insert this simple page

	// About.vue

	<template>
		<div>
			<p>About</p>
		</div>
	</template>

Ok we need to add the path to the router that is in the index.js file.

It should look like this for the two components

	import Vue from 'vue'
	import Router from 'vue-router'

	const routerOptions = [
	  { path: '/', component: 'Home' },
	  { path: '/about', component: 'About' }
	]

	const routes = routerOptions.map(route => {
	  return {
	    ...route,
	    component: () => import(`@/components/${route.component}.vue`)
	  }
	})

	Vue.use(Router)

	export default new Router({
	  routes,
	  mode: 'history'
	})

We need a dist dir to have our output from compiling the bundles.

These frontend frameworks have alot of boilerplate code, because what they do is allow you to develop on them, to their specifications and then compile them into regular html/css/js that can be found in the dist dir. 

So in the 

	frontend/config/index.js

add another level

	../

to the 

	index: path.resolve(__dirname, '../dist/index.html'),
	assetsRoot: path.resolve(__dirname, '../dist'),

So the dist folder will have the same level as the frontend.

This is because we have a folder system like this

	main project folder

		- frontend

		- backend

We want to have the dist folder to be built in the same level as the frontend.

So at the end it will look like this

	main project folder

		- frontend

		- backend

		- dist

Ok, build the bundle.

	npm run build

Now for the backend

We need be using python3

Create the backend dir

	mkdir backend

now cd into the backend dir

	cd backend

We will create a virtual enviroment for our python project.

A virtual env is basically a virtual seperation between the folder and the rest of the system.

This is important because we may have to install dependencies that are not relevant to other folder in the system.

Make sure to have virtualenv install, the easiest way is with pip.

Python and pip should be installed by default

	pip install virtualenv

Ok, now create the virtualenv for the folder

	virtualenv -p python3 venv

this is create the venv folder where the dependencies for virtualenv are. You can change the name of it anyway you like

	virtualenv -p python3 virtualenv

Ok, to activate the virtualenv just use this command

	source venv/bin/activate

You should see the

	(venv)

This shows we are in the virtual environment.

Now time to install flask

	pip install flask

Flask is like a barebones version of Django, minus the frontend part and the CMS. Actually Django is quite different but they are both written in Python.

It will allow you create a backend for web application very simply.

We need a run.py for the flask server.

in the root dir

	touch run.py

Input this simple server side script

	from flask import Flask, render_template

	app = Flask(__name__,
            static_folder = "./dist/static",
            template_folder = "./dist")

	@app.route('/')
	def index():
	    return render_template("index.html")

Ok lets run flask

	FLASK_APP=run.py FLASK_DEBUG=1 flask run

Ok we have the web server on localhost:5000

So far we have the frontend

	localhost:8080

and the backend

	localhost:5000

Now we can add API endpoints to allow the backend to talk to the frontend.

This is basics of creating a full stack web application with Vue.js and Flask.

# Angular Test

My machine is a 64bit Window10 that is running all these commands on the Linux Subsystem. 

The version of linux is Ubunutu 16.04.

Make sure to have node and npm installed.

I used nvm to install the most recent versions of node and npm.

Now install the angular CLI. If already installed skip this step.

	npm install -g @angular/cli

if there are any administrator issues, either run the terminal as an admin OR just do a "sudo" before the command.

	sudo npm install -g @angular/cli

Create the workspace

	ng new my-app

This will create a new app named "my-app"

If everything install correctly then go to next step.

If not then fix errors.

Most of the time it seems like it's a version issue.

Make sure node and npm are latest version.

Go into my-app workspace

	cd my-app

then run the application on a local machine

	ng serve --open

--open command opens it for you.

if you do 

	ng serve

Then just go to

	localhost:4200

on a web browser and there should be a web page.

# Making simple edits

Components are in the

	src/app

For a new app there is a default component

	src/app/app.component.ts

	src/app/app.component.css

For this test I will be adding Angular Material

Which seems to be a open source libaray for front-end components.

In the workspace run the following command

	ng add @angular/material

To start out I'm only going to use the materialNav

	ng generate @angular/material:materialNav --name myNav

It will create a myNav component in the app folder.

Just add the

	<my-nav></my-app>

in the

	src/app/app.component.ts

I'm running into a problem.

Basically, I was running node 8 instead of 11.

So now it is throwing errors about node-sass not matching the version.

Right now I deleted the node-sass folder in the 

	C:\Users\myUserName\AppData\Roaming\npm-cache

And then I am running 

	npm install

So it should rebuild with a new version of sass.

It didn't work I am going to force npm to rebuild node-sass

	npm rebuild node-sass --force

This allowed the compiler to successfully compile the project and now it runs on the server.

BTW if you want to do the production version

the command is 

	npm build --prod

or something similar.

This will create the compiled version of the angular site

in the 

	dist 

dir and then you can host that on a server

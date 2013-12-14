# Dr. Rendr or how I learned to love Backbone + Node

!

## Overview of Architecture

- Single page web application
- Uses AJAX to drive page loads
- Uses MVC style patterns
- Uses industry standard BackboneJS
- Uses NodeJS and Rendr to allow server side rendering
- Precompiled assets

!

## Set it up and Git it running

- Uses NPM for loading dependencies
- Uses NodeJS to compile JS and CSS assets
- Uses NodeJS to run ExpressJS Server for serving requests
- GruntJS scripts build, run and deploy tasks

```bash
$ npm install

$ grunt server
```

!

# The Handlebar is connected to the Backbone

!

## Typical flow of web pages

1. Browser requests a page from the server 
	- www.icecream.com/cones
2. Server loads any related data needed to render page
	- API: /icecream/cones
3. Server responds with an rendered HTML page
4. Browser renders page and loads related JS script files
5. User clicks on a link on page
6. Back to step 1

!

## Backbone to the Rescue

- Single page web application
- Only load the data needed to render the next page
- Smaller request downloads
- Faster rendering times

!

## Typical flow of Backbone App

1. Browser requests a page from the server 
	- www.icecream.com/cones
2. Server responds with an rendered HTML page
3. Browser renders page and loads related JS script files
4. Browser loads any related data needed to render page
	- API: /icecream/cones
5. Browser renders the page using data

!

## Typical flow of Backbone App - Client side

1. User clicks on a link on the page
2. Backbone intercepts link
3. loads related data needed to render page
	- API: /icecream/cones/waffle
4. Browser renders the page using new data

### *Note*

- steps 3 and 4 is same as 4 and 5 from server rendering!
- simplified workflow

!

# Rendr adds the C in MVC

- Backbone is typically MVVP
	- too much controller logic is typically placed in the view
	- views become monolithic
- Backbone shows an interstitial loading page prior to loading data
- Search engines can't crawl single page web applications

!

## Rendr to the rescue!

- Enforces a controller layer to separate data from views
- Adds server side rendering of views
- Search engines can crawl pages like any other web site
- Browsers display pages immediately with or without JS
- Has a cool name

!

## Typical flow of Rendr App

1. Browser requests a page from the server 
	- www.icecream.com/cones
2. Server loads any related data needed to render page
	- API: /icecream/cones
3. Server responds with an rendered HTML page
4. Browser renders page and loads related JS script files
5. Browser bootstraps any data that was used to load the page into scope
	- session data (if logged in) 
	- list of athlete models
6. Browser initializes Backbone

!

## The contoller layer

- Defines what data is needed to load for a given request
- Decorates the data as needed for application
- Defines the view to render the data

!

## Typical controller

```javascript
# routes.js
match( '/cones/:name', 'cones#view' );

# controllers\cones_controller.js
view: function(params, callback){
  var spec = { model: {
        model: 'cones',
        name: params.name
      } };
  this.app.fetch(spec, function(err, result){
    callback(err, 'cones', result);
  } );
}
```

!

## The view layer

- Specifies the template to render
- Prepares the data for the template
- Handles any user interactions

!

## Typical view

```javascript
# views\cones.js
getTemplateData: function(){
  var d = this.model.toJSON( );
  d.message = 'Do you like ' + d.name + '?';
  return d;
},

events: {
  'click .like-button': 'likeProfile'
},

likeProfile: function(ev){ ... }
```

!

## Templates - Handlebars

- Simple syntax
- Almost logicless templates
- Helpers add super powers
- Compilable and super fast!

!

## Typical template

```handlebars
<h2>Welcome to the {{name}} page!</h2>
{{#if isLiked}}
<div>
	<span>Haha! You like {{name}}</span>
	<span>Now the whole world knows!</span>
</div>
{{else}}
<div>
	<span>{{message}}</span>
	<button class="like-button">Like</button>
</div>
{{/if}}
```

!

# Lost? SourceMaps to the rescue!

- Shows same file structure on browsers as in project
- Shows original source code with comments regardless if code is minified or obfuscated
- Works on most modern browsers including mobile ones!
	- Chrome, Firefox, Safari
	- Mobile Safari, Chrome for Andriod

!

# Ugly Mocha Lint - the spice of life

- JS is a scripting language, uncompiled, untyped, and bloated. Pretty ugly!
- JS is a scripting language, there is no structure and uniformity. Code starts to get dirty and junk up!
- JS is a scripting language, tons of bugs can pop up. Wasts many hours and late nights debugging and fixing.

!

## Uncompiled, untype code is ugly? - Uglify it!

- Compiles JS files into a single file
- Sanity checks JS files
- Minifies and compresses code
- As close as we can get to compiled JS - not TypeSafe be we are getting there!

!

## Dirty code? - Lint it!

- Code Nazi
- Enforces coding standards
- Restricts usage of common pit falls
- Enforces coding style

!

## Late nights fixing code - Mocha

- Unit testing framework
- Uses simple BDD style syntax
- Runs on both server (headless) and client (browser)

!

# That's a wrap!

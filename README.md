# Easel

A boilerplate for building large Backbone projects that can run on the server and client. Modeled after the architecture of production apps at [Artsy](http://artsy.net/).

[Download Easel in Javascript](https://github.com/artsy/easel/archive/master.zip)

[Download Easel in Coffeescript](https://github.com/artsy/easel/archive/master.zip)

## Introduction

Easel makes it easy to start building flexible & modular Backbone apps that can run in the browser and on the server using [node.js](http://nodejs.org/). Built on popular libraries like [Express](http://expressjs.com/), [Backbone](http://backbonejs.org/), and [Browserify](http://browserify.org/), Easel isn't a framework of it's own, but rather a boilerplate of libraries and patterns that can be leveraged or abandoned as needed.

## Getting Started

### Installation

1. Install [node.js](http://nodejs.org/)
2. Download the boilerplate: [Javascript](https://github.com/artsy/easel/archive/master.zip) | [Coffeescript](https://github.com/artsy/easel/archive/master.zip)
3. Install node modules `npm install`
4. Run the server `make s`
5. Visit [localhost::4000](http://localhost:4000) and see an example that uses the Github API.

### Overview

Easel is composed of some core tools you should learn before diving in.

* [Backbone](http://backbonejs.org/)
* [Express](http://expressjs.com/)
* [Browserify](https://github.com/substack/node-browserify)
* [Sharify](https://github.com/artsy/sharify)
* [Benv](https://github.com/artsy/benv)

Some modules come with Easel by default, but could easily be swapped out with your own preference.

* [Jade](https://github.com/visionmedia/jade)
* [Stylus](https://github.com/learnboost/stylus)
* [Mocha](https://github.com/OliverJAsh/node-jadeify2)
* [Should](https://github.com/visionmedia/should.js/)
* [Sinon](http://sinonjs.org/)
* [Zombie](http://zombie.labnotes.org/)
* [Jquery](https://github.com/components/jquery)

Easel is a Backbone app at it's heart and therefore relies on an external API as it's data source. This can come in a [variety](https://github.com/intridea/grape) [of](http://expressjs.com/) [forms](http://flask.pocoo.org/), and it's up to you to choose the best technology to serve your data over HTTP.

Once you understand how the above projects work, diving into Easel is just a matter of understanding it's patterns. When you're ready you can delete all of the example files and start clean by running `make clean`.

### Project vs. Apps vs. Components

Monolithic frameworks tend to organize your code by type such as /views, /stylesheets, /javascripts, etc. As your app grows larger this becomes an awkward and unmaintable way to delineate parts of your project. Easel encourages grouping files into conceptual pieces instead of by type. There are three different levels of this organization:

#### Project

Refers to the root, "global", level and contains the initial setup/server code and project-wide modules such as models, collections, and libraries. Setup is extracted into /lib/setup to encourage modularizing and testing your setup code.

#### Apps

Apps are small express applications that are [mounted into the main project(http://vimeo.com/56166857). What deliniates apps from one another is they conceptually deal with a certain section of your project, and they are often separated by a full page-refresh.

An app could be a complex thick-client "search" app, or a simple static "about" page. The organization of these apps are up to you, for a simple app you may put all of your code into an index.js file, while more complex apps may have their own /models, /components, /stylesheets, /templates, etc. folders.

#### Components

Components are small portions of UI re-used across apps and generally contain a mix of stylesheets, templates, and client-side code. Examples can be complex, like an autocomplete widget, or simple as a headers stylesheet styling h1-h6 tags.

### Models & Collections

Model code is meant to work on the server and client so it must be strictly domain logic, and can't use APIs only available to the browser or node such as accessing the file system or the `XMLHttpRequest` object.

Backbone.sync is used as a layer over HTTP accessible on both sides. Any HTTP requests therefore need be wrapped in a Backbone class or used by an anonymous instance e.g. `new Backbone.Model({ url: '/api/system/up' }).fetch({ success: //... })`.

### Libraries

Libraries are a place to store modules that are used across apps and don't pertian to domain logic or UI that can be better handled by models or components. These can be server only such as a converting an uploaded jpeg file into thumbnails, browser-only such as an HTML5 Canvas library, or even shared such as a date parsing library that can be utilized on both the server and client.

### Testing

Tests are broken up into project-level and app-level tests that are run together in `make test`. This boilerplate comes stocked with a suite of tests as examples to work from.

#### Project-level Tests

Project-level tests involve any component, library, model, or collection tests. Because Easel model code can run on the server you can easily test it in node without any extra ceremony. However components and some libraries are meant to be run in the browser. For these you can use [benv](http://github.com/artsy/benv) to set up a fake browser environment and require these modules for unit testing like anything else.

#### App-level Tests

App-level tests can come in a number of different forms, but often involve some combination of route, template, client-side, and integration tests. Given that apps can vary in complexity and number of components they use, it's up to you to decide how to structure and test their parts.

Some common practices are to split up your route handlers into libraries of functions that pass in stubbed request and response objects. Templates can simply be compiled and asserted against the generated html. Client-side code can be unit tested in node using [benv](http://github.com/artsy/benv) (using Backbone views can make this easier). Finally a suite of integration tests use [Zombie](http://zombie.labnotes.org/) to boot up a version of the project with a fake API server found under /test/helpers/integration.

### Build Scripts & Configuration

Easel uses simple configuration and build tools that are standard with most environments.

A [Makefile](http://en.wikipedia.org/wiki/Make_(software) designates build commands. When more complex build scripts are needed it's encouraged to wrap them in modules that can be run via `node lib/script.js`.

Configuration is handled entirely by [environment variables](http://en.wikipedia.org/wiki/Environment_variable). For ease of setup there is a /config.js file that wraps `process.env` and declares sensible defaults for development.

### Asset Pipeline

Easel's asset compilation is mostly handled by [Browserify](https://github.com/substack/node-browserify) and [Stylus](https://github.com/learnboost/stylus) with middleware added to lib/setup and a `make assets` task to build something more production ready.

Place your asset packages in /assets and point your script or style tags to /assets/{filename} in your views and Easel will wire these up to compile on request in development and generate minifed assets for production under public/assets.

## License

(The MIT License)

Copyright (c) Craig Spaeth craigspaeth@gmail.com, Art.sy, 2013

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
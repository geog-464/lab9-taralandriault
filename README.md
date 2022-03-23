# lab9 - Intro to JavaScript and web mapping

The present lab is intended to introduce you to [JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/What_is_JavaScript) (JS) and how it integrates with webpages to make them more dynamic. The content presented to you in this tutorial will involve the creation of standard JS functions to manipulate [DOM objects](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction), and will have you integrate external JS libraries for web mapping.

Note that the current instructions are best viewed directly on Github from your web browser.

# Setting up

- Clone/download your repo to your machine.

You can go about this exercise using the command line or your operating system's graphical file browser interface (e.g. Windows Explorer, Mac's Finder). Do whatever you're most comfortable with.

- Make sure your repo/workspace directory is set up with the following subdirectories (i.e. Create the following folders):
	- js/
	  - Any custom JS code will be stored in .js files inside this folder
	- css/
	  - Where you will put - you guessed it - CSS stylesheets!
	- data/
	  - A *data* folder is not necessary for all websites, but very relevant when making websites that use client-side data (e.g. GeoJSON, CSV, etc.)
	- imgs/
	  - If your website has images, this is where they should be stored.
	- lib/
	  - In here we will store any third-party libraries, such as the web mapping JS libraries we will be using.
- Open the html file provided for you called `index.html` inside a text/code editor of your choice. This file contains some *boilerplate* code which you will be adding to in this exercise.
- Notice that inside your .html file, at line 9 and 20, two files are linked/imported...
  - Open `index.html` in your web browser.
  - When opened, open your web browser's [*inspect*](https://www.thoughtco.com/get-inspect-element-tool-for-browser-756549) tool pane. You will be returning to this pane often!
  - Navigate to the [*console*](https://support.monday.com/hc/en-us/articles/360002197259-How-to-Open-the-Developer-Console) in your inspect window.
  - Depending on your browser, you should see an error or two indicating that the linked files couldn't load. Indeed, this is because they don't exist yet.
- These files are linked to (imported) from inside the *js/* and *css/* subfolders respectively. Create a file called `main.js` in the first and `styles.css` in the second.
- Now reload your webpage (Ctrl/Cmd+R) and there should no longer be any errors in the console (this is also where you will be checking for any JS-related errors going forward!)

Like when adding your name to a map you've created, it's good to add your name to code you've created.

- 📝 [Q1] [Comment out](https://coder-coder.com/comment-in-html-css-javascript/) the first line of both your js and css files and inside this comment notation, add that the file was created by YOUR NAME, followed by the YEAR.

# Understanding JavaScript DOM manipulation

- On line 16 of your html file, paste the following:
```html
<div id="div1">
	This is inside basic html container
	<p>This is text contained inside of a standard easily-stylable tag</p>
</div>
```
- in your styles file, add:
```css
#div1 {
    height: 200px;
    width: 80%;
    padding: 10px;
    margin: 0 auto;
    background-color: cyan;
    border-style: solid;
    border-width: 5px;
    border-color: black;
}
```

As always, save the files you just edited (Ctrl/Cmd+S) and refresh your web browser. Before continuing, make sure you understand what each of these code snippets is doing.

- 📝 [Q2] Why is the JS script loaded at the end of the html file and not the beginning like the stylesheet? (write your answer inside a <p> tag inside your *div1* at line 18 of `index.html`. Prepend your answer with the number *2.* to clearly identify it!).

- Next, in your `main.js`, you will paste all of the following code:

```javascript
//initialize function called when the script loads
function initialize(){
    cities();
};

//function to create a table with cities and their populations
function cities(){
    //define an array of objects for cities and population
    var cityPop = [
        { 
            "city": "Madison",
            "population": 233209
        },
        {
            "city": "Milwaukee",
            "population": 594833
        },
        {
            "city": "Green Bay",
            "population": 104057
        },
        {
            "city": "Superior",
            "population": 27244
        }
    ];

    //create the table element
    var table = document.createElement("table");

    //create a header row
    var headerRow = document.createElement("tr");

    //add the "City" column
    var cityHeader = document.createElement("th");
    cityHeader.innerHTML = "City";
    headerRow.appendChild(cityHeader);

    //add the "Population" column
    var popHeader = document.createElement("th");
    popHeader.innerHTML = "Population";
    headerRow.appendChild(popHeader);

    //add the row to the table
    table.appendChild(headerRow);

    //loop to add a new row for each city
    for (var i = 0; i < cityPop.length; i++){
        var tr = document.createElement("tr");

        var city = document.createElement("td");
        city.innerHTML = cityPop[i].city;
        tr.appendChild(city);

        var pop = document.createElement("td");
        pop.innerHTML = cityPop[i].population;
        tr.appendChild(pop);

        table.appendChild(tr);
    };

    //add the table to the div in index.html
    var myDiv =  document.getElementById("div1");
    myDiv.appendChild(table);
};

//call the initialize function when the window has loaded
window.onload = initialize();
```

- Rerun your html page, and you should see a table loaded onto your page inside the div you created. Study this code in detail and make sure you understand what *every step* is doing. Some key methods are presented such as `createElement`, `innerHTML`, `appendChild`, etc. You will also notice data types and functionality which should be familiar to you already (e.g. functions, for loops).

- 📝 [Q3] What is the `onload` method doing? Why is it good to load JavaScript in this way? (Add your answer inside a p tag in your html file directly under your previous answer)

# Creating some sample data using online tools

In this lesson, you will be asked to create your own data for display on a map. We will stick to using either CSV (points) or GeoJSON (all geometries).

If you already have a CSV or GeoJSON file containing coordinates you would like to work with, add it to your *data/* folder.

However, to keep things simple, some data has been provided to you entitled `megacities.csv`, which contains the top 20 most populous cities worldwide according to [Wikipedia](https://en.wikipedia.org/wiki/List_of_largest_cities):
- Move this file into your *data/* folder.
- Open it in a spreadsheet viewer (e.g. Excel) and use the [localfocus](https://geocode.localfocus.nl/) tool to geocode the cities.
  - You will want to paste only the first column (the city names) into the box entitled 2. (make sure Worldwide is selected).
  - Once geocoded, make sure *Decimals with dots* is checked before copy-pasting the results into your spreadsheet.
  - In your spreadsheet, make sure you have entitled each column appropriately (i.e. *city*, *latitude*, *longitude*, *population*). It's important to always use conventional terms for latitude and longitude since most software will automatically recognize it as geographic data if well labeled.
- Once your data table is cleaned up and ready, load it into [geojson.io](http://geojson.io/) to turn it into a GeoJSON file.
  - Click *Open* > *File* to upload your CSV (or drag it into the browser)
  - Your data should get imported automatically and render as a GeoJSON.
- Copy all the GeoJSON data from the side panel and paste it into a blank new file opened in your text/code editor.
- Save this file as `megacities.geojson`. Make sure the extension of the file is indeed geojson!

JSON, or JavaScript Object Notation, is an alternative data format native to JavaScript and the Open Web that stores data as one long JavaScript object for referencing and manipulation in the DOM. JSON keys are strings and the values may be strings, numbers, arrays, or other objects. GeoJSON was invented to take advantage of JSON syntax for geospatial data. GeoJSON is the most common and compatible web format for geospatial data.

📝 [Q4] Observe the JS code you pasted into `main.js` in the previous section: What is the data type of `cityPop`, and What type of objects does it contain? Finally, what Python data type do these objects resemble most? (add your 3 answers to this question below your previous ones inside your html file).

# Creating a simple web map

We will continue by adding a simple web map instance to our html page. This will involve several steps...

## Adding data to your webpage

Loading data into a webpage can be done in many ways. For this exercise, we will be using the simplest way, but note that if larger data quantities had to be loaded in more dynamic ways on a webserver, more steps would be involved...

### Loading data from a local folder

Here, we will import the geojson data for use inside our webpage in the simplest way possible: by linking to it much like we would any external .js file! More precisely, we will import your [GeoJSON](https://datatracker.ietf.org/doc/html/rfc7946) data as a [JSON object](https://www.w3schools.com/js/js_json_objects.asp) stored inside of a standard [.js](https://docs.fileformat.com/web/js/#:~:text=JS%20Code-,What%20is%20a%20JS%20file%3F,or%20include%20a%20JS%20file.) file:
- Replace the `.geojson` file extension with `.js` and then open it inside your code editor
- On the first line, prepend the *{* with a variable declaration as follows:
```javascript
var geojsonData = 
```

You have now turned your GeoJSON data into a JS object variable. This allows us to very simply import the data into a local web development environment.

We will now add a link to this script file so that it can be read by the web map we will be adding:
- add the following to the head section of your html document:
```html
<!-- link to data -->
<script type="text/javascript" src="data/megacities.js"></script>
```
- once added and saved, reload your html document in the web browser
- inside the console, enter `geojsonData`. The variable should be available for use within your webpage.

### FYI: Fetching data asynchronously

Javascript is a [synchronous](https://www.freecodecamp.org/news/synchronous-vs-asynchronous-in-javascript/), single-threaded language. Since it runs on a single thead, it can only execute one command at a time, meaning other commands need to wait a previous command's execution before running. Fetching data from a server can be more complicated though, since once the HTTP request is made, JavaScript doesn't know how long the data (potentially on the other side of the world), will take to be retrieved. You could go a long time learning and coding in JavaScript without engaging with [synchronicity/asynchronicity](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Introducing), but that would limit you when it comes to dynamically retrieving data to populate something like a web map. The `fetch` method is used to accomodate JavaScript by telling it to continue running only when a given file has been received.

However, since these are methods that interface with the web, we need to have our website loaded onto an actual webserver for such methods to work (security reasons!). Instead of loading data asynchronously, we will be loading it like anything else. This is OK for the moment, since our data are locally accessible inside our website file system (inside *data/*) and since our webpage will be fairly simple.

## Creating the leaflet web map

Here we will use basic leaflet functionality to add your GeoJSON data as a layer inside a leaflet web map.

- In `index.html`, directly following your *div1*, add an empty div with id `mapdiv`. This is what we will use as a container for the map.
- In your CSS stylesheet, add a style selector that points to the *mapdiv* id and provides it with a *height* value of at least *200px* as well as a width value of *auto*.

### Importing the leaflet library

- Go to the [Leaflet download page](https://leafletjs.com/SlavaUkraini/download.html) and download the latest stable version.
- Extract the folder and place it inside your *lib/* directory. Make sure leaflet's contents are directly available inside *lib/leaflet/* of your workspace directory.
- Add the following to the head section of your `index.html`:
```html
<!-- link to leaflet styles -->
<link rel="stylesheet" type="text/css" href="lib/leaflet/leaflet.css" />
<!-- link to leaflet scripts -->
<script type="text/javascript" src="leaflet/leaflet.js"></script>
```

As you perform these steps, don't forget to save and reload your page while checking the console to make sure no errors occur. This is good practice for catching and resolving errors soon before they stack up and become more difficult to trace.

### Using leaflet in our JS code to create the map

Now, we will keep some of the structure we created in your `main.js`, but we can remove the table we made in a previous step:
- Delete all the contents of your `cities` function and change the function name to `loadMap`.
- Then, inside your `initialize` function, instead of calling `cities`, make it so it calls `loadMap`.

- Inside the `loadMap` function, paste the following code which uses the leaflet library's functionality to create a webmap:
```javascript
	//create a basemap style. You can find other options at https://leaflet-extras.github.io/leaflet-providers/preview/
	var CartoDB_Positron = L.tileLayer(
		'https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', 
		{
			attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors &copy; <a href="https://carto.com/attributions">CARTO</a>',
			subdomains: 'abcd'
		}
	)
	//add this basemap style to a JS object, to which you could also add other baselayers. This object is loaded as a basemap selector as seen further down
	var baseLayers = {
		"CartoDB": CartoDB_Positron
		//,...
	};
	// create the map
	var mymap = L.map('mapdiv', {
		center: [45.50, -73.58]
		,zoom: 3
		,maxZoom: 18
		,minZoom: 3
		,layers: CartoDB_Positron
	});
		
	// parse json object (var geojsonData) and turn into loadable layer
	geojsonLayer = L.geoJSON(geojsonData);
	
	//add geojsonData to map
	geojsonLayer.addTo(mymap);// add json element to map
	
	//declare basemap selector widget
	var lcontrol = L.control.layers(baseLayers);
	//add it to the map
	lcontrol.addTo(mymap);
```

There are some leaflet methods here you might want to read up on, such as [`L.map`](https://leafletjs.com/SlavaUkraini/reference.html#map),  [`L.TileLayer`](https://leafletjs.com/SlavaUkraini/reference.html#tilelayer) (note that you can find other open source tile layers [here](https://leaflet-extras.github.io/leaflet-providers/preview/)), [`L.control.layers`](https://leafletjs.com/SlavaUkraini/reference.html#control-layers),  [`L.geoJSON`](https://leafletjs.com/SlavaUkraini/reference.html#geojson), etc.

Try to add some functionality or styling to your webmap. This could be in the form of making the map features clickable in order to generate a popup showing related attributes, or changing the place markers to a differently styled point, or adding another tile layer option to your widget, or anything else!

# Resources for styling your web map

For more, see this basic tutorial for [adding and styling data](https://leafletjs.com/SlavaUkraini/examples/geojson/) to a leaflet map or this (somewhat dated but widely respected)[workshop presentation](https://maptimeboston.github.io/leaflet-intro/) from a conference that you might find nevertheless extremely useful. If you're better with documentation (and as a general reference), always consult the [leaflet docs](https://leafletjs.com/SlavaUkraini/reference.html). Lastly, use your search engine: there are plenty of resources out there for learning to map with leaflet...

# Deliverables

Answer the 4 questions (8 points) and push your repo when it contains a functional web map (6 points) with one added modification in either map symbology or functionality (4 points). Your webmap must be deployed as a github page (2 points) so that it is easily viewable online.




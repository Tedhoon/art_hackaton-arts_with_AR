# Letsee WebAR Demo - Dynamic DOM Element Sample

This is example of using Letsee WebaR SDK for planar image tracking: Dynamic DOM Element Sample.

- - -

## What you should see
<br/><br/>
<center><img src="../resources/screenshots/helloworld-screenshot.jpg" width="360" height="700" style="border:1px solid black; border-radius: 5px"/></center>

## Quick Start
#### 1. Setup JSON configuration file:

*letsee-marker.json:*
```json
{
	"uri":"letsee-marker.json",
	"name":"Letsee WebAR SDK",
	"size":{
		"width": 200,
		"height": 140
	},
	"scale": 1,
	"image": "/path/to/image.jpg"
}
```

**Please check this carefully before you go more further in your steps:**

|Properties|Required|Explanation|
|:---|:---|:---|
|`uri`|Required|A unique value that specifies the object. This `uri` is the same name as the file name.<br/>Here we name as `letsee-marker.json`.|
|`name`|Optional|Name for the object.<br/>You can declare with any name you prefer. For demo purpose, we use our marker and name as `Letsee WebAR SDK`.|
|`size`|Required|Enter the actual size of the object. The unit is `mm`. <br/><ul><li>planner tracker : Actual size of the image target.</li><li>marker tracker: Actual size of the marker you wish to use.</li></ul>|
|`scale`|Optional|The scale of the content being augmented. Default is `1`. If the `"scale": 1` means the augmented content matches 1:1 with the size enter above.|
|`image`|Required|For `IMAGE` Tracking.<br/><br/>Reference image to use for planar tracker. You can enter a URL or a relative path that begins with `https`.<br/>However, if you enter a relative path, be careful with the path of the web page where the AR app is running.</br><ul><li>Extension: Must be in **.jpg** or **.png** formats.</li><li>Capacity: Up to 2MB in size.</li><li>Pixel size: Up to 640,000 horizontal and vertical pixels.</li></ul> |
|`letseeMarkerId`|Required|For `MARKER` Tracking.<br/><br/>The marker ID to be used for tracking.<br/>A total of 250 markers are available, from 0 to 249 can be used. Input is `"marker_” + numeric`.|

For `IMAGE` tracking, you can use any planar image such as movie posters or product packaging, then put them into your project directory to start
tracking and building your application.

#### 2. Setup CSS `media="place"`:

```html
<head>
...
    <style media="place" type="text/css">
        #container {
            -letsee-target: uri('letsee-marker.json');
        }
    </style>
...
</head>
```

Please put this CSS `media="place"` in your `<head>` tag, and `media="media"` MUST BE included for rendering AR content.

**By adding a media query `media="place"`, you are gonna to tell the browser this page is available or ready for AR-enabled.**

|Attributes|Description|
|:---|:---|
|`#container`|CSS selector: Same as the element selector used in normal CSS.|
|`-letsee-target`|The path of the `.json` file containing the entity configuration.|
|`-letsee-transform`|Any position with respect to the center point of the recognized object on the screen.|

#### 3. App setting:
This will show how to setup and create an app using the Letsee WebAR SDK.
This script loads the SDK according to the configuration file (`.json`) and `YOUR_APP_KEY`.

```html
<body>

	<!-- Loading the Letsee WebAR Engine -->
	<script>
		const config = {
			"appKey": "YOUR_APP_KEY", 
	        "trackerType": "IMAGE",
		};
		const app = new Letsee(config);
		
		// Engine tracking event
        letsee.onLoad(function() {
            const helloworld = document.createElement('h1');
            helloworld.appendChild(document.createTextNode('Hello World!'));
            
            const renderable = new DOMRenderable(helloworld);
            const entity = letsee.getEntity('letsee-marker.json');
            
            entity.addRenderable(renderable);
        });
	</script>
	
</body>
```

|Attributes|Required|Description|
|:---|:---|:---|
|`appKey`|Required|AppKey for license authorization.|
|`trackerType`|Required|Target recognition mode.<br/>You can set `IMAGE` or `MARKER` depends on what you want to track.<br/><br/>The `IMAGE` tracker recognizes and tracks one entity of the first declared `-letsee-target`. The `MARKER` tracker can track five markers simultaneously.|

You first need to replace `YOUR_APP_KEY` with your own app key. In case of planar image tracking, the `trackerType` should be defined as `IMAGE`.
Then, the `<div>` tag has an `id` which MUST BE the same as the CSS selector defined in `media="media"` `<style>` tag. Here is `#container`.
Inside this `<div>`, you can create any content as much as you like to augment it and see the magic happens.

Letsee WebAR SDK provides some tracking events which you can use to handle during working on application.

a. `onLoad(callback)`:
```
app.onLoaded((e) => {console.log(e)});
```
This will indicate whether the engine loads successfully or not.

b. `onTrackStart(callback)`:
```
app.onTrackStart((e) => {console.log(e)});
```
This means the object enter the view and the engine starts tracking.

c. `onTrackMove(callback)`:
```
app.onTrackMove((e) => {console.log(e)});
```
When the object moves within the view. The engines now tracks every movement, positions and coordinate of the object and stores it as a
3D transformation matrix of the object relates to the camera device.

d. `onTrackEnd(callback)`:
```
app.onTrackEnd((e) => {console.log(e)});
```
When the object leaves the view and the engine stops tracking.

For detailed information of engine tracking events, please check out our [documentation](http://intra.letsee.io/docsify/#/).

#### 4. Serve Application
##### Deploy to A Web Server
If you have a development web server, upload the sample project and connect from a mobile device.
You'll need to **connect via HTTPS** as browsers require `HTTPS` certificates to access the camera on your phone through a browser.

##### Deploy to A Local Server
Developers are encouraged to build local web servers because in most cases they are developed on personal computers and verified on
mobile devices.

Setting up a local web server can usually be done in several way. This article describes two different methods using
Python (SimpleHTTPServer) or [Node.js](https://www.npmjs.com/package/http-server) as an easy way to set up an HTTPS
connection to an installed local web server.

###### Using Node.js
1. Install Node.js and npm:<br>
If you don't already have Node.js and npm installed, please get it here [https://www.npmjs.com/get-npm](https://www.npmjs.com/get-npm)
and for installation instruction, check it [here](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm).

2. Open Terminal and type following commands:
```bash
$ npm install http-server -g
$ cd <your_project_directory>
$ http-server
```

By default, this package sets the root of the `./public` folder (or the `./` folder) which is the point of execution and
binds it to port `8080`. For more options, please refer to [here](https://www.npmjs.com/package/http-server).

###### Using Python
Open Terminal and type following commands:

```bash
// Checking python version
$ python -v

$ cd <your_project_directory>

// Python 2.x
$ python -m SimpleHTTPServer

// Python 3.x
$ python -m http.server
```

This module defaults to the root of the command execution and binds to port 8000. See the following links for more options:
([SimpleHTTPServer-Python 2.x](https://docs.python.org/2/library/simplehttpserver.html), [http.server-Python 3](https://docs.python.org/3/library/http.server.html))

###### Using ngrok
Alternatively, you can use [ngrok](https://ngrok.com/download) to server application locally.
The following command executes the tunneling function of `ngrok`, which can access the local web server on port `8000` from the outside.

For detailed `ngrok` configuration, please check it [here]((https://ngrok.com/docs)).

```bash
$ ngrok http 8000
```

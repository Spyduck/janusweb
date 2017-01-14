#JanusWeb

JanusWeb is a web-based client for JanusVR rooms.  JanusVR rooms are 3D environments 
written in HTML and hosted in the same manner of regular websites.  JanusWeb allows
users to view and interact with others in these environments using only their web
browser.

## Features

- Build immersive 3D environments for desktop, mobile, and VR devices using HTML and JS
- Support for 2D, sbs3D/ou3d, and 360 degree video textures using HTML5 Video
- 3D positional audio
- Gamepad support via the HTML5 Gamepad API
- Rendering functionality provided by Three.js / WebGL
- Oculus Rift, Vive, GearVR, Daydream, and Cardboard support via WebVR API
- Supports hand tracking peripherals like Leap Motion, Oculus Touch, and Vive controllers
- Realtime collaboration across all devices via built-in networking
- Scriptable client enables many customized uses

## Using

There are several different ways to use JanusWeb, depending on how much control you want to 
have over the whole system.

### Use our viewer
Our default viewer is always available at https://web.janusvr.com/.  You can write an HTML
page with your [JanusVR Markup](http://janusvr.com/guide/markuplanguage/index.html) and host
it anywhere you would normally host a static website.  Any regular webhost, AWS S3 static 
sites, CDNs, or even more exotic locations like [IPFS distributed filesystems](https://ipfs.io) will work.
Then just load your page in our viewer by entering the URL into the navigation bar, and you 
can link to it and share on social media.

### Pull our scripts into your page
Using the above method, all of your links would go through our servers.  If you'd prefer to 
link to your own servers, you can pull our JS into your page and use JanusWeb as a scriptable
client via its API.  This looks something like this:

```html
<html>
  <head>
    <title>My JanusVR Room</title>
  </head>
  <body>
    <FireBoxRoom>
      <Room use_local_asset="room1">
        <Object id="cube" pos="0 1 5" />
        <Text col="1 0 0" pos="0 2 4">My First Room</Text>
      </Room>
    </FireBoxRoom>
    <script src="https://web.janusvr.com/janusweb.js"></script>
    <script>elation.janusweb.init({url: document.location.href})</script>
  </body>
</html>
```

The `elation.janusweb.init()` function can take a number of arguments, and returns a promise which
receives an instance of the client.  This client reference can be controlled via its API.  See the 
sections on Arguments and Scripting below.

### Install from ZIPs
(TODO - we will start shipping zip builds of JanusWeb once we release v1.0)

### Install from NPM
(TODO - we will start shipping official NPM packages of JanusWeb once we release v1.0)
```bash
npm install janusweb
```

### Build from source

If you'd like to build JanusWeb from source, you can check it out from Github and build using the 
following steps:

```bash
$ git clone https://github.com/jbaicoianu/janusweb
$ cd janusweb
$ npm install
$ npm run build
```

This will give you a full build of the latest verson of JanusWeb in your `build/` directory.  You 
can then modify `build/index.html` however you see fit, and host it as suggested above.

NOTE - if you see errors relating to node-canvas, make sure your system has `libgif-dev` installed.

## Arguments
JanusWeb supports several arguments at initialization time to control how it behaves.

<table>
  <tr><th> Name           </th><th> Description                            </th><th> Default                  </th></tr>
  <tr><td> homepage       </td><td> Default page to go to when user presses 
                                    home button                            </td><td> https://web.janusvr.com/ </td></tr>
  <tr><td> url            </td><td> Default page to load                   </td><td> (homepage)               </td></tr>
  <tr><td> shownavigation </td><td> Control visibility of navigation bar   </td><td> true                     </td></tr>
  <tr><td> showchat       </td><td> Control visibility of chat             </td><td> true                     </td></tr>
  <tr><td> usevoip        </td><td> Enable or disable VOIP functionality
                                    (NOTE - disabled pending browser support 
                                    for Opus via WebAudio)                 </td><td> false                    </td></tr>
  <tr><td> resolution     </td><td> If specified, restrict the renderer to 
                                    a the specified size                   </td><td> (none)                   </td></tr>
  <tr><td> networking     </td><td> Enable or disable networking           </td><td> true                     </td></tr>
  <tr><td> autoload       </td><td> Load URL by default or wait for script </td><td> true                     </td></tr>
</table>

## Scripting
After initializing the client, `elation.janusweb.init()` returns a Promise which provides a reference to the client.
You can programatically control this client to do all sorts of things.  For instance, we can make the client load a
URL, wait for the world and all of its assets to load, and then take a screenshot of the world after a specified delay:

```javascript
var pageinfo = elation.utils.parseURL(document.location.href),
    urlargs = pageinfo.args || {},
    hashargs = pageinfo.hash || {};

var url = elation.utils.any(hashargs.url, urlargs.url, 'http://www.janusvr.com/index.html'),
    delay = elation.utils.any(hashargs.delay, urlargs.delay, 1000);

elation.janusweb.init({
  url: url,
  resolution: '1920x1080',
  showchat: false,
  shownavigation: false
}).then(function(client) {
  elation.events.add(client.janusweb.currentroom, 'room_load_complete', function() {
    setTimeout(function() {
      client.hideMenu();
      client.screenshot(types[type]).then(function(imagefile) {
        // upload imagefile somewhere via XHR
        console.log('Screenshot complete!');
      });
    }, delay);
  });
});

```

Many other aspects of the JanusWeb client can be controlled this way as well.  Our users are
always thinking up new and inventive ways of using the JanusWeb client.  Embed it in your blog
posts, use it to render 3D content behind your 2d webpage, put a virtual security camera in your
world and view a live stream of the virtual world from any web browser.  Control the virtual world
via a web interface.  This is your scriptable live portal into the metaverse, to do with whatever
you please.  The possibilities are endless!

You can even run JanusWeb in NodeJS for headless server-side operations.  Use it to write a bot that
wanders the metaverse, or run your game logic and physics on a server to have one authoritative 
source of state for your world.  If this sounds interesting to you let us know, we will be more
than happy to help you through this (it's all very experimental right now!)

## Contributing
JanusWeb is open source, and we welcome any contributions!  Please do report bugs using GitHub Issues,
and all pull requests will be considered.  We could especially use help with documentation!

## Who is responsible for this?
JanusWeb was created by James Baicoianu, and is now an official open source project of JanusVR, Inc.
The JanusWeb software and its API are published under the MIT license, and are free to use for whatever
uses you can think of.  If you build something cool, let us know!
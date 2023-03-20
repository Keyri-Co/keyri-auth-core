# Keyri Front End
Some of the services Keyri provides are QR Login through an app, QR Login _without_ an app, passkey creation, passkey validation, and passkey storage. 

The aim of this library get you up and running with the our API with as little hassle as possible.

__Key Features:__ 

Below are some of the neat things you can do with the library:

* Create Dynamic Logon QRs
* Register and Validate Passkeys on Your User's Desktop
* Register and Validate Passkeys on Your User's Phone (via QR!) 

# Installation

__Using Yarn or NPM__

`$ npm insall --save keyri-front-end`

or

`$ yarn add keyri-front-end`

# High Level Objectives

At the end of the day, the goal of Keyri is to prove to your __SERVER__ that a trusted device (__THE USER'S PHONE__) legitimately gave permission to an unknown device (__A DESKTOP BROWSER__) to do stuff on its behalf.

This library ties these three things together as safe and easily as possible so you can focus on building your business; not user auth.

# Hello World App

If you want to set up a "demo" web-application to get a feel for how everything ties together, do the following:

```
$ git clone https://github.com/Keyri-Co/keyri-hello-world
$ cd ./keyri-hello-world
$ npm install
$ node server.js
```

Set your browser at `http://localhost` and it should work.

# DESKTOP Quickstart

In order to get a QR on your login page, you'll need to do the following:

1. Serve an iframe

2. Embed the iframe on your login page

3. Set up an events listener to handle events emitting from this library

## Serving An IFrame

1. Create a file called `qr.html` and serve it from the same origin as your login page (e.g. a `/public` directory)

2. _RECOMMENDED_: serve everything on your page's origin with the header `X-Frame-Options: SAMEORIGIN` (examples [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options#examples))

3. Include the following in the body of the iframe:

```html
    <div class="pre-blurry" id="qr-target"></div>
    <div id="qr-lay-over"></div>
```

4. Start the `IFrameManager`

```mjs
  import { IFrameManager } from 'keyri-front-end';
  const iFrameManager = new IFrameManager();
  await iFrameManager.start();
```



## Embedding the IFrame

Embed an iframe in your authentication page in the desired DOM element with ./KeyriQR.html as its src

```html
<!-- PRODUCTION -->
<iframe
    scrolling="no"
    frameborder="0"
    height="500"
    width="500"
    src="./qr.html?qsd=false&mobile=http://localhost/mobile.html"
    id="qr-iframe"
></iframe>
```

```html
<!-- DEVELOPMENT -->
<iframe
    scrolling="no"
    frameborder="0"
    height="500"
    width="500"
    src="./qr.html?qsd=false&Origin=your.site.com&Environment=dev&mobile=http://localhost/mobile.html"
    id="qr-iframe"
></iframe>
```
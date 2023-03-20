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

# DESKTOP

To put a QR on your login page you'll need to do the following:

1. Serve an iframe

2. Embed the iframe on your login page

3. Set up an events listener to handle events emitting from this library

## Serving An IFrame

1. Create a file called `qr.html` (or whatever) and serve it from the same origin as your login page (e.g. a `/public` directory)

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
    src="./qr.html?qsd=false&mobile=http://my.site.com/mobile.html"
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

## QR Event Handling

### - Getting Data
So you've got a QR code on your page now? Great! Next, you'll probably want to something with the information coming out of it and your user's phone.

### - Event Types
Below are the event types that are currently avaible to listen to from the library:

* qr_event_mobile_connect - this is when a mobile device connects to your QR Code

* qr_event_socket_error - when there's an error coming from the API

* qr_event_risk_data - Risk Data about the connection coming from the API

* qr_event_session_validate - Decrypted data from the mobile app

* qr_event_appless_login - If you're using mobile QR login, this is what will be emmitted when the user is logged in

* qr_event_appless_registration - When the user registers their phone via QR for appless, this is the event that it emmitted

### - Setup

```mjs
// /src/mjs/eventManager.mjs

import {EventManager} from "keyri-front-end";
window.eventManager = new EventManager(window);

// Set handler for standard Keyri App Based Validation
window.addEventListener("qr_event_session_validate", async (evt) => {
  alert("SESSION VALIDATE!!");
  console.log("SESSION VALIDATE", evt);
});

// Set handler for whatever risk data comes from the QR
window.addEventListener("qr_event_risk_data", async (evt) => {
  let data = evt.detail.data.information.data;
  console.log("RISK DATA", JSON.parse(atob(data)));
});

// Set handler for any errors coming out of the QR
window.addEventListener("qr_event_socket_error", async (evt) => {
  alert(evt?.detail?.data);
});

// Set handler for full cycle appless login
window.addEventListener("qr_event_appless_login", async (evt) => {
  console.log(evt);
  alert("LOGGED IN FROM PHONE");
});

// Set handler for full cycle appless registration
window.addEventListener("qr_event_appless_registration", async (evt) => {
  console.log(evt);
  alert("REGISTERED PHONE");
});

export default true;

```
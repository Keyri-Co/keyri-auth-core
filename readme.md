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

# Hello World App

If you want to set up a "toy" web-application to get a feel for how everything ties together, do the following:

```
$ git clone https://github.com/Keyri-Co/keyri-hello-world
$ cd ./keyri-hello-world
$ npm install
$ node server.js
```

Set your browser at `http://localhost` and it should work.

# Desktop QR Quickstart

In order to get a QR on your login page, you'll need to do the following:

1. Serve an iframe

2. Embed the iframe on your login page

3. Set up an events listener to handle events emitting from this library

## Serve An IFrame

1. Create a file called `qr.html` and serve it from the same origin as your login page (e.g. a `/public` directory)

2. _RECOMMENDED_: serve everything on your page's origin with the header `X-Frame-Options: SAMEORIGIN` (examples [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options#examples))

3. Include the following in the body of the iframe:

```html
    <div class="pre-blurry" id="qr-target"></div>
    <div id="qr-lay-over"></div>
```

4. Start the `IFrameManager`

```html
<script src="./dist/index.min.js"></script>
<script type="module">
  const { IFrameManager } = KeyriFrontEnd;
  const iFrameManager = new IFrameManager();
  await iFrameManager.start();
</script>
```
# Ad-rotator [![npm version](https://badge.fury.io/js/ad-rotator.svg)](https://badge.fury.io/js/ad-rotator) [![Build Status](https://travis-ci.org/niketpathak/adRotator.svg?branch=master)](https://travis-ci.org/niketpathak/adRotator) [![License](https://img.shields.io/github/license/mashape/apistatus.svg)](https://opensource.org/licenses/MIT)

A fast, light-weight and highly configurable JS library to rotate advertisements.

**Ad-rotator.js**
- is a vastly performant library in pure Javascript
- has NO DEPENDENCIES :D 
- allows you to **display native advertisements to adblock users** as well
- enables custom dynamic advertisements to be injected 
- is a light-weight library, only [![](http://img.badgesize.io/https://cdn.jsdelivr.net/npm/ad-rotator?compression=gzip)](https://cdn.jsdelivr.net/npm/ad-rotator) minified and gzipped
- supports **multiple instances** of advertisements in different sizes with many custom configuration options
- Native support to pause ad-rotation on hover
- Responsive/Optimised for mobiles & tablets
- has built-in support for **_sticky_ advertisements**
- is completely free and open source

---
## Install
```shell script
# you can install ad-rotator with npm
$ npm install --save ad-rotator

# Alternatively you can use Yarn
$ yarn add ad-rotator
```
Then include the library in your App/Page.

**As a module,** 
```javascript
// using ES6 modules
import adRotator from 'ad-rotator'

// using CommonJS modules
var adRotator = require("ad-rotator")
```

**In the browser context,**
```html
<!-- Include the library -->
<script src="./node_modules/ad-rotator/dist/ad-rotator.js"></script>

<!-- Alternatively, you can use a CDN -->
<script src="https://cdn.jsdelivr.net/npm/ad-rotator"></script>
```
The library will be available as a global object at `window.adRotator`

## Configuration

Ad-rotator.js requires 2 mandatory parameters to be setup. A 3rd optional parameter can be provided to override default values.
- `DOM element` (required) - A container Element where the Ads should be displayed
- `Array` (required) - An Array of Advertisements(`[{url: '', img: ''},...]`) to be displayed. Each advertisement is expected to be an object with 2 mandatory keys `img` and `url` -
```javascript
let items = [
  {img: './assets/image.jpg', url: 'https://xyz#1'},                      // ad 1
  {img: 'https://xyz/image.png', url: 'https://xyz#2'},                   // ad 2
  {img: 'data:image/jpeg;base64,/9j/4AAQSkZJRg...', url: 'http://xyz#3'}  // ad 3
]
```

`img` can be an absolute URL, a relative URL or even a base-64 encoded image.
- `Object` (optional) - An Object with custom configuration options to override default values. (See all [`configuration options`](#configuration-options))

## Usage 

In Html, add a container Element.
```html
<div id="containerElement"></div>
```

Then create an `Array` with the advertisements to be displayed.

```javascript
// An array with the advertisements to display
let items = [
    { url: 'https://niketpathak.com#1', img: 'https://niketpathak.com/images/works/gkm_pic_sq.jpg'},
    { url: 'https://digitalfortress.tech#2', img: 'https://niketpathak.com/images/works/maestrobits_sq.jpg'}
];
```
Then Initialize **adRotator** by passing the `DOM Element` and the `Array` of advertisements as parameters
```javascript
// initialize adRotator
const rotator = new AdRotator(
  document.getElementById('containerElement'),
  items,
  { debug: true }       // optional
);
// start the rotation
rotator.start(); 
```
That's it! You should now have Ad-rotation in action! By default, the Ads are rotated in a random fashion. See [`configuration options`](#configuration-options) for more variations. 


_**_NOTE:_**_ By default, `adRotator` is designed to **fail silently** for any configuration error. This means that it will neither pollute the DOM nor will it attach any events if case of an error. It will also not log any console error messages. This can make it difficult to diagnose an error, hence during development, it is recommended to turn on the `{debug: true}` config option. 

### API

* [`AdRotator.start()`](#adrotatorstart)
* [`AdRotator.pause()`](#adrotatorpause)
* [`AdRotator.resume()`](#adrotatorresume)
* [`AdRotator.add()`](#adrotatoradd)
* [`AdRotator.remove()`](#adrotatorremove)
* [`AdRotator.destroy()`](#adrotatordestroy)


#### AdRotator.`start()`

Starts the Ad-Rotation

```javascript
const rotator = new AdRotator(
    document.getElementById('containerElement'),
    [
        { url: 'https://gospelmusic.io#1', img: 'https://niketpathak.com/images/works/gkm_pic_sq.jpg'},
        { url: 'https://digitalfortress.tech#2', img: 'https://niketpathak.com/images/works/maestrobits_sq.jpg'}
    ],
    {shape: "sidebar"}    // configuration options
);
rotator.start(); // starts the rotation
```

#### AdRotator.`pause()`

Pauses the Rotation. 
```javascript
const rotator = new AdRotator( /* options */ )
rotator.pause();        // pauses the rotation
```
To resume the rotation, simply call `AdRotatorInstance.resume()`

#### AdRotator.`resume()`

Resumes the Rotation. 
```javascript
const rotator = new AdRotator( /* options */ )
rotator.pause();
rotator.resume();        // resumes the rotation
```
Use `AdRotatorInstance.resume()` to resume a paused rotation.


#### AdRotator.`add()`

Inject a new Advertisement into the AdRotator.
```javascript
const rotator = new AdRotator( /* options */ )
rotator.add(
  {
    url: 'https://gospelmusic.io',
    img: './path-to-img'
  } 
);        
```
The newly injected Advertisement will be displayed in the next rotation cycle

#### AdRotator.`remove()`

Remove an item from the Advertisements array. 
```javascript
const rotator = new AdRotator( /* options */ )
rotator.remove(); // remove the last item
rotator.remove(   // remove a specific item
  {
    url: 'https://digitalfortress.tech', // url is optional
    img: './path-to-img'
  } 
);        
```
The `remove()` method deletes the last item in the advertisements array. To remove a particular advertisement, you can also pass it a parameter (**`rotatorInstance.remove({ img: 'xyz.gif'})`**). The change in the Advertisements array will be reflected in the next rotation cycle

#### AdRotator.`destroy()`

Destroys Ad Rotation. Cleans up the DOM and removes all associated events.

```javascript
const rotator = new AdRotator( /* options */ )
rotator.destroy();        // destroys the rotation, DOM and events
```
To reactivate AdRotator, simply call `AdRotatorInstance.start()`


## Configuration Options

ad-rotator.js is instantiated with the default configuration parameters as shown below - 
```
// Default Configuration parameters
{
    shape: "square",
    height: 300,
    width: 250,
    imgClass: "",
    linkClass: "",
    objectFit: "inherit",
    sticky: null,
    timer: 10000,
    random: true,
    newTab: false,
    debug: false
}
```
---

#### Description - 

1. **Shape** (_`String`_, default - `"square"`) - This is the expected shape of the Ad. It can also be set to `leaderboard` or `sidebar`. When set to **Leaderboard**, the standard horizontal size of height - 90px, width - 728px is used, whereas when shape is set to **Sidebar**, the standard size of height - 600px, width - 300px is used.
2. **Height** (_`Int`_, default - `300` _px_) - The height of the advertisement
3. **Width** (_`Int`_, default - `250` _px_) - The width of the advertisement
4. **imgClass** (_`String`_, default - `""`) - Class that should be added to the image Tag
5. **linkClass** (_`String`_, default - `""`) - Class that should be added to the link Tag
6. **objectFit**: (_`String`_, default - `"inherit"`) - The `object-fit` property that should be used for the image (`inherit`,`contain`,`cover`, `fill`,...)
7. **sticky**: (_`Object|null`_, default - `null`) - By default, the advertisement shown is not sticky. You can make it sticky by providing a configuration object -
```javascript
sticky: {
    beforeEl: document.querySelector('.heading'),
    afterEl: document.querySelector('.summary'),
    offsetTop: '10',        // or '10px' (defaults to 30px)
    offsetBottom: '150px',  // or '150'  (defaults to 0px)
}
// beforeEl -> Element before the advertisement
// afterEl  -> Element after the advertisement
```
8. **Timer**: (_`Int`_, default - `10000` _ms_). The time after which an advertisement will be rotated
9. **random**: (_`Bool`_, default - `true`) The advertisements are rotated in a random fashion by default. Set to `false` to have them rotated sequentially
10. **newTab**: (_`Bool`_, default - `false`) Set to `true` to open the advertisement URL in a new Tab
11.  **debug**: (_`Bool`_, default - `false`) Set to `true` to see error information in the console. Recommended to turn this on during development.

#### Note
It is also possible to change configuration options after instantiation. (The only exception being that changing the `shape` option will not automatically change the `height` & `width` options) 
```javascript
// init AdRotator with default options
const rotator = new AdRotator( /* options */ )
// update config after instantiation to change to sequential rotation
rotator.conf.random = false; 
```
---

### Contribute

Interested in contributing features and fixes?

[Read more on contributing](./contributing.md).

### Changelog

See the [Changelog](https://github.com/niketpathak/adRotator/wiki/Changelog)

### License

[MIT](LICENSE) © [Niket Pathak](https://niketpathak.com)
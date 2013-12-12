PuppetJs
========

Client side library that binds Web Components or AngularJs to server side view models using JSON-Patch

Implements [Server communication](https://github.com/Starcounter-Jack/PuppetJs/wiki/Server-communication).

### Usage

Install by adding source to your head:

```html
<!-- include PuppetJs with dependencies -->
<script src="lib/json-patch/src/json-patch-duplex.js"></script>
<script src="src/puppet.js"></script>
```

After DOM is ready, initialize with the constructor:

```javascript
/**
 * Defines a connection to a remote PATCH server, returns callback to a object that is persistent between browser and server
 * @param remoteUrl If undefined, current window.location.href will be used as the PATCH server URL
 * @param callback Called after initial state object is loaded from the server
 */
var puppet = new Puppet(null, function callback (obj) {
  //called when loaded
});
puppet.onRemoteChange = function (patches) {
  //this is a helper callback triggered each time a patch is obtained from server
};
```

### Sending client changes to server

PuppetJs detects changes to the observed object in real time. However, change patches are
queued and not sent to the server until a `blur` event occurs. It is because normally there is no business need to save
a partially filled field and in many cases it may be harmful to data integrity. Another benefit is performance
 improvement due to reduced number of requests.

To force sending changes on each key stroke (for example to implement live search), you can configure it
per input field (given that this field is bound to observed object):

```html
<input update-on="blur"><!-- default behavior: update server on blur -->
<input update-on="input"><!-- update server on key stroke -->
```

### Ignoring local changes

If you want to create a property in the observed object that will remain local, there is an `ignoreAdd` property that
let's you disregard client-side "add" operations in the object using a regular expression. Sample usage:

```javascript
puppet.ignoreAdd = null;  //undefined or null means that all properties added on client will be sent to server
puppet.ignoreAdd = /./; //ignore all the "add" operations
puppet.ignoreAdd = /\/\$.+/; //ignore the "add" operations of properties that start with $
puppet.ignoreAdd = /\/_.+/; //ignore the "add" operations of properties that start with _
```

### Dependencies

PuppetJs is dependent on [Starcounter-Jack/JSON-Patch](https://github.com/Starcounter-Jack/JSON-Patch) to observe changes in local scope, generate patches to be sent to the server and apply changes received from the server.

### Testing

Open `test/SpecRunner.html` in your web browser to run Jasmine test suite.

### Changelog

#### 0.1.2 (Dec 13, 2013)

- New property `puppet.ignoreAdd` allows to ignore local "add" operations in the observed object, allowing client-only properties that will not be propagated to server (solves [#10](https://github.com/PuppetJs/PuppetJs/issues/10) and [#12](https://github.com/PuppetJs/PuppetJs/issues/12))
- Fixed lint code errors
- Fixed exception in Angular example (`ng-partial` directive)
- Upgrade to [&lt;x-html&gt;](https://github.com/PuppetJs/x-html) to v0.0.20131213
- Upgrade Polymer to v0.1.0

#### 0.1.1 (Dec 4, 2013)

- HTTP referer header (described in [Server communication](https://github.com/Starcounter-Jack/PuppetJs/wiki/Server-communication)) will now show an error if set in a XHR response. The only permitted way is to set it in the main HTML document response HTTP headers
- Upgrade to [&lt;x-html&gt;](https://github.com/PuppetJs/x-html) to v0.0.20131126
- Refactor examples in `lab/` to use the updated &lt;x-html&gt;
- In case of error, display the error message with absolute positioning

#### 0.1.0 (Nov 6, 2013)

First numbered version
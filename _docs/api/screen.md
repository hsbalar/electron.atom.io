---
version: v1.2.1
category: API
redirect_from:
    - /docs/v0.24.0/api/screen/
    - /docs/v0.25.0/api/screen/
    - /docs/v0.26.0/api/screen/
    - /docs/v0.27.0/api/screen/
    - /docs/v0.28.0/api/screen/
    - /docs/v0.29.0/api/screen/
    - /docs/v0.30.0/api/screen/
    - /docs/v0.31.0/api/screen/
    - /docs/v0.32.0/api/screen/
    - /docs/v0.33.0/api/screen/
    - /docs/v0.34.0/api/screen/
    - /docs/v0.35.0/api/screen/
    - /docs/v0.36.0/api/screen/
    - /docs/v0.36.3/api/screen/
    - /docs/v0.36.4/api/screen/
    - /docs/v0.36.5/api/screen/
    - /docs/v0.36.6/api/screen/
    - /docs/v0.36.7/api/screen/
    - /docs/v0.36.8/api/screen/
    - /docs/v0.36.9/api/screen/
    - /docs/v0.36.10/api/screen/
    - /docs/v0.36.11/api/screen/
    - /docs/v0.37.0/api/screen/
    - /docs/v0.37.1/api/screen/
    - /docs/v0.37.2/api/screen/
    - /docs/v0.37.3/api/screen/
    - /docs/v0.37.4/api/screen/
    - /docs/v0.37.5/api/screen/
    - /docs/v0.37.6/api/screen/
    - /docs/v0.37.7/api/screen/
    - /docs/v0.37.8/api/screen/
    - /docs/latest/api/screen/
source_url: 'https://github.com/electron/electron/blob/master/docs/api/screen.md'
excerpt: "Retrieve information about screen size, displays, cursor position, etc."
title: "screen"
sort_title: "screen"
---

# screen

> Retrieve information about screen size, displays, cursor position, etc.

You cannot not use this module until the `ready` event of the `app` module is
emitted (by invoking or requiring it).

`screen` is an [EventEmitter](http://nodejs.org/api/events.html#events_class_events_eventemitter).

**Note:** In the renderer / DevTools, `window.screen` is a reserved DOM
property, so writing `let {screen} = require('electron')` will not work.
In our examples below, we use `electronScreen` as the variable name instead.
An example of creating a window that fills the whole screen:

```javascript
const {app, BrowserWindow, screen: electronScreen} = require('electron');

let win;

app.on('ready', () => {
  const {width, height} = electronScreen.getPrimaryDisplay().workAreaSize;
  win = new BrowserWindow({width, height});
});
```

Another example of creating a window in the external display:

```javascript
const {app, BrowserWindow, screen: electronScreen} = require('electron');

let win;

app.on('ready', () => {
  let displays = electronScreen.getAllDisplays();
  let externalDisplay = null;
  for (let i in displays) {
    if (displays[i].bounds.x !== 0 || displays[i].bounds.y !== 0) {
      externalDisplay = displays[i];
      break;
    }
  }

  if (externalDisplay) {
    win = new BrowserWindow({
      x: externalDisplay.bounds.x + 50,
      y: externalDisplay.bounds.y + 50
    });
  }
});
```

## The `Display` object

The `Display` object represents a physical display connected to the system. A
fake `Display` may exist on a headless system, or a `Display` may correspond to
a remote, virtual display.

* `display` object
  * `id` Integer - Unique identifier associated with the display.
  * `rotation` Integer - Can be 0, 90, 180, 270, represents screen rotation in
    clock-wise degrees.
  * `scaleFactor` Number - Output device's pixel scale factor.
  * `touchSupport` String - Can be `available`, `unavailable`, `unknown`.
  * `bounds` Object
  * `size` Object
  * `workArea` Object
  * `workAreaSize` Object

## Events

The `screen` module emits the following events:

### Event: 'display-added'

Returns:

* `event` Event
* `newDisplay` Object

Emitted when `newDisplay` has been added.

### Event: 'display-removed'

Returns:

* `event` Event
* `oldDisplay` Object

Emitted when `oldDisplay` has been removed.

### Event: 'display-metrics-changed'

Returns:

* `event` Event
* `display` Object
* `changedMetrics` Array

Emitted when one or more metrics change in a `display`. The `changedMetrics` is
an array of strings that describe the changes. Possible changes are `bounds`,
`workArea`, `scaleFactor` and `rotation`.

## Methods

The `screen` module has the following methods:

### `screen.getCursorScreenPoint()`

Returns the current absolute position of the mouse pointer.

### `screen.getPrimaryDisplay()`

Returns the primary display.

### `screen.getAllDisplays()`

Returns an array of displays that are currently available.

### `screen.getDisplayNearestPoint(point)`

* `point` Object
  * `x` Integer
  * `y` Integer

Returns the display nearest the specified point.

### `screen.getDisplayMatching(rect)`

* `rect` Object
  * `x` Integer
  * `y` Integer
  * `width` Integer
  * `height` Integer

Returns the display that most closely intersects the provided bounds.

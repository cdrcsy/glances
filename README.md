# glances

A heads-up dashboard for displaying thing-statuses.

![](screenshot.png?raw=true)

## Demo

Example: https://github.com/nicinabox/glances-example

Demo: https://glances-example.herokuapp.com/

## Overview

Glances is designed to be displayed on a monitor and works primarily over websockets. Built with React and Socket.io for lightning fast updates.

## Usage

1. `npm install -S glances-app`
2. `echo "require('glances-app')()" > server.js`
3. Add tiles to `tiles/`
4. Start your server: `npm start`

And remember: it's just JavaScript, there's nothing fancy here.

## Tile Anatomy

A tile may be either a function or an object and include the following keys:

* title
* value
* moreInfo
* color
* position
* display (see Tile Layouts below)

Example:

```javascript
var state = {
  id: 'counter',
  title: 'Counter',
  color: 'teal',
  value: 0
}

module.exports = {
  state,

  onRequest($, body) {
    Object.assign(state, {
      value: +body.counter
    })

    return $.emitChange(state)
  },

  schedule($) {
    $.every('2 sec', 'update counter', () => {
      Object.assign(state, {
        value: state.value + 1
      })
      return $.emitChange(state)
    })
  }
}
```

Tip: Treat your tile objects as immutable to avoid sneaky bugs.

## Tile Layouts

Currently supported tile layouts:

* standard
* ticker
* list

For ticker and list types, pass a key/value object, a flat array, or an array of pairs as the `value`.

## TODO

* [ ] More robust scheduler
* [x] More robust value types based on data structures or setting
* [x] Dynamic tile routes to POST data
* [x] Pluggable, reusable tiles (should be possible currently, but need to investigate)
* [ ] Management UI to search, install, and configure tiles

See [NEXT.md](NEXT.md) for brainstorming on upcoming todos.

## Prior Art

Glances was inspired by, and borrows heavily from [Dashing](http://dashing.io/) by Shopify.

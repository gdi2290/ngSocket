# ngSocket

## Status: In-Development

An AngularJS 1.2 service for connecting applications to servers with WebSocket support.

## Usage

bower install ngSocket

```javascript
.controller('SomeCtrl', function (ngWebSocket) {
  //Open a WebSocket connection
  var ws = ngWebSocket('ws://foo/bar');

  //Can call before socket has opened
  ws.send({foo: 'bar'});
});
```

## API

### Factory: `ngWebSocket`

returns instance of NGWebSocket



### Methods

name        | arguments                                              | description
------------|--------------------------------------------------------|------------
ngWebSocket <br>_constructor_ | url:String                           | Creates and opens a [WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket) instance
send        | data:String,Object                                     | Adds data to a queue, and attempts to send if socket is ready. Accepts string or object, and will stringify objects before sending to socket.
onMessage   | callback:Function <br>pattern:String,RegExp:_optional_ | Register a callback to be fired on every message received from the websocket, or optionally just when the message's `data` property matches the string or pattern provided. Callback gets called with a [MessageEvent](https://developer.mozilla.org/en-US/docs/Web/API/MessageEvent?redirectlocale=en-US&redirectslug=WebSockets%2FWebSockets_reference%2FMessageEvent) object.
onOpen      | callback:Function                                      | Function to be executed each time a socket connection is opened for this instance.

### Properties
name               | type             | description
-------------------|------------------|------------
socket             | window.WebSocket | [WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket) instance.
sendQueue          | Array<function>  | Queue of `send` calls to be made on socket when socket is able to receive data. List is populated by calls to the `send` method, but this array can be spliced if data needs to be manually removed before it's been sent to a socket. Data is removed from the array after it's been sent to the socket.
onOpenCallbacks    | Array<function>  | List of callbacks to be executed when the socket is opened, initially or on re-connection after broken connection. Callbacks should be added to this list through the `onOpen` method.
onMessageCallbacks | Array<function>  | List of callbacks to be executed when a message is received from the socket. Callbacks should be added via the `onMessage` method.

## Logical Questions

 * *Q.*: What if the browser doesn't support WebSockets?
 * *A.*: This module will not help; it does not have a fallback story for browsers that do not support WebSockets.

## Development

`$ npm install .`
`$ bower install`

### Unit Tests
`$ npm test` Starts karma and watches files for changes

### Manual Tests

In the project root directory:

`$ node test-server` Starts a sample web socket server to send/receive messages
`$ ./node_modules/.bin http-server` - Basic http server to seve a static file
Open localhost:8081/test-app.html and watch browser console and node console to see messages passing

### Distribute
`$ ./dist.sh` For now just copies `src/ngSocket.js` to the root (bower is configured to ignore src/ and test, plus pretty much everything else)

## TODO
 * Add support for close method
 * Automatic re-connection when connection lost
 * Consider support for ArrayBuffer and Blob datatypes
 * Add `onerror` to allow applications to respond to socket errors in their own ways
 * Expand on the built-in `readyState` constants to include implementation-specific states (like "Re-connecting")
 * Return a promise when calling `send()`
 * Be able to cancel a send without looking it up in an array.
 * Rewrite in ES6

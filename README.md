# Node-Gameroom
My basic starter kit for creating realtime web-based games with nodejs.

  Includes

  - Bugs - this is alpha baby!
  - Express for handling your sites content
  - Socket.io for mananging your games realtime events and data
  - Tiny client-side helper lib to simplify encoding/decoding and message passing (no JSON which is slow IMO)
  - Crappy docs

  Excludes

  - Any database support. The choice is yours.

# Example
`$ git clone git://github.com/aheckmann/node-gameroom.git --recursive`

`$ sudo node server.js` 

Then navigate to `http://0.0.0.0/` and click Play.

## Serverside
// ...


## Clientside

There is a global `room` object you use to send/receive events from the server
to notify other players etc.

**room.join()** 
Joins a gameroom

**room.send(message)** 
Encodes message(s) and sends to server 

**room.log**
A typical cross-browser safe console logger

**room.bind(name, fn)** 
Registers an event handler (`fn`) for the given event `name`. Name can be either a single event name or a space seperated list of event names to which `fn` is bound.
    room.bind("something-neat", function(eventName, color, amount){
      room.log(eventName + " happened. I saw " + amount + " " + color + " cats skydiving!")
    })
    room.trigger("something-neat", ["blue", 99]) ->  "something-neat happened. I saw 99 blue cats skydiving!"

**room.unbind(name, fn)** 
The inverse of `room.bind`.

**room.trigger(name, args)** 
Executes all bound handlers for event `name` passing in `args` to each.
    room.trigger("wackamole", ["you", "know", "you", "wish", "you", "could"])


## Events

There are a number of Node-Gameroom provided events to help you get your games started.

  - Client-side events
    - gameplayerid(uid) 
      - uid: unique id of the current player (you)
    - gamejoin(uid, totalPlayers)
      - uid: unique id of player that just joined (could be same as gameplayerid)
      - totalPlayers: total number of players in the gameroom
    - gamecountdown(count)
      - count: (int) If you specify a countdown on the server, all clients are notified each second: (10, 9, 8, ...) This is useful if you need your players to wait a certain amount of time before the game starts.
    - gamestart - Signals the start of the game.
    - gametimer(timer)
      - timer: (int) Sent once per second along with the number of seconds left in the game.
    - gameover - Auto sent when the gametimer runs out or whenever you want. Ends the game.

  - Server-side events
    - // ...


If you want to track the number of players in a gameroom, just listen for the `gamejoin` event. When a player joins (including yourself) the `gamejoin` event fires on the client passing along the uid of each player in the gameroom as seperate args. Just use `arguments.length - 1`.

    room.bind("gamejoin", function(event, id1, id2, ...){
      room.log("There are " + arguments.length - 1 + " players in the game room")
    })
    
See the [demo](http://github.com/aheckmann/node-gameroom/blob/master/views/room.jade) for another example of event handling.

## License 

(The MIT License)

Copyright (c) 2010 [Aaron Heckmann](aaron.heckmann+github@gmail.com)

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

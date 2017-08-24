<p align="center">
<img src="https://cdn.rawgit.com/lakefox/LakeFox/4dfc27d8/lakefox.png">
</p>

# What is LakeFox?
Multiplayer video games are huge, they have millions to hundreds of millions of players playing at a time. Most game development companies focuses on the game development (which make sense), but fail to realize how much goes into making the game multiplayer. Some developers will give up on making their game multiplayer and just make their game single player, while some will go through the pain staking process of doing it them selves. If they do succeeded in building the server, they have to host the server which is expensive, which discourages the little guys from building games and forces the big guys to charge or put ads on their games. While creating a game we realized how hard this should be simple process of making a multiplayer game is, so we created LakeFox. LakeFox simplfies the networking for video games and makes a peer to peer network for each player to communicate with each other.

### Peer to Peer?
Most prebuilt multiplayer server don't use p2p because having the player connect to 20 different computers (Full Mesh Topology) will slow their computers down taking away from the experince the game gives. The way they do it instead is a client server topology.

<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b8/FullMeshNetwork.svg/2000px-FullMeshNetwork.svg.png" width="50%"><br>
  Full Mesh Topology<br>
  <img src="http://practice.geeksforgeeks.org/ckeditor/images/uploads/1491250148_client_server.png" width="50%"><br>
  Client Sever Topology
</p>

In this model all the clients send their game-state to the server, when the server gets all the game-states it sends all the clients the synced version. LakeFox Work by not connecting the players in a client server topology or full mesh topology, but a peer neighbor mesh topology.

<p align="center">
	<img src="https://cdn.rawgit.com/lakefox/LakeFox/76fedf98/topology.png" width="50%"><br>
  	Peer Neighbor Mesh Topology
</p>

In peer mesh toplogy each player (peer) is connected to two other players. When the (player) recives some data from another player (From) it keeps a copy of the data and sends the other player (To).

<p align="center">
	<img src="https://cdn.rawgit.com/lakefox/LakeFox/f7db608e/connections.png" width="50%"><br>
</p>

The network is self healing so when a player disconnects the server sends out a message to all the players in the room and they will automatically reconnect keeping the network running.

# Demo

Go to [pnm.lakefox.net](pnm.lakefox.net). This will connect you to LOBBY: njvscb, ROOM: 0, you will connect to everyone that opens this demo up.

# How to use it? (Node.JS)

## Downloading

``` shell
$ npm install lakefox
```

## Running

LakeFox is setup to run multiple games on the same server for so the way the connections are broken up are through lobbies and rooms.

## Connecting

LakeFox is a platform/language indepentent framework, it communtinicates through websockets and runs anywhere Node.Js runs. The reason for running everything through websockets is most games aren't writen in Node.Js, but in faster languages like C++. To make LakeFox run with any language you want we decided to use a universal connection protocall that allow two-way communication (Websockets) to connect the platform. To connect and send messages through the websockets you will need to connect to http://localhost:PORT, (PORT being what ever you have set it to be), then once the connection is established you will send and receive everything through channel. After the websocket connection is made and all the p2p cnnections are made the server will send ```{"CONNECTED": true}```

Start LakeFox on the players computer
``` shell
$ node fox LOBBY ROOM PORT
```
Then using a pesudo code example (Using a JS syntax)

_Note: this code should be part of your game_

_Note 2: this code doesn't work in any language it just looks like JS_
#### Sending/Receiving
``` javascript
var connected = false;

// Connect to the WebSocket Server hosted on the player's computer
var ws = Websocket("http://localhost:8080");

// Listen for the WebSocket connection to be made
ws.on("connect", (connection)=>{
	// Listen for the DATA event
	connection.on("message", (raw)=>{
    	// Parse the raw JSON
    	var data = JSON.parse(raw);
        // See if you are already connected
        if (connected) {
  		// HANDLE THE DATA
        handleFunc(data);
        // Send data to the other players
        ws.send(newData);
	} else {
          // Check to see if the server sent the connection message
          if (data.CONNECTED == true) {
              // Store that you are connected
              connected = true;
          }
        }
    });
});
```

# How to use it? (Browser)
## Downloading

Download [fox.min.js](https://github.com/lakefox/Fox)

## Include the script

``` html
<script src="fox.min.js"></script>
```

## Basic Usage

``` javascript
var fx = new fox(LOBBY, ROOM, (msg, senderId)=>{
  // Handle the msg
}, (HOST));

//Send a message
fx.msg("Send Message");
```

_For a real example go to [LakeFox.html](https://github.com/lakefox/Fox/blob/master/lakefox.html)_

# Configuration Options

### Lobby
Lobbies are basically the game, so if I created a game called Ninja's vs. Cowboy's TM my lobby name could be njvscb
``` shell
$ node fox njvscb ROOM PORT (HOST)
```

### ROOM

Rooms are subcatagories for the lobbies, so in Ninja's vs. Cowboy's TM there are 2 v 2 room's that four people can fight each other. So I will create a room using a simple counter so the first room is room 0.

_Note: these name's are just used as an example you can use anything for the LOBBY or ROOM_
``` shell
$ node fox njvscb 0 PORT (HOST)
```

### PORT (Node.JS Only)

The port is left open for the developer (you) to decide. It is left open so if the game needs to use a port for another feature.

_Note: there isn't a default port so if you leave it blank the software **will** crash_
``` shell
$ node fox njvscb 0 8080 (HOST)
```

### (HOST)

HOST is the only optional parmeter it will only be used if you want to use a self hosted version of [lake.js](https://github.com/lakefox/Lake/blob/master/lake.js) it defaults to [pnm.lakefox.net](http://pnm.lakefox.net) (Recommended)

**You should only use this feature for development purposes to ensure stability**
``` shell
$ node fox njvscb 0 8080 http://localhost:3000
```

# License

Please read the [LICENSE](https://github.com/lakefox/LakeFox.github.io/blob/master/LICENSE)

### Want to use LakeFox

If you are interested in using LakeFox for your project, or have any questions please email me at mason@lakefox.net

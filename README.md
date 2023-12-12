# Serverless Peer to Peer Encrypted Connection

## Description

Proove you can create a Peer to Peer connection without a signaling server.

Normally a signaling server is used to exchange connection information between two peers. Commonly done through a websocket. This project is proof of concept to create a peer to peer connection without a signaling server.

Currently there is a [STUN](https://en.wikipedia.org/wiki/STUN) server used to calculate the public internet address. If the peers are on the same network, the STUN server is not needed. 

This project uses the [WebRTC](https://en.wikipedia.org/wiki/WebRTC) javascript implementation supported by most modern browsers. If you want to create a peer to peer connection without the use of a helper library then this may be a good place to start.


## Developement Environment

```bash
git clone git@github.com/popkinj/webrtc.git
cd webrtc
npm ci
npm run dev
```

## Current Workflow
1. Open two browser windows to the testing server.
>  - [Page 1](http://localhost:5173/artifact1)
>  - [Page 2](http://localhost:5173/artifact2)
2. Create and pass the connection information between page 1 and page 2.
3. When a data connection is established, test sending messages between the two pages. Currently all messages are sent to the console.

## Future Workflow

1. Instigator creates a QR-Code which contains a link to the testing server. The url will contain all the connection information of the Instigator.
1. The responder scans the QR-Code and opens the link in the browser.
1. The responder stores the connection information into Peer object.
1. The instigator gets the signal through webrtc that the responder is ready to connect.
1. The instigator's camera is turned on ready to scan the responders QR-Code.
1. The responder creates a QR-Code which contains a link to the testing server. The url will contain all the connection information of the Responder.
1. The instigator scans the QR-Code of the responder and saves the connection information into a Peer object.

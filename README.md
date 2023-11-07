# Serverless Peer to Peer Proof of Concept

## Description

Proove you can create a Peer to Peer conection without a signaling server.

A testing server is running here in the place of having two separate wallet applications. The QR Code could eventually use deep links to open the wallet application directly.

## Flow

1. Instigator creates a QR-Code which contains a link to the testing server. The url will contain all the connection information of the Instigator.
1. The responder scans the QR-Code and opens the link in the browser.
1. The responder stores the connection information into Peer object.
1. The instigator gets the signal through webrtc that the responder is ready to connect.
1. The instigator's camera is turned on ready to scan the responders QR-Code.
1. The responder creates a QR-Code which contains a link to the testing server. The url will contain all the connection information of the Responder.
1. The instigator scans the QR-Code of the responder and saves the connection information into a Peer object.

<!--
  Create a data connection without a Stun or Signal server.
  Must be on the same network.
-->
<script>
  import { onMount } from 'svelte';
  import QRCode from 'easyqrcodejs'

  let signalOffer = '';
  let signalAccept = '';
  let peerConnection;
  let dataChannel;

  // No STUN or Signal server
  const config = {
    iceServers: [ ]
  };

  const instigate = async () => {
    console.log('instigate');

    peerConnection = new RTCPeerConnection(config);
    dataChannel = peerConnection.createDataChannel('dataChannel', {
      negotiated: true,
      id: 0
    });

    const offer = await peerConnection.createOffer();
    await peerConnection.setLocalDescription(offer);
    peerConnection.onicecandidate = ({ candidate }) => {
      if (candidate) return;
      signalOffer = peerConnection.localDescription.sdp;
	  const uri = encodeURIComponent(JSON.stringify({ offer: signalOffer }));
	  console.log('uri', `localhost:5173/serverless#offer=${uri}`);
	  console.log('signalOffer', signalOffer)
    };

    /*
      TODO:
      - URI Encode the sdb object
      - Create total url string
      - Create QRCode from the url string
    */
    const options = {
      text: "https://github.com/ushelp/EasyQRCodeJS"
    };
    
    // Create QRCode Object
    new QRCode(document.getElementById("qrcode"), options);
  }


  const hashchange = () => {
    const hash = window.location.hash;

    if (hash) {
      /*  States
       * - offer
       * - counter
       * - live
       */
      if (hash.includes('#offer=')) {
        // Add remote in Peer object
        // create counter offer QR-Code
        console.log('offer');
      } else if (hash.includes('#counter=')) {
        // Add remote in Peer object
        // complete connection... move to #live
        console.log('counter');
      } else if (hash.includes('#live')) {
        // Connection is active
        console.log('live')
      } else {
        // If another other hash
        instigate();
      }
    } else {
      // instigate connection
      instigate();
    }
  };

  onMount(hashchange);
</script>

<svelte:window on:hashchange={hashchange} />

<div id="qrcode"></div>
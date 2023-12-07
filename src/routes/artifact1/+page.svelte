<script>
  import { onMount } from 'svelte';
  // import { writable } from 'svelte/store';

  let signalOffer = '';
  let signalAccept = '';
  let peerConnection;
  let dataChannel;
  let txtMessage = '';

  const config = {
    iceServers: [
      {
        urls: ['stun:stun.l.google.com:19302']
      }
    ]
  };

  // Create a new RTCPeerConnection
  onMount(() => {
    peerConnection = new RTCPeerConnection(config);
    dataChannel = peerConnection.createDataChannel('dataChannel', {
      negotiated: true,
      id: 0
    });
  });

  // Function to start a webrtc peer 2 peer connection
  const startP2P = async () => {
    // Create an offer
    const offer = await peerConnection.createOffer();
    await peerConnection.setLocalDescription(offer);
    peerConnection.onicecandidate = ({ candidate }) => {
      if (candidate) return;
      signalOffer = peerConnection.localDescription.sdp;
    };

    setInterval(() => {
      console.log('connection state', peerConnection.connectionState);
    }, 1000);
  };

  const acceptP2P = async () => {
    // Listen for messages
    dataChannel.addEventListener('message', (event) => {
      console.log(event.data);
    });

    console.log('accept')
    await peerConnection.setRemoteDescription({
      type: 'answer',
      sdp: signalAccept
    });
    console.log('accepted')
  };

  const send = () => {
    dataChannel.send(txtMessage);
  };

</script>

<h3>Artifact 1</h3>

<!-- Inputs that just need to display things can have 1 way data binding -->
<div>
  <button on:click={startP2P}>Start Peer 2 Peer Connection</button>
  <textarea readonly rows="10" value={signalOffer} />
</div>
{#if signalOffer}
  <div>
    <label>
      Paste the counter offer here:
      <textarea rows="10" bind:value={signalAccept} />
    </label>
    {#if signalAccept}
      <button class="secondary" on:click={acceptP2P}>Accept Peer 2 Peer Connection</button>
    {/if}
  </div>
{/if}

<!--
  This is for testing
-->
{#if true}

<!-- {#if peerConnection?.connectionState === 'connected'} -->
  <div>
    <label>
      Send a message:
      <input type="text" bind:value={txtMessage}/>
    </label>
    <button class="secondary" on:click={send}>Send</button>
  </div>
{/if}

<script>
  import { onMount } from 'svelte';

  let signalOffer = '';
  let signalAccept = '';
  let peerConnection;
  let dataChannel;
  let txtMessage = '';

  const config = {
    iceServers: [
      {
        urls: 'stun:stun.l.google.com:19302' // list of free STUN servers: https://gist.github.com/zziuni/3741933
      }
    ]
  };

  onMount(() => {
    peerConnection = new RTCPeerConnection(config);
    dataChannel = peerConnection.createDataChannel('dataChannel', {
      negotiated: true,
      id: 0
    });

    // peerConnection.addEventListener('iceconnectionstatechange', (event) => {
    //   console.log('state change - connection state change', peerConnection.connectionState);
    // });
  });

  const acceptP2P = async () => {
	console.log('accept')
	dataChannel.addEventListener('message', (event) => {
      console.log(event.data);
	});

    await peerConnection.setRemoteDescription({
      type: 'offer',
      sdp: signalOffer
    });
    await peerConnection.setLocalDescription(await peerConnection.createAnswer());
    peerConnection.onicecandidate = ({ candidate }) => {
      if (candidate) return;
      signalAccept = peerConnection.localDescription.sdp;
    };
  };

  const send = () => {
    dataChannel.send(txtMessage);
  };
</script>

<h3>Artifact 2</h3>
<!-- Inputs that can be edited need 2 way data binding -->
<div>
  <label>
    Paste the offer here:
    <textarea rows="10" bind:value={signalOffer} />
  </label>
  <button class="secondary" on:click={acceptP2P}>Accept Peer 2 Peer Connection</button>
</div>

{#if signalAccept}
  <div>
    <label>
      Counter offer:
      <textarea rows="10" value={signalAccept} />
    </label>
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

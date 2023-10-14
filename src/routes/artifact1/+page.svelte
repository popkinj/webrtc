<script>
	import { onMount } from 'svelte';
	// import { writable } from 'svelte/store';

	let signalOffer = '';
	let signalAccept = '';
	let peerConnection;

	// Create a new RTCPeerConnection
	onMount(() => {
		peerConnection = new RTCPeerConnection();
	});

	// Function to start a webrtc peer 2 peer connection
	const startP2P = async () => {
		// Create a data channel
		const dataChannel = peerConnection.createDataChannel('dataChannel');

		// Listen for messages
		dataChannel.addEventListener('message', (event) => {
			console.log(`Message from DataChannel '${dataChannel.label}'`);
			console.log(event.data);
		});

		// Create an offer
		const offer = await peerConnection.createOffer();
		await peerConnection.setLocalDescription(offer);
		signalOffer = JSON.stringify(offer.toJSON());
		// console.log(signalOffer);
	};

	const acceptP2P = async () => {
		console.log('signalAccept', signalAccept);
		// TODO: Parse signalAccept and use it to create an accept offer

		// signalAccept = signalAccept;
		// Create an answer
		// const answer = await peerConnection.createAnswer();
		// await peerConnection.setLocalDescription(answer);
		// signalAccept = JSON.stringify(answer.toJSON());
		// console.log(signalAccept);
	};

	// Function to accept a webrtc peer 2 peer connection
	// const acceptP2P = async () => {
</script>

<h3>Artifact 1</h3>

<!-- Inputs that just need to display things can have 1 way data binding -->
<div>
	<button on:click={startP2P}>Start Peer 2 Peer Connection</button>
	<textarea readonly rows="10" value={signalOffer} />
</div>
<hr />
<!-- Inputs that can be edited need 2 way data binding -->
<div>
	<textarea rows="10" bind:value={signalAccept} />
	<button class="secondary" on:click={acceptP2P}>Accept Peer 2 Peer Connection</button>
</div>

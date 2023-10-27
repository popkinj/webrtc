<script>
	import { onMount } from 'svelte';
	// import { writable } from 'svelte/store';

	let signalOffer = '';
	let signalAccept = '';
	let peerConnection;
	let dataChannel;

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
		// Create a data channel

		// Listen for messages
		// dataChannel.addEventListener('message', (event) => {
		// 	console.log(`Message from DataChannel '${dataChannel.label}'`);
		// });

		// Create an offer
		const offer = await peerConnection.createOffer();
		await peerConnection.setLocalDescription(offer);
		peerConnection.onicecandidate = ({ candidate }) => {
			if (candidate) return;
			signalOffer = peerConnection.localDescription.sdp;
		};
	};

	// TODO: Think I need to listen for Ice candidates here
	// const acceptP2P = async () => {
	// 	const accept = JSON.parse(signalAccept);
	// 	console.log('accept', accept);
	// 	await peerConnection.setRemoteDescription(accept);
	// 	setInterval(() => {
	// 		console.log('connection state', peerConnection.connectionState);
	// 	}, 1000);
	// };
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

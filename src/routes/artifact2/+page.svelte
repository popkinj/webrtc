<script>
	import { onMount } from 'svelte';
	let signalAccept = '';
	let peerConnection;
	onMount(() => {
		peerConnection = new RTCPeerConnection();
	});

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

<h3>Artifact 2</h3>
<!-- Inputs that can be edited need 2 way data binding -->
<div>
	<label>
		Paste the offer here:
		<textarea rows="10" bind:value={signalAccept} />
	</label>
	<button class="secondary" on:click={acceptP2P}>Accept Peer 2 Peer Connection</button>
</div>

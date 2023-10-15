<script>
	import { onMount } from 'svelte';

	let signalOffer = '';
	let signalAccept = '';
	let peerConnection;

	onMount(() => {
		peerConnection = new RTCPeerConnection();

		peerConnection.addEventListener('icecandidate', (event) => {
			console.log('icecandidate - connection state', peerConnection.connectionState);
			if (event.candidate) {
				console.log('New ICE candidate');
				console.log(event.candidate);
			}
		});

		peerConnection.addEventListener('iceconnectionstatechange', (event) => {
			console.log('state change - connection state change', peerConnection.connectionState);
		});
	});

	const acceptP2P = async () => {
		const offer = JSON.parse(signalOffer);
		// Create an answer
		await peerConnection.setRemoteDescription(offer);
		const answer = await peerConnection.createAnswer();
		await peerConnection.setLocalDescription(answer);
		signalAccept = JSON.stringify(answer.toJSON());
		console.log('connection state', peerConnection.connectionState);
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

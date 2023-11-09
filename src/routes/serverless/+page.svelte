<script>
	import { onMount } from 'svelte';
	import QRCode from 'easyqrcodejs'

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
				console.log('instigate');
			}
		} else {
			// instigate connection
			console.log('instigate');
		}
	};

	onMount(hashchange);
</script>

<li>If there is no hash then instigate a connection</li>
<li>If there is a hashchange containing an offer then create a counter offer and update hash</li>
<li>If there is a hashchange containing a counter offer complete connection</li>
<svelte:window on:hashchange={hashchange} />

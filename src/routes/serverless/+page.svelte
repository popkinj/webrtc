<script>
	import { onMount } from 'svelte';

	const hashchange = () => {
		const hash = window.location.hash;
		console.log('hashchange', hash);
		if (hash) {
			/* TODO: Use a switch statement to handle all hashchange states and cases
			 * - instigate
			 * - offer
			 * - counter
			 * - complete
			 * TODO: Not sure what to do regarding svelte state
			 */
			if (hash.includes('#offer=')) {
				// Add remote in Peer object
				// create counter offer QR-Code
				console.log('offer');
			} else if (hash.includes('#counter=')) {
				// Add remote in Peer object
				// complete connection
				console.log('counter');
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

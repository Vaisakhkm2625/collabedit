<script>
	import * as Y from 'yjs';
	import { WebrtcProvider } from 'y-webrtc';

	const doc = new Y.Doc();
	const provider = new WebrtcProvider('webrtc-test', doc, { signaling: ['ws://localhost:4444'] });

	const yarray = doc.get('array', Y.Array);

	console.log(yarray.toJSON());

	let text = '';
	let arr = [];
	provider.on('synced', (synced) => {
		console.log('synced:', synced);
	});
	yarray.observeDeep(() => {
		console.log('yarray updated: ', yarray.toJSON());
		arr = yarray.toArray();
	});
	function press() {
		yarray.push([text]);
		console.log('hi');
	}

	// 	doc.on('update', (updateMessage, origin, doc) => {
	// 		console.log(updateMessage);
	// 	});
	//
</script>

<input type="text" bind:value={text} />
<button on:click={press}> </button>

{#each arr as i}
	<!-- content here -->
	{i} <br />
{/each}

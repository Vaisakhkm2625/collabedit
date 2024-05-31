<script>
	import '@fortawesome/fontawesome-free/css/all.min.css';

	import { Editor } from '@tiptap/core';
	import { StarterKit } from '@tiptap/starter-kit';
	import Collaboration from '@tiptap/extension-collaboration';
	import CollaborationCursor from '@tiptap/extension-collaboration-cursor';
	import * as Y from 'yjs';
	import { HocuspocusProvider } from '@hocuspocus/provider';
	import { onMount } from 'svelte';

	let ydoc = new Y.Doc();
	let element;
	let editor;

	let provider = new HocuspocusProvider({
		url: 'ws://127.0.0.1:1234',
		name: 'example-document',
		document: ydoc
	});

	onMount(() => {
		editor = new Editor({
			element: element,
			extensions: [
				StarterKit.configure({
					history: false
				}),
				Collaboration.configure({
					document: ydoc
				}),
				CollaborationCursor.configure({
					provider,
					user: { name: 'John Doe', color: '#ffcc00' }
				})
			],
			content: `
                <p>
                    Hello, world!
                </p>
            `,
			onTransaction: () => {
				// force re-render so `editor.isActive` works as expected
				editor = editor;
			}
		});
	});
</script>

editer
{#if editor}
	<br />
	<button on:click={() => editor.chain().focus().toggleItalic().run()}>
		<i class="fa-solid fa-italic"></i>
	</button>
	<br />
{/if}

<div bind:this={element} />

<style>
	:global(.ProseMirror) {
		border: 1px solid #ddd;
		border-radius: 5px;
	}
</style>

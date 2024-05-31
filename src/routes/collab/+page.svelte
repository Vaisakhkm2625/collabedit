<script>
	import '@fortawesome/fontawesome-free/css/all.min.css';

	import { Editor } from '@tiptap/core';
	import { StarterKit } from '@tiptap/starter-kit';
	import Collaboration from '@tiptap/extension-collaboration';
	import CollaborationCursor from '@tiptap/extension-collaboration-cursor';
	import Highlight from '@tiptap/extension-highlight';
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
				}),
				Highlight.configure({
					HTMLAttributes: {
						class: 'text-highlighted'
					}
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

	//	const buttons = [
	//		{
	//			name: 'italics',
	//			icon: 'fa-solid fa-italic',
	//			command: () => editor.chain().focus().toggleItalic().run()
	//		},
	//		{
	//			name: 'bold',
	//			icon: 'fa-solid fa-bold',
	//			command: () => editor.chain().focus().toggleBold().run()
	//		}
	//	];

	const buttons = [
		{
			name: 'bold',
			icon: 'fa-solid fa-bold',
			command: () => editor.chain().focus().toggleBold().run(),
			disabled: () => !editor.can().chain().focus().toggleBold().run(),
			isActive: () => editor.isActive('bold')
		},
		{
			name: 'italic',
			icon: 'fa-solid fa-italic',
			command: () => editor.chain().focus().toggleItalic().run(),
			disabled: () => !editor.can().chain().focus().toggleItalic().run(),
			isActive: () => editor.isActive('italic')
		},
		{
			name: 'strike',
			icon: 'fa-solid fa-strikethrough',
			command: () => editor.chain().focus().toggleStrike().run(),
			disabled: () => !editor.can().chain().focus().toggleStrike().run(),
			isActive: () => editor.isActive('strike')
		},
		{
			name: 'code',
			icon: 'fa-solid fa-code',
			command: () => editor.chain().focus().toggleCode().run(),
			disabled: () => !editor.can().chain().focus().toggleCode().run(),
			isActive: () => editor.isActive('code')
		},
		{
			name: 'clear marks',
			icon: 'fa-solid fa-eraser',
			command: () => editor.chain().focus().unsetAllMarks().run()
		},
		{
			name: 'clear nodes',
			icon: 'fa-solid fa-ban',
			command: () => editor.chain().focus().clearNodes().run()
		},
		{
			name: 'paragraph',
			icon: 'fa-solid fa-paragraph',
			command: () => editor.chain().focus().setParagraph().run(),
			isActive: () => editor.isActive('paragraph')
		},
		{
			name: 'h1',
			icon: 'fa-solid fa-heading',
			command: () => editor.chain().focus().toggleHeading({ level: 1 }).run(),
			isActive: () => editor.isActive('heading', { level: 1 })
		},
		{
			name: 'h2',
			icon: 'fa-solid fa-heading',
			command: () => editor.chain().focus().toggleHeading({ level: 2 }).run(),
			isActive: () => editor.isActive('heading', { level: 2 })
		},
		{
			name: 'h3',
			icon: 'fa-solid fa-heading',
			command: () => editor.chain().focus().toggleHeading({ level: 3 }).run(),
			isActive: () => editor.isActive('heading', { level: 3 })
		},
		{
			name: 'h4',
			icon: 'fa-solid fa-heading',
			command: () => editor.chain().focus().toggleHeading({ level: 4 }).run(),
			isActive: () => editor.isActive('heading', { level: 4 })
		},
		{
			name: 'h5',
			icon: 'fa-solid fa-heading',
			command: () => editor.chain().focus().toggleHeading({ level: 5 }).run(),
			isActive: () => editor.isActive('heading', { level: 5 })
		},
		{
			name: 'h6',
			icon: 'fa-solid fa-heading',
			command: () => editor.chain().focus().toggleHeading({ level: 6 }).run(),
			isActive: () => editor.isActive('heading', { level: 6 })
		},
		{
			name: 'bullet list',
			icon: 'fa-solid fa-list-ul',
			command: () => editor.chain().focus().toggleBulletList().run(),
			isActive: () => editor.isActive('bulletList')
		},
		{
			name: 'ordered list',
			icon: 'fa-solid fa-list-ol',
			command: () => editor.chain().focus().toggleOrderedList().run(),
			isActive: () => editor.isActive('orderedList')
		},
		{
			name: 'code block',
			icon: 'fa-solid fa-code',
			command: () => editor.chain().focus().toggleCodeBlock().run(),
			isActive: () => editor.isActive('codeBlock')
		},
		{
			name: 'blockquote',
			icon: 'fa-solid fa-quote-right',
			command: () => editor.chain().focus().toggleBlockquote().run(),
			isActive: () => editor.isActive('blockquote')
		},
		{
			name: 'horizontal rule',
			icon: 'fa-solid fa-minus',
			command: () => editor.chain().focus().setHorizontalRule().run()
		},
		{
			name: 'hard break',
			icon: 'fa-solid fa-arrow-down',
			command: () => editor.chain().focus().setHardBreak().run()
		},
		{
			name: 'undo',
			icon: 'fa-solid fa-undo',
			command: () => editor.chain().focus().undo().run(),
			disabled: () => !editor.can().chain().focus().undo().run()
		},
		{
			name: 'redo',
			icon: 'fa-solid fa-redo',
			command: () => editor.chain().focus().redo().run(),
			disabled: () => !editor.can().chain().focus().redo().run()
		},
		{
			name: 'purple',
			icon: 'fa-solid fa-palette',
			command: () => editor.chain().focus().setColor('#958DF1').run(),
			isActive: () => editor.isActive('textStyle', { color: '#958DF1' })
		},
		{
			name: 'purple',
			icon: 'fa-solid fa-palette',
			command: () => editor.commands.setHighlight(),
			isActive: () => editor.isActive('textStyle', { color: '#958DF1' })
		}
	];
</script>

editer
{#if editor}
	<br />
	{#each buttons as button}
		<button title={button.name} on:click={button.command}>
			<i class={button.icon}></i>
		</button>
	{/each}
	<br />
{/if}

<div bind:this={element} />

<style>
	:global(.ProseMirror) {
		border: 1px solid #ddd;
		border-radius: 5px;
	}
</style>

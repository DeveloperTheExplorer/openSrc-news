<svelte:head>
<link rel="stylesheet"
      href="//cdnjs.cloudflare.com/ajax/libs/highlight.js/11.5.1/styles/atom-one-dark.min.css">
</svelte:head>

<style lang="scss" global>
  @import './main.sass';
</style>

<script lang="ts">
	import { onMount } from 'svelte';
	import EditorToolbar from '@/components/EditorToolbar/index.svelte';
	import hljs from 'highlight.js';
	
	let editor;
	let quill;

	onMount(
		async () => {
			const { default: Quill } = await import('quill');

			hljs.configure({
				languages: ['javascript', 'ruby', 'python']
			});

			quill = new Quill(editor, {
				modules: {
					syntax: {
						highlight: (text: string) => {
							return hljs.highlightAuto(text).value;
						}
					},
					toolbar: '#toolbar'
				},
				theme: 'snow',
				placeholder: 'Here is the truth...'
			})
		}
	);
</script>

<div class="editor-container">
	<EditorToolbar quill={quill} />
	<input id="title" type="text" class="text-3xl" placeholder="The story of...">
	<div id="editor" class="editor" bind:this={editor}></div>
</div>

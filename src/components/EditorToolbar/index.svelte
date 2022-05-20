
<script lang="ts">
    import { onMount } from 'svelte';
    
    export let quill;

    let editorBounds: DOMRect;
    let toolbarBounds = {
        top: 0,
        left: 0,
        status: "",
    };

    const updateXY = () => {
        const editor = document.querySelector("#editor");
        editorBounds = editor!.getBoundingClientRect();
    };

    const init = () => {
        document
            .querySelector('#toolbar')!
            .addEventListener('click', (e) => {
                e.stopPropagation();
            });
        document
            .querySelector('#editor')!
            .addEventListener('click', (e) => {
                e.stopPropagation();
            });

        updateXY();
    }

    onMount(
        () => {
            window.addEventListener("resize", updateXY);
            window.onload = init;

            document.body.addEventListener('click', () => {
                toolbarBounds.status = "";
            });
        }
    )

    const initQuillListeners = (q) => {
        if (!q) {
            return;
        }

        q.on("selection-change", (e) => {
            if (!e || e.length === 0) {
                toolbarBounds.status = "";

                return;
            }
            const { left, top, width } = q.getBounds(e.index, e.length);

            toolbarBounds = {
                top: top + editorBounds.top - 48,
                left: left + editorBounds.left + width / 2,
                status: "active",
            };
        });
    };

    $: initQuillListeners(quill);
</script>

<div id="toolbar">
    <div
        id="popup-toolbar"
        class="toolbar {toolbarBounds.status}"
        style:top="{toolbarBounds.top}px"
        style:left="{toolbarBounds.left}px"
    >
        <button class="editor-button ql-header ql-size ql-active" value={3}>
            H1
        </button>

        <span class="divider" />

        <button class="editor-button ql-bold" />
        <button class="editor-button ql-italic" />
        <button class="editor-button ql-underline" />

        <span class="divider" />

        <button class="editor-button ql-align ql-active" value="" />
        <button class="editor-button ql-align" value="center" />
        <button class="editor-button ql-align" value="right" />
        <button class="editor-button ql-align" value="justify" />

        <span class="divider" />

        <button class="editor-button ql-link" />
        <button class="editor-button ql-code-block" />
    </div>

    <div id="static-toolbar" class="toolbar">
        <button class="editor-button ql-header ql-size ql-active" value={3}>
            H1
        </button>

        <span class="divider" />

        <button class="editor-button ql-bold" />
        <button class="editor-button ql-italic" />
        <button class="editor-button ql-underline" />

        <span class="divider" />
        
        <button class="editor-button ql-align ql-active" value="" />
        <button class="editor-button ql-align" value="center" />
        <button class="editor-button ql-align" value="right" />
        <button class="editor-button ql-align" value="justify" />
        
        <span class="divider" />

        <button class="editor-button ql-link" />
        <button class="editor-button ql-image" />
        <button class="editor-button ql-code-block" />
    </div>
</div>

<style lang="sass" global>
    @import './main.sass'
</style>

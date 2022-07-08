<script>
    import {onMount} from 'svelte'
    import { draggable } from '@neodrag/svelte';
    export let title
    export let x
    export let y
    export let gpuAcceleration
    export let disabled
    disabled = !disabled
    let layer = 1
    let defaultPos = {x: x, y: y}
    let options = { gpuAcceleration: gpuAcceleration, position: defaultPos, defaultPosition: defaultPos, disabled: disabled, bounds: { left: 0, top: 0, bottom: 0, right: 0 }, handle: '.drag-handle' }
    let nodeRef
    function delElement() {
        nodeRef.parentNode.removeChild(nodeRef)
    }
    function maximize(e) {
        nodeRef.style.minWidth = '100vw'
        nodeRef.style.minHeight = '100vh'
        nodeRef.style.width = '100vw'
        nodeRef.style.height = '100vh'
        nodeRef.style.zIndex = '10000000000000000000000000000000000000000000000000000000000000000000000000000000000000'
        nodeRef.style.margin = '0'
        options = { gpuAcceleration: true, defaultPosition: defaultPos, position: {x: -8, y: -8}, handle: '.drag-handle' }
        e.target.setAttribute('draggable', 'false')
        options = { gpuAcceleration: gpuAcceleration, position: { x: -8, y: -8 }, disabled: true, bounds: { left: 0, top: 0, bottom: 0, right: 0 }, handle: '.drag-handle' }
        e.target.style.cursor = 'pointer'
    }
    function minimize() {
        nodeRef.style.minWidth = '300px'
        nodeRef.style.minHeight = 'fit-content'
        nodeRef.style.height = null
        nodeRef.style.width = null
        nodeRef.style.zIndex = null
        options = { gpuAcceleration: gpuAcceleration, position: defaultPos, defaultPosition: defaultPos, disabled: disabled, bounds: { left: 0, top: 0, bottom: 0, right: 0 }, handle: '.drag-handle' }
        e.target.style.cursor = 'grab'
    }
    function startDrag(e) {
        e.target.style.cursor = 'grabbing'
        let topLayer = localStorage.getItem('window-topLayer')
        topLayer = parseInt(topLayer)
        if (topLayer == null || typeof topLayer !== 'number') {
            localStorage.setItem('window-topLayer', layer)
            topLayer = 1
        }
        const newLayer = topLayer + 1
        e.target.style.zIndex = newLayer
        layer = newLayer
        localStorage.setItem('window-topLayer', layer)
    }
    function endDrag(e) {
        e.target.style.cursor = 'grab'
    }
    onMount(() => {
        window.localStorage.setItem('window-topLayer', layer)
    })
</script>
<div class="window window-shadow resizable" draggable={disabled} use:draggable={options} on:neodrag:start={(e) => startDrag(e)} on:neodrag:end={(e) => endDrag(e)} bind:this={nodeRef}>
    <div class="top-bar drag-handle">
        <button title="Close window" class="close-button" on:click={delElement}></button>
        <button title="Minimize window" class="minimize-button" on:click={minimize}></button>
        <button title="Maximize window" class="maximize-button" on:click={maximize}></button>
        <div class="title">
            <p class="title-text">{title}</p>
        </div>
    </div>
    <div id="content">
        <slot />
    </div>
</div>
<style>
    #content {
        overflow-wrap: break-word;
        display: grid;
        place-items: center;
        padding: 0.5rem;
    }
    .title-text {
        margin-bottom: 0.5rem;
        margin-top: -2px;
        color: #423f42
    }
    .title {
        position: absolute;
        display: grid;
        place-items: center;
        width: 100%;
        z-index: 1;
        user-select: none !important;
    }
    .close-button {
        background-color: #ff5f58;
        border-color: #e1483f;
        border-width: 1px;
        border-style: solid;
        border-radius: 9999px;
        padding: 0;
        width: 12px;
        height: 12px;
        cursor: pointer;
        display: inline;
        z-index: 10;
        margin-left: 0.5rem;
        margin-bottom: 1rem;
        margin: 0.25rem;
    }
    .close-button:active {
        background-color: #bf4942;
    }
    .minimize-button {
        background-color: #ffbe2f;
        border-color: #e0a028;
        border-width: 1px;
        border-style: solid;
        border-radius: 9999px;
        padding: 0;
        width: 12px;
        height: 12px;
        cursor: pointer;
        display: inline;
        z-index: 10;
        margin-bottom: 0.5rem;
        margin: 0.25rem;
    }
    .minimize-button:active {
        background-color: #bf9e28;
    }
    .maximize-button {
        background-color: #2ac940;
        border-color: #2bac2d;
        border-width: 1px;
        border-style: solid;
        border-radius: 9999px;
        padding: 0;
        width: 12px;
        height: 12px;
        cursor: pointer;
        display: inline;
        z-index: 10;
        margin-bottom: 0.5rem;
        margin: 0.25rem;
    }
    .maximize-button:active {
        background-color: #1d9730;
    }
    .top-bar {
        background-image: linear-gradient(180deg,#e7e6e7,#d1d0d1);
        box-shadow: inset 0 0 0 0 hsla(0,0%,100%,.6),inset 0 0 0 0 rgba(0,0,0,.2);
        width: 100%;
        border-top-right-radius: 5px;
        border-top-left-radius: 5px;
        top: 5px;
        bottom: 5px;
        left: 6px;
        padding-top: 0.25rem;
        --tw-bg-opacity: 1;
        background-color: rgb(107 114 128 / var(--tw-bg-opacity));
        display: flex;


    }
    .window-shadow {
        box-shadow: 0 0 1px 0 rgba(0,0,0,.9),0 20px 30px 0 rgba(0,0,0,.3),0 10px 50px 0 rgba(0,0,0,.2)
    }
    .resizable {
        resize:both;
        overflow:auto;
    }
    *::-webkit-resizer {
        background-color: transparent
    }
    .window {
        border-radius: 5px;
        background-color: #ececec;
        overflow-wrap: break-word;
        max-width: 100vw;
        min-width: 300px;
        min-height: fit-content;
        position: absolute;
        cursor: grab;
        font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif !important;
    }
</style>
<script>
 import { onMount } from 'svelte';
 import {flip} from 'svelte/animate';
 import {crossfade} from 'svelte/transition';
 const [send, receive] = crossfade({});

 const people = ['Marcel', 'Petr', 'Karsten', 'Darya', 'Steven', 'Sebastian']
 var params;
 var knownOpts;
 var available = $state([]);
 var present = $state([]);
 var iframeBaseUrl="https://wheel.agilator.cz/?items="
 var iframeSrc = $derived(iframeBaseUrl + present.join(","));
 onMount(() => {
     params = new URLSearchParams(window.location.search);
     knownOpts = params.get("knownOptions").split(",");
     present.push(...knownOpts);
     console.log(knownOpts);
 });

 function move(item, from, to) {
     to.push(item);
     return [from.filter(i => i !== item), to];
 }

 function moveLeft(item) {
     [present, available] = move(item, present, available);
 }

 function moveRight(item) {
     [available, present] = move(item, available, present);
 }



</script>
<div class="list">
    <h1>Available options:</h1>
    {#each available as item (item)}
        <button
            animate:flip
			in:receive={{key: item}}
			out:send={{key: item}}
			on:click={() => moveRight(item)}
            >{item}</button>
    {/each}
</div>
<div class="list">
    <h1>The selected subset:</h1>
    {#each present as item (item)}
        <button
            animate:flip
			in:receive={{key: item}}
			out:send={{key: item}}
			on:click={() => moveLeft(item)}
            >{item}</button>
    {/each}
</div>
<!-- <iframe title="The mighty wheel"
     id="theiframe"
     src="{iframeSrc}"></iframe> -->
<div id="link"><a href="{iframeSrc}" target="_blank">{iframeSrc}</a></div>
<style>
 button {
	 background-color: cornflowerblue;
	 border: none;
	 color: white;
	 padding: 10px;
	 margin-bottom: 10px;
	 width: 100%;
 }

 .list {
	 display: inline-block;
	 margin-right: 30px;
	 vertical-align: top;
	 width: 170px;
     height: 500px;
 }
 #theiframe {
     width: 900px;
     height:900px;

 }

 #link {
     position: absolute;
     bottom: 30px;
 }
</style>

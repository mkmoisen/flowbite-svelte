<script lang="ts">
  import Input from './Input.svelte';

  export let value: number = 0;
  export let element: HTMLInputElement | undefined = undefined;
</script>

<Input let:props {...$$restProps}>
  <input {...props} type="number" bind:value bind:this={element} on:blur on:change on:click on:contextmenu on:focus on:keydown on:keypress on:keyup on:mouseover on:mouseenter on:mouseleave on:paste on:input />
</Input>

<!--
@component
[Go to docs](https://flowbite-svelte.com/)
## Props
@prop export let value: number = 0;
@prop export let element: HTMLInputElement | undefined = undefined;
-->

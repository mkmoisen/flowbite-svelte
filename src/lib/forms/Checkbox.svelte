<script lang="ts">
  import { twMerge } from 'tailwind-merge';
  import { getContext } from 'svelte';
  import type { FormColorType } from '../types';
  import { labelClass, inputClass } from './Radio.svelte';
  import Label from './Label.svelte';

  // properties forwarding
  export let color: FormColorType = 'primary';
  export let custom: boolean = false;
  export let inline: boolean = false;
  export let group: (string | number)[] = [];
  export let value: string | number = 'on';
  export let checked: boolean | undefined = undefined;
  export let spacing: string = 'me-2';
  export let element: HTMLInputElement | undefined = undefined;

  // tinted if put in component having its own background
  let background: boolean = getContext('background');

  // react on external group changes
  function init(_: HTMLElement, _group: (string | number)[]) {
    if (checked === undefined) checked = _group.includes(value);
    onChange();

    return {
      update(_group: (string | number)[]) {
        checked = _group.includes(value);
      }
    };
  }

  function onChange() {
    // There's a bug in Svelte and bind:group is not working with wrapped checkbox
    // This workaround is taken from:
    // https://svelte.dev/repl/de117399559f4e7e9e14e2fc9ab243cc?version=3.12.1
    const index = group.indexOf(value);

    if (checked === undefined) checked = index >= 0;

    if (checked) {
      if (index < 0) {
        group.push(value);
        group = group;
      }
    } else {
      if (index >= 0) {
        group.splice(index, 1);
        group = group;
      }
    }
  }
</script>

<Label class={labelClass(inline, $$props.class)} show={$$slots.default}>
  <input use:init={group} type="checkbox" bind:checked bind:this={element} on:keyup on:keydown on:keypress on:focus on:blur on:click on:mouseover on:mouseenter on:mouseleave on:paste on:change={onChange} on:change {value} {...$$restProps} class={inputClass(custom, color, true, background, spacing, $$slots.default || $$props.class)} />
  <slot />
</Label>

<!--
@component
[Go to docs](https://flowbite-svelte.com/)
## Props
@prop export let color: FormColorType = 'primary';
@prop export let custom: boolean = false;
@prop export let inline: boolean = false;
@prop export let group: (string | number)[] = [];
@prop export let value: string | number = 'on';
@prop export let checked: boolean | undefined = undefined;
@prop export let spacing: string = 'me-2';
@prop export let element: HTMLInputElement | undefined = undefined;
-->

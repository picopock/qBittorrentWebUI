<script lang="ts">
  import { noop } from '$lib/utils/noop';
  export let name = '';
  export let onClick = noop;

  $: className = `svg-icon ${name ? 'svg-icon-' + name : ''}`;
</script>

<svg class={className} on:click={onClick}>
  <use xlink:href={name} />
</svg>

<style lang="scss">
  .svg-icon {
    width: 1em;
    height: 1em;
    overflow: hidden;
    vertical-align: -0.15em;
    fill: currentColor;
  }
</style>

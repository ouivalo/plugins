<script>
  import { getContext } from "svelte"

  export let title
  export let meta_description

  const { styleable } = getContext("sdk")
  const component = getContext("component")
</script>

<svelte:head>
  <meta name="description" content={meta_description}>
  <title>{title}</title>
</svelte:head>

<div use:styleable={$component.styles}>
  <slot />
</div>

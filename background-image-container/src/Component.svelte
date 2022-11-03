<script>
  import { getContext } from "svelte"

  export let url
  export let size
  export let position
  export let repeat

  const { styleable } = getContext("sdk")
  const component = getContext("component")

</script>

<div use:styleable={$component.styles} class="background-image-container" >
  <div class="background-image" style="background-image: url({url}); background-size:{size}; background-position: {position}; background-repeat:{repeat ? "repeat" : "no-repeat"}"/>
  <div style="position:relative"><slot/></div>
</div>

<style>
  .background-image-container {
    height: 100%;
    width:100%;
    position : relative
  }

  .background-image {
    height: 100%;
    width: 100%;
    position: absolute;
  }
</style>

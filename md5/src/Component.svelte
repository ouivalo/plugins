<script>
  import { getContext } from "svelte"
  import md5 from "md5"

  export let secret

  const { styleable, Provider  } = getContext("sdk")
  const component = getContext("component")

  let token = md5('ouivalo'+secret+Math.floor(Math.random() * 10000)).toString();
  $: dataContext = {
    token
  }
</script>

<div use:styleable={$component.styles}>
  <Provider data={dataContext}>
    <slot />
  </Provider>
</div>

<script>
  import { getContext } from "svelte"

  import H1 from './h1.svelte';
  import H2 from './h2.svelte';
  import H3 from './h3.svelte';
  import H4 from './h4.svelte';
  import H5 from './h5.svelte';
  import H6 from './h6.svelte';
  import P from './p.svelte';
  import Span from './span.svelte';

  export let text
  export let type
  export let color
  export let alignment
  export let bold
  export let italic
  export let underline
  export let size
  
  let sizeClass = `font-size-${size || "m"}`
  let alignClass = `align--${alignment || "initial"}`

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  // Add color styles to main styles object, otherwise the styleable helper
  // overrides the color when it's passed as inline style.
  let styles = color ? {...$component.styles,normal: {...$component.styles?.normal,color}} : styles

</script>


<div 
  use:styleable={styles}
  class:bold="{!!bold}"
  class:italic="{!!italic}"
  class:underline="{!!underline}"
  class="custom-text {sizeClass} {alignClass}"
>
  {#if type === 'h1'} <H1 text={text}><slot/></H1>
  {:else if type=== 'h2'} <H2 text={text}><slot/></H2>
  {:else if type === 'h3'} <H3 text={text}><slot/></H3>
  {:else if type === 'h4'} <H4 text={text}><slot/></H4>
  {:else if type === 'h5'} <H5 text={text}><slot/></H5>
  {:else if type === 'h6'} <H6 text={text}><slot/></H6>
  {:else if type === 'p'} <P text={text}><slot/></P>
  {:else if type === 'span'} <Span text={text}><slot/></Span>
  {/if}
</div>

<style>
  .custom-text {
    font-weight: 600;
    display: inline;
  }
  .bold {
    font-weight: 700;
  }
  .italic {
    font-style: italic;
  }
  .underline {
    text-decoration: underline;
  }
  .align--initial {
    text-align: initial;
  }
  .align--left {
    text-align: left;
  }
  .align--center {
    text-align: center;
  }
  .align--right {
    text-align: right;
  }
  .align-justify {
    text-align: justify;
  }
  .font-size-xs{
    font-size: var(--heading-font-size-xs)
  }
  .font-size-s{
    font-size: var(--heading-font-size-s)
  }
  .font-size-m{
    font-size: var(--heading-font-size-m)
  }
  .font-size-l{
    font-size: var(--heading-font-size-l)
  }
  .font-size-xl{
    font-size: var(--heading-font-size-xl);
  }
  .font-size-2xl{
    font-size: 3.5rem;
  }
  .font-size-3xl{
    font-size: 4rem;
  }
</style>
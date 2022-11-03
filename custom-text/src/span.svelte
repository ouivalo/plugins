<script>
  export let text
</script>
<span>{text}<slot/></span>

<style>
  span{
    white-space: pre-wrap;
    font-weight:inherit;
    font-size: inherit;
    margin : 0;
  }
</style>
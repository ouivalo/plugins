<script>
  export let text
</script>
<h3>{text}<slot/></h3>

<style>
  h3{
    white-space: pre-wrap;
    font-weight:inherit;
    font-size: inherit;
    margin : 0;
}
</style>
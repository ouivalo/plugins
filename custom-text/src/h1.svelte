<script>
  export let text
</script>
<h1>{text}<slot/></h1>

<style>
  h1{
    white-space: pre-wrap;
    font-weight:inherit;
    font-size: inherit;
    margin : 0;
}
</style>

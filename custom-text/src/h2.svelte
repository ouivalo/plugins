<script>
  export let text
</script>
<h2>{text}<slot/></h2>

<style>
  h2{
    white-space: pre-wrap;
    font-weight:inherit;
    font-size: inherit;
    margin : 0;
}
</style>

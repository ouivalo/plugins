<script>
  export let text
</script>
<h4>{text}<slot/></h4>

<style>
  h4{
    white-space: pre-wrap;
    font-weight:inherit;
    font-size: inherit;
    margin : 0;
}
</style>
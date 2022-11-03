<script>
  export let text
</script>
<h6>{text}<slot/></h6>

<style>
  h6{
    white-space: pre-wrap;
    font-weight:inherit;
    font-size: inherit;
    margin : 0;
}
</style>
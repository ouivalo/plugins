<script>
  export let text
</script>
<h5>{text}<slot/></h5>

<style>
  h5{
    white-space: pre-wrap;
    font-weight:inherit;
    font-size: inherit;
    margin : 0;
}
</style>
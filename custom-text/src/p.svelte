<script>
  export let text
</script>
<p>{text}<slot/></p>

<style>
  p{
    white-space: pre-wrap;
    font-weight:inherit;
    font-size: inherit;
    margin : 0;
}
</style>
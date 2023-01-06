<script>
  import { getContext } from "svelte"
  import CryptoJS from "crypto-js"

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  export let vads_trans_id
  export let vads_order_id

  //mdp carte : 1356
  function complete(value){
    return value < 10 ? '0'+value : value.toString()
  }

  let now = new Date(Date.now())
  const AAAAMMJJHHMMSS = now.getUTCFullYear()+complete(now.getUTCMonth()+1)+complete(now.getUTCDate())+complete(now.getUTCHours())+complete(now.getUTCMinutes())+complete(now.getUTCSeconds())
  const data = {
    vads_action_mode : 'INTERACTIVE',
    vads_amount : 100,
    vads_capture_delay : 0,
    vads_ctx_mode : "PRODUCTION",
    vads_currency : 978,
    vads_order_id : vads_order_id,
    vads_page_action : "PAYMENT",
    vads_payment_cards : "CB",
    vads_payment_config : "SINGLE",
    vads_site_id : 29747086,
    vads_trans_date : AAAAMMJJHHMMSS,
    vads_trans_id : vads_trans_id,
    vads_version : "V2",
  }
  const secret = 'LyOuEbpRWMuYHF2l'

  let message = ""
  let datakeys = Object.getOwnPropertyNames(data).sort()
  for(let keys of datakeys)
    message += data[keys] + '+'

  message += secret

  let message8 = CryptoJS.enc.Utf8.parse(message)
  let secret8 = CryptoJS.enc.Utf8.parse(secret)

  const signature = CryptoJS.enc.Base64.stringify(CryptoJS.HmacSHA256(message8,secret8))

  console.log(message,message8)
  console.log(secret,secret8)
  console.log(signature)
  
</script>

<div use:styleable={$component.styles}>
  <iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="300"
    height="200"
    src="https://www.openstreetmap.org/export/embed.html?bbox=-0.004017949104309083%2C51.47612752641776%2C0.00030577182769775396%2C51.478569861898606&layer=mapnik">
  </iframe>
  <form method="POST" action="https://sogecommerce.societegenerale.eu/vads-payment/">
    <input type="hidden" name="vads_action_mode" value="{data.vads_action_mode}" />
    <input type="hidden" name="vads_amount" value="{data.vads_amount}" />
    <input type="hidden" name="vads_capture_delay" value="{data.vads_capture_delay}" />
    <input type="hidden" name="vads_ctx_mode" value="{data.vads_ctx_mode}" />
    <input type="hidden" name="vads_currency" value="{data.vads_currency}" />
    <input type="hidden" name="vads_order_id" value="{data.vads_order_id}" />
    <input type="hidden" name="vads_page_action" value="{data.vads_page_action}" />
    <input type="hidden" name="vads_payment_cards" value="{data.vads_payment_cards}" />
    <input type="hidden" name="vads_payment_config" value="{data.vads_payment_config}" />
    <input type="hidden" name="vads_site_id" value="{data.vads_site_id}" />
    <input type="hidden" name="vads_trans_date" value="{data.vads_trans_date}" />
    <input type="hidden" name="vads_trans_id" value="{data.vads_trans_id}" />
    <input type="hidden" name="vads_version" value="{data.vads_version}" />
    <input type="hidden" name="signature" value="{signature}"/>
    <input type="submit" name="payer" value="Payer"/>
  </form>
</div>

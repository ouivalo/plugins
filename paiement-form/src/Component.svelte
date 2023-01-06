<script>
  import { getContext } from "svelte"
  import CryptoJS from "crypto-js"

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  //mdp carte : 1356
  const data = {
    vads_action_mode : 'INTERACTIVE',
    vads_amount : 5124, //100
    //vads_capture_delay : 0,
    vads_ctx_mode : "TEST", //"PRODUCTION"
    vads_currency : 978,
    //vads_order_id : "CX-1254",
    vads_page_action : "PAYMENT",
    //vads_payment_cards : "CB",
    vads_payment_config : "SINGLE",
    vads_site_id : 12345678, //29747086
    vads_trans_date : 20170129130025,
    vads_trans_id : "123456",
    vads_version : "V2",
  }
  const secret = "1122334455667788" //'o7rMBE2Z2UgVWmxE6iLjzc9xS1RCzCS7L7IZqtitGJg47'

  let message = ""
  let datakeys = Object.getOwnPropertyNames(data).sort()
  for(let keys of datakeys)
    message += data[keys] + '+'

  message += secret

  let message8 = CryptoJS.enc.Utf8.parse(message)
  let secret8 = CryptoJS.enc.Utf8.parse(secret)

  const signature = CryptoJS.enc.Base64.stringify(CryptoJS.HmacSHA256(message8,secret8))
  
</script>

<div use:styleable={$component.styles}>
  <iframe id="inlineFrameExample"
    title="Inline Frame Example"
    width="300"
    height="200"
    src="https://www.openstreetmap.org/export/embed.html?bbox=-0.004017949104309083%2C51.47612752641776%2C0.00030577182769775396%2C51.478569861898606&layer=mapnik">
  </iframe>
  <form method="POST" action="https://sogecommerce.societegenerale.eu/vads-payment/">
    <input type="hidden" name="vads_action_mode" value="INTERACTIVE" />
    <input type="hidden" name="vads_amount" value="100" />
    <input type="hidden" name="vads_capture_delay" value="0" />
    <input type="hidden" name="vads_ctx_mode" value="PRODUCTION" />
    <input type="hidden" name="vads_currency" value="978" />
    <input type="hidden" name="vads_order_id" value="CX-1254" />
    <input type="hidden" name="vads_page_action" value="PAYMENT" />
    <input type="hidden" name="vads_payment_cards" value="CB" />
    <input type="hidden" name="vads_payment_config" value="SINGLE" />
    <input type="hidden" name="vads_site_id" value="29747086" />
    <input type="hidden" name="vads_trans_date" value="20190626101407" />
    <input type="hidden" name="vads_trans_id" value="pt156G" />
    <input type="hidden" name="vads_version" value="V2" />
    <input type="hidden" name="signature" value="{signature}"/>
    <input type="submit" name="payer" value="Payer"/>
  </form>
</div>

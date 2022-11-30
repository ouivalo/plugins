<script>
  import { getContext } from "svelte"
  //import KRGlue from "@lyracom/embedded-form-glue"; -> uninstall this

  const triggerUrl = "https://mortimer.budibase.app/api/webhooks/trigger/app_dev_mortimer_91e341f02b624a8b9f1d8fb5d3b42199/wh_58b80c04b51348178c20af48ff9ec714"

  export let amount
  export let orderId
  export let paymentMethodToken
  export let formAction
  export let customer_reference
  export let customer_email
  export let customer_billingDetails_firstName

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  const options = {
    method : 'POST',
    headers : {
      accept: 'application/json'
    },
    body : {
      amount:amount,
      orderId:orderId,
      paymentMethodToken:paymentMethodToken,
      formAction:formAction,
      customer_reference:customer_reference,
      customer_email:customer_email,
      customer_billingDetails_firstName:customer_billingDetails_firstName
    }
  }


  fetch(triggerUrl,options)
  .then(response => { console.log(response) })
 /*
  const options = {
    method : 'POST',
    headers : {
      accept: 'application/json',
      Authorization : ''
    },
  }

  const body = {
    currency : currency,
    subMerchantDetails : { name : subMerchantDetails_name },
  }

  switch(formAction) {
    case "REGISTER_PAY" : 
      body.formAction = formAction ;
    case "PAYMENT" : 
      body.amount = amount ;
      body.orderId = orderId;
      body.customer.reference = customer_reference;
      body.customer.email = customer_email;
      body.customer.billingDetails.firstName = customer_billingDetails_firstName;
    break ;
    case "SILENT" :
      body.amount = amount ;
      body.formAction = formAction ;
      body.paymentMethodToken = paymentMethodToken
    break ;
    case "CREATE_ALIAS" : 
      body.orderId = orderId;
      body.customer.reference = customer_reference;
      body.customer.email = customer_email;
      body.customer.billingDetails.firstName = customer_billingDetails_firstName;
    break ;
    default : console.log("erreur formAtcion invalid")
  }

  if(formAction=="CREATE_ALIAS"){
    fetch('https://api.lyra.com/api-payment/V4/Charge/CreateToken', {...options, body : JSON.stringify(body) })
    .then(response => console.log(response))
    .then(reponse => { 
      if(reponse.status == "SUCCESS"){
        let paymentMethodToken = transactions[0].paymentMethodToken
        console.log('alias :',paymentMethodToken)
      }
      else 
        console.log("erreur lors de l'initialisation du paiement")
    })
  }
  else{
    fetch('https://api.lyra.com/api-payment/V4/Charge/CreatePayment', {...options, body : JSON.stringify(body) })
    .then(response => console.log(response))
    .then(reponse => { 
      if(reponse.status == "SUCCESS"){
        let formToken = reponse.answer.formToken
        console.log('formToken :',formToken)
      }
      else 
        console.log("erreur lors de l'initialisation du paiement")
    })
  }
  */
</script>

<svelte:head>
  <link rel="stylesheet" href="https://api-sogecommerce.societegenerale.eu/static/js/krypton-client/V4.0/ext/material-reset.css">
  <script src="https://api-sogecommerce.societegenerale.eu/static/js/krypton-client/V4.0/ext/material.js"></script>
</svelte:head>

<div use:styleable={$component.styles}>
  <div id="myPaymentForm" class="kr-embedded" style="position: relative;">
    <div class="lds-ellipsis"><div></div><div></div><div></div><div></div></div>
    <div class="kr-pan"></div>
    <div class="kr-expiry"></div>
    <div class="kr-security-code"></div>
    <button class="kr-payment-button"></button>
    <div class="kr-form-error"></div>
</div>
</div>

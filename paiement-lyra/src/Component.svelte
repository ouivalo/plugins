<svelte:head>
  <meta http-equiv="Content-Security-Policy" content="
  default-src 'self';
  script-src 'report-sample' 'self' https://api-sogecommerce.societegenerale.eu/static/js/krypton-client/V4.0/ext/material.js https://cdn.budi.live/app_mortimer_91e341f02b624a8b9f1d8fb5d3b42199/budibase-client.js;
  style-src 'report-sample' 'self' https://api-sogecommerce.societegenerale.eu https://cdn.jsdelivr.net https://fonts.googleapis.com https://rsms.me;
  object-src 'none';
  base-uri 'self';
  connect-src 'report-sample' 'self' https://app.ouivalo.fr https://api-iam.intercom.io https://api-iam.intercom.io  https://api-ping.intercom.io https://app.posthog.com wss://nexus-websocket-a.intercom.io  wss://nexus-websocket-b.intercom.io https://nexus-websocket-a.intercom.io https://nexus-websocket-b.intercom.io  https://uploads.intercomcdn.com  https://uploads.intercomusercontent.com https://*.s3.amazonaws.com https://*.s3.us-east-2.amazonaws.com https://*.s3.us-east-1.amazonaws.com https://*.s3.us-west-1.amazonaws.com https://*.s3.us-west-2.amazonaws.com https://*.s3.af-south-1.amazonaws.com https://*.s3.ap-east-1.amazonaws.com https://*.s3.ap-southeast-3.amazonaws.com https://*.s3.ap-south-1.amazonaws.com https://*.s3.ap-northeast-3.amazonaws.com https://*.s3.ap-northeast-2.amazonaws.com https://*.s3.ap-southeast-1.amazonaws.com https://*.s3.ap-southeast-2.amazonaws.com https://*.s3.ap-northeast-1.amazonaws.com https://*.s3.ca-central-1.amazonaws.com https://*.s3.cn-north-1.amazonaws.com https://*.s3.cn-northwest-1.amazonaws.com https://*.s3.eu-central-1.amazonaws.com https://*.s3.eu-west-1.amazonaws.com https://*.s3.eu-west-2.amazonaws.com https://*.s3.eu-south-1.amazonaws.com https://*.s3.eu-west-3.amazonaws.com https://*.s3.eu-north-1.amazonaws.com https://*.s3.sa-east-1.amazonaws.com https://*.s3.me-south-1.amazonaws.com https://*.s3.us-gov-east-1.amazonaws.com https://*.s3.us-gov-west-1.amazonaws.com;
  font-src 'report-sample' 'self' https://cdn.jsdelivr.net https://fonts.gstatic.com https://rsms.me;
  frame-src 'self';
  img-src 'report-sample' 'self' https://i.imgur.com https://ouivalo.fr;
  manifest-src 'self';
  media-src 'self';
  report-uri https://63878083158758247890ab01.endpoint.csper.io/?v=0;
  worker-src 'none';">
  <link rel="stylesheet" href="https://api-sogecommerce.societegenerale.eu/static/js/krypton-client/V4.0/ext/material-reset.css">
  <script src="https://api-sogecommerce.societegenerale.eu/static/js/krypton-client/V4.0/ext/material.js"></script>
</svelte:head>

<script>
  import { getContext } from "svelte"
  import KRGlue from "@lyracom/embedded-form-glue";

  //const currency = "EUR"
  //const subMerchantDetails_name = "OUIVALO"
  const urlSoge = "https://app.ouivalo.fr/support/paiement/soge"
  const endpoint = "https://api-sogecommerce.societegenerale.eu/";
  const publicKey = "29747086:publickey_gN0U2uZxAOeCW2YvjLlLBAS6ECnMkfUpFohH9zmL54zLG";

  export let amount
  export let orderId
  export let paymentMethodToken
  export let formAction
  export let customer_reference
  export let customer_email
  export let customer_billingDetails_firstName

  const { styleable } = getContext("sdk")
  const component = getContext("component")
 
 creerForm(orderId,customer_email,customer_billingDetails_firstName,paymentMethodToken,formAction=="REGISTER_PAY"||formAction=="CREATE_ALIAS",amount,amount,amount)
  .then( data => {
        const formToken = data.substr(1,data.length -2 );
        KRGlue.loadLibrary(endpoint,publicKey)// Load the remote library
           .then(({ KR }) => {
              try{
                KR.setFormConfig({
                  // set the minimal configuration
                  formToken: formToken,
                  "kr-language": "fr-FR" // to update initialization parameter
                })
                .then(({ KR }) =>
                  KR.addForm("#myPaymentForm"), //ajoute et lie le form au html
                )
                .then(({ KR, result }) =>
                  KR.showForm(result.formId) // l'affiche
                )
                .catch(
                  ({KR, result}) => {
                    console.log('probleme formulaire',result); 
                  }
                );
                KR.onFormReady(() => {
                });
                KR.onError( (KRError) => {
                  if(KRError.errorCode == '100' ){
                    KRError.errorMessage = "Veuillez recharger la page"
                  }
                });
                KR.onSubmit((result )=>{
                  if (result.clientAnswer.orderStatus === "PAID") {
                    KR.removeForms();

                    // Si besoin d'un abonnement
                    if(formAction=="REGISTER_PAY"||formAction=="CREATE_ALIAS"){
                      let token = result.clientAnswer.transactions[0].paymentMethodToken;
                      creerAbo(orderId,customer_email,token,amount)
                      .then(
                        data => {

                          //ok abonnement reçu
                          console.log("abo reçu");
                          //todo => que faire une fois paiement réussie

                        },
                        error => {
                          console.log("erreur init paiement");
                          //todo => que faire une fois paiement échoué
                        }
                      );

                    }else{
                      // question suivante
                      console.log("page suivante")
                      //todo => que faire une fois paiement réussie

                    }
                  } else {
                    console.log('erreur de paiement',result.clientAnswer.orderStatus);
                    //todo => que faire une fois paiement échoué
                  }
                });
              }catch(error){
                console.log('probleme de KR',error);
                //todo => que faire une fois paiement échoué
              }
            })
            .catch(
              ({KR, result}) => {
                console.log('probleme load librairie',result);
                //todo => que faire une fois paiement échoué
              }
            )
      },
      error => {
        console.log("erreur init paiement");
       //todo => que faire une fois paiement échoué
      }
  );

  function creerForm(idCommande,email,nom,alias,abonnement,montantRef,montant,montantAbo){
    let url =  urlSoge  +"/creer-formulaire/";

    var json = '{"cle":"'+calculToken()+'"';
    json += ',"informations": {"idCommande":"'+idCommande+'","email":"'+email+'","nom":"'+nom+'", "montant":"'+montant+'", "alias":"'+alias+'", "abonnement":"'+abonnement+'", "montantRef":"'+montantRef+'", "montantAbo":"'+montantAbo+'"}';
    json+='}';

    return fetch(url, {method : 'POST', headers : {responseType: 'text'}, body : JSON.stringify(json) })

  }
  function creerAbo(idCommande,email,token,montant){
    let url = urlSoge + "/creer-abonnement/";

    var json = '{"cle":"'+calculToken()+'"';
    json += ',"informations": {"email":"'+email+'", "idCommande":"'+idCommande+'", "token":"'+token+'", "montant":"'+montant+'"}';
    json +='}';

    return fetch(url, {method : 'POST', headers : {responseType: 'text'}, body : JSON.stringify(json) })

  }
 function calculToken(){
    var cle_API = "";
    var user = "OUI_APP";
    var pw = "#OUIValo1356K";
    var fin = "BLAOKI";
    var date = new Date();
    var nb1=Math.trunc(2*3.14*(date.getMonth()+1)+4);
    var nb2=Math.trunc(2*(date.getDate())+13.56);
    
    cle_API=user+nb1+":"+nb2+pw+fin;
    
    let cle_API_codee=window.btoa(unescape(encodeURIComponent( cle_API )))
    
    return cle_API_codee;
  }
</script>

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

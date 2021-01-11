## Razorpay-integration-in-PHP-LARAVEL
<h6>Razorpay integration steps</h6>
<ol><li><strong>Generate API Keys</strong></li><li><strong>Install Razorpay library</strong></li><li><strong>Create web routes</strong></li><li><strong>Make payment page</strong></li><li><strong>New Stylesheet file</strong></li><li><strong>Make Payment Controller</strong></li><li><strong>Create Checkout blade page</strong></li><li><strong>Create functions for transaction verification</strong></li></ol>
<p><strong>1 . Generate API Keys</strong></p>
Log into your <a href="https://dashboard.razorpay.com/app/dashboard">Dashboard</a> with appropriate credentials. Select the mode (Test or Live) for which you want to generate the API key.

Navigate to **Settings** → **API Keys** → Generate Key to generate the key for the selected mode.

The <code>Key Id</code> and <code>Key Secret</code> appear in a pop-out window as shown below:

<img loading="lazy" width="600" height="338" src="http://codepickup.in/wp-content/uploads/2020/06/generate-api-keys-1.gif.pagespeed.ce.dBt8lz5lsB.gif" alt="" class="wp-image-159" data-pagespeed-url-hash="3357266679" onload="pagespeed.CriticalImages.checkImageForCriticality(this);">

#### 2 . Install Razorpay library

Razorpay provides a library for developers for creating an order. Install the library using the below command in the terminal.

```composer require razorpay/razorpay:2.*```

<p>If you want to install the latest library of Razorpay. </p>

<p>You can check here – <a href="https://razorpay.com/docs/server-integration/php/" target="_blank" rel="noreferrer noopener">click here</a></p>

#### 3 . Create web routes

Create routes for payment page initiate, initiate order, and complete order. Create web routes under routes → **web.php** file.

```php
// This route is for payment initiate page
Route::get('/payment-initiate',function(){
    return view('payment-initiate');
});

// for Initiate the order
Route::post('/payment-initiate-request','PaymentController@Initiate');

// for Payment complete
Route::post('/payment-complete','PaymentController@Complete');
```


#### 4 . Make payment page

Make payment page where user fill their details which use for creating an order. In our demo, We put Username, Email, Contact Number, Address and Amount fields in payment form. You can use fields as per your need in your website.

Create payment page under **resources → views → payment-initiate.blade.php** and copy the below code in your payment page.

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">

        <title>Payment</title>
        
        <!-- Add stylesheet -->
        <link href="{{asset('css/style.css')}}" rel="stylesheet">
    </head>
    <body>
        <div class="container login-container">
            <div class="login-card">
                <div class="login-card-body">
                    <form action="{{url('/payment-initiate-request')}}" method="POST">
                        <div class="row">
                            <input type="hidden" value="{{csrf_token()}}" name="_token" />
                            <label for="name">Name</label> : 
                            <input type="text" class="form-control" id="name"  name="name">
                        </div></br>
                        <div class="row">
                            <label for="email">Email</label> : 
                            <input type="text" class="form-control" id="email" name="email">
                        </div></br>
                        <div class="row">
                            <label for="contactNumber">Contact Number</label> : 
                            <input type="text" class="form-control" id="contactNumber" name="contactNumber">
                        </div></br>
                        <div class="row">
                            <label for="address">Address</label> : 
                            <input type="text" class="form-control" id="address" name="address">
                        </div></br>
                        <div class="row">
                            <label for="amount">Amount</label> : 
                            <input type="text" class="form-control" id="amount" name="amount">
                        </div></br>
                        <button type="submit" class="btn btn-primary">Submit</button>
                    </form>                    
                </div>
            </div>
        </div>
    </body>
</html>
```

<p><strong>5 . New Stylesheet file</strong></p>

<p><strong> </strong>Make a new stylesheet file for payment page look nice.</p>

<p>Copy below css code and paste it in <strong>public →</strong> <strong>css →</strong> <strong>style.css</strong> file.</p>

```css
body{
    color: #2f2d2d;
    font-family: 'Open Sans', sans-serif !important;
    font-size: 14px;
    font-weight: 400;
    line-height: 1.2;
    letter-spacing: .25px;
    overflow: hidden;
    overflow-y: auto;
    -webkit-font-smoothing: antialiased;
    background-color: #fafbfe}
*, *:after, *:before {
    outline: none !important;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    -ms-box-sizing: border-box;
    -o-box-sizing: border-box;
    box-sizing: border-box }
.clear-fix,.clear-fix:before,.clear-fix:after,.container:before,.container:after,.row:before,.row:after,.centered:before,.centered:after{
    content: "";
    display: table;
    clear: both }
html,body,div,span,h1,h2,h3,h4,h5,h6,p,a,img,b,ul,li,form,label,nav,section,menu{border: 0;margin: 0;padding: 0;vertical-align: baseline}
article,aside,details,figcaption,figure,footer,header,hgroup,main,menu,nav,section,summary{display: block}
a{cursor: pointer;text-decoration: none}
.container{
    width: 100%;
    max-width: 1090px;
    margin: 0 auto;
    float: none !important}
/* Input */
input{
    -webkit-appearance: none;
    -moz-appearance: none;
    -ms-appearance: none;
    -o-appearance: none;
    appearance: none;
    width: 100%;
    height: 36px;
    color: #2f2d2d;
    font-family: inherit;
    font-size: 14px;
    line-height: 1.5;
    padding: 7.5px 15px;
    margin: 0;
    margin-bottom: 15px;
    background-color: transparent;
    border: 1px solid #dee2e6;
    border-radius: 4px;
    resize: none;
    box-shadow: none;
    -webkit-transition: border-color 0.15s linear, background 0.15s linear;
    -moz-transition: border-color 0.15s linear, background 0.15s linear;
    -ms-transition: border-color 0.15s linear, background 0.15s linear;
    -o-transition: border-color 0.15s linear, background 0.15s linear;
    transition: border-color 0.15s linear, background 0.15s linear}
input:focus{border-color: #2000cc}
label{
    color: #2f2d2d;
    cursor: pointer;
    max-width: 100%;
    display: inline-block;
    font-size: 14px;
    font-weight: 600;
    line-height: 1.5}
form label{
    float: left;
    font-size: 13px;
    text-align: left;
    margin-bottom: 8px}

/* Button */
button{
    display: inline-block;
    color: #fff;
    font-size: 14px;
    line-height: 1.5;
    padding: 7.5px 15px;
    vertical-align: middle;
    text-align: center;
    background-color: #7512e7;
    border: none;
    border-radius: 4px;
    cursor: pointer}

/* Body */
.row,.login-card,.login-card-body{
    width: 100%;
    float: left}


/* Login */
.login-container{
    max-width: 475px;
    text-align: center;
    padding: 70px 15px 30px}
.login-card{
    overflow: hidden;
    margin-bottom: 30px;
    background-color: #fff;
    border-radius: 4px;
    -webkit-box-shadow: 0 0 35px 0 rgba(154,161,171,.15);
    box-shadow: 0 0 35px 0 rgba(154,161,171,.15)}
.login-card-body{padding: 35px}
.login-title-container{width: 75%}
```

#### 6 . Make Payment Controller

Make a new controller with the name of **PaymentController.php** where we create an order and return a response to initiate the payment.

We save Razorpay Id and Key in controller.

```php
<?php
 
namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Http\Controllers\Controller;
use Razorpay\Api\Api;
use Illuminate\Support\Str;

class PaymentController extends Controller
{
    private $razorpayId = "-- Your razorpay Id --";
    private $razorpayKey = "-- Your razorpay key --";

    public function Initiate(Request $request)
    {
        // Generate random receipt id
        $receiptId = Str::random(20);
        
        // Create an object of razorpay
        $api = new Api($this->razorpayId, $this->razorpayKey);

        // In razorpay you have to convert rupees into paise we multiply by 100
        // Currency will be INR
        // Creating order
        $order = $api->order->create(array(
            'receipt' => $receiptId,
            'amount' => $request->all()['amount'] * 100,
            'currency' => 'INR'
            )
        );

        // Return response on payment page
        $response = [
            'orderId' => $order['id'],
            'razorpayId' => $this->razorpayId,
            'amount' => $request->all()['amount'] * 100,
            'name' => $request->all()['name'],
            'currency' => 'INR',
            'email' => $request->all()['email'],
            'contactNumber' => $request->all()['contactNumber'],
            'address' => $request->all()['address'],
            'description' => 'Testing description',
        ];

        // Let's checkout payment page is it working
        return view('payment-page',compact('response'));
    }
}
```

#### 7 . Create Checkout blade page

On this page, we provide the order details to the Razorpay function to show the transaction page. When payment is done successfully than the handler function holds the payment data and then we send this data to the controller for the payment verification process.

Create a checkout page under **resources → views → payment-page.blade.php** and copy the below code on your payment page.


```php
<!-- // Let's Click this button automatically when this page load using javascript -->
<!-- You can hide this button -->
<button id="rzp-button1" hidden>Pay</button>  
<script src="https://checkout.razorpay.com/v1/checkout.js"></script>
<script>
var options = {
    "key": "{{$response['razorpayId']}}", // Enter the Key ID generated from the Dashboard
    "amount": "{{$response['amount']}}", // Amount is in currency subunits. Default currency is INR. Hence, 50000 refers to 50000 paise
    "currency": "{{$response['currency']}}",
    "name": "{{$response['name']}}",
    "description": "{{$response['description']}}",
    "image": "https://example.com/your_logo", // You can give your logo url
    "order_id": "{{$response['orderId']}}", //This is a sample Order ID. Pass the `id` obtained in the response of Step 1
    "handler": function (response){
        // After payment successfully made response will come here
        // send this response to Controller for update the payment response
        // Create a form for send this data
        // Set the data in form
        document.getElementById('rzp_paymentid').value = response.razorpay_payment_id;
        document.getElementById('rzp_orderid').value = response.razorpay_order_id;
        document.getElementById('rzp_signature').value = response.razorpay_signature;

        // // Let's submit the form automatically
        document.getElementById('rzp-paymentresponse').click();
    },
    "prefill": {
        "name": "{{$response['name']}}",
        "email": "{{$response['email']}}",
        "contact": "{{$response['contactNumber']}}"
    },
    "notes": {
        "address": "{{$response['address']}}"
    },
    "theme": {
        "color": "#F37254"
    }
};
var rzp1 = new Razorpay(options);
window.onload = function(){
    document.getElementById('rzp-button1').click();
};

document.getElementById('rzp-button1').onclick = function(e){
    rzp1.open();
    e.preventDefault();
}
</script>

<!-- This form is hidden -->
<form action="{{url('/payment-complete')}}" method="POST" hidden>
        <input type="hidden" value="{{csrf_token()}}" name="_token" /> 
        <input type="text" class="form-control" id="rzp_paymentid"  name="rzp_paymentid">
        <input type="text" class="form-control" id="rzp_orderid" name="rzp_orderid">
        <input type="text" class="form-control" id="rzp_signature" name="rzp_signature">
    <button type="submit" id="rzp-paymentresponse" class="btn btn-primary">Submit</button>
</form>
```

#### 8 . Create functions for transaction verification

After creating the checkout page. Let’s verify the transaction data which comes through the hander function in checkout page. Razorpay provide payment Id, order Id, and signature. Signature is use for verify the payment id and order id not intercepted in between this process.

Add these two functions in **PaymentController.php** to verify the payment.

```php
public function Complete(Request $request)
{
    // Now verify the signature is correct . We create the private function for verify the signature
    $signatureStatus = $this->SignatureVerify(
        $request->all()['rzp_signature'],
        $request->all()['rzp_paymentid'],
        $request->all()['rzp_orderid']
    );

    // If Signature status is true We will save the payment response in our database
    // In this tutorial we send the response to Success page if payment successfully made
    if($signatureStatus == true)
    {
        // You can create this page
        return view('payment-success-page');
    }
    else{
        // You can create this page
        return view('payment-failed-page');
    }
}

// In this function we return boolean if signature is correct
private function SignatureVerify($_signature,$_paymentId,$_orderId)
{
    try
    {
        // Create an object of razorpay class
        $api = new Api($this->razorpayId, $this->razorpayKey);
        $attributes  = array('razorpay_signature'  => $_signature,  'razorpay_payment_id'  => $_paymentId ,  'razorpay_order_id' => $_orderId);
        $order  = $api->utility->verifyPaymentSignature($attributes);
        return true;
    }
    catch(\Exception $e)
    {
        // If Signature is not correct its give a excetption so we use try catch
        return false;
    }
}
```

In this tutorial, I send the user to success or failed page according to Signature verification. You can change it according to your need.

You can see the process of payment below

<figure class="aligncenter size-large"><img loading="lazy" width="600" height="338" src="http://codepickup.in/wp-content/uploads/2020/06/razorpay-payment.gif.pagespeed.ce.OC9NgMQdBh.gif" alt="" class="wp-image-161" data-pagespeed-url-hash="1214510647" onload="pagespeed.CriticalImages.checkImageForCriticality(this);"></figure>


> youtube video
<a href="https://www.youtube.com/embed/wgkFdniYJCQ?feature=oembed" target="_blank">click here...</a>

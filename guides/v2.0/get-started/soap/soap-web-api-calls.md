---
layout: default
group: get-started
subgroup: 30_SOAP
title: Use SOAP Services
menu_title: Use SOAP Services
menu_order: 1
menu_node: parent
version: 2.0
github_link: get-started/soap/soap-web-api-calls.md
redirect_from:
  - /guides/v1.0/rest/soap/soap-web-api-calls.html
  - /guides/v2.0/get-started/soap/soap-front.html

---


<h2 id="wsdl">WSDL File</h2>

A WSDL file is generated only for services that you request. This means that different clients may use different services and therefore use different WSDLs.

The Magento web API uses WSDL 1.2, which complies with WS-I 2.0 Basic Profile.

Each Magento service interface that is part of a service contract is represented as a separate service in the WSDL.

To consume several services, you must specify them in the WSDL endpoint URL.


<table style="width:100%">
   <colgroup>
      <col width="20%">
      <col width="40%">
      <col width="40%">
   </colgroup>
   <thead>
      <tr>
         <th>Service</th>
         <th>WSDL endpoint URL</th>
         <th>Available services</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>customer</td>
         <td>http://magentohost/soap?wsdl&services=customerV1</td>
         <td>\Magento\Customer\Service\V1\CustomerService</td>
      </tr>
       <tr>
         <td>customer, catalogProduct</td>
         <td>http://magentohost/soap/custom_store?wsdl&services=customerCustomerAccountServiceV1,catalogProductV2</td>
         <td>\Magento\Customer\Service\V1\CustomerAccountServiceInterface, \Magento\Catalog\Service\V2\ProductInterface</td>
      </tr>
   </tbody>
</table>

The WSDL URL follows the following pattern:

`http://<magento.host>/soap/<optional_store_code>?wsdl&services=<service_name_1>,<service_name_2>`

You must specify each service version in the endpoint URL.

This way, you can have a strict contract between your application and the service provider.
<h3>Service class-to-service name conversion rules</h3>

Service names use the following conventions:

* CamelCase is used for service naming.
* The string `Service` is omitted.
* The `Magento` prefix is omitted.
* The `Interface` suffix is omitted.
* If the service name is the same as the module name, the module name is omitted. For example, if there is a customer service interface in the customer module, the word `customer` will be used in the service name only once.

<table>
<thead>
<tr>
<th>Original Service Interface Name</th>
<th>Service Name</th>
</tr>
</thead>
<tbody>
<tr>
<td>\Magento\Customer\Service\V1\CustomerInterface</td>
<td>customerV1</td>
</tr>
<tr>
<td>\Magento\Customer\Service\V1\CustomerAccountServiceInterface </td>
<td>customerCustomerAccountServiceV1</td>
</tr>
<tr>
<td>\Enterprise\Customer\Service\V3\Customer\AddressInterface</td>
<td>enterpriseCustomerAddressV3</td>
</tr>

</tbody>
</table>

<h2 id="auth">Authentication</h2>

Protected SOAP resources can be accessed using bearer tokens (OAuth access tokens) over HTTP. Access tokens are strings representing an access authorization issued to the client. For more information, see <a href="{{page.baseurl}}get-started/authentication/gs-authentication-oauth.html">OAuth-based authentication</a>

The following PHP script illustrates how to get an access token:

{% highlight php %}
<?php
$opts = array(
            'http'=>array(
                'header' => 'Authorization: Bearer 36849300bca4fbff758d93a3379f1b8e'
            )
        );
$wsdlUrl = 'http://magento.ll/soap/default?wsdl=1&services=testModule1AllSoapAndRestV1';
$serviceArgs = array("id"=>1);

$context = stream_context_create($opts);
$soapClient = new SoapClient($wsdlUrl, ['version' => SOAP_1_2, 'context' => $context]);

$soapResponse = $soapClient->testModule1AllSoapAndRestV1Item($serviceArgs); ?>
{% endhighlight %}

<h2 id="related">Related topics</h2>
* <a href="{{page.baseurl}}get-started/authentication/gs-authentication-oauth.html">OAuth-based authentication</a>
* <a href="{{page.baseurl}}extension-dev-guide/service-contracts/service-contracts.html">Service contracts</a>
* <a href="{{page.baseurl}}soap/bk-soap.html">SOAP Reference</a>

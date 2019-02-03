---
layout: post
title:  "Asynchronous Payment Processing in Sterling OMS"
categories: [Payment]
---
There are scenarios where payment system might respond Asynchronously to Authorization or Charge or Refund requests. To accommodate asynchronous processing Sterling OMS Payment processing Userexit supports a flag “asynchRequestProcess”. If this flag is set to true, payment status will move to “Requested_Auth” or “Requested_Charge”. Once the payment processing system responds asynchronously through a webhook/MQ or any other mechanisn, we then can invoke recordExternalCharges API to set the asynchronously Authorized or Captured or Refunded amount.

To enable asynchronous payment processing as part of the CollectionCreditCardUE or CollectionOthersUE set the following attributes in com.yantra.yfs.japi.YFSExtnPaymentCollectionOutputStruct


```
outStruct.asynchRequestProcess=true;
outStruct.authTime=current time;
outStruct.authorizationExpirationDate =current time;
outStruct. tranAmount =0.0;
outStruct.authorizationAmount=0.0;
outStruct.authReturnCode=”APPROVED”;
outStruct.tranType= instruct.chargeType;
outStruct. sCVVAuthCode =”Y”;
```

Once these attributes are set in the output of the Collection outstruct, payment status will then move to “Requested_Auth” or “Requested_Charge” depending the transaction (i.e. Authorization vs Charge/Refund)

Now once we get a webhook response from payment system invoke recordExternalCharges API with the following input

```
<RecordExternalCharges DocumentType="0001" EnterpriseCode="Matrix" OrderNo ="321121112">
	<PaymentMethod  PaymentKey= "paymentKey">
		<PaymentDetailsList>
			<PaymentDetails AuthAvs="E" AuthCode="AUTHORIZED" AuthorizationID ="AuthID" ChargeType="CHARGE" ProcessedAmount="22.33" RequestedAmount="22.33"/>
		</PaymentDetailsList>
	</PaymentMethod>
</RecordExternalCharges>
```

References:

IBM Sterling Support [Link](http://www-01.ibm.com/support/docview.wss?uid=swg21563186)

Sterling Payments Features [Link](ftp://ftp.networking.ibm.com/software/is/b2b/education/August_27_2013_Sterling_Payments1.pdf)


 
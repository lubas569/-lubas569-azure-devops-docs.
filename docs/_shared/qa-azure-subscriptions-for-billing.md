

<a name="which-azure-sub-for-billing"></a>
###Q: Which Azure subscriptions can I link to my VSTS account and use for billing?

A: You can use an [Azure subscription](https://azure.microsoft.com/en-us/pricing/purchase-options/) 
that's not:

*	An [Azure Free Trial](https://azure.microsoft.com/en-us/offers/ms-azr-0044p/)
*	A [free Azure AD subscription](https://technet.microsoft.com/library/dn832618.aspx)

If you have one of these subscriptions, set up a separate 
[Pay-As-You-Go Azure subscription](https://azure.microsoft.com/en-us/offers/ms-azr-0003p/) 
instead. 

**Important for Visual Studio subscribers and Visual Studio Dev Essentials members**:

*	You can't use [monthly credits for Visual Studio subscriptions](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/) 
or [monthly credits for Visual Studio Dev Essentials](https://azure.microsoft.com/en-us/offers/ms-azr-0022p/) to pay for VSTS purchases. 
You can still use these types of Azure subscriptions for billing, 
but the monthly credits won't apply, so your credit card will be billed for charges instead.
 
*	Before you use these Azure subscriptions for billing, you must remove their 
[default spending limit indefinitely](https://azure.microsoft.com/en-us/pricing/spending-limits/).

    This prevents disabling your Azure subscription 
    when your monthly charges are billed the next month. 
    Otherwise, all resources billed to this subscription 
    will be suspended, including VSTS purchases,
    Visual Studio Marketplace purchases, and Azure resources.

    <img alt="Spending limit" src="_img/spending-limit.png" style="border: 1px solid #CCCCCC" />

    <img alt="Remove spending limit indefinitely" src="_img/azure-remove-spending-limit.png" style="border: 1px solid #CCCCCC" />

	If you have Account Administrator permissions for your subscription, 
	remove the spending limit by visiting the 
	[Azure Account Center](https://account.windowsazure.com):
	
	0.	Sign in to [Azure Account Center (**Account** > **subscriptions**)](https://portal.azure.com). 
	0.	Select your Azure subscription. 
	0.	Remove your spending limit **indefinitely**.

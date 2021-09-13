---

copyright:
  years: 2019, 2021
lastupdated: "2021-09-13"

keywords: key management,byok,key encryption,encrypt key,root key

subcollection: watson

---

{{site.data.keyword.attribute-definition-list}}

# Protecting sensitive information in your {{site.data.keyword.watson}} service
{: #keyservice}

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud_notm}} only**

You can add a higher level of encryption protection and control to your data at rest (when it is stored) and data in motion (when it is transported) by enabling integration with {{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

The data that you store in {{site.data.keyword.cloud_notm}} is encrypted at rest by using a randomly generated key. If you need to control the encryption keys, you can integrate {{site.data.keyword.keymanagementserviceshort}}. This process is commonly referred to as _Bring your own keys_ (BYOK). With {{site.data.keyword.keymanagementserviceshort}} you can create, import, and manage encryption keys. You can assign access policies to the keys, assign users or service IDs to the keys, or give the key access only to a specific {{site.data.keyword.watson}} service. The first 20 keys are free.

Data encryption of {{site.data.keyword.watson}} services requires a new Premium plan instance. You cannot encrypt an existing service instance or instances not in the Premium plan. Not all {{site.data.keyword.watson}} services support Premium plans. For more information about Premium plans, [contact a {{site.data.keyword.watson}} representative](https://ibm.biz/contact-wdc-premium)
{: tip}

## About customer-managed encryption
{: #keyservice-about}

{{site.data.keyword.watson}} uses [envelope encryption](#x9860393){: term} to implement customer-managed keys. Envelope encryption describes encrypting one encryption key with another encryption key. The key used to encrypt the actual data is known as a [data encryption key (DEK)](#x4791827){: term}. The DEK itself is never stored but is wrapped by a second key that is known as the key encryption key (KEK) to create a wrapped DEK. To decrypt data, the wrapped DEK is unwrapped to get the DEK. This process is possible only by accessing the KEK, which in this case is your root key that is stored in {{site.data.keyword.keymanagementserviceshort}}.

{{site.data.keyword.keymanagementserviceshort}} keys are secured by FIPS 140-2 Level 3 certified cloud-based [hardware security modules (HSMs)](#x6704988){: term}.

## Enabling customer-managed keys with {{site.data.keyword.watson}}
{: #keyservice-steps}

Some {{site.data.keyword.watson}} services, such as {{site.data.keyword.conversationshort}}, have additional details about how to work with {{site.data.keyword.keymanagementserviceshort}}. For more information, see the [docs](/docs/home/alldocs?category=ai) for the service.
{: important}

Integrating {{site.data.keyword.keymanagementserviceshort}} with {{site.data.keyword.watson}} Premium services involves these steps in the {{site.data.keyword.cloud_notm}} console.

1.  Create an instance of {{site.data.keyword.keymanagementserviceshort}}.
1.  Add a root key to the {{site.data.keyword.keymanagementserviceshort}} instance.
1.  Grant {{site.data.keyword.keymanagementserviceshort}} access to all instances of your {{site.data.keyword.watson}} service.
1.  Encrypt the {{site.data.keyword.watson}} service data

### Step 1. Create the {{site.data.keyword.keymanagementserviceshort}} instance
{: #provision-keyservice}

Create an instance of {{site.data.keyword.keymanagementserviceshort}} to hold your root keys.

1.  Go to the [{{site.data.keyword.keymanagementserviceshort}}](https://{DomainName}/catalog/key-protect){: external} page in the {{site.data.keyword.cloud_notm}} catalog **Security and Identity** category.
1.  Select a region.

    Make sure to create the instance in the same location as the {{site.data.keyword.watson}} services you want to encrypt.
1.  Name the service and click **Create**.

### Step 2. Add a key
{: #add-key}

You use {{site.data.keyword.keymanagementserviceshort}} to generate a key or import your own key.

#### Create a root key
{: #create-key}

When you use {{site.data.keyword.keymanagementserviceshort}} to create a key, the service generates cryptographic key material that is rooted in cloud-based HSMs.

After you create an instance of the service, add a root key.

1.  If you're not already on the details page, click the name of the {{site.data.keyword.keymanagementserviceshort}} instance in your [resource list](https://{DomainName}/resources?groups=resource-instance){: external}.
1.  Add a {{site.data.keyword.keymanagementserviceshort}} [root key](#x6946961){: term}:
    1.  Click **Manage** from the left navigation pane of the service details and click **Add key**.
    1.  Select **Create a key** and the **Root key** [type](/docs/key-protect?topic=key-protect-envelope-encryption#key-types).
    1.  Give the key a name that you can recognize and click **Create key**.

        Make sure that the key name does not contain personal information, such as your name or location.

#### Import a key
{: #import-key}

If you need to generate keys with your own solution, you can use {{site.data.keyword.keymanagementserviceshort}} to secure the keys.

After you create an instance of the service, add a root key.

1.  Click the name of the {{site.data.keyword.keymanagementserviceshort}} instance in your [resource list](https://{DomainName}/resources?groups=resource-instance){: external}.
1.  Import a root key:
    1.  Click **Manage** from the left navigation pane of the service details and click **Add key**.
    1.  Select **Import your own key** the **Root key** type.
    1.  In **Key material**, specify the base64 encoded key material, such as an existing key-wrapping key.

        - The key must be 128, 192, or 256 bits.
        - The bytes of data, for example 32 bytes for 256 bits, must be encoded by using base64 encoding.
    1.  Give the key a name that you can recognize and click **Import key**.

        Make sure that the key name does not contain personal information, such as your name or location.

### Step 3. Grant service access to {{site.data.keyword.keymanagementserviceshort}}
{: #serviceauth-key}

After you add a root key to {{site.data.keyword.keymanagementserviceshort}}, you use {{site.data.keyword.iamshort}} (IAM) to authorize access between the {{site.data.keyword.watson}} service and {{site.data.keyword.keymanagementserviceshort}}.

You must be the account owner or administrator on the {{site.data.keyword.keymanagementserviceshort}} service instance and at least the viewer role on all instances of the {{site.data.keyword.watson}} services.

1.  Go to the IAM [Authorizations](https://{DomainName}/iam/authorizations){: external} page. (From the {{site.data.keyword.cloud_notm}} console menu bar, select **Manage** > **Access IAM**, and then click **Authorizations**.)
1.  Click **Create**.
1.  Select the {{site.data.keyword.watson}} service as the **Source service**. Leave **All resources** selected to scope the access.
1.  Select {{site.data.keyword.keymanagementserviceshort}} as the **Target service**.
1.  Select **Resources based on selected attributes**, and then select **Instance ID** for the target service attribute. Select the {{site.data.keyword.keymanagementserviceshort}} service instance that you created earlier.
1.  Authorize dependent services by selecting **Enable authorization to be delegated**.

    Delegation allows the {{site.data.keyword.keymanagementserviceshort}} instance to propagate its authorizations to this service.
1.  Make sure that the **Reader** role is enabled.

    Reader permissions allow the {{site.data.keyword.watson}} service to see the root keys in the {{site.data.keyword.keymanagementserviceshort}} instance.
1.  Click **Authorize** and confirm delegated authorization.

### Step 4. Encrypt the {{site.data.keyword.watson}} service data
{: #keyservice-grant}

After you grant the {{site.data.keyword.watson}} service the authorization to use your keys, you create another instance on your Premium plan and supply the key name or CRN. The service uses your encryption key to encrypt your data.

You must have the administrator or editor role on the {{site.data.keyword.watson}} service.

1.  Go to [**AI**](https://{DomainName}/catalog/?category=ai){: external} category page.
1.  Select the {{site.data.keyword.watson}} service that you authorized in [Step 3. Grant service access](#serviceauth-key)
1.  Select a region.

    Make sure to create the instance in the same location as the {{site.data.keyword.keymanagementserviceshort}} service that you created in Step 1.
1.  Select the Premium plan. This feature is supported on the Premium plan only.
1.  Encrypt the service data with {{site.data.keyword.keymanagementserviceshort}}:

    1.  Select **Yes** to "Encrypt service with {{site.data.keyword.keymanagementserviceshort}}."
    1.  Select the {{site.data.keyword.keymanagementserviceshort}} instance that you authorized earlier.
    1.  Select the root key that you authorized earlier.
1.  Click **Create**.

The data that is associated with this {{site.data.keyword.watson}} service instance is now encrypted.

## Working with customer-managed keys
{: #keyservice-using}

After you enable a customer-managed key, the service operates normally, and you can manage access to the data with {{site.data.keyword.keymanagementserviceshort}}.

### Temporarily prevent access to your data
{: #keyservice-prevent-access}

To temporarily prevent access, remove all authorizations between the {{site.data.keyword.watson}} service and {{site.data.keyword.keymanagementserviceshort}}:

- Remove the authorization that you created in Step 3 from the IAM [Authorizations](https://{DomainName}/iam/authorizations){: external} page.
- Remove all other authorizations between other services connected with the {{site.data.keyword.watson}} service and {{site.data.keyword.keymanagementserviceshort}}.

    1.  Find the Cloud Resource Name identifier (CRN) for the {{site.data.keyword.watson}} service that you want to remove access to. You find the CRN by clicking the service instance row in your [Resource list](https://{DomainName}/resources?groups=resource-instance){: external}. The CRN is displayed in the details pane.
    1.  Back on the Authorizations page, search for that CRN in the list.
    1.  Remove every authorization with the CRN of the {{site.data.keyword.watson}} service as the source and {{site.data.keyword.keymanagementserviceshort}} as the target.

    These other authorizations might exist because the {{site.data.keyword.watson}} service can create delegated policies from the authorization that you created.

The {{site.data.keyword.watson}} instance can no longer access the data because no authorizations exist to access the key. For more information, search for [Removing an authorization](/docs/account?topic=account-serviceauth#remove-auth) in "Using authorizations to grant access between services".

### Restore temporary access
{: #keyservice-restore-access}

To restore access after you temporarily remove it, follow these steps:

1.  Re-create the authorization between your {{site.data.keyword.watson}} service and {{site.data.keyword.keymanagementserviceshort}} instance as shown in [Step 3. Grant service access](#serviceauth-key).
1.  Tell your {{site.data.keyword.IBM_notm}} Client Success Manager (CSM) that you want to restore access through {{site.data.keyword.keymanagementserviceshort}}.

After you re-create the authorization and your CSM confirms {{site.data.keyword.watson}} that any delegated authorizations are reconnected, the {{site.data.keyword.watson}} instance starts accepting connections again.

### Permanently prevent access to your data
{: keyservice-delete}

To delete your data securely, delete both the encrypted {{site.data.keyword.watson}} service instance and the {{site.data.keyword.keymanagementserviceshort}} key.

When you delete a data encryption key, it is not recoverable and you cannot decrypt the key. You prevent further access to the data, but you also cannot recover the data.
{: important}

For more information about deleting keys, see [Deleting keys](/docs/key-protect?topic=key-protect-delete-keys) in the {{site.data.keyword.keymanagementserviceshort}} docs.

### Rotating keys
{: keyservice-rotate-keys}

Key rotation is an important part of mitigating the risk of a data breach. Periodically changing keys reduces the potential data loss if the key is lost or compromised. For more information, see [Setting a rotation policy](/docs/key-protect?topic=key-protect-set-rotation-policy) in the {{site.data.keyword.keymanagementserviceshort}} docs.

### Identify which key is encrypting your service instance
{: #keyservice-view}

To see which {{site.data.keyword.keymanagementserviceshort}} instance is used to encrypt the data, use the link in the service details.

1.  Go to your [resource list](https://{DomainName}/resources){: external}.
1.  Click the name of the {{site.data.keyword.watson}} service instance.
1.  Click the **Manage** tab on the left pane.
1.  Find "Encrypted with a {{site.data.keyword.keymanagementserviceshort}} root key". Click **View KeyProtect Instance** to see details of the keys in that {{site.data.keyword.keymanagementserviceshort}} instance.

## Next steps
{: #keyservice-next}

- Read more about [Bringing your encryption keys to the cloud](/docs/key-protect?topic=key-protect-importing-keys) in the {{site.data.keyword.keymanagementserviceshort}} docs.
- See which [Activity Tracker events](/docs/key-protect?topic=key-protect-at-events) are recorded with {{site.data.keyword.keymanagementserviceshort}}.

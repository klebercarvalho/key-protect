---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-16"

keywords: get instance ID, get instance GUID, service instance

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:term: .term}
{:ui: .ph data-hd-interface='ui'}
{:api: .ph data-hd-interface='api'}
{:cli: .ph data-hd-interface='cli'}

# Retrieving your instance ID and cloud resource name (CRN)
{: #retrieve-instance-ID}

You can target an individual {{site.data.keyword.keymanagementservicelong}} instance for operations by including its unique identifier, or instance ID, in API requests to the service. Similarly, it can also be helpful to know your relevant [cloud resource name (CRN)](/docs/account?topic=account-crn).
{: shortdesc}

## Viewing your instance ID and CRN in the {{site.data.keyword.cloud_notm}} console
{: #view-instance-ID}
{: ui}

You can view the instance ID that is associated with your {{site.data.keyword.keymanagementserviceshort}} instance by navigating to your {{site.data.keyword.cloud_notm}} resource list.

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.

2. Go to **Menu** &gt; **Resource List**, and then click **Services** to browse a list of your cloud services.

3. Click the table row. This opens the resources table side panel, where you can see the CRN and instance GUID.

4. Click on the instance name link. This will open your {{site.data.keyword.keymanagementserviceshort}} instance.

4. From the **Keys** page, click the **Details** box to open a side panel. Your instance ID and CRN are listed here as code snippets.

## Retrieving an instance ID and CRN with the CLI
{: #retrieve-instance-ID-cli}
{: cli}

You can also retrieve the instance ID for your
{{site.data.keyword.keymanagementserviceshort}} instance by using the
[{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started){: external}.

1. Log in to {{site.data.keyword.cloud_notm}} with the
    [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started){: external}.

    ```sh
    ibmcloud login
    ```
    {: pre}

    If the login fails, run the `ibmcloud login --sso` command to try again. The
    `--sso` parameter is required when you log in with a federated ID. If this
    option is used, go to the link listed in the CLI output to generate a
    one-time passcode.
    {: note}

2. Select the account, region, and resource group that contain your provisioned
    instance of {{site.data.keyword.keymanagementserviceshort}}.

3. Retrieve the Cloud Resource Name (CRN) that uniquely identifies your
    {{site.data.keyword.keymanagementserviceshort}} instance.

    ```sh
    ibmcloud resource service-instance <instance_name> --id
    ```
    {: pre}

    Replace `<instance_name>` with the unique alias that you assigned to your
    {{site.data.keyword.keymanagementserviceshort}} instance. If you do not know the name of your instance,
    run the [`ibmcloud resource service-instances` command](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_service_instances) to retrieve a list of
    instance names that you provisioned in the selected region.

    The following truncated example shows the CLI output.

    ```plaintext
    crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:42454b3b-5b06-407b-a4b3-34d9ef323901:: 42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}

    The _42454b3b-5b06-407b-a4b3-34d9ef323901_ value is an example instance ID.

## Retrieving an instance ID with the API
{: #retrieve-instance-ID-api}
{: api}

You might want to retrieve the instance ID programmatically to help you build
and connect your application. You can call the
[{{site.data.keyword.cloud_notm}} Resource Controller API](/apidocs/resource-controller){: external},
and then pipe the JSON output to `jq` to extract this value.

1. [Retrieve an {{site.data.keyword.cloud_notm}} IAM access token](/docs/key-protect?topic=key-protect-retrieve-access-token).

2. Call the
    [Resource Controller API](/apidocs/resource-controller){: external}
    to retrieve your instance ID.

    ```sh
    $ curl -X GET \
        "https://resource-controller.cloud.ibm.com/v2/resource_instances" \
        -H "authorization: Bearer <IAM_token>" | jq -r '.resources[] | select(.name | contains("<instance_name>")) | .guid'
    ```
    {: codeblock}

    Replace `<instance_name>` with the unique alias that you assigned to your
    {{site.data.keyword.keymanagementserviceshort}} instance. If you do not know the name of your instance,
    make a [`GET /v2/resource_instances` request](/apidocs/resource-controller/resource-controller#list-resource-instances){: external}
    to retrieve a list of instance names that you provisioned in the selected region.

    The following output shows an example instance ID:

    ```plaintext
    42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}



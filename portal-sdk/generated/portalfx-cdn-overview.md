<a name="getting-started-with-azure-cdn"></a>
# Getting started with Azure CDN

The Azure Content Delivery Network (CDN) is designed to send audio, video, images, and other files faster and more reliably to customers by using servers that are closest to the users. These built-in CDN capabilities in the SDK dramatically increase speed and availability, and result in significant improvements in the user experience.

Developers may use a Content Delivery Network(CDN) to serve static images, scripts, and stylesheets. The Azure Portal SDK does not require the use of a CDN, or the use of a specific  CDN. 

The benefits of using the CDN to cache web site assets include the following.

* Better performance and user experience for end users

    This is especially true for applications that require multiple round-trips to load content.

* Large scaling to better handle single-event high loads
    
    For example, the start of a product launch event will cause a spike in user access that is not predictable on an annual basis

* Less traffic is sent to the origin

    User requests are distributed, and edge servers serve content

The `CdnIntegrationBlade` allows customers to create and manage CDN endpoints for their existing Azure resources, as in the following example.

![alt-text](../media/portalfx-pde/CdnIntegrationBlade.png "Cdn integration blade")

 **NOTE**: The CdnIntegrationBlade only works in public Azure and is NOT available in national clouds like MoonCake, or BlackForest. 

<a name="getting-started-with-azure-cdn-the-cdnintegration-blade"></a>
### The CdnIntegration blade

Through simple integration, your customers can enable CDN on their Azure resources within your extension without having to navigate to the CDN extension. Use the following steps to embed the CDN integration blade into your extension.

1. Import the CDN Extension NuGet Package
1. Reference the CDN PDE
1. Reference the CDN Integration Blade


<a name="getting-started-with-azure-cdn-1-importing-cdn-extension-nuget-package"></a>
## >
<li>Importing CDN Extension NuGet Package</li>
<

To be able to use the CDN integration blade, you will need to reference Microsoft.Portal.Extensions.Cdn nuget package.
For CoreXT based environemtns, you can add a reference to the package in your **corext.config** or **packages.config** file as shown below. If you are not using CoreXT, please reference the package as appropriate in your environment.

```xml
<package id="Microsoft.Portal.Extensions.Cdn" version="1.0.13.177" />
```

*Note:* you will need to update the version with the latest from [here](https://msazure.visualstudio.com/DefaultCollection/One/_apps/hub/ms.feed.feed-hub?feedName=Official&protocolType=NuGet&packageName=microsoft.portal.extensions.cdn)

<a name="getting-started-with-azure-cdn-2-referencing-cdn-pde"></a>
## >
<li>Referencing CDN PDE</li>
<

In your extension *.csproj file, you will need to add a reference to the Microsoft_Azure_Cdn.pde similar to this:
```xml
<ExtensionReference Include="$(PkgMicrosoft_Portal_Extensions_Cdn)\content\Client\_extensions\Cdn\Microsoft_Azure_Cdn.pde" />
```

<a name="getting-started-with-azure-cdn-3-referencing-cdn-integration-blade"></a>
## >
<li>Referencing CDN Integration Blade</li>
<

For the CdnIntegrationBlade to show up in your extension, you may reference it in one of the following ways:

 - You can add it as an item in your resource menu similar to the code blow: 
```ts
{
    id: "cdnIntegration",
    displayText: "Azure CDN",
    enabled: ko.observable(true),
    visible: ko.observable(true), //Shouldn't be visible in national clouds
    keywords: [
        "cdn",
        "endpoint",
        "profile",
        "domain"
    ],
    icon: MsPortalFx.Base.Images.Polychromatic.Cdn(),
    supplyBladeReference: () => {
        return new CdnBladeReferences.CdnIntegrationBladeReference(
            {
                resourceId: <your resource Id>,
                location: <your resource location>,
                originHostname: <your resource hostname>
            });
    }
}
```

 - You can alternatively open the CdnIntegrationBlade as a DynamicBladeSelection, which doesn't require importing the CDN nuget package, as shown in this example:

```ts
this._container.selectable.selectedValue(<MsPortalFx.ViewModels.DynamicBladeSelection>{
        extension: "Microsoft_Azure_Cdn",
        detailBlade: "CdnIntegrationBlade",
        detailBladeInputs: {
            resourceId: this.resourceUri(),
            location: this._siteView.item().Location(),
            originHostname: this._siteView.item().DefaultHostName()
        }
    });            
```

<a name="getting-started-with-azure-cdn-3-referencing-cdn-integration-blade-blade-inputs"></a>
### Blade Inputs
The CdnIntegrationBlade takes the following inputs:

1. **resourceId**:
This is the id of your ARM resource. For example, if you are calling the Azure CDN blade from Storage resource menu, this is what the resourceId looks like for a storage account
"/subscriptions/93456ca3-e4aa-4986-ab1c-98afe7a12345/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/storagetest1"

2. **location**:
The AzureLocation of your resource, like *"West US"*, *"East Asia"*, etc.

3. **originHostname**:
The hostname of your service which is used as an origin for the created CDN endpoints. This shouldn't include any slashes or protocols, only the domain name, like *"storagetest1.blob.core.windows.net"* or *"webapptest2.azurewebsites.net"*.



<a name="getting-started-with-azure-cdn-4-telemetry-and-monitoring"></a>
## >
<li>Telemetry and Monitoring</li>
<
We are tracking the usage and actions on CDN integration blade through following metrics:

 - Number of CDN Endpoints created from partner extensions vs. CDN extension.
 - Number of customers clicking Azure CDN from partner extension.
 - Number of customers managing CDN from partner extension.
 - Percentage of customers initiating a CDN create operation after landing into the Azure CDN blade. 
 - Percentage of success and failure of create operations from the Azure CDN blade.

You shouldn't need to add any extra telemetry on your side.

<a name="getting-started-with-azure-cdn-4-telemetry-and-monitoring-contact-us"></a>
### Contact Us
You can start development today! Create a code review and add  and "cdneng" as reviewers.

Don't hesitate to contact us for any questions, concerns, or bug reports.
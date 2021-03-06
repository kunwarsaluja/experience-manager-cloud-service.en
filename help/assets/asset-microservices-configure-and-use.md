---
title: Configure and use asset microservices for asset processing
description: Learn how to configure and use the cloud-native asset microservices to process assets at scale.
contentOwner: AG
---

# Get started using asset microservices {#get-started-using-asset-microservices}

<!--
* Current capabilities of asset microservices offered. If workers have names then list the names and give a one-liner description. (The feature-set is limited for now and continues to grow. So will this article continue to be updated.)
* How to access the microservices. UI. API. Is extending possible right now?
* Detailed list of what file formats and what processing is supported by which workflows/workers process.
* How/where can admins check what's already configured and provisioned.
* How to create new config or request for new provisioning/purchase.

* [DO NOT COVER?] Exceptions or limitations or link back to lack of parity with AEM 6.5.
-->

Asset microservices provide a scalable and resilient processing of assets using cloud services. Adobe manages the services for optimal handling of different asset types and processing options.

Asset processing depends on the configuration in **[!UICONTROL Processing Profiles]**, which provide a default set up, and allow an administrator to add more specific asset processing configuration. Administrators can create and maintain the configurations of post-processing workflows, including optional customization. Customizing workflows allows for extensibility and full customization.

A high-level flow for asset processing is below.

<!-- Proposed DRAFT diagram for asset microservices flow - see section "asset-microservices-flow.png (asset-microservices-configure-and-use.md)" in the PPTX deck

https://adobe-my.sharepoint.com/personal/gklebus_adobe_com/_layouts/15/guestaccess.aspx?guestaccesstoken=jexDC5ZnepXSt6dTPciH66TzckS1BPEfdaZuSgHugL8%3D&docid=2_1ec37f0bd4cc74354b4f481cd420e07fc&rev=1&e=CdgElS
-->

![asset-microservices-flow](assets/asset-microservices-flow.png)

>[!NOTE]
>
> The asset processing described here replaces the `DAM Update Asset` workflow model that exists in the previous versions of Experience Manager. Most of the standard rendition generation and metadata-related steps are replaced by the asset microservices processing, and remaining steps, if any, can be replaced by the post-processing workflow configuration.

## Get started with asset processing {#get-started}

Asset processing with asset microservices is pre-configured with a default configuration, ensuring that the default renditions required by the system are available. It also ensure that metadata extraction and text extraction operations are available. Users can start uploading or updating assets immediately and basic processing is available by default.

For specific rendition generation or asset processing requirements, an AEM administrator can create additional [!UICONTROL Processing Profiles]. Users can assign one or more of the available profiles to specific folders to get additional processing done. Say for example, to generate web, mobile, and tablet specific renditions. The following video illustrates how to create and apply [!UICONTROL Processing Profiles] and how to access the created renditions.

>[!VIDEO](https://video.tv.adobe.com/v/29832?quality=9)

To change existing profile, see [configurations for asset microservices](#configure-asset-microservices).
To create custom processing profiles specific to your custom requirements, say to integrate with other systems, see [post-processing workflows](#post-processing-workflows).

## Configurations for asset microservices {#configure-asset-microservices}

To configure asset microservices, administrators can use configuration user interface under **[!UICONTROL Tools > Assets > Processing Profiles]**.

### Default configuration {#default-config}

With the default configuration, only the standard processing profile is configured. The standard processing profile is not visible on the user interface and you cannot modify it. It always executes to process uploaded assets. A standard processing profile ensures that all the basic processing required by Experience Manager is completed on all assets.

<!-- ![processing-profiles-standard](assets/processing-profiles-standard.png) -->

The standard processing profile provides the following processing configuration:

* Standard thumbnails used by Asset user interface (48, 140, and 319 px)
* Large preview (web rendition - 1280 px)
* Metadata extraction
* Text extraction

### Supported file formats {#supported-file-formats}

Asset microservices provide support for a wide variety of file formats in terms of the ability to generate renditions or extract metadata. See [supported file formats](file-format-support.md) for the full list.

### Add additional processing profiles {#processing-profiles}

Additional processing profiles can be added using the **[!UICONTROL Create]** action.

Each processing profile configuration includes a list of renditions. For each rendition, you can specify the following:

* Rendition name.
* Supported rendition format, such as JPEG, PNG, or GIF.
* Rendition width and height in pixels. If it is not specified, the full pixel size of the original image is used.
* Rendition quality of JPEG in percent.
* Included and excluded MIME types to define the applicability of a profile.

![processing-profiles-adding](assets/processing-profiles-adding.png)

When you create and save a new processing profile it is added to the list of configured processing profiles. You can apply these processing profiles to folders in the folder hierarchy to make them effective for asset uploads and assets processing.

<!-- Removed per cqdoc-15624 request by engineering.
 ![processing-profiles-list](assets/processing-profiles-list.png) -->

#### Rendition width and height {#rendition-width-height}

Rendition width and height specification provides maximum sizes of the generated output image. Asset microservice tries to produce the largest possible rendition, which width and height is not bigger than the specified width and height, respectively. The aspect ratio is preserved, that is the same as the original.

An empty value means that asset processing assumes the pixel dimension of the original.

#### MIME type inclusion rules {#mime-type-inclusion-rules}

When an asset with a specific MIME type is processed, the MIME type is first checked against the excluded MIME types value for the rendition specification. If it matches that list, this specific rendition is not generated for the asset ("blacklisting").

Otherwise, the MIME type is checked against the included MIME type, and if it matches the list, the rendition is generated ("whitelisting").

#### Special FPO rendition {#special-fpo-rendition}

When placing large-sized assets from AEM into Adobe InDesign documents, a creative professional must wait for a substantial time after they [place an asset](https://helpx.adobe.com/indesign/using/placing-graphics.html). Meanwhile, the user is blocked from using InDesign. This interrupts creative flow and negatively impacts the user experience. Adobe enables temporarily placing small-sized renditions in InDesign documents to begin with, which can be replaced with full-resolution assets on-demand later. Experience Manager provides renditions that are used for placement only (FPO). These FPO renditions have a small file size but are of the same aspect ratio.

The processing profile can include a FPO (For Placement Only) rendition. See Adobe Asset Link [documentation](https://helpx.adobe.com/enterprise/using/manage-assets-using-adobe-asset-link.html) to understand if you need to turn it on for your processing profile. For more information, see [Adobe Asset Link complete documentation](https://helpx.adobe.com/enterprise/using/adobe-asset-link.html).

## Use asset microservices to process assets {#use-asset-microservices}

Create and apply the additional, custom processing profiles to specific folders for Experience Manager to process for assets uploaded to or updated in these folders. The default, in-built standard processing profile is always executed but is not visible on the user interface. If you add a custom profile, then both profiles are used to process the uploaded assets.

There are two ways to get processing profiles applied to folders:

* Administrators can select a processing profile definition in **[!UICONTROL Tools > Assets > Processing Profiles]**, and use **[!UICONTROL Apply Profile to Folder(s)]** action. It opens a content browser that allow you to navigate to specific folders, select them and confirm the application of the profile.
* Users can select a folder in the Assets user interface, use **[!UICONTROL Properties]** action to open folder properties screen, click on the **[!UICONTROL Processing Profiles]** tab, and in the drop-down, select the right processing profile for that folder. The choice will be save upon **[!UICONTROL Save & Close]** action.

>[!NOTE]
>
>Only one processing profile can be applied to a specific folder. If you need more renditions generated, you can add more rendition definitions to the processing profile.

After a processing profile is applied to a folder, all the new assets uploaded (or updated) in this folder or any of it's sub-folders are processed using the additional processing profile configured. This additional processing is in addition to the standard, default profile. If you apply multiple profiles to a folder, the uploaded or updated assets are processed using each of these profiles.

>[!NOTE]
>
>When assets are uploaded to a folder, Experience Manager checks the containing folder's properties for a processing profile. If none is applied, it goes up in the folder tree until it finds a processing profile applied, and uses it for the asset. That means that a processing profile applied to a folder works for the entire tree, but can be over-ridden with another profile applied to a sub-folder.

Users can check that the processing actually took place by opening a newly-uploaded asset for which processing has finished, opening asset preview, and clicking on the left-hand rail's **[!UICONTROL Renditions]** view. The specific renditions in the processing profile, for which the specific asset's type matches the MIME type inclusion rules, should be visible and accessible.

![additional-renditions](assets/renditions-additional-renditions.png)
*Figure: Example of two additional renditions generated by a processing profile applied to the parent folder*

## Post-processing workflows {#post-processing-workflows}

For situation, where additional processing of assets is required that cannot be achieved using the processing profiles, additional post-processing workflows can be added to the configuration. This allows for adding fully customized processing on top of the configurable processing using asset microservices.

Post-processing workflows, if configured, are automatically executed by AEM after the microservices processing finishes. There is no need to add workflow launchers manually to trigger them.

Examples include:

* custom workflow steps for processing assets, for example, Java code to generate renditions from proprietary file formats.
* integrations to add metadata or properties to assets from external systems, for example, product or process information.
* additional processing done by external services

Adding a post-processing workflow configuration to Experience Manager is comprised of the following steps:

* Creating one or more workflow models. We'll call them "post-processing workflow models," but they are regular AEM workflow models.
* Adding specific workflow steps to these models. These steps will be execution on the assets based on workflow model configuration.
* The last step of such a model must be the `DAM Update Asset Workflow Completed Process` step. This is required to ensure that AEM knows the processing has ended and the asset can be marked as processed ("New")
* Creating a configuration for the Custom Workflow Runner Service, which allows for configuring execution of a post-processing workflow model either by path (folder location) or regular expression

### Create post-processing workflow models {#create-post-processing-workflow-models}

Post-processing workflow models are regular AEM workflow models. Create different models if you need different processing for different repository locations or asset types.

Processing steps should be added based on needs. You can use any supported steps available, as well as any custom-implemented workflow steps.

Ensure that the last step of each post-processing workflows is `DAM Update Asset Workflow Completed Process`. The last step helps ensure that Experience Manager knows when asset processing is completed.

### Configure post-processing workflow execution {#configure-post-processing-workflow-execution}

To configure the post-processing workflow models to be executed for assets uploaded or updated in the system after the asset microservices processing finishes, the Custom Workflow Runner service needs to be configured.

The Custom Workflow Runner service (`com.adobe.cq.dam.processor.nui.impl.workflow.CustomDamWorkflowRunnerImpl`) is an OSGi service and provides two options for configuration:

* Post-processing workflows by path (`postProcWorkflowsByPath`): Multiple workflow models can be listed, based on different repository paths. Paths and models should be separated by a colon. Simple repository paths are supported and should be mapped to a workflow model in the `/var` path. For example: `/content/dam/my-brand:/var/workflow/models/my-workflow`.
* Post-processing workflows by expression (`postProcWorkflowsByExpression`): Multiple workflow models can be listed, based on different regular expressions. Expressions and models should be separated by a colon. The regular expression should point to the Asset node directly, and not one of the renditions or files. For example: `/content/dam(/.*/)(marketing/seasonal)(/.*):/var/workflow/models/my-workflow`.

>[!NOTE]
>
>Configuration of the Custom Workflow Runner is a configuration of an OSGi service. See [deploy to Experience Manager](/help/implementing/deploying/overview.md) for information on how to deploy an OSGi configuration.
> OSGi web console, unlike in on-premise and managed services deployments of AEM, is not directly available in the cloud service deployments.

For details about which standard workflow step can be used in the post-processing workflow, see [workflow steps in post-processing workflow](developer-reference-material-apis.md#post-processing-workflows-steps) in the developer reference.

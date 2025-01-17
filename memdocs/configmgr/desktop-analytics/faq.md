---
title: FAQ for Desktop Analytics
titleSuffix: Configuration Manager
description: Frequently asked questions for Desktop Analytics.
ms.date: 04/02/2021
ms.prod: configuration-manager
ms.technology: configmgr-analytics
ms.topic: conceptual
ms.assetid: e0db3311-2303-4013-a906-76b408172d3c
author: aczechowski
ms.author: aaroncz
manager: dougeby
ms.reviewer: acabello
---

# Desktop Analytics FAQ

## Prerequisites

### <a name="bkmk_intune"></a> Can I use cloud-enabled analytics with Intune-managed devices?

Not today, but see the announcement from Microsoft Ignite 2019 on [insights-driven device management](https://myignite.microsoft.com/archives/IG19-BRK3086). This planned solution is a successor to Device Health and Upgrade Readiness.

Most customers that can benefit from the Desktop Analytics workflow use Configuration Manager to deploy Windows. We know Intune customers love the insights from analytics data, and we're working on ways to share insights with them as well.

### It's been over 72 hours and the portal is still processing data, what next?

When you first set up Desktop Analytics, the reports in Configuration Manager and the Desktop Analytics portal may not show complete data right away. It can take 2-3 days for the service to process the data. If it's been over 72 hours, and the portal is still processing data, follow these steps:

- To confirm that active devices are properly configured, use the [Connection Health dashboard](monitor-connection-health.md). This dashboard doesn't update in real time.
- Make sure devices are sending diagnostic data to the Desktop Analytics service. For more information, see [Enable data sharing](enable-data-sharing.md).
- Provision [Azure AD applications](troubleshooting.md#bkmk_AzureADApps) on your Azure AD.

If devices are properly configured, and you're still not seeing data in your workspace, [contact Microsoft support](https://support.serviceshub.microsoft.com/supportforbusiness).

## Connect Configuration Manager

### Can I change the target or other collections?

Yes, use the following process:

- In the Configuration Manager console, go to the **Administration** workspace, expand **Cloud Services**, and select the **Azure Services** node. Open the properties for the entry associated with your Desktop Analytics service.

- On the **Desktop Analytics Connection** tab, change the **Target Collection** or manage the other collections.

<!-- 7130169 -->
> [!Note]
> Don't include more than 20 collections to the list of additional collections. Be cautious about the total number of devices in each collection. Always include your [Global pilot include and exclude collections](deploy-pilot.md#bkmk_GlobalPilot).  

> [!IMPORTANT]  
> Configuration Manager uses a settings policy to configure devices in the target collection. This policy includes the diagnostic data settings to enable devices to send data to Microsoft. Changing the target collection doesn't undo the settings policy on devices no longer in the target collection. If you don't want your devices to continue sending diagnostic data, [reconfigure the devices](account-close.md#reconfigure-clients).

## Windows upgrade

### Can I upgrade Windows and change architecture?

Desktop Analytics is designed to best support in-place upgrades. In-place upgrades don't support migrations from 32-bit to 64-bit architecture. If you need to migrate computers in this scenario, use the refresh scenario. Desktop Analytics insights are still valuable in this scenario, but you can ignore upgrade-specific guidance.

For more information, see [Refresh an existing computer with a new version of Windows](../osd/deploy-use/refresh-an-existing-computer-with-a-new-version-of-windows.md).

### Can I change from BIOS to UEFI when upgrading Windows?

Yes. For more information, see [Convert from BIOS to UEFI during an in-place upgrade](../osd/deploy-use/task-sequence-steps-to-manage-bios-to-uefi-conversion.md#bkmk_ipu).

### Can I use Desktop Analytics with Windows 10 LTSC?

Starting in version 2010, you can use Configuration Manager to enroll Windows 10 Enterprise long-term servicing channel (LTSC) 2019 devices to Desktop Analytics. Once you enroll these devices, you can evaluate them in your deployment plans to shift from LTSC to the semi-annual servicing channel.<!--6107649-->
  

### Can I reduce the amount of time it takes for data to refresh in my Desktop Analytics portal?

There are two types of data in the Desktop Analytics portal: Administrator data and diagnostic data. To refresh administrator data on-demand, open the data currency flyout and select **Apply changes**. This action immediately triggers a one-time refresh of any pending administrator changes in your workspaces. The changes propagate and are generally available within 15-60 minutes. The timing depends on the size of your workspace and the scope of pending changes. You can request an on-demand data refresh up to six times within a 24-hour period.

All data is updated automatically once daily, even if you don't request an on-demand data refresh. There's no way to trigger an on-demand refresh of diagnostic data. For more information about the different types of data in Desktop Analytics, see [Data latency](troubleshooting.md#data-latency).

## Privacy

### Can Desktop Analytics be used without a direct client connection to the Microsoft Data Management Service?

No, the entire service is powered by Windows diagnostic data, which requires that devices have this direct connectivity.

### Can I choose the data center location?

For Azure Log Analytics: Yes, when you set up Desktop Analytics and create the Log Analytics workspace.

For the Microsoft Data Management Service and Analytics Azure Storage: No, these two services are hosted in the United States.

### Where is my organization's data stored?

Windows diagnostic data from your computers is encrypted, sent to, and processed at Microsoft-managed secure data centers located in the United States. Microsoft provides the analysis of the Desktop Analytics-related data to you through the Desktop Analytics solution in the Azure portal. Desktop Analytics is supported in most regions where [Log Analytics is available](https://azure.microsoft.com/global-infrastructure/services/?products=monitor&regions=all). If you select an international Azure region, your diagnostic data is still sent to and processed in Microsoft's secure data centers in the United States.

## Existing Windows Analytics customers

> [!Important]  
> The Windows Analytics service is retired as of January 31, 2020.
>
> For more information, see [KB 4521815: Windows Analytics retirement on January 31, 2020](/lifecycle/announcements/windows-analytics-retirement).

### Can I use Update Compliance together with Desktop Analytics?

Yes. If you use [Update Compliance](/windows/deployment/update/update-compliance-get-started#add-update-compliance-to-your-azure-subscription), use that same workspace for Desktop Analytics. Devices can only send diagnostic data to one workspace. Reenroll devices to Desktop Analytics that you previously enrolled in Update Compliance. For the best experience, use Configuration Manager. Configuration Manager provides an integrated experience for managing and deploying these settings to clients.

### Are there any Windows Analytics features that aren't available in Desktop Analytics?

<!-- 3616924 -->
Yes, the following Windows Analytics features were either retired or aren't yet available in Desktop Analytics:

#### General

- Support for scenarios that don't require Configuration Manager. For example, [Intune support](#bkmk_intune).
- Licensing prerequisite of any valid Windows license versus E3, E5
- Support for multiple workspaces per Azure AD tenant
- Ability to run custom queries and export raw solution data
- Data model documentation for custom reports

#### Upgrade Readiness

- Internet Explorer Site Discovery data
- Microsoft 365 Apps add-in insights (now [available in Configuration Manager](#bkmk_office))
- Feedback Hub insights

#### Update Compliance

- Support for Windows Update for Business
- Delivery Optimization insights
- Support for Windows 10 long-term servicing channel (LTSC)
- Windows Insider reports
- Windows Defender status

> [!Note]
> All existing Update Compliance features, including those not available in Desktop Analytics, remain available in the [Update Compliance](/windows/deployment/update/update-compliance-get-started) solution in the Azure portal.

#### Device Health

- Driver health
- App health (outside of a deployment plan)
- Frequently crashing devices or driver-induced crashes
- Windows sign-in health
- Windows Information Protection
- Support for Windows Server

## Other

### Why aren't new Windows releases available immediately in Desktop Analytics?

Desktop Analytics uses several signals to generate the [compatibility assessment](compat-assessment.md) for applications. One of these signals is [Ready for Windows](compat-assessment.md#ready-for-windows) data. This data is based on the adoption status of every combination of application and OS versions from commercial devices that share data with Microsoft. This signal is more accurate from 30 to 60 days after the general availability of a new OS release. To avoid many of your assets having an **Unknown** or **Insufficient data** compatibility risk assessment, we wait 1-2 months before we add it to the list of target OS versions.

### Where is the Security update tile in Desktop Analytics?

<!-- 8099536 -->

In the Desktop Analytics portal, the **Security updates** tile on the home page and the **Security updates** page were removed in March 2021. To monitor your devices' Windows updates and Microsoft Defender status, use [Update Compliance](/windows/deployment/update/update-compliance-get-started) in the same workspace as Desktop Analytics. Devices can only send diagnostic data to one workspace.

### <a name="bkmk_office"></a> Can I use Desktop Analytics for my Microsoft 365 Apps upgrades?

No, Desktop Analytics is focused on Windows. Microsoft developed Desktop Analytics in close collaboration with many customers. Customer feedback is about how Desktop Analytics improves their ability to confidently manage Windows deployments. They also tell us they want [Microsoft 365 Apps readiness](../sum/deploy-use/office-365-dashboard.md#bkmk_o365_readiness) more closely integrated with Microsoft 365 Apps management tools in Configuration Manager and Intune. Microsoft continues to invest in those areas, while focusing on Windows scenarios in Desktop Analytics.

### How can I provide feedback about Desktop Analytics?

To share your feedback about Desktop Analytics, select the **Send a Smile** icon at the top of the portal. Include a screenshot with your submission to help Microsoft better understand your feedback. You can also submit product suggestions and upvote other ideas on [UserVoice](https://configurationmanager.uservoice.com/forums/300492-ideas?category_id=366805).

> [!Note]
> Never share private information via Send a Smile or UserVoice. Such private information includes your Tenant Id or Commercial Id. Microsoft doesn't provide support via the Send a Smile or UserVoice channels. To get help with your Desktop Analytics workspace, select the **Help and Support** link in the navigation menu to open a support ticket.

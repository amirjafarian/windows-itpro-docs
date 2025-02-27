---
title: Windows feature updates
description: This article explains how Windows feature updates are managed in Autopatch
ms.date: 02/02/2023
ms.prod: windows-client
ms.technology: itpro-updates
ms.topic: conceptual
ms.localizationpriority: medium
author: tiaraquan
ms.author: tiaraquan
manager: dougeby
msreviewer: andredm7
---

# Windows feature updates

Microsoft provides robust mobile device management (MDM) solutions such as Microsoft Intune, Windows Update for Business, Configuration Manager etc. However, the administration of these solutions to keep Windows devices up to date with the latest Windows feature releases rests on your organization’s IT admins. The Windows feature update process is considered one of the most expensive and time consuming tasks for IT since it requires incremental rollout and validation.

Windows feature updates consist of:

- Keeping Windows devices protected against behavioral issues.
- Providing new features to boost end-user productivity.

Windows Autopatch makes it easier and less expensive for you to keep your Windows devices up to date so you can focus on running your core businesses while Windows Autopatch runs update management on your behalf.

## Enforcing a minimum Windows OS version

Once devices are registered with Windows Autopatch, they’re assigned to deployment rings. Each of the four deployment rings have its Windows feature update policy assigned to them. This is intended to minimize unexpected Windows OS upgrades once new devices register with the service.

The policies:

- Contain the minimum Windows 10 version being currently serviced by the [Windows servicing channels](/windows/release-health/release-information?msclkid=ee885719baa511ecb838e1a689da96d2). The current minimum OS version is **Windows 10 20H2**.
- Set a bare minimum Windows OS version required by the service once devices are registered with the service.

If a device is registered with Windows Autopatch, and the device is:

- Below the service's currently targeted Windows feature update, that device will update to the service's target version when it meets the Windows OS upgrade eligibility criteria.
- On, or above the currently targeted Windows feature update version, there won't be any Windows OS upgrades to that device.

## Windows feature update policy configuration

If your tenant is enrolled with Windows Autopatch, you can see the following policies created by the service in the Microsoft Intune portal:

| Policy name | Feature update version | Rollout options | First deployment ring availability | Final deployment ring availability | Day between deployment rings | Support end date |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| Windows Autopatch – DSS Policy [Test] | Windows 10 20H2 | Make update available as soon as possible | N/A | N/A | N/A | 5/8/2023, 7:00PM |
| Windows Autopatch – DSS Policy [First] | Windows 10 20H2 | Make update available as soon as possible | N/A | N/A | N/A | 5/8/2023, 7:00PM |
| Windows Autopatch – DSS Policy [Fast] | Windows 10 20H2 | Make update available as soon as possible | 12/14/2022 | 12/21/2022 | 1 | 5/8/2023, 7:00PM |
| Windows Autopatch – DSS Policy [Broad] | Windows 10 20H2 | Make update available as soon as possible | 12/15/2022 | 12/29/2022 | 1 | 5/8/2023, 7:00PM |

> [!IMPORTANT]
> If you’re ahead of the current minimum OS version enforced by Windows Autopatch in your organization, you can [edit Windows Autopatch’s default Windows feature update policy and select your desired targeted version](/mem/intune/protect/windows-10-feature-updates#create-and-assign-feature-updates-for-windows-10-and-later-policy).

> [!NOTE]
> The four minimum Windows 10 OS version feature update policies were introduced in Windows Autopatch in the 2212 release milestone. Its creation automatically unassigns the previous four feature update policies targeting Windows 10 21H2 from all four Windows Autopatch deployment rings:<ul><li>**Modern Workplace DSS Policy [Test]**</li><li>**Modern Workplace DSS Policy [First]**</li><li>**Modern Workplace DSS Policy [Fast]**</li><li>**Modern Workplace DSS Policy [Broad]**</li><p>Since the new Windows feature update policies that set the minimum Windows 10 OS version are already in place, the Modern Workplace DSS policies can be safely removed from your tenant.</p>

## Test Windows 11 feature updates

You can test Windows 11 deployments by adding devices either through direct membership or by bulk importing them into the **Modern Workplace - Windows 11 Pre-Release Test Devices** Azure AD group. There’s a separate Windows feature update policy (**Modern Workplace DSS Policy [Windows 11]**) targeted to this Azure AD group, and its configuration is set as follows:

| Policy name | Feature update version | Rollout options | First deployment ring availability | Final deployment ring availability | Day between deployment rings | Support end date |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| Modern Workplace DSS Policy [Windows 11] | Windows 11 22H2 | Make update available as soon as possible | N/A | N/A | N/A | 10/13/2025, 7:00PM |

> [!IMPORTANT]
> Windows Autopatch neither applies its deployment ring distribution, nor configures the [Windows Update for Business gradual rollout settings](/mem/intune/protect/windows-update-rollout-options) in the **Modern Workplace DSS Policy [Windows 11]** policy.<p>Once devices are added to the **Modern Workplace - Windows 11 Pre-Release Test Devices** Azure AD group, the devices can be offered the Windows 11 22H2 feature update at the same time.</p>

## Manage Windows feature update deployments

Windows Autopatch uses Microsoft Intune’s built-in solution, which uses configuration service providers (CSPs), for pausing and resuming both [Windows quality](windows-autopatch-windows-quality-update-overview.md#pausing-and-resuming-a-release) and [Windows feature updates](#pausing-and-resuming-a-release).

Windows Autopatch provides a permanent pause of a Windows feature update deployment. The Windows Autopatch service automatically extends the 35-day pause limit (permanent pause) established by Microsoft Intune on your behalf. The deployment remains permanently paused until you decide to resume it.

## Pausing and resuming a release

> [!IMPORTANT]
> Pausing or resuming an update can take up to eight hours to be applied to devices. Windows Autopatch uses Microsoft Intune as its management solution and that's the average frequency devices take to communicate back to Microsoft Intune with new instructions to pause, resume or rollback updates.<p>For more information, see [how long does it take for devices to get a policy, profile, or app after they are assigned from Microsoft Intune](/mem/intune/configuration/device-profile-troubleshoot#how-long-does-it-take-for-devices-to-get-a-policy-profile-or-app-after-they-are-assigned).</p>

**To pause or resume a Windows feature update:**

1. Go to the [Endpoint Manager admin center](https://go.microsoft.com/fwlink/?linkid=2109431).
2. Select **Devices** from the left navigation menu.
3. Under the **Windows Autopatch** section, select **Release management**.
4. In the **Release management** blade, select either: **Pause** or **Resume**.
5. Select the update type you would like to pause or resume.
6. Select a reason from the dropdown menu.
7. Optional. Enter details about why you're pausing or resuming the selected update.
8. If you're resuming an update, you can select one or more deployment rings.
9. Select **Okay**.

If you've paused an update, the specified release will have the **Customer Paused** status. The Windows Autopatch service can't overwrite a customer-initiated pause. You must select **Resume** to resume the update.

> [!NOTE]
> The Service Paused status only applies to [Windows quality updates](../operate/windows-autopatch-windows-quality-update-overview.md#pausing-and-resuming-a-release). Windows Autopatch doesn't pause Windows feature updates on your behalf.

## Rollback

Windows Autopatch doesn’t support the rollback of Windows Feature updates.

> [!CAUTION]
> It’s not recommended to use [Microsoft Intune’s capabilities](/mem/intune/protect/windows-10-update-rings#manage-your-windows-update-rings) to pause and rollback a Windows feature update. However, if you choose to pause, resume and/or roll back from Intune, Windows Autopatch is **not** responsible for any problems that arise from rolling back the Windows feature update.

## Contact support

If you’re experiencing issues related to Windows feature updates, you can [submit a support request](../operate/windows-autopatch-support-request.md). Email is the recommended approach to interact with the Windows Autopatch Service Engineering Team.

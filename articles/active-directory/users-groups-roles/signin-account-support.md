---
title: 我的 Azure AD 登录页是否接受 Microsoft 帐户 |Microsoft Docs
description: 屏幕上的消息如何反映登录期间的用户名查找
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 221ab7c50a84650f1b2adf3fdb2b284365795f42
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2019
ms.locfileid: "74024287"
---
# <a name="sign-in-options-for-microsoft-accounts-in-azure-active-directory"></a>Azure Active Directory 中的 Microsoft 帐户的登录选项

Azure Active Directory (Azure AD) 的 Microsoft 365 登录页支持工作或学校帐户和 Microsoft 帐户，但会根据用户的情况采取具体的支持措施。 例如，Azure AD 登录页支持：

* 接受从两类帐户登录的应用
* 接受来宾的组织

## <a name="identification"></a>识别
若要确定组织使用的登录页是否支持 Microsoft 帐户，可以查看用户名字段中的提示文本。 如果提示文本指出了“电子邮件、手机或 Skype”，则登录页支持 Microsoft 帐户。

![帐户登录页之间的差异](./media/signin-account-support/ui-prompt.png)

[其他登录选项仅适用于个人 Microsoft 帐户](https://azure.microsoft.com/updates/microsoft-account-signin-options/ )，而不能用于登录到工作或学校帐户资源。

## <a name="next-steps"></a>后续步骤

[自定义登录品牌](../fundamentals/add-custom-domain.md)
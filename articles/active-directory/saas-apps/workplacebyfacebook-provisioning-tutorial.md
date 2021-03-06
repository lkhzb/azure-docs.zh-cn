---
title: 教程：使用 Azure Active Directory 为 Workplace by Facebook 配置自动用户预配 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Workplace by Facebook 间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 6341e67e-8ce6-42dc-a4ea-7295904a53ef
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 11a5e92ccf1104f36b3f2b045f9922158b1f7330
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2020
ms.locfileid: "77064135"
---
# <a name="tutorial-configure-workplace-by-facebook-for-automatic-user-provisioning"></a>教程：为 Workplace by Facebook 配置自动用户预配

本教程介绍了需要在 Workplace by Facebook 和 Azure Active Directory （Azure AD）中执行的步骤，以配置自动用户预配。 配置时，Azure AD 会使用 Azure AD 预配服务自动将用户和组预配到[Workplace By Facebook](https://work.workplace.com/)并取消其预配。 有关此服务的功能、工作原理以及常见问题的重要详细信息，请参阅[使用 Azure Active Directory 自动将用户预配到 SaaS 应用程序和取消预配](../manage-apps/user-provisioning.md)。

## <a name="migrating-to-the-new-workplace-by-facebook-application"></a>通过 Facebook 应用程序迁移到新的工作区
如果已与 Workplace by Facebook 集成，请参阅以下有关即将发生的更改的部分。 如果你是第一次使用 Facebook 设置工作区，则可以跳过此部分并转到支持的功能。 

#### <a name="whats-changing"></a>有什么变化？
* Azure AD 端的更改：在工作区中预配用户的授权方法一直以来都是长期的机密令牌。 不久，你会看到授权方法已更改为 OAuth 授权授权。 
* 工作区端的更改：之前的 Azure AD 应用是 Workplace by Facebook 中的自定义集成。 现在，你将在工作区集成目录中看到 Azure AD 为第三方应用程序。 

 

#### <a name="what-do-i-need-to-do-to-migrate-my-existing-custom-integration-to-the-new-application"></a>将现有自定义集成迁移到新应用程序需要执行哪些操作？
如果现有的工作区集成具有有效的令牌，则**无需执行任何操作**。 我们会将每周的客户自动迁移到新应用程序。 这完全在幕后完成。 如果你不能等待，并且想要手动移动到新应用程序，可以从库中添加新的工作区实例，并重新配置设置。 工作区的所有新实例将自动使用新的应用程序版本。 

 
如果你的工作区集成处于隔离区，你将需要再次提供有效的令牌，以便我们迁移你的。 "管理员凭据" 部分将灰显，但你可以将以下项（ **？Microsoft_AAD_IAM_userProvisioningEnableCredentialsOverride = true**）到 URL 再次保存凭据。 

https://portal.azure.com/?Microsoft_AAD_IAM_userProvisioningEnableCredentialsOverride=true

 
#### <a name="the-admin-credentials-section-is-greyed-out-on-my-application-and-i-cant-save-why"></a>"管理员凭据" 部分在我的应用程序上灰显，无法保存。 为什么?
我们锁定了现有工作区客户的 "管理员凭据" 部分。 当你的租户已迁移到新的工作区应用程序后，你将能够再次更新 "管理员凭据" 部分。 如果无法等待，可以使用上述 URL 来编辑应用程序。 

 
#### <a name="when-will-these-changes-happen"></a>何时会发生这些更改？
工作区的所有新实例都已在使用新的集成/授权方法。 现有集成将于2月逐渐迁移。 该月结束后，将针对所有租户完成迁移。 

## <a name="capabilities-supported"></a>支持的功能
> [!div class="checklist"]
> * 在 Workplace by Facebook 中创建用户
> * 不需要访问权限时，在 Workplace by Facebook 中删除用户
> * 使用户属性在 Azure AD 和 Workplace by Facebook 之间保持同步
> * 针对 Workplace by Facebook 的[单一登录](https://docs.microsoft.com/azure/active-directory/saas-apps/workplacebyfacebook-tutorial)（建议）

## <a name="prerequisites"></a>先决条件

本教程中概述的方案假定你已具有以下先决条件：

* [Azure AD 租户](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Azure AD 有[权](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)配置预配的用户帐户（例如，应用程序管理员、云应用程序管理员、应用程序所有者或全局管理员）
* 已启用 Workplace by Facebook 单一登录的订阅

> [!NOTE]
> 不建议使用生产环境测试本教程中的步骤。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="step-1-plan-your-provisioning-deployment"></a>步骤 1。 规划预配部署
1. 了解[预配服务的工作原理](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)。
2. 确定将处于[预配范围内的](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)用户。
3. 确定要[在 Azure AD 和 Workplace By Facebook 之间映射的](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)数据。

## <a name="step-2-configure-workplace-by-facebook-to-support-provisioning-with-azure-ad"></a>步骤 2. 配置 Workplace by Facebook 以支持使用 Azure AD 进行预配

在配置和启用预配服务前，需确定 Azure AD 中哪些用户和/或组表示需要访问 Workplace by Facebook 应用的用户。 确定后，可按照此处的说明将这些用户分配到你的 Workplace by Facebook 应用：

*   建议将单个 Azure AD 用户分配到 Workplace by Facebook 以测试预配配置。 其他用户和/或组可以稍后分配。

*   将用户分配到 Workplace by Facebook 时，必须选择有效的用户角色。 “默认访问权限”角色不可用于预配。

## <a name="step-3-add-workplace-by-facebook-from-the-azure-ad-application-gallery"></a>步骤 3。 从 Azure AD 应用程序库中添加 Workplace by Facebook

从 Azure AD 应用程序库中添加 Workplace by Facebook，开始管理到 Workplace by Facebook 的预配。 如果以前已通过 Facebook 为 SSO 设置了 Workplace，则可以使用相同的应用程序。 但建议您在最初测试集成时创建一个单独的应用程序。 在[此处](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)了解有关从库中添加应用程序的详细信息。

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>步骤 4. 定义将在设置范围内的人员 

Azure AD 预配服务允许你确定将根据分配给应用程序的人员，或基于用户/组属性进行预配的用户的范围。 如果选择将根据分配预配到你的应用的用户的范围，则可以使用以下[步骤](../manage-apps/assign-user-or-group-access-portal.md)将用户和组分配给应用程序。 如果选择仅根据用户或组的属性设置的作用域，则可以使用[此处](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)所述的范围筛选器。 

* 将用户和组分配到 Workplace by Facebook 时，必须选择 "**默认" 访问权限**以外的其他角色。 具有默认访问角色的用户将从预配中排除，并在预配日志中被标记为不有效。 如果应用程序上唯一可用的角色是默认访问角色，则可以[更新应用程序清单](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps)来添加其他角色。 

* 从小开始。 在向所有人推出之前，请使用少量的用户和组进行测试。 如果设置的作用域设置为 "分配的用户和组"，则可以通过将一个或两个用户或组分配到应用来对此进行控制。 当作用域设置为 "所有用户和组" 时，可以指定[基于属性的范围筛选器](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)。 

1. 登录 [Azure 门户](https://portal.azure.com)。 选择 "**企业应用程序**"，并选择 "**所有应用程序**"。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

2. 在应用程序列表中，选择“Workplace by Facebook”。

    ![应用程序列表中的 Workplace by Facebook 链接](common/all-applications.png)

3. 选择“预配”选项卡。

    ![设置选项卡](common/provisioning.png)

4. 将“预配模式”设置为“自动”。

    ![设置选项卡](common/provisioning-automatic.png)

5. 在 "**管理员凭据**" 部分中，单击 "**授权**"。 你将被重定向到 Workplace by Facebook 的授权页。 输入 Workplace by Facebook 用户名，并单击 "**继续**" 按钮。 单击 "**测试连接**" 以确保 Azure AD 可以连接到 Workplace by Facebook。 如果连接失败，请确保 Workplace by Facebook 帐户具有管理员权限，然后重试。

    ![预配](./media/workplacebyfacebook-provisioning-tutorial/provisioning.png)

    ![授权](./media/workplacebyfacebook-provisioning-tutorial/workplacelogin.png)

6. 在 "**通知电子邮件**" 字段中，输入应接收预配错误通知的人员或组的电子邮件地址，并选中 "**发生故障时发送电子邮件通知**" 复选框。

    ![通知电子邮件](common/provisioning-notification-email.png)

7. 选择“保存”。

8. 在 "**映射**" 部分下，选择 "**将 Azure Active Directory 用户同步到 Workplace by Facebook**"。

9. 在 "**属性映射**" 部分中，查看从 Azure AD 同步到 Workplace by Facebook 的用户属性。 选为“匹配”属性的属性将用于匹配 Workplace by Facebook 中的用户帐户以执行更新操作。 如果选择更改[匹配的目标属性](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)，将需要确保 Workplace BY Facebook API 支持基于该属性筛选用户。 选择“保存”按钮以提交任何更改。

   |属性|类型|
   |---|---|
   |userName 下方|String|
   |displayName|String|
   |活动|Boolean|
   |title|Boolean|
   |emails[type eq "work"].value|String|
   |name.givenName|String|
   |name.familyName|String|
   |名称。已设置格式|String|
   |地址 [类型 eq "work"]。格式|String|
   |addresses[type eq "work"].streetAddress|String|
   |地址 [类型 eq "work"]。位置|String|
   |地址 [类型 eq "work"]。区域|String|
   |地址 [类型 eq "work"]。国家/地区|String|
   |addresses[type eq "work"].postalCode|String|
   |地址 [type eq "other"]。格式|String|
   |phoneNumbers[type eq "work"].value|String|
   |phoneNumbers[type eq "mobile"].value|String|
   |phoneNumbers[type eq "fax"].value|String|
   |externalId|String|
   |preferredLanguage|String|
   |urn： ietf： params： scim：架构：扩展： enterprise：2.0： User： manager|String|
   |urn： ietf： params： scim：架构：扩展： enterprise：2.0： User：部门|String|

10. 若要配置范围筛选器，请参阅[范围筛选器教程](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)中提供的以下说明。

11. 若要为 Workplace by Facebook 启用 Azure AD 预配服务，请在 "**设置**" 部分中将 "**预配状态**" 更改为 **"打开**"。

    ![预配状态已打开](common/provisioning-toggle-on.png)

12. 通过在 "**设置**" 部分的 "**范围**" 中选择所需的值，定义要预配到 Workplace by Facebook 的用户和/或组。

    ![预配范围](common/provisioning-scope.png)

13. 已准备好预配时，单击“保存”。

    ![保存预配配置](common/provisioning-configuration-save.png)

此操作将启动 "**设置**" 部分的 "**范围**" 中定义的所有用户和组的初始同步循环。 初始周期比后续循环长，只要 Azure AD 预配服务正在运行，就大约每40分钟执行一次。 

## <a name="step-6-monitor-your-deployment"></a>步骤 6. 监视部署
配置预配后，请使用以下资源来监视部署：

1. 使用[预配日志](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs)来确定哪些用户已成功设置或失败
2. 检查[进度栏](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user)，查看设置周期的状态以及完成操作的方式
3. 如果预配配置似乎处于不正常状态，则应用程序将进入隔离区。 [在此处](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)了解有关隔离状态的详细信息。

## <a name="troubleshooting-tips"></a>故障排除提示
*  如果你看到某个用户未成功创建，并且存在代码为 "1789003" 的审核日志事件，则表示该用户来自未经验证的域。

## <a name="additional-resources"></a>其他资源

* [管理企业应用的用户帐户预配](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>后续步骤

* [了解如何查看日志并获取有关预配活动的报告](../manage-apps/check-status-user-account-provisioning.md)

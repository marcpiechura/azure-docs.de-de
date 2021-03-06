---
title: 'Azure AD: Erteilen einer mandantenweiten Administratoreinwilligung für eine Anwendung'
description: Erfahren Sie, wie Sie eine mandantenweite Einwilligung für eine Anwendung erteilen, sodass die Endbenutzer beim Anmelden bei der Anwendung keine Einwilligung erteilen müssen.
services: active-directory
author: psignoret
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: mimart
ms.reviewer: phsignor
ms.collection: M365-identity-device-management
ms.openlocfilehash: c515fef4997720435c64bd5f3ae7b18f8921fc5d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2020
ms.locfileid: "75475272"
---
# <a name="grant-tenant-wide-admin-consent-to-an-application"></a>Erteilen einer mandantenweiten Administratoreinwilligung für eine Anwendung

Erfahren Sie, wie Sie die Benutzererfahrung verbessern können, indem Sie eine mandantenweite Administratoreinwilligung für eine Anwendung erteilen. In diesem Artikel werden die verschiedenen Methoden dafür erläutert. Diese Methoden gelten für alle Endbenutzer in Ihrem Azure AD-Mandanten.

Weitere Informationen zur Einwilligung für Anwendungen finden Sie unter [Azure Active Directory-Zustimmungsframework](../develop/consent-framework.md).

## <a name="prerequisites"></a>Voraussetzungen

Sie müssen sich als [globaler Administrator](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator), als [Anwendungsadministrator](../users-groups-roles/directory-assign-admin-roles.md#application-administrator) oder als [Cloudanwendungsadministrator](../users-groups-roles/directory-assign-admin-roles.md#cloud-application-administrator) anmelden, um eine mandantenweite Administratoreinwilligung erteilen zu können.

> [!IMPORTANT]
> Wenn für eine Anwendung die mandantenweite Administratoreinwilligung erteilt wurde, können sich alle Benutzer bei der Anwendung anmelden, es sei denn eine Benutzerzuweisung ist erforderlich. Sie benötigen die Benutzerzuweisung, und müssen dann Benutzer oder Gruppen der Anwendung zuweisen, um zu bestimmen, welche Benutzer sich bei der Anwendung anmelden dürfen. Weitere Informationen finden Sie unter [Zuweisen von Benutzern und Gruppen zu einer Anwendung in Azure Active Directory](methods-for-assigning-users-and-groups.md).

> [!WARNING]
> Durch das Erteilen der mandantenweiten Administratoreinwilligung für eine Anwendung erhält die App und der Herausgeber der App Zugriff auf die Daten Ihrer Organisation. Überprüfen Sie vor dem Erteilen einer Einwilligung sorgfältig die Berechtigungen, die von der Anwendung angefordert werden.

## <a name="grant-admin-consent-from-the-azure-portal"></a>Erteilen der Administratoreinwilligung über das Azure-Portal

### <a name="grant-admin-consent-in-enterprise-apps"></a>Erteilen der Administratoreinwilligung in Unternehmensanwendungen

Sie können die mandantenweite Administratoreinwilligung über *Unternehmensanwendungen* erteilen, wenn die Anwendung bereits in Ihrem Mandanten bereitgestellt wurde. Eine Anwendung wurde z. B. in Ihrem Mandanten bereitgestellt, wenn mindestens ein Benutzer bereits eine Einwilligung für die Anwendung erteilt hat. Weitere Informationen finden Sie unter [Wie und warum werden Anwendungen zu Azure AD hinzugefügt?](../develop/active-directory-how-applications-are-added.md).

So erteilen Sie die mandantenweite Administratoreinwilligung für eine in **Unternehmensanwendungen** aufgeführte Anwendung:

1. Melden Sie sich als [globaler Administrator](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator), als [Anwendungsadministrator](../users-groups-roles/directory-assign-admin-roles.md#application-administrator) oder als [Cloudanwendungsadministrator](../users-groups-roles/directory-assign-admin-roles.md#cloud-application-administrator) beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Azure Active Directory** und anschließend auf **Unternehmensanwendungen**.
3. Wählen Sie die Anwendung aus, der Sie eine mandantenweite Administratoreinwilligung erteilen möchten.
4. Wählen Sie **Berechtigungen** aus, und klicken Sie dann auf **Administratoreinwilligung gewähren**.
5. Überprüfen Sie sorgfältig die Berechtigungen, die die Anwendung anfordert.
6. Erteilen Sie die Einwilligung, wenn Sie mit den angeforderten Berechtigungen der Anwendung einverstanden sind. Klicken Sie andernfalls auf **Abbrechen**, oder schließen Sie das Fenster.

### <a name="grant-admin-consent-in-app-registrations"></a>Erteilen der Administratoreinwilligung in App-Registrierungen

Bei Anwendungen, die Ihre Organisation entwickelt hat, oder die direkt in Ihrem Azure AD-Mandant registriert sind, können Sie auch eine mandantenweite Administratoreinwilligung in **App-Registrierungen** im Azure-Portal erteilen.

So erteilen Sie die mandantenweite Administratoreinwilligung in **App-Registrierungen**:

1. Melden Sie sich als [globaler Administrator](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator), als [Anwendungsadministrator](../users-groups-roles/directory-assign-admin-roles.md#application-administrator) oder als [Cloudanwendungsadministrator](../users-groups-roles/directory-assign-admin-roles.md#cloud-application-administrator) beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **Azure Active Directory** und anschließend auf **App-Registrierungen**.
3. Wählen Sie die Anwendung aus, der Sie eine mandantenweite Administratoreinwilligung erteilen möchten.
4. Wählen Sie **API-Berechtigungen** aus, und klicken Sie dann auf **Administratoreinwilligung erteilen**.
5. Überprüfen Sie sorgfältig die Berechtigungen, die die Anwendung anfordert.
6. Erteilen Sie die Einwilligung, wenn Sie mit den angeforderten Berechtigungen der Anwendung einverstanden sind. Klicken Sie andernfalls auf **Abbrechen**, oder schließen Sie das Fenster.

## <a name="construct-the-url-for-granting-tenant-wide-admin-consent"></a>Erstellen der URL zum Erteilen der mandantenweiten Administratoreinwilligung

Wenn Sie mithilfe einer der oben beschriebenen Methoden die mandantenweite Administratoreinwilligung erteilen, öffnet sich im Azure-Portal ein Fenster mit der Aufforderung zur mandantenweiten Administratoreinwilligung. Wenn Sie die Client-ID (auch Anwendungs-ID) der Anwendung kennen, können Sie die gleiche URL erstellen, um die mandantenweite Administratoreinwilligung zu erteilen.

Die URL für die mandantenweite Administratoreinwilligung verfügt über folgendes Format:

    https://login.microsoftonline.com/{tenant-id}/adminconsent?client_id={client-id}

Dabei gilt:

* `{client-id}` ist die Client-ID der Anwendung, auch App-ID.
* `{tenant-id}` ist die Mandanten-ID Ihrer Organisation oder ein beliebiger verifizierter Domänenname.

Überprüfen Sie vor dem Erteilen einer Einwilligung wie immer sorgfältig die Berechtigungen, die von einer Anwendung angefordert werden.

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren der Art und Weise, wie Endbenutzer Anwendungen zustimmen können](configure-user-consent.md)

[Konfigurieren des Workflows für die Administratoreinwilligung](configure-admin-consent-workflow.md)

[Berechtigungen und Zustimmung im Microsoft Identity Platform-Endpunkt](../develop/active-directory-v2-scopes.md)

[Azure AD bei StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)

---
title: Anpassen von RDP-Eigenschaften mit PowerShell – Azure
description: Vorgehensweise zum Anpassen von RDP-Eigenschaften für Windows Virtual Desktop mit PowerShell-Cmdlets
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 66b76fcdd9729b2a92ea2d561c740dbe148e0bbe
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/30/2020
ms.locfileid: "82611550"
---
# <a name="customize-remote-desktop-protocol-properties-for-a-host-pool"></a>Anpassen der Remotedesktopprotokoll-Eigenschaften für einen Hostpool

>[!IMPORTANT]
>Dieser Artikel gilt für das Update vom Frühjahr 2020 mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie das Windows Virtual Desktop-Release vom Herbst 2019 ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/customize-rdp-properties-2019.md).
>
> Das Windows Virtual Desktop-Update vom Frühjahr 2020 befindet sich derzeit in der öffentlichen Vorschauphase. Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. 
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Durch Anpassen von RDP-Eigenschaften (Remotedesktopprotokoll) eines Hostpools, z.B. Unterstützung mehrerer Monitore und Audioumleitung, können Sie für Benutzer eine optimale Bedienumgebung bereitstellen, die deren Anforderungen entspricht. Sie können RDP-Eigenschaften in Windows Virtual Desktop anpassen, indem Sie entweder das Azure-Portal oder den Parameter *-CustomRdpProperty* im Cmdlet **Update-AzWvdHostPool** verwenden.

Eine vollständige Liste der unterstützten Eigenschaften samt deren Standardwerten finden Sie unter [Unterstützte RDP-Dateieinstellungen](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/rdp-files?context=/azure/virtual-desktop/context/context).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen, befolgen Sie die Anweisungen unter [Einrichten des Windows Virtual Desktop PowerShell-Moduls](powershell-module.md), um Ihr PowerShell-Modul einzurichten und sich bei Azure anzumelden.

## <a name="default-rdp-properties"></a>RDP-Standardeigenschaften

Veröffentlichte RDP-Dateien enthalten standardmäßig die folgenden Eigenschaften:

|RDP-Eigenschaften | Desktops | RemoteApps |
|---|---| --- |
| Mehrfachmonitor-Modus | Aktiviert | – |
| Aktivierte Laufwerksumleitungen | Laufwerke, Zwischenablage, Drucker, COM-Anschlüsse, USB-Geräte und Smartcards| Laufwerke, Zwischenablage und Drucker |
| Remoteaudio-Modus | Lokale Wiedergabe | Lokale Wiedergabe |

Alle benutzerdefinierten Eigenschaften, die Sie für den Hostpool definieren, überschreiben diese Standardeinstellungen.

## <a name="configure-rdp-properties-in-the-azure-portal"></a>Konfigurieren von RDP-Eigenschaften im Azure-Portal

So konfigurieren Sie RDP-Eigenschaften im Azure-Portal:

1. Melden Sie sich unter <https://portal.azure.com> bei Azure an.
2. Geben Sie **windows virtual desktop** in die Suchleiste ein.
3. Wählen Sie unter „Dienste“ die Option **Windows Virtual Desktop** aus.
4. Wählen Sie auf der Seite „Windows Virtual Desktop“ im Menü auf der linken Seite **Hostpools** aus.
5. Wählen Sie **den Namen des Hostpools** aus, den Sie aktualisieren möchten.
6. Wählen Sie im Menü auf der linken Bildschirmseite **Eigenschaften** aus.
7. Wählen Sie **RDP-Einstellungen** aus, um die RDP-Eigenschaften zu bearbeiten.
8. Wenn Sie fertig sind, wählen Sie **Speichern** aus, um Ihre Änderungen zu speichern.

Wenn eine Einstellung, die Sie bearbeiten möchten, im Menü „RDP-Einstellungen“ nicht angezeigt wird, müssen Sie sie manuell bearbeiten, indem Sie Cmdlets in PowerShell ausführen. In den nächsten Abschnitten erfahren Sie, wie Sie benutzerdefinierte RDP-Eigenschaften manuell in PowerShell bearbeiten.

## <a name="add-or-edit-a-single-custom-rdp-property"></a>Hinzufügen oder Bearbeiten einer einzelnen benutzerdefinierten RDP-Eigenschaft

Wenn Sie eine einzelne benutzerdefinierte RDP-Eigenschaft hinzufügen oder bearbeiten möchten, führen Sie das folgende PowerShell-Cmdlet aus:

```powershell
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -CustomRdpProperty <property>
```

Führen Sie dieses Cmdlet aus, um zu überprüfen, ob das soeben ausgeführte Cmdlet die Eigenschaft aktualisiert hat:

```powershell
Get-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> | format-list Name, CustomRdpProperty

Name              : <hostpoolname>
CustomRdpProperty : <customRDPpropertystring>
```

Wenn Sie z. B. die Eigenschaft „audiocapturemode“ für einen Hostpool mit dem Namen 0301HP überprüfen möchten, geben Sie dieses Cmdlet ein:

```powershell
Get-AzWvdHostPool -ResourceGroupName 0301rg -Name 0301hp | format-list Name, CustomRdpProperty

Name              : 0301HP
CustomRdpProperty : audiocapturemode:i:1;
```

## <a name="add-or-edit-multiple-custom-rdp-properties"></a>Hinzufügen oder Bearbeiten von mehreren benutzerdefinierten RDP-Eigenschaften

Wenn Sie mehrere benutzerdefinierte RDP-Eigenschaften hinzufügen oder bearbeiten möchten, führen Sie die folgenden PowerShell-Cmdlets aus, wobei Sie die benutzerdefinierten RDP-Eigenschaften als eine Zeichenfolge mit Semikolons als Trennzeichen angeben:

```powershell
$properties="<property1>;<property2>;<property3>" 
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -CustomRdpProperty $properties 
```

Sie können überprüfen, ob die RDP-Eigenschaft hinzugefügt wurde, indem Sie das folgende Cmdlet ausführen:

```powershell
Get-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> | format-list Name, CustomRdpProperty 

Name              : <hostpoolname>
CustomRdpProperty : <customRDPpropertystring>
```

Wenn Sie basierend auf unserem vorherigen Cmdlet-Beispiel mehrere RDP-Eigenschaften für den Hostpool 0301HP einrichten, sieht Ihr Cmdlet wie folgt aus:

```powershell
Get-AzWvdHostPool -ResourceGroupName 0301rg -Name 0301hp | format-list Name, CustomRdpProperty 

Name              : 0301HP 
CustomRdpProperty : audiocapturemode:i:1;audiomode:i:0;
```

## <a name="reset-all-custom-rdp-properties"></a>Zurücksetzen aller benutzerdefinierten RDP-Eigenschaften

Sie können eine einzelne benutzerdefinierte RDP-Eigenschaft auf deren Standardwert zurücksetzen, indem Sie den Anweisungen unter [Hinzufügen oder Bearbeiten einer einzelnen benutzerdefinierten RDP-Eigenschaft](#add-or-edit-a-single-custom-rdp-property) folgen, oder Sie können alle benutzerdefinierten RDP-Eigenschaften für einen Hostpool zurücksetzen, indem Sie das folgende PowerShell-Cmdlet ausführen:

```powershell
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -CustomRdpProperty ""
```

Um sicherzustellen, dass Sie die Einstellung erfolgreich entfernt haben, geben Sie dieses Cmdlet ein:

```powershell
Get-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> | format-list Name, CustomRdpProperty 

Name              : <hostpoolname> 
CustomRdpProperty : <CustomRDPpropertystring>
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die RDP-Eigenschaften eines bestimmten Hostpools angepasst haben, können Sie sich bei einem Windows Virtual Desktop-Client anmelden, um die Einstellungen als Teil einer Benutzersitzung zu testen. In den nächsten Vorgehensweisen erfahren Sie, wie Sie mit dem Client Ihrer Wahl eine Verbindung mit einer Sitzung herstellen:

- [Herstellen einer Verbindung mit dem Windows-Desktopclient](connect-windows-7-and-10.md)
- [Herstellen einer Verbindung mit dem Webclient](connect-web.md)
- [Herstellen einer Verbindung mit dem Android-Client](connect-android.md)
- [Herstellen einer Verbindung mit dem macOS-Client](connect-macos.md)
- [Herstellen einer Verbindung mit dem iOS-Client](connect-ios.md)
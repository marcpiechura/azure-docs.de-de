---
title: Verwenden von Desired State Configuration mit VM-Skalierungsgruppen
description: Verwenden von VM-Skalierungsgruppen mit der Azure Desired State Configuration-Erweiterung zum Konfigurieren von virtuellen Computern.
author: ju-shim
ms.author: jushiman
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: extensions
ms.date: 04/05/2017
ms.reviewer: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: 4a972cab0559ff8a4bb22588c712515daa2fab16
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2020
ms.locfileid: "83124840"
---
# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Verwenden von VM-Skalierungsgruppen mit der Azure DSC-Erweiterung
[VM-Skalierungsgruppen](virtual-machine-scale-sets-overview.md) können mit dem Erweiterungshandler [Azure-Konfiguration des gewünschten Zustands](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) verwendet werden. VM-Skalierungsgruppen bieten eine Möglichkeit, eine große Anzahl von virtuellen Computern bereitzustellen und zu verwalten, und lassen sich je nach Auslastung elastisch auf- und abskalieren. DSC dient zum Konfigurieren der VMs, sobald sie online geschaltet wurden, damit sie in der Produktionssoftware ausgeführt werden.

## <a name="differences-between-deploying-to-virtual-machines-and-virtual-machine-scale-sets"></a>Unterschiede zwischen der Bereitstellung in virtuellen Computern und VM-Skalierungsgruppen
Die zugrunde liegende Vorlagenstruktur für eine VM-Skalierungsgruppe unterscheidet sich geringfügig von einem einzelnen virtuellen Computer. Ein Punkt ist, dass bei einer einzelnen VM Erweiterungen unter dem Knoten „virtualMachines“ bereitgestellt werden. Es gibt einen Eintrag des Typs „Extensions“. Hier wird DSC der Vorlage hinzugefügt.

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

Ein Knoten einer VM-Skalierungsgruppe weist den Abschnitt „properties“ mit dem Attribut „VirtualMachineProfile“, „extensionProfile“ auf. DSC wird unter „extensions“ hinzugefügt.

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-a-virtual-machine-scale-set"></a>Verhalten bei einer VM-Skalierungsgruppe
Das Verhalten bei einer VM-Skalierungsgruppe entspricht dem Verhalten bei einem einzelnen virtuellen Computer. Beim Erstellen ein neues virtuellen Computers wird er automatisch mit der DSC-Erweiterung bereitgestellt. Wenn eine neuere Version des WMF von der Erweiterung angefordert wird, wird die VM neu gestartet, ehe sie online geschaltet wird. Sobald sie online ist, lädt sie die ZIP-Datei mit der DSC-Konfiguration herunter und stellt sie auf dem virtuellen Computer bereit. Weitere Informationen finden Sie in der [Übersicht über die Azure DSC-Erweiterung](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die [Azure Resource Manager-Vorlage für die DSC-Erweiterung](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)an.

Erfahren Sie, [wie die DSC-Erweiterung Anmeldeinformationen sicher verarbeitet](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Weitere Informationen zum Azure DSC-Erweiterungs-Handler finden Sie unter [Einführung in den Handler der Azure-Erweiterung zum Konfigurieren des gewünschten Zustands](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Weitere Informationen zu PowerShell DSC finden Sie im [PowerShell-Dokumentationscenter](/powershell/scripting/dsc/overview/overview). 


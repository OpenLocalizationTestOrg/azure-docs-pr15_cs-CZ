<properties
   pageTitle="Vyplňování šablona správce stavu konfigurace zdroje | Microsoft Azure"
   description="Definice šablony správce prostředků pro požadované konfigurace stavu v Azure s příklady a řešení potíží"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a>Windows VMSS a vyplňování konfigurace stavu šablonami správce prostředků Azure
Tento článek popisuje šabloně správce prostředků pro [rutiny rozšíření žádoucí konfigurace stavu](virtual-machines-windows-extensions-dsc-overview.md). 

## <a name="template-example-for-a-windows-vm"></a>Příklad šablony pro Windows OM

Následující úryvek přejde do části zdroje šabloně.

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }

```

## <a name="template-example-for-windows-vmss"></a>Příklad šablony pro VMSS systému Windows

VMSS uzel má oddíl "vlastnosti" "VirtualMachineProfile", "extensionProfile" atribut. DSC se přidá ve skupinovém rámečku "rozšíření". 

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": true,
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

## <a name="detailed-settings-information"></a>Podrobné informace o nastavení informace

Následující schéma je určený pro nastavení část koncovku Azure DSC v šabloně aplikace Správce prostředků Azure.

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  } 
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a>Podrobnosti
| Název vlastnosti | Typ | Popis |
| --- | --- | --- |
| settings.wmfVersion | řetězec | Určuje verzi produktu Windows Management Framework nainstalovaného na vaše OM. Nastavení této vlastnosti na nejnovější"instalace nejčastěji aktualizovanou verzi WMF. Jen aktuální možné hodnoty této vlastnosti jsou **"4.0", "5.0", "5.0PP' a"nejnovější"**. Tyto možné hodnoty se vztahují aktualizace. Výchozí hodnota je "nejnovější".|
| Settings.Configuration.URL | řetězec | Určuje adresu URL umístění, ze kterého si chcete stáhnout DSC konfigurační zip soubor. Pokud adresu URL poskytne vyžaduje token přidružení zabezpečení pro přístup k, budete muset nastavit vlastnost protectedSettings.configurationUrlSasToken hodnotu tokenu přidružení zabezpečení. Tato vlastnost se když jsou definované settings.configuration.script a/nebo settings.configuration.function. |
| Settings.Configuration.Script | řetězec | Určuje název souboru skript, který obsahuje definici konfiguraci DSC. Tento skript musí být v kořenové složce zip soubor stáhnout z adresa URL zadaná ve vlastnosti configuration.url. Tato vlastnost se když jsou definované settings.configuration.url a/nebo settings.configuration.script. |
| Settings.Configuration.Function | řetězec | Určuje název DSC konfigurace. Konfigurace s názvem musí být součástí skript definované configuration.script. Tato vlastnost se když jsou definované settings.configuration.url a/nebo settings.configuration.function. |
| settings.configurationArguments | Kolekce | Definuje všechny parametry, které chcete předat konfiguraci DSC. Tato vlastnost není zašifrován. |
| settings.configurationData.url | řetězec | Určuje adresu URL ze které chcete stáhnout konfiguračního souboru dat (.pds1) použijte jako vstup pro konfiguraci vašeho DSC. Pokud adresu URL poskytne vyžaduje token přidružení zabezpečení pro přístup k, budete muset nastavit vlastnost protectedSettings.configurationDataUrlSasToken hodnotu tokenu přidružení zabezpečení.|
| settings.privacy.dataEnabled | řetězec | Povolí nebo zakáže telemetrie kolekce. Možná pouze hodnoty této vlastnosti jsou **Povolit, "Zakázat", ", nebo $null**. Tato vlastnost necháte prázdné nebo obsahovat hodnotu null umožňuje telemetrie. Výchozí hodnota je ". [Další informace](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings | Kolekce | Definuje alternativní umístění, ze které chcete stáhnout WMF. [Další informace](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments | Kolekce | Definuje všechny parametry, které chcete předat konfiguraci DSC. Tato vlastnost je zašifrován. |
| protectedSettings.configurationUrlSasToken | řetězec | Určuje token přidružení zabezpečení pro přístup k URL definované configuration.url. Tato vlastnost je zašifrován. |
| protectedSettings.configurationDataUrlSasToken | řetězec | Určuje token přidružení zabezpečení pro přístup k URL definované configurationData.url. Tato vlastnost je zašifrován. |

## <a name="settings-vs-protectedsettings"></a>Nastavení porovnání ProtectedSettings
Všechna nastavení se ukládají do textového souboru nastavení v OM.
Vlastnosti v části "nastavení" jsou veřejné vlastnosti, protože nejsou šifrované v nastavení textový soubor.
Vlastnosti v části "protectedSettings" jsou šifrované pomocí certifikátu a nejsou zobrazeny ve formátu prostého textu v tomto souboru na OM.

Pokud konfigurace vyžaduje přihlašovací údaje, může být součástí protectedSettings:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a>Příklad

V následujícím příkladu je odvozena z části "Začínáme" [stránka DSC rozšíření rutiny přehled](virtual-machines-windows-extensions-dsc-overview.md).
Tento příklad používá správce prostředků šablony místo rutiny pro nasazení rozšíření. Uložení konfigurace "IisInstall.ps1", umístěte ji v. ZIP souboru a nahrajte soubor přístupných osobám s postižením adresy URL. Tento příklad používá úložišti objektů blob Azure, ale je možné stáhnout. ZIP soubory z libovolného umístění.

Následující kód v šabloně správce prostředků Azure pokyn OM správné soubor stáhnout a spustit funkci vhodná PowerShell:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
    } 
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-the-previous-format"></a>Aktualizace z v předchozím formátu
Nastavení ve starším formátu (obsahující veřejné vlastnosti ModulesUrl, ConfigurationFunction, SasToken nebo vlastnosti) automaticky upravit na aktuální formát a spusťte stejně jako před.

Následující schéma je jaké předchozí nastavení schématu vypadalo:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1" 
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": { 
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Tady je, jak v předchozím formátu přizpůsobuje na současný formát:

| Název vlastnosti | Předchozí ekvivalent schématu |
| --- | --- |
| settings.wmfVersion | nastavení. WMFVersion |
| Settings.Configuration.URL | nastavení. ModulesUrl |
| Settings.Configuration.Script | První část nastavení. ConfigurationFunction (před "\\\\") |
| Settings.Configuration.Function | Druhá část nastavení. ConfigurationFunction (po "\\\\") |
| settings.configurationArguments | nastavení. Vlastnosti |
| settings.configurationData.url | protectedSettings.DataBlobUri (bez přidružení zabezpečení token) |
| settings.privacy.dataEnabled | nastavení. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings | nastavení. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments | protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken | nastavení. SasToken |
| protectedSettings.configurationDataUrlSasToken | Přidružení zabezpečení token z protectedSettings.DataBlobUri |


## <a name="troubleshooting---error-code-1100"></a>Poradce při potížích – kód chyby 1100
Kód chyby 1100 označuje, že došlo k potížím s vstup DSC rozšíření.
Text tyto chyby je proměnná a může se změnit.
Tady jsou některé chyby, které můžou nastat a jak je vyřešit.

### <a name="invalid-values"></a>Neplatné hodnoty
"Privacy.dataCollection, je {0}. Možná pouze hodnoty jsou ","Povolení"a"Zakázat"" "WmfVersion, je {0}. Pouze možné hodnoty jsou... a "nejnovější" "

Problém: Zadaná hodnota není povolená.

Řešení: Změna neplatnou hodnotu na platnou hodnotu. Najdete v tabulce v části Podrobnosti.

### <a name="invalid-url"></a>Neplatné adresy URL
"ConfigurationData.url, je {0}. Toto není platná adresa URL""DataBlobUri, je {0}. Toto není platná adresa URL""Configuration.url, je {0}. Toto není platná adresa URL"

Problém: A za předpokladu, že adresa URL není platná.

Řešení: Zaškrtněte všechny zadané URL. Zkontrolujte, že všechny adresy URL překládal platná umístění, že přípona přístup ve vzdáleném počítači.

### <a name="invalid-configurationargument-type"></a>Neplatný ConfigurationArgument typ
"Neplatné configurationArguments zadejte {0}"

Problém: K objektu Hashtable nerozpoznává vlastnost ConfigurationArguments. 

Řešení: Zkontrolujte vlastnost ConfigurationArguments Hashtable. Postupujte podle předchozí příklad formát. Pozor na nabídek, čárky a závorky.

### <a name="duplicate-configurationarguments"></a>Duplikování ConfigurationArguments
"Ve veřejných a chráněné configurationArguments nebyl nalezen duplicitní argumenty {0}."

Problém: ConfigurationArguments v nastavení na veřejném a ConfigurationArguments v nastavení chráněného obsahují vlastnosti se stejným názvem.

Řešení: Odeberte jednu duplicitní vlastnosti.

### <a name="missing-properties"></a>Chybějící vlastnosti
"Configuration.function vyžaduje configuration.url nebo configuration.module je"

"Configuration.url vyžaduje že není zadán, které configuration.script"

"Configuration.script vyžaduje že není zadán, které configuration.url"

"Configuration.url vyžaduje že není zadán, které configuration.function"

"ConfigurationUrlSasToken vyžaduje že není zadán, které configuration.url"

"ConfigurationDataUrlSasToken vyžaduje že není zadán, které configurationData.url"

Problém: Definovaný vlastností, musí další vlastnost, která není k dispozici.

Řešení: 
- Zadejte chybějící vlastnosti.
- Odebrání vlastnost, která potřebuje chybějící vlastnosti.


## <a name="next-steps"></a>Další kroky
Další informace o DSC a měřítko virtuálního počítače sady v [Použití virtuálního počítače měřítko sad s příponou DSC Azure](../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)

Najdete další informace o [řízení na DSC přihlašovacích údajů zabezpečeného](virtual-machines-windows-extensions-dsc-credentials.md). 

Další informace o rutiny rozšíření Azure DSC najdete v článku [Úvod do rozšíření rutiny konfigurace stavu požadované Azure](virtual-machines-windows-extensions-dsc-overview.md). 

Další informace o prostředí PowerShell DSC, [Navštěvujte blog o Centru si přečtěte následující dokumentaci Powershellu](https://msdn.microsoft.com/powershell/dsc/overview). 

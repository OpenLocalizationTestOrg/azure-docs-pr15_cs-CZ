<properties
   pageTitle="Vlastní skripty na Linux VMs | Microsoft Azure"
   description="Automatizace úkolů konfigurace Linux OM pomocí vlastní skript rozšíření"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="nepeters"/>

# <a name="using-the-azure-custom-script-extension-with-linux-virtual-machines"></a>Rozšíření Azure vlastní skript pomocí Linux virtuálních počítačích

Vlastní skript linky a soubory ke stažení spustí skripty na Azure virtuálních počítačích. Tuto linku je užitečné pro nasazení konfigurace příspěvku, instalace softwaru nebo jiná konfigurace a správy úkolů. Skripty můžete stáhnout z Azure úložiště nebo jiných přístupných osobám s postižením umístění v Internetu nebo uvedených v článku rozšíření čas spuštění. Rozšíření vlastní skript lze integrovat s správce prostředků Azure šablony a můžete taky spustit pomocí rozhraní příkazového řádku Azure, Powershellu, Azure portál nebo REST API Azure virtuálního počítače.

Tento dokument podrobné informace o používání koncovku vlastní skript z Azure rozhraní příkazového řádku a správce prostředků Azure šablony a taky podrobné informace o řešení potíží v systémech Linux.

## <a name="extension-configuration"></a>Konfigurace rozšíření

Konfigurace vlastní skript rozšíření určuje, například umístění skriptu a příkaz spustit. Toto nastavení můžete uložené v souborech konfigurace, zadali do příkazového řádku nebo v šabloně aplikace Správce prostředků Azure. Citlivá data mohou být uložena v zamknutém konfiguraci, která je zašifrován a pouze dešifrovat do virtuálního počítače. Chráněné konfigurace je užitečná, když spuštění příkazu obsahuje tajemství například hesla.

### <a name="public-configuration"></a>Veřejné konfigurace

Schéma:

- **commandToExecute**: (potřeby řetězec) skript čárky položky
- **fileUris**: (volitelné, řetězců) adresy URL pro soubory ke stažení.
- **časové razítko** (volitelné, integer) umožňuje v tomto poli pouze aktivovat znovu spustit skriptu změnou hodnoty tohoto pole.

```none
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Chráněné konfigurace

Schéma:

- **commandToExecute**: (volitelné, řetězec) skript čárky položky. Použijte toto pole Pokud váš příkaz obsahuje tajemství například hesla.
- **storageAccountName**: (volitelné, řetězec) název účtu úložiště. Pokud chcete zadat úložiště pověření, musí být všechny fileUris adresy URL pro objekty BLOB Azure.
- **storageAccountKey**: (volitelné, řetězec) přístupová klávesa účtu úložiště.


```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Azure rozhraní příkazového řádku

Při používání rozhraní příkazového řádku Azure spustit vlastní skript rozšíření, vytvořte konfigurační soubor nebo soubory obsahující minimálně uri souboru a spuštění příkazu skriptu.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /scirpt-config.json
```

V případě potřeby příkazu lze spustit pomocí `--public-config` a `--private-config` možnost, která umožňuje konfiguraci být zadán při spuštění a bez samostatné konfiguračního souboru.

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config '{"fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],"commandToExecute": "./hello.sh"}'
```

### <a name="azure-cli-examples"></a>Příklady Azure rozhraní příkazového řádku

**Příklad 1** – veřejné konfigurace s soubor skriptu.

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
  "commandToExecute": "./hello.sh"
}
```

Příkaz Azure rozhraní příkazového řádku:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Příklad 2** – veřejné konfigurace s žádné soubor skriptu.

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Příkaz Azure rozhraní příkazového řádku:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path /public.json
```

**Příklad 3** – veřejné konfiguračního souboru slouží k určení soubor skriptu URI a chráněné konfiguračního souboru slouží k určení na příkaz spustit.

Veřejné konfiguračního souboru:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"],
}
```

Chráněný konfiguračního souboru:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Příkaz Azure rozhraní příkazového řádku:

```none
azure vm extension set <resource-group> <vm-name> CustomScript Microsoft.Azure.Extensions 2.0 --auto-upgrade-minor-version --public-config-path ./public.json --private-config-path ./protected.json
```

## <a name="resource-manager-template"></a>Správce prostředků šablony

Rozšíření Azure vlastní skript poběží v době nasazení virtuálního počítače pomocí šablony správce prostředků. Postup přidání správně formátované JSON nasazení šablony.

### <a name="resource-manager-examples"></a>Příklady správce prostředků

**Příklad 1** – veřejné konfigurace.

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

**Příklad 2** – spuštění příkazu chráněné konfigurace.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

V tématu .net Core hudbu úložiště ukázku kompletní příklad – [Ukázka hudbu úložiště přihlašovacích údajů](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).

## <a name="troubleshooting"></a>Řešení potíží

Při spuštění koncovku skript vlastní skript vytvořili či stáhli dříve do adresáře podobně jako v následujícím příkladu. Výstup příkazu je rovněž uloženo do tohoto adresáře `stdout` a `stderr` soubor.

```none
/var/lib/azure/custom-script/download/0/
```

Rozšíření skript Azure vytvoří protokol, které tady najdete.

```none
/var/log/azure/customscript/handler.log
```

Stav provádění koncovku vlastní skript můžete taky pomocí rozhraní příkazového řádku Azure načíst.

```none
azure vm extension get <resource-group> <vm-name>
```

Výstup vypadá jako následující text:

```none
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Další kroky

Informace o jiných rozšíření skript OM najdete v tématu [Přehled Azure skript rozšíření Linux](./virtual-machines-linux-extensions-features.md).
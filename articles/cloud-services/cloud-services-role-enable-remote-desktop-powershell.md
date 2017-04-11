<properties
pageTitle="Povolit připojení ke vzdálené ploše pro roli v Azure Cloud Services pomocí prostředí PowerShell"
description="Jak nakonfigurovat aplikaci služby azure cloudu pomocí prostředí PowerShell povolit připojení ke vzdálené ploše"
services="cloud-services"
documentationCenter=""
authors="thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/05/2016"
ms.author="adegeo"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>Povolit připojení ke vzdálené ploše pro roli v Azure Cloud Services pomocí prostředí PowerShell

>[AZURE.SELECTOR]
- [Azure klasické portálu](cloud-services-role-enable-remote-desktop.md)
- [Prostředí PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Vzdálené plochy umožňuje přístup k plochy spuštěné v Azure role. Připojení ke vzdálené ploše slouží k řešení potíží s a Diagnostika problémů s aplikací je spuštěná.

Tento článek popisuje, jak povolit vzdálená plocha na vaše role služby cloudu pomocí Powershellu. Zjistěte, [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) pro požadavcích pro tento článek. Prostředí PowerShell využívá koncovku vzdálené plochy, můžete povolit vzdálené plochy po nasazení aplikace.


## <a name="configure-remote-desktop-from-powershell"></a>Konfigurace připojení ke vzdálené ploše z prostředí PowerShell

Rutina [Set-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) umožňuje povolit vzdálená plocha na určité role nebo všechny úlohy nasazení služby cloudu. Rutiny vám umožní určit uživatelské jméno a heslo pro vzdálené plochy uživatele pomocí *přihlašovacích údajů* parametr, který přijme PSCredential objektu.

Používáte prostředí PowerShell interaktivně, můžete snadno objektu můžete nastavit PSCredential tak, že zavoláte rutinu [Get-pověření](https://technet.microsoft.com/library/hh849815.aspx) .

```
$remoteusercredentials = Get-Credential
```

Tento příkaz zobrazí dialogové okno umožňující zadejte uživatelské jméno a heslo pro vzdálené uživatele zabezpečené způsobem.

Protože prostředí PowerShell pomáhá v automatizaci scénáře, můžete nastavit taky objekt **PSCredential** způsobem, který nevyžaduje interakci uživatele. Nejdřív budete muset nastavit hesla. Začněte s zadání hesla ve formátu prostého textu převést na zabezpečené řetězce pomocí [ConvertTo SecureString](https://technet.microsoft.com/library/hh849818.aspx). Pak budete muset převést zabezpečené řetězec šifrovaného standardní řetězce pomocí [ConvertFrom SecureString](https://technet.microsoft.com/library/hh849814.aspx). Teď můžete uložit šifrované standardní řetězec do souboru pomocí [Nastavení obsahu](https://technet.microsoft.com/library/ee176959.aspx).

Můžete také vytvořit soubor zabezpečené heslo tak, že nebudete muset pokaždé, když zadejte do pole heslo. Zabezpečené heslo soubor je také lepší než soubor ve formátu prostého textu. Vytvoření souboru zabezpečené heslo pomocí následující Powershellu:

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

>[AZURE.IMPORTANT] Po nastavení hesla, ujistěte se, že splníte [požadavky na složitost](https://technet.microsoft.com/library/cc786468.aspx).

Vytvoření objektu přihlašovací údaje ze souboru zabezpečené heslo, musíte číst obsah souboru a převést zpět do zabezpečeného řetězce pomocí [ConvertTo SecureString](https://technet.microsoft.com/library/hh849818.aspx).

Rutina [Set-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) zpracuje také *vypršení platnosti* parametr, který určuje **datum a čas** , kdy vyprší jejich platnost uživatelského účtu. Je třeba nastavit účet platnosti několik dnů od aktuálního data a času.

V tomto příkladu prostředí PowerShell ukazuje, jak nastavit koncovku Vzdálená plocha na do cloudové služby:

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
Můžete taky Volitelně určete nasazení úsek a role, které chcete povolit vzdálená plocha na. Pokud nejsou zadány tyto parametry, umožňuje rutinu Vzdálená plocha na všech rolí v úsek nasazení **výroby** .

Přípona vzdálené plochy je přidružená k na nasazení. Pokud vytvoříte nové nasazení služby, je třeba povolit vzdálená plocha na tomto nasazení. Pokud chcete vždy mít povolena Vzdálená plocha, měli zvážit, integrace skriptů Powershellu do pracovních postupů nasazení.


## <a name="remote-desktop-into-a-role-instance"></a>Vzdálená plocha do instanci rolí
Rutina [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) slouží k Vzdálená plocha na konkrétní rolí výskyt cloudové služby. Použít parametr *LocalPath* ke stažení souboru RDP místně. Nebo můžete použít parametr *spuštění* přímo spustit dialogové okno připojení ke vzdálené ploše pro přístup k instanci rolí služby cloudu.

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Zkontrolujte, zda je povolena rozšíření Vzdálená plocha na službu
Rutina [Get-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495261.aspx) slouží k zobrazení, která povolit nebo zakázat o nasazení služeb Vzdálená plocha. Rutiny vrátí uživatelské jméno pro vzdálené plochy uživatelů a rolí, které vzdálené plochy rozšíření aktivované řešení. Ve výchozím nastavení se stane s úsek nasazení a můžete místo toho použít pracovní pozici.

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>Odebrání vzdálené plochy rozšíření od služby
Pokud už povolili koncovku vzdálené plochy na nasazení a potřebujete aktualizovat nastavení vzdálené plochy, nejdřív odeberte rozšíření. A znovu povolit nové nastavení. Například pokud chcete nastavit nový vypršení platnosti hesla vzdálený uživatelský účet nebo účet. To je potřeba na existující nasazení s příponou vzdálené plochy povolené. Nové nasazení můžete jednoduše použít rozšíření přímo.

Chcete-li odebrat vzdálené plochy rozšíření nasazení, můžete použít rutinu [AzureServiceRemoteDesktopExtension odebrat](https://msdn.microsoft.com/library/azure/dn495280.aspx) . Můžete taky můžete také určit nasazení úsek a role, ze kterého chcete odebrat koncovku vzdálené plochy.

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

>[AZURE.NOTE] Konfigurace rozšíření úplně odebrat, je potřeba zavolat rutinu *Odebrat* s parametrem **UninstallConfiguration** .
>
>Parametr **UninstallConfiguration** odinstaluje jakékoli konfigurace linky, které je použité pro službu. Konfigurace služby přidružen každé konfigurace rozšíření. Volání rutinu *Odebrat* bez **UninstallConfiguration** zrušíte <mark>nasazení</mark> z konfigurace rozšíření, tedy účinně odebrání rozšíření. Konfigurace rozšíření však zůstává přidružené ke službě.



## <a name="additional-resources"></a>Další zdroje informací

[Postup při konfiguraci Cloudovým službám](cloud-services-how-to-configure.md)

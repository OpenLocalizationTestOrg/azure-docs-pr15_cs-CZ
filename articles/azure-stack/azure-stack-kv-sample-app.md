<properties
    pageTitle="Povolit aplikaci revtrieve Azure zásobníku klíč trezoru tajemství | Microsoft Azure"
    description="Použití vzorku aplikace pro práci s Azure zásobníku klíč trezoru"
    services="azure-stack"
    documentationCenter=""
    authors="rlfmendes"
    manager="natmack"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="ricardom"/>

# <a name="run-the-sample-application-for-key-vault"></a>Spusťte aplikaci vzorku pro trezoru klíč 

V této příručce použijete ukázku aplikace k načtení tajemství a hesla z trezoru klíče.

## <a name="download-the-samples-and-prepare"></a>Stáhněte si ukázky a připravit

Stažení ukázek klienta Azure klíč trezoru z [trezoru klíč Azure klienta ukázky stránky](https://www.microsoft.com/en-us/download/details.aspx?id=45343).

Extrahování obsahu souboru ZIP do místního počítače.

Přečtěte si soubor **README.md** (Toto je textového souboru) a pak postupujte podle pokynů.

## <a name="run-sample-1--hellokeyvault"></a>Spuštění ukázkové #1 – HelloKeyVault
HelloKeyVault je konzoly aplikace, která provede uvedené hlavní příklady situací, které jsou podporovány trezoru klíč:

  1. Vytvoření a import klávesu HSM nebo software
  2. Šifrování tajná pomocí obsahu
  3. Zalomit klávesu obsahu pomocí klávesy trezoru klíče
  4. Zrušit zalomení klávesu obsahu
  5. Dešifrování tajná
  6. Nastavit tajná

Aplikace konzoly by měla běžet s beze změn, s tím rozdílem, že správné konfiguraci nastavení App.Config bude aktualizován podle následujících kroků:

1. Aktualizace nastavení konfigurace aplikací v HelloKeyVault\App.config s trezoru URL, hlavní ID aplikace a tajná. Informace o můžete volitelně generovaný pomocí **scripts\GetAppConfigSettings.ps1**.
2. Aktualizujte hodnoty povinné proměnné GetAppConfigSettings.ps1.
3. Spusťte okně prostředí Windows PowerShell.
4. V okně prostředí PowerShell spusťte skript GetAppConfigSettings.ps1.
5. Zkopírování výsledky skriptu HelloKeyVault\App.config soubor.


## <a name="next-steps"></a>Další kroky

[Nasazení OM heslem trezoru klíč](azure-stack-kv-deploy-vm-with-secret.md)

[Nasazení OM certifikátem trezoru klíč](azure-stack-kv-push-secret-into-vm.md)
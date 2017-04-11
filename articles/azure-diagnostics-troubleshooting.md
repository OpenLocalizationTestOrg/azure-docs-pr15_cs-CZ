<properties
    pageTitle="Poradce při potížích s Azure diagnostiky"
    description="Řešení potíží při použití Azure Diagnostika v Azure Cloud Services virtuálních počítačích a "
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="robb"/>


# <a name="azure-diagnostics-troubleshooting"></a>Poradce při potížích s Azure diagnostických nástrojů
Poradce při potížích s informace týkající se používání Azure diagnostiky. Další informace o Azure diagnostiky najdete v článku [Přehled diagnostiky Azure](azure-diagnostics.md#cloud-services).

## <a name="azure-diagnostics-is-not-starting"></a>Není od Azure diagnostiky

Diagnostika se skládá ze dvou částí: sledování agenta a modulu plug-in agent pro hosty. Můžete kontrola souborů protokolu **DiagnosticsPluginLauncher.log** a **DiagnosticsPlugin.log** informace o proč diagnostiky nejde spustit.  
  
V roli cloudové služby jsou součástí protokoly pro modul plug-in agent Host: 

``` 
C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.6.3.0\ 
``` 

V Azure virtuálního počítače protokoly pro modul plug-in agent Host jsou umístěny v: 

``` 
C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.6.3.0\Logs\ 
``` 
 
Poslední řádek souborů protokolu bude obsahovat kód ukončení.  

``` 
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0 
``` 

Modul plug-in vrátí následující kódy konec:

Ukončete kód|Popis
---|---
0|Úspěšné.
-1|Obecné chyby.
-2|Nelze načíst rcf soubor.<p>Toto je vnitřní chyba, která by měla pouze příčiny Spouštěči hosta agent modul plug-in ručně vyvolání, nesprávně, v OM.
-3|Načtení konfiguračního souboru diagnostických nástrojů.<p><p>Řešení: Toto je výsledek konfigurační soubor není předávání schémat. Řešení je stanovit konfigurační soubor, který splňuje se schématem.
-4|Další instanci sledování agent diagnostiky používá adresáři místních zdrojů.<p><p>Řešení: Zadejte jinou hodnotu pro **LocalResourceDirectory**.
-6|Host agent modul plug-in Spouštěči pokusu o spuštění diagnostických nástrojů s Neplatný příkazový řádek.<p><p>Toto je vnitřní chyba, která by měla pouze příčiny Spouštěči hosta agent modul plug-in ručně vyvolání, nesprávně, v OM.
-10|Modul plug-in diagnostiky opustila ukončený kvůli neošetřené výjimce.
-11|Agent hosta nemohl vytvořit proces zodpovědný za spuštění a sledování agenta sledování.<p><p>Řešení: Ověřte, jestli jsou k dispozici pro spuštění nové procesy dostatečné systémové prostředky.<p>
-101|Neplatné argumenty při volání modul plug-in diagnostických nástrojů.<p><p>Toto je vnitřní chyba, která by měla pouze příčiny Spouštěči hosta agent modul plug-in ručně vyvolání, nesprávně, v OM.
-102|Proces modul plug-in se nepodařilo inicializace.<p><p>Řešení: Ověřte, jestli jsou k dispozici pro spuštění nové procesy dostatečné systémové prostředky.
-103|Proces modul plug-in se nepodařilo inicializace. Konkrétně nelze vytvořit objekt protokolování.<p><p>Řešení: Ověřte, jestli jsou k dispozici pro spuštění nové procesy dostatečné systémové prostředky.
-104|Nelze načíst zadaný soubor rcf agentem Host.<p><p>Toto je vnitřní chyba, která by měla pouze příčiny Spouštěči hosta agent modul plug-in ručně vyvolání, nesprávně, v OM.
-105|Modul plug-in diagnostiky nejde otevřít soubor konfigurace diagnostiky.<p><p>Toto je vnitřní chyba, která by měla pouze příčiny modul plug-in diagnostiky ručně vyvolání, nesprávně, v OM.
-106|Konfigurační soubor diagnostiky nemůže přečíst.<p><p>Řešení: Toto je výsledek konfigurační soubor není předávání schémat. Řešení je vlastně stanovit konfigurační soubor, který splňuje se schématem. Můžete najít dat XML, která doručeny do složky *%SystemDrive%\WindowsAzure\Config* na OM rozšíření diagnostických nástrojů. Otevřete odpovídající souboru XML a vyhledávání pro **Microsoft.Azure.Diagnostics**a pak pro pole **xmlCfg** . Data jsou ve formátu Base 64 zakódovaný takže musíte [dekódovat](http://www.bing.com/search?q=base64+decoder) zobrazíte dat XML, která byla zavedená diagnostických nástrojů.<p>
-107|Průchod adresářem zdroje k sledování agent není platný.<p><p>Toto je vnitřní chyba, která by měla pouze příčiny sledování agent ručně vyvolání, nesprávně, v OM.</p>
– 108    |Nelze převést soubor konfigurace diagnostiky konfiguračního souboru sledování agent.<p><p>Toto je vnitřní chyba, která by měla pouze příčiny modul plug-in diagnostiky ručně vyvolání neplatný konfigurační soubor.
-110|Chyba konfigurace diagnostiky Obecné.<p><p>Toto je vnitřní chyba, která by měla pouze příčiny modul plug-in diagnostiky ručně vyvolání neplatný konfigurační soubor.
-111|Nejde spustit agenta sledování.<p><p>Řešení: Ověřte, jestli jsou k dispozici dostatečné systémové prostředky.
-112|Obecné chyby


## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Diagnostika Data není přihlášení k Lyncu k základnímu úložišti Azure
Azure diagnostiky ukládá všechna data v úložišti Azure.

Nejčastější příčiny chybějící data události je nesprávně definovaný úložiště informací o účtu.

Řešení: Oprava konfiguračního souboru diagnostických nástrojů a znovu nainstalujte diagnostických nástrojů.
Pokud problém přetrvává po přeinstalace koncovku diagnostiky pak budete muset ladění další tak, že podívat sledování agent chyby. Před data události nahráli ke svému účtu úložiště je uložený v LocalResourceDirectory.

Role cloudové služby LocalResourceDirectory je:

    C:\Resources\Directory\<CloudServiceDeploymentID>.<RoleName>.DiagnosticStore\WAD<DiagnosticsMajorandMinorVersion>\Tables

Pro virtuálních počítačích LocalResourceDirectory je:

    C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD<DiagnosticsMajorandMinorVersion>\Tables

Pokud nejsou žádné soubory ve složce LocalResourceDirectory, sledování agent se nepodařilo spustit. To obvykle způsobeno tím neplatné konfiguračního souboru, události, která by měla být uvedena v CommandExecution.log.

Pokud Agent sledování úspěšně shromažďování dat události uvidíte .tsf souborů pro každou událost definice v souboru konfigurace. Agent sledování protokoly jeho chyby v souboru MaEventTable.tsf. Kontrolovat obsah tohoto souboru můžete v aplikaci tabel2csv .tsf soubor můžete převést na soubor values(.csv) oddělené čárkou:

Na roli cloudové služby:

    %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

*% Systémová_jednotka* na roli cloudové služby je obvykle D:

Virtuální počítače:

    C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

Výše uvedených příkazů vygeneruje protokolu soubor *maeventtable.csv*, který můžete otevřít a zkontrolovat, jestli selhání zpráv.    


## <a name="diagnostics-data-tables-not-found"></a>Diagnostika data tabulky nenašlo se.
Tabulky v Azure úložiště ukládání dat Azure diagnostiky mají název pomocí následující kód:

        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;

Tady je příklad:

        <EtwEventSourceProviderConfiguration provider=”prov1”>
          <Event id=”1” />
          <Event id=”2” eventDestination=”dest1” />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider=”prov2”>
          <DefaultEvents eventDestination=”dest2” />
        </EtwEventSourceProviderConfiguration>

Které vygeneruje 4 tabulky:

Události|Název tabulky
---|---
Zprostředkovatel = "prov1" &lt;id události = "1" /&gt;|WADEvent + MD5("prov1") + "1"
Zprostředkovatel = "prov1" &lt;id události = "2" eventDestination = "dest1" /&gt;|WADdest1
Zprostředkovatel = "prov1" &lt;DefaultEvents /&gt;|WADDefault+MD5("prov1")
Zprostředkovatel = "prov2" &lt;DefaultEvents eventDestination = "dest2" /&gt;|WADdest2

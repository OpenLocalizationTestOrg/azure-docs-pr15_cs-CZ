<properties
    pageTitle="Historie verzí Azure diagnostiky"
    description="Vysvětlení změn v různých verzích Azure diagnostiky jako dodané s různými verzemi Microsoft Azure SDK."
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
    ms.date="02/12/2016"
    ms.author="robb"/>


# <a name="microsoft-azure-diagnostics-version-history"></a>Historie verzí diagnostických nástrojů Microsoft Azure

Začínáte s Azure diagnostiky? V tématu [Přehled Azure diagnostiky](azure-diagnostics.md).

Každý verzi Azure SDK obvykle dodává s novou verzi Azure diagnostiky. Následující tabulka popisuje Azure SDK a Azure diagnostice verze přidružené k SDK vydání.



Azure verze SDK | Azure verze diagnostiky | Model
--- | --- | ---
1.x      | 1.0 | modul plug-in
2.0 až 2.4| 1.0 | "
2.5      | 1.2 | rozšíření
2.6      | 1.3 | "
2.7      | 1.4 | "
2,8      | 1.5 | "


Nejnovější verze je 1,5, které součástí 2,8 SDK Azure. Verzi Azure diagnostiky rozšíření, který se dodává s SDK se používá pouze pro místní emulátoru scénáře. Nasazení aplikace použitým nejnovější verzi při spuštění v Azure, bez ohledu na to, kterou verzi SDK aplikace vytvořené pomocí. Ale pokud instalace nejnovějších SDK Azure vám nejsou přiřazena všechny nástroje, které vám pomohou používat s novými funkcemi.

Různé funkce a změny píše níže.

## <a name="azure-sdk-28"></a>Azure SDK 2,8
Azure 2,8 SDK přidá možnost posílání diagnostiky dat pro [Přehledy aplikace](./application-insights/app-insights-cloudservices.md) a snadněji Diagnostika problémů s přes aplikaci, stejně jako úroveň systému a infrastrukturu.

## <a name="azure-26-diagnostics-changes"></a>Změny diagnostiky Azure 2.6

Pro Azure SDK 2.6 cloudové služby projekty ve Visual Studiu tyto změny provedené. (Použijí tyto změny taky na novější verzi Azure SDK.)

- Místní emulátoru nyní podporuje diagnostických nástrojů. To znamená, že můžete shromažďovat data o diagnostických nástrojů a zajistěte, aby že aplikace je vytváření správné trasování, zatímco jste vývoj a testování ve Visual Studiu. Připojovací řetězec `UseDevelopmentStorage=true` umožňuje diagnostiky shromažďování dat a používáte cloudové služby projektu ve Visual Studiu pomocí emulátoru Azure úložiště. Všechna data diagnostických nástrojů se shromažďují účtu úložiště (vývoj úložiště).

- Diagnostika úložiště účtu připojovací řetězec (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) je uložen ještě jednou v souboru konfigurace (.cscfg) služby. V Azure SDK 2,5 účtu úložiště diagnostiky zadaná v souboru diagnostics.wadcfgx.

Existuje několik důležitých rozdíly mezi jak připojovací řetězec fungovala Azure SDK 2.4 v dřívější a jak to funguje v Azure SDK 2.6 a novějších verzích.

- V Azure SDK 2.4 a starších verzích připojovací řetězec byl použit jako modul runtime modul plug-in Diagnostika získání informací o účtu úložiště pro přenos protokolování diagnostiky.

- V Azure SDK 2.6 a v novějších verzích připojovací řetězec diagnostiky používá Visual Studio nakonfigurovat koncovku diagnostických nástrojů s informací o účtu odpovídající úložiště při publikování. Připojovací řetězec můžete nastavit jiné úložiště účty pro jiné službě konfigurace, které aplikace Visual Studio použije při publikování. Však protože modul plug-in diagnostice už není dostupná (po Azure 2,5 SDK), souboru .cscfg samostatně není možné povolit koncovku diagnostických nástrojů. Je třeba povolit koncovku samostatně pomocí nástrojů Visual Studia ATP Powershellu.

- Zjednodušit proces konfigurace koncovku diagnostiky pomocí prostředí PowerShell, výstup balíček aplikace Visual Studio obsahuje také veřejné konfigurace XML pro rozšíření diagnostických nástrojů pro každou roli. Visual Studio používá připojovací řetězec diagnostiky k naplnění informace o účtu úložiště účastní veřejné konfigurace. Veřejné konfigurace soubory vytvořené ve složce rozšíření a postupujte podle vzorku PaaSDiagnostics. <RoleName>. PubConfig.xml. Všechny prostředí PowerShell založeny nasazení lze použít tento každou konfiguraci přiřadit roli.

- Připojovací řetězec do souboru .cscfg taky používá Azure portálu pro přístup k datům diagnostiky tak, aby se zobrazil na kartě **Sledování** . Připojovací řetězec je potřeba ke konfiguraci služby zobrazení Podrobný monitorování dat na portálu.

### <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Migrace projektů Azure SDK 2.6 a novější

Při migraci z Azure SDK 2,5 Azure SDK 2.6 nebo novější, pokud máte účet úložiště diagnostice podle .wadcfgx soubor, potom zůstane tam. Umožní využít výhod flexibilní použití účty různých úložiště u různých úložiště konfigurací budete muset ručně přidat připojovací řetězec do projektu. Projekt se migraci z Azure SDK 2.4 nebo dřívější na aplikaci a Azure SDK 2.6, se zachovají řetězce Diagnostika připojení. Dejte pozor, změny v tom, jak připojení řetězce jsou považovány v Azure SDK 2.6 určené v předchozí části.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Způsob účtu úložiště diagnostických nástrojů Visual Studio

- Je-li připojovací řetězec Diagnostika v souboru .cscfg, Visual Studio se používá ke konfiguraci koncovku diagnostiky při publikování a při vytváření souborů xml veřejné konfigurace během balení.

- Je-li žádné diagnostiky připojovací řetězec v souboru .cscfg, pak Visual Studio přejde pomocí účtu úložiště zadaného v souboru .wadcfgx konfigurace koncovku diagnostiky při publikování a generování souborů xml veřejné konfigurace při sbalení.

- Diagnostika připojovací řetězec do souboru .cscfg přednost před účtu úložiště v souboru .wadcfgx. Je-li připojovací řetězec Diagnostika v souboru .cscfg, Visual Studio používá a ignoruje účtu úložiště v .wadcfgx.

### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>Co znamená "aktualizace vývoj úložiště připojení řetězce..." zaškrtávací políčko dělat?

Zaškrtávací políčko pro **aktualizaci vývoj úložiště připojovací řetězec pro diagnostiku a ukládání do mezipaměti s Microsoft Azure úložiště přihlašovací údaje účtu při publikování na Microsoft Azure** vám pohodlný způsob, jak aktualizovat všechny vývojové úložiště účtu připojení řetězce účet Azure úložiště zadaný při publikování.

Předpokládejme například, zaškrtněte toto políčko a připojovací řetězec diagnostiky Určuje `UseDevelopmentStorage=true`. Při publikování projektu do Azure Visual Studio bude automaticky aktualizován připojovací řetězec diagnostiky úložiště účet, který jste zadali v Průvodci publikovat. Ale pokud účet skutečné úložiště byla specifikována jako diagnostiky připojovací řetězec, pak tento účet bude použita.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnostika funkce rozdíly mezi Azure SDK 2.4 a starší a Azure SDK 2,5 a novější

Pokud upgradujete projektu z Azure SDK 2.4 2,5 SDK Azure nebo novější, byste měli mít na paměti následující funkce rozdíly diagnostických nástrojů.

- **Rozhraní API konfigurace jsou změněny** – programové konfigurace diagnostiky je k dispozici v Azure SDK 2.4 a starších verzích, ale se nedá použít v Azure SDK 2,5 a novějších verzích. Pokud konfigurace diagnostiky je aktuálně definované v kódu, musíte nakonfigurovat nastavení těchto přímo v migrované projektu v pořadí diagnostických nástrojů na pokračovat v práci. Konfigurační soubor diagnostiky Azure SDK 2.4 je diagnostics.wadcfg a diagnostics.wadcfgx Azure SDK 2,5 a v novějších verzích.

- **Diagnostika pro cloudové služby aplikace je možné konfigurovat jenom na úrovni role, nikoli na úrovni instance.**

- **Pokaždé, když nasadíte aplikace se aktualizuje konfigurace diagnostiky** – Pokud změnit konfiguraci diagnostiky z Průzkumníka serveru a pak znovu nasadit aplikaci to může způsobovat problémy s dostupná.

- **V Azure SDK 2,5 a novější, selhat výpisy nejsou nakonfigurováni v diagnostiky konfiguračního souboru, nejsou v kódu** – Pokud máte výpisy nakonfigurováno v kódu, budete muset ručně přepojit konfiguraci z kódu konfiguračního souboru, protože nejsou převedeny výpisy během migrace k Azure SDK 2.6.

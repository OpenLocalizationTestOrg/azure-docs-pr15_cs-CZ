<properties
   pageTitle="Začínáme s Azure protokolu integrace | Microsoft Azure"
   description="Zjistěte, jak nainstalovat integrace služby Azure protokolu a integrace protokoly z Azure úložiště, protokolů auditování Azure a upozornění na Centrum zabezpečení Azure."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="get-started-with-azure-log-integration-preview"></a>Začínáme s Azure protokolu integrace (verze Preview)

Integrace Azure log umožňuje integraci neformátovaných protokolů z Azure zdrojů do místního informací o zabezpečení a události Management (SIEM) systémy. Tato integrace poskytuje jednotné řídicí panel pro všechny vaše majetek místní nebo v cloudu, tak, aby se daly agregovat, sladit, analyzovat a výstrahy zabezpečení události spojené s aplikací.

Tento kurz vás provede instalaci Azure protokolu integrace a integrace protokoly z Azure úložiště, protokolů auditování Azure a upozornění na Centrum zabezpečení Azure. Předpokládanou dobu dokončení tohoto kurzu je hodinu.

## <a name="prerequisites"></a>Zjistit předpoklady pro

Tento kurz musí mít takto:

- Počítači (místní nebo v cloudu) instalace integrace služby Azure protokolu. Tento počítač musí běžet s .net 4.5.1 nainstalovaný 64bitová verze operačního systému Windows. Tento počítač se nazývá **Azlog integrátor**.
- Azure předplatného. Pokud nemáte jeden, můžete registraci [bezplatný účet](https://azure.microsoft.com/free/).
- Azure diagnostiky aktivované Azure virtuálních počítačích (VMs). Povolit diagnostických nástrojů pro cloudové služby, najdete v článku [Povolení Azure Diagnostika v Azure Cloud Services](../cloud-services/cloud-services-dotnet-diagnostics.md). Povolení diagnostických nástrojů pro Azure OM s Windows, najdete v článku [použití povolit Azure diagnostiky ve počítače virtuální systém Windows](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md).
- Připojení z integrátor Azlog k základnímu úložišti Azure k ověření a povolte Azure předplatné.
- Protokoly Azure OM musíte mít na integrátor Azlog nainstalované agenta SIEM (například univerzální předávání Splunk sběru událostí systému Windows ArcSight HP agent a IBM QRadar WinCollect).

## <a name="deployment-considerations"></a>Důležité informace o nasazení

Spuštěním více instancí integrátor Azlog nastavená dostatečně vysoká hlasitost události. Diagnostika Azure úložiště účtů pro Windows *(WAD)* a počet předplatná poskytovat instancí služby Vyrovnávání zatížení vycházet z kapacity.

V počítači procesorem 8 (základní) můžete jeden výskyt Azlog integrátor zpracování asi 24 milionů událostí denně (~1M/hour).

Na počítači procesorem 4 (základní) můžete jeden výskyt Azlog integrátor zpracování asi 1,5 milion událostí denně (~62.5K/hour).

## <a name="install-azure-log-integration"></a>Instalace Azure protokolu integrace

Stáhněte si [Azure protokolu integrace](https://www.microsoft.com/download/details.aspx?id=53324).

Integrace služby Azure protokolu shromažďuje telemetrickými daty z počítače, na kterém je nainstalovaná.  Telemetrie dat shromážděných je:

- Integrace protokol výjimek, ke kterým dochází při provádění Azure
- Metriky o počet dotazů a zpracovaných událostí
- Statistických údajů o které Azlog.exe jsou použity parametry příkazového řádku

> [AZURE.NOTE] Sběr údajů telemetrie můžete vypnout tak, že zrušíte zaškrtnutí této možnosti.

## <a name="integrate-azure-vm-logs-from-your-azure-diagnostics-storage-accounts"></a>Integrace Azure OM protokoly z účtů úložiště diagnostiky Azure

1. Zkontrolujte požadavky výše uvedené zajistit, že účtu úložiště WAD je shromáždění protokolů před pokračováním integrace Azure protokolu. Pokud váš účet úložiště WAD není shromáždění protokolů není proveďte následující kroky.

2. Otevřete příkazový řádek a **cd** do **c:\Program Files\Microsoft Azure protokolu integrace**.

3. Spusťte příkaz

        azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>

      Nahraďte název Azure úložiště účet nakonfigurovaný pro příjem diagnostiky událostí z vaší OM StorageAccountName.

        azlog source add azlogtest WAD azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==

      Pokud byste chtěli id předplatného neprojeví v případě XML, připojte k popisný název ID předplatného:

        azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>

4. Počkejte 30 60 minut (ji může trvat až jednu hodinu) a potom zobrazit události, které jsou doplněné z účtu úložiště. Chcete-li zobrazit, otevřete **Prohlížeč událostí > protokoly systému Windows > přesměrování události** na integrátor Azlog.

5. Ujistěte se, že vaše standardní spojnice SIEM v počítači nainstalován nakonfigurovaný tak, aby vyberte události z složce **Předáním události** a kanálu SIEM instanci. Kontrola konfigurace konkrétní SIEM ke konfiguraci a najdete v článku protokoly integrace.

## <a name="what-if-data-is-not-showing-up-in-the-forwarded-events-folder"></a>Co když data, nezobrazuje ve složce předáním událostí?

Pokud za hodinu dat nezobrazuje ve složce **Předáním události** , potom:

1. Zkontrolujte počítač a ověřte, jestli mají přístup k Azure. Chcete-li otestovat připojení, pokuste se otevřít [Azure portál](http://portal.azure.com) z prohlížeče.

2. Zkontrolujte, že účet uživatele **azlog** má oprávnění k zápisu na složku **users\azlog**.

3. Ujistěte se, úložiště účtu přidali do příkazu **Přidat zdroj azlog** koncovém při spuštění příkazu **azlog zdrojovém seznamu**.
4. Přejděte na **Prohlížeč událostí > protokoly systému Windows > aplikace** podíváte se obsahuje všechny chyby nahlášené z integrace Azure protokolu.

Pokud se pořád nezobrazuje události, potom:

1. Stáhněte si [Průzkumníka úložišť Microsoft Azure](http://storageexplorer.com/).

2. Připojení k účtu úložiště přidána do příkazu **Přidat azlog zdroje**.

3. V Průzkumníku úložiště Microsoft Azure přejděte do tabulky **WADWindowsEventLogsTable** zobrazíte, pokud je všechna data. V opačném případě pak Diagnostika v OM není správná.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integrace protokolů auditování Azure a upozornění na Centrum zabezpečení

1. Otevřete příkazový řádek a **cd** do **c:\Program Files\Microsoft Azure protokolu integrace**.

2. Spusťte příkaz

        azlog createazureid

      Tento příkaz zobrazí výzvu k zadání Azure přihlašovací jméno. Příkaz vytvoří [Azure Active Directory služby jistiny](../active-directory/active-directory-application-objects.md) v Azure AD klienti, které hostují Azure předplatných ve kterých je přihlášený uživatel správce, spolu správce nebo vlastník. Příkaz se nezdaří, pokud je přihlášený uživatel jenom uživatel Host v klientovi Azure AD. Ověřování Azure probíhá prostřednictvím Azure Active Directory (AD).  Vytvoření hlavního služby pro integraci Azlog vytvoří identitu Azure AD, která bude mít přístup pro čtení z Azure předplatného.

3. Spusťte příkaz

        azlog authorize <SubscriptionID>

      To přiřadí přístup čtenáře předplatného služby základní vytvořili v kroku 2. Pokud nezadáte SubscriptionID, se pokusí přiřadit roli služby základní Čtenář Všechna předplatná, ke které se mají přístup.

        azlog authorize 0ee9d577-9bc4-4a32-a4e8-c29981025328

      > [AZURE.NOTE] Pokud příkaz **povolte** bezprostředně za příkazu **createazureid** se může zobrazit upozornění. Existuje několik latenci mezi při vytvoření účtu Azure AD a kdy je k dispozici pro použití účtu. Pokud Počkejte asi 10 sekund po spuštění příkazu **createazureid** ke spuštění příkazu **povolte** byste neměli vidět tato upozornění.

4. Zkontrolujte těchto složek a potvrďte, že jsou JSON souborů protokolu auditování:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Zkontrolujte těchto složek a potvrďte existenci upozornění na Centrum zabezpečení v nich:

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. Standardní konektor předávání SIEM soubor přejděte na požadovanou složku pro kanálu data, která chcete SIEM instance. Možná budete muset některé mapování polí na základě SIEM produktu, který používáte.

Pokud máte nějaké otázky o Integration protokolu Azure, pošlete nám prosím e-mailu pro [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Další kroky

V tomto kurzu se naučíte instalace Azure protokolu integrace a integrace protokoly z Azure úložiště. Další informace najdete v těchto článcích:

- [Microsoft Azure protokolu integrace Azure protokolů (verze Preview)](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center podrobnosti, systémové požadavky a pokyny k instalaci integraci Azure protokolu.
- [Úvod do integrace Azure protokolu](security-azure-log-integration-overview.md) – tento dokument vás seznámí s Azure protokolu integrace klíčové funkce, a jak to funguje.
- [Kroky konfigurace partnera](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – tento příspěvek blogu se dozvíte, jak nakonfigurovat integraci Azure protokolu pro práci s partnerských řešení Splunk, HP ArcSight a IBM QRadar.
- [Azure protokolu integrace často kladené otázky (Nejčastější dotazy týkající se)](security-azure-log-integration-faq.md) – nejčastější dotazy týkající se tento najdete odpovědi na otázky týkající se integrace Azure protokolu.
- [Upozornění na Centrum zabezpečení integrace s Azure protokolu integrace](../security-center/security-center-integrating-alerts-with-log-integration.md) – tohoto dokumentu uvidíte, jak synchronizovat upozornění na Centrum zabezpečení, spolu s virtuální počítač událostí zabezpečení shromážděná diagnostiky Azure a protokolů auditování Azure se protokolu analýzy nebo SIEM řešení.
- [Nové funkce služby Azure diagnostiky a protokolů auditování Azure](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – tento příspěvek blogu vás seznámí s protokolů auditování Azure a další funkce, které vám pomůžou získat podstatu operace Azure prostředků.

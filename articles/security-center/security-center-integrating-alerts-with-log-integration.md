<properties
   pageTitle="Integrace upozornění na Centrum zabezpečení Azure integrací Azure protokolu (verze Preview) | Microsoft Azure"
   description="Tento článek vám pomůže začít s integrací upozornění na Centrum zabezpečení integrací Azure protokolu."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="terrylan"/>

# <a name="integrating-security-center-alerts-with-azure-log-integration-preview"></a>Integrace upozornění na Centrum zabezpečení integrací Azure protokolu (verze Preview)

Mnoho operace zabezpečení a vyřešení incidentu odpověď týmy spolehnout jako výchozí bod pro triaging a vyšetřování výstrah zabezpečení informací o zabezpečení a události Management (SIEM) řešení. Integrace Azure protokolu zákazníci synchronizovat Centrum zabezpečení Azure upozornění, spolu s virtuální počítač událostí zabezpečení shromážděná diagnostiky Azure a protokolů auditování Azure, s jejich protokolu analýzy a řešení SIEM v blízkosti v reálném čase.

Integrace Azure protokolu spolupracuje HP ArcSight Splunk, IBM QRadar a ostatní.

## <a name="what-logs-can-i-integrate"></a>K čemu protokoly můžete integrovat?

Azure vytvoří rozsáhlé protokolování pro každou službu. Jsou zařazené v těchto protokolů:

- **Ovládací prvek a Správa protokolů** umožňuje zobrazit na operace vytvoření správce prostředků Azure, aktualizovat a odstranit.
- **Protokoly dat rovině** zviditelnění do události aktivované při použití Azure zdroje. Příklad je protokolu událostí systému Windows – zabezpečení a aplikace protokoly do virtuálního počítače.

Integrace Azure protokolu v současné době podporuje integrace:

- Azure OM protokoly
- Protokolů auditování Azure
- Upozornění na Centrum zabezpečení Azure

## <a name="install-azure-log-integration"></a>Instalace Azure protokolu integrace

Stáhněte si [Azure protokolu integrace](https://www.microsoft.com/download/details.aspx?id=53324).

Integrace služby Azure protokolu shromažďuje telemetrickými daty z počítače, na kterém je nainstalovaná.  Telemetrie dat shromážděných je:

- Integrace protokol výjimek, ke kterým dochází při provádění Azure
- Metriky o počet dotazů a zpracovaných událostí
- Statistických údajů o které Azlog.exe jsou použity parametry příkazového řádku

> [AZURE.NOTE] Sběr údajů telemetrie můžete vypnout tak, že zrušíte zaškrtnutí této možnosti.

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integrace protokolů auditování Azure Centrum zabezpečení a upozornění

1. Otevřete příkazový řádek a **cd** do **c:\Program Files\Microsoft Azure protokolu integrace**.

2. Spusťte příkaz **azlog createazureid** vytvoření [Azure Active Directory služby jistiny](../active-directory/active-directory-application-objects.md) v Azure Active Directory (AD) klienti, které hostují Azure předplatná.

    Zobrazí se výzva pro Azure přihlašovací jméno.

    > [AZURE.NOTE] Musí být předplatné vlastník nebo správce spolu předplatného.

    Ověřování Azure probíhá prostřednictvím Azure AD.  Vytvoření hlavního služby Azure protokolu integrace vytvoří identitu Azure AD, která bude mít přístup pro čtení z Azure předplatného.

3. Spustit **azlog povolte <SubscriptionID> ** příkaz přiřadit přístup čtenáře předplatným služby jistinu vytvořili v kroku 2. Pokud nezadáte **SubscriptionID**, pak jistinu služby se automaticky přiřadí roli Čtenář Všechna předplatná, ke kterým máte přístup.

    > [AZURE.NOTE] Po spuštění příkazu **povolte** bezprostředně za příkazu **createazureid** protože některé latenci mezi při vytvoření účtu Azure AD a kdy účet je k dispozici pro použití se může zobrazit upozornění. Pokud Počkejte asi 10 sekund po spuštění příkazu **createazureid** ke spuštění příkazu **povolte** byste neměli vidět tato upozornění.

4. Zkontrolujte těchto složek a potvrďte, že jsou JSON souborů protokolu auditování:

  - **c:\Users\azlog\AzureResourceManagerJson**
  - **c:\Users\azlog\AzureResourceManagerJsonLD**

5. Zkontrolujte těchto složek a potvrďte existenci upozornění na Centrum zabezpečení v nich:

  - **c:\Users\azlog\ AzureSecurityCenterJson**
  - **c:\Users\azlog\AzureSecurityCenterJsonLD**

6. Standardní konektor předávání SIEM soubor přejděte na požadovanou složku pro kanálu data, která chcete SIEM instance. Konfigurace SIEM získáte [SIEM konfigurace](https://azsiempublicdrops.blob.core.windows.net/drops/ALL.htm) .

Pokud máte nějaké otázky o Integration protokolu Azure, pošlete nám prosím e-mailu pro [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Další kroky

Další informace o protokolech auditování Azure a definice vlastností najdete v tématu:

- [Auditování operace s správce prostředků](../resource-group-audit.md)
- [Seznam správy událostí v předplatné](https://msdn.microsoft.com/library/azure/dn931934.aspx) – k načtení událostí protokolu auditování.

Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení.
- [Časté otázky k Centru zabezpečení Azure](security-center-faq.md) – najít nejčastější dotazy k použití služby.
- [Zabezpečení Azure blog](http://blogs.msdn.com/b/azuresecurity/) – získání nejnovější informace Azure zabezpečení a informací.

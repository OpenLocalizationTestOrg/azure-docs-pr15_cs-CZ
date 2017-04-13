<properties
   pageTitle="Centrum zabezpečení Azure dat zabezpečení | Microsoft Azure"
   description="Tento dokument vysvětluje způsob správy a chráněny v Centru zabezpečení Azure data."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-data-security"></a>Centrum zabezpečení Azure zabezpečení dat.
Pro zákazníky, kvůli kterým, zjišťování a odpovědět na hrozeb, Centrum zabezpečení Azure shromažďuje a zpracovává data o Azure zdrojů, včetně informací o konfiguraci, metadata, protokoly událostí docházet k chybám výpis soubory a dělat Další. Provedeme silných závazky chránit osobní údaje a zabezpečuje data. Microsoft vyhovuje pro striktně zabezpečení a dodržování – z kódování provozní služby. 

Tento článek popisuje způsob správy a zachovány v Centru zabezpečení Azure data.

## <a name="data-sources"></a>Zdroje dat

Centrum zabezpečení Azure analýzy dat v následujících zdrojích:

- Azure služby: Načte informace o konfiguraci služby Azure, že jste nainstalovali komunikace u tohoto zdroje poskytovatele.
- V síti: Čtení odběr sítě přenosy metadata ze infrastruktury společnosti Microsoft, třeba zdrojových a cílových/port IP, velikost paketu a síťový protokol.
- Partnerských řešení: Shromažďuje výstrah zabezpečení z integrované partnerských řešení, například bránách firewall a antimalware řešení. Tato data uložena v Centru zabezpečení Azure úložiště, právě umístěný ve Spojených státech.
- Virtuálních počítačích: Centrum zabezpečení Azure můžete shromáždit informace o konfiguraci a informace o událostech zabezpečení, například událostí systému Windows a auditování protokoly IIS protokoly, syslog zprávy a soubory se stavem systému z vaší virtuálních počítačích pomocí agenti kolekce dat. Naleznete v části "Správa shromažďování dat" pod další podrobnosti.  

Kromě toho je informace týkající se upozornění zabezpečení, doporučení a zabezpečení stavu uložené v úložišti Centrum zabezpečení Azure, právě umístěný ve Spojených státech. Tyto informace mohou obsahovat informace související konfiguraci a událostí zabezpečení odebrané virtuálních počítačích v případě potřeby poskytnout upozornění zabezpečení, doporučení nebo zabezpečení stavu.

## <a name="data-protection"></a>Ochrana dat

**Oddělení dat**: dat bude k dispozici logicky samostatné na jednotlivé součásti v rámci služby. Všechna data označen za organizace. Tento označování zůstává v celém životním cyklu dat a nevynucují u každé vrstvy služby. Kromě toho odebrané virtuálních počítačích ukládají se data do úložiště účty.

**Přístup k datům**: abyste mohli poskytnout doporučení týkající se zabezpečení a prošetřit potenciální hrozeb zabezpečení, pracovníky společnosti Microsoft mohou získat přístup k informacím shromažďované a analyzovat službou Azure, včetně soubory se stavem systému. Soubory se stavem systému a proces vytváření událostí omylem je možné zákaznická Data nebo osobní údaje z virtuálních počítačích. Jsme dodržovat [Microsoft Online Services termínů](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) a [Zásady ochrany osobních údajů](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), které stát, že nebude Microsoft pomocí Data o zákaznících nebo dědí informace z něho pro obchodní účely reklamní nebo podobné. Doporučujeme použít jenom Data o zákaznících v případě potřeby k poskytování služby Azure, včetně účely kompatibilní s poskytování těchto služeb. Zachovat všechny práva Data o zákaznících.

**Použití dat**: Společnost Microsoft použije vzorů a měřítka hrozbou, že vidět přes víc tenantů k vylepšení naše funkce Zabránění a zjišťování; jsme to udělat v souladu s závazky ochrany osobních údajů podle naše [Zásady ochrany osobních údajů](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

**Umístění dat**: účet úložiště je určený pro každou oblast, kde jsou spuštěné virtuálních počítačích. Umožňuje ukládat data v oblasti stejný jako virtuálního počítače, ze kterého se shromažďují data. Tato data, včetně soubory se stavem systému, bude trvale uložené ve vašem účtu úložiště. Služba také informacemi o výstrah zabezpečení, včetně upozornění od integrované partnerských řešení, doporučení a stavu zabezpečení v Centru zabezpečení Azure úložiště, právě umístěný ve Spojených státech.

## <a name="managing-data-collection-from-virtual-machines"></a>Správa shromažďování dat z virtuálních počítačích

Když budete chtít povolit Centrum zabezpečení Azure, shromažďování dat zapnuté pro všechny vaše předplatné. Shromažďování dat v části "Zásady zabezpečení" řídicího panelu Centra zabezpečení Azure můžete vypnout. Když je zapnutý shromažďování dat, podporované Centrum zabezpečení Azure ustanovení agenta sledování Azure na všechny existující virtuálních počítačích a jenom na nově vytvořené. Rozšíření sledování zabezpečení Azure vyhledává různých zabezpečení týkající se konfigurace a událostí do [Trasování událostí pro systém Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (trasování událostí pro Windows) sleduje. Kromě toho vyvolá operační systém událostí v protokolu událostí v průběhu spuštěné v počítači. Příklady tyto údaje: typ operačního systému a verze operačního systému protokoly (protokoly událostí Windows), systémy procesy, název počítače, IP adresy přihlášení uživatele a ID klienta. Agent sledování Azure přečte položky protokolu událostí a trasování událostí pro Windows sleduje a zkopíruje ke svému účtu úložiště pro analýzu. 

Účet úložiště je určený pro každou oblast, ve kterém máte virtuálních počítačích systém, kde údaje z virtuálních počítačích v, který je uložený stejné oblasti. Usnadňuje můžete uchovávat data v stejné zeměpisné pro účely svrchovanost data a osobní údaje. V části "Zásady zabezpečení" řídicího panelu Centra zabezpečení Azure můžete nakonfigurovat úložiště účty pro každou oblast.

Agent sledování Azure také zkopíruje soubory se stavem systému ke svému účtu úložiště.  Centrum zabezpečení Azure shromažďuje dočasné kopií vaše soubory se stavem systému a analyzuje důkaz pokusy o zneužití a úspěšně kompromisům.  Centrum zabezpečení Azure provádí tuto analýzu v rámci stejné zeměpisná oblast účtem úložiště a odstraní dočasné kopie po dokončení analýzy.

Shromažďování dat z virtuálních počítačích kdykoli, která odebere všechny sledování agentů dříve nainstalována Centrum zabezpečení Azure můžete zakázat.


## <a name="see-also"></a>Viz taky

V tomto dokumentu jste se naučili způsob správy a chráněny v Centru zabezpečení Azure data. Další informace o Centru zabezpečení Azure, najdete tady:

- [Plánování Centrum zabezpečení Azure a příručka](security-center-planning-and-operations-guide.md) – informace o plánování a návrh Principy přijmout Azure Centrum zabezpečení.
- [Sledování v Centru zabezpečení Azure stavu zabezpečení](security-center-monitoring.md) – zjistěte, jak sledovat stav Azure prostředků.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení
- [Sledování partnerských řešení s Azure Centrum zabezpečení](security-center-partner-solutions.md) – zjistěte, jak sledovat stav partnerských řešení.
- [Časté otázky k Centru zabezpečení Azure](security-center-faq.md) – najít časté otázky k používání služby
- [Azure zabezpečení Blog](http://blogs.msdn.com/b/azuresecurity/) – najít příspěvky Azure zabezpečení a dodržování předpisů

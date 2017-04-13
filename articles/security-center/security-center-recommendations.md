<properties
   pageTitle="Správa doporučení týkající se zabezpečení v Centru zabezpečení Azure | Microsoft Azure"
   description="Tento dokument provede vás jak doporučení v Centru zabezpečení Azure vám pomůže chránit vaše Azure zdroje a zůstat souladu s zásady zabezpečení."
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
   ms.date="09/25/2016"
   ms.author="terrylan"/>

# <a name="managing-security-recommendations-in-azure-security-center"></a>Správa doporučení týkající se zabezpečení v Centru zabezpečení Azure

Tento dokument vás provede používání doporučení v Centru zabezpečení Azure k ochraně Azure zdroje.

> [AZURE.NOTE] Tento dokument zavádí službu pomocí nasazení příklad.  Toto není podrobného průvodce.

## <a name="what-are-security-recommendations"></a>Jaké jsou doporučení týkající se zabezpečení?
Centrum zabezpečení pravidelně analyzuje stav zabezpečení Azure prostředků. Když Centrum zabezpečení identifikuje potenciální zabezpečením, vytvoří doporučení. Doporučení vás provede procesem konfigurace potřebných ovládacích prvků.

## <a name="implementing-security-recommendations"></a>Provádění doporučení týkající se zabezpečení

### <a name="set-recommendations"></a>Nastavení doporučení

[Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md)můžete se naučit:

- Konfigurace zásad zabezpečení.
- Shromažďování dat zapněte.
- Zvolte které doporučení zobrazíte jako součást svoje zásady zabezpečení.

Aktuální zásady doporučení Centrum kolem aktualizace systému, pravidla podle směrného plánu, antimalware programy, [skupiny zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) podsítí a síťových rozhraní SQL databáze auditování, SQL průhledné šifrování databáze a brány firewall webové aplikace.  [Nastavení zásad zabezpečení](security-center-policies.md) obsahuje popis jednotlivých možností doporučení.

### <a name="monitor-recommendations"></a>Sledování doporučení
Po nastavení zásad zabezpečení, Centrum zabezpečení analyzuje stav zabezpečení prostředky k identifikaci potenciální chyby. Na dlaždici **doporučení** na zásuvné **Centrum zabezpečení** značí celkový počet doporučení označenými Centrum zabezpečení.

![Dlaždice doporučení][1]

Pokud chcete zobrazit podrobnosti každé doporučení:

1. Klikněte **doporučení dlaždice** na zásuvné **Centrum zabezpečení** . Otevře se zásuvné **doporučení** .

Doporučení jsou zobrazeny ve formátu tabulky, kde každý řádek představuje jeden konkrétní doporučení. Sloupce v této tabulce jsou:

- **Popis**: vysvětluje doporučení a co je potřeba udělat při řešení ho.
- **Zdroje**: seznam zdrojů, ke kterým se vztahuje tato doporučení.
- **Stav**: popisuje aktuální stav doporučení:
    - **Otevřít**: doporučení není zatím adresovaná.
    - **V průběhu**: doporučení je právě použita ke zdrojům, a můžete je potřeba žádná akce.
    - **Vyřešený**: doporučení již byly provedeny (v tomto případě řádku bude zobrazit zašedle).
- **ZÁVAŽNOSTI**: Popisuje závažnosti této konkrétní doporučení:
    - **Vysoký**: chybu existuje s smysluplné zdroj (třeba aplikace, virtuálního počítače nebo skupinu zabezpečení sítě) a vyžaduje pozornost.
    - **Střední**: obsahuje chybu a nekritické nebo další kroky jsou potřeba k jeho odstranění nebo k dokončení procesu.
    - **Nízká**: chybu, která by měla být adresovaná ale nevyžaduje okamžitou pozornost. (Ve výchozím nastavení nejsou uvedeny nízké doporučení, ale je můžete vyfiltrovat nízké doporučení Pokud si chcete prohlédnout.)

Použijte v následující tabulce jako odkaz na vám pomůže pochopit dostupné doporučení a co každý z nich bude dělat, když ho použijete.

> [AZURE.NOTE] Budete chtít porozumět [klasické a modelů nasazení Správce prostředků](../azure-classic-rm.md) pro Azure zdroje.

|Doporučení|Popis|
|-----|-----|
|[Povolení shromažďování dat u předplatného](security-center-enable-data-collection.md)|Doporučuje zapnutí shromažďování dat v zásady zabezpečení pro platnost předplatného a všechny virtuálních počítačích (VMs) v předplatné.|
|[Nápravě chyby OS](security-center-remediate-os-vulnerabilities.md)|Doporučuje zarovnání konfiguraci OS s pravidly doporučená konfigurace například nepovolují ukládání hesel.|
|[Použití aktualizací systému](security-center-apply-system-updates.md)|Doporučuje nasazení chybějící systém zabezpečení a důležité aktualizace pro VMs.|
|[Restartujte po aktualizace systému](security-center-apply-system-updates.md#reboot-after-system-updates)|Doporučuje restartovat OM dokončete proces instalace aktualizací systému.|
|[Přidání bránu firewall webové aplikace](security-center-add-web-application-firewall.md)|Doporučuje nasadit web aplikace brány firewall (WAF) pro koncové body web. Přidáním tyto aplikace do stávajících nasazení WAF můžete chránit více webových aplikací v Centru zabezpečení. WAF spotřebiče (vytvořené pomocí Správce prostředků nasazení modelu) musí být nasazené na samostatném virtuální sítě. WAF spotřebiče (vytvořená pomocí klasické nasazení modelu) jsou používat skupiny zabezpečení sítě. Toto odborné pomoci bude prodlužuje na plně přizpůsobené nasazení (klasické) WAF zařízení v budoucnu. Centrum zabezpečení doporučí zřízení WAF chcete chránit před útoky zacílení webových aplikací na VMs a v aplikaci služby prostředí řízení. Další informace o pomocného mechanismu řízení, najdete v [Dokumentaci prostředí aplikace](../app-service/app-service-app-service-environments-readme.md). |
|[Dokončení Ochrana aplikace](security-center-add-web-application-firewall.md#finalize-application-protection)|Dokončete konfiguraci WAF musí přenosy přesměrovány WAF zařízení. Sledovat tento doporučení dokončí potřeby vzhled.|
|[Přidání dalšího brány Firewall generování](security-center-add-next-generation-firewall.md)|Doporučuje zvýšit zabezpečení ochrany přidat brány Firewall pro další generování (NGFW) od partnera společnosti Microsoft.|
|[Směrování přenosů NGFW pouze](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Doporučuje konfigurovat sítě zabezpečení skupiny (NSG) pravidla, která vynutit příchozích OM prostřednictvím svého NGFW.|
|[Instalace Endpoint Protection](security-center-install-endpoint-protection.md)|Doporučuje zřízení antimalware programů VMs (pouze Windows VMs).|
|[Řešení Endpoint Protection stavu upozornění](security-center-resolve-endpoint-protection-health-alerts.md)|Doporučuje vyřešit chyby ochrany koncového bodu.|
|[Povolení skupiny zabezpečení sítě podsítí nebo virtuálních počítačích](security-center-enable-network-security-groups.md)|Doporučuje povolit NSGs na podsítí nebo VMs.|
|[Omezit přístup prostřednictvím Internetu koncový bod](security-center-restrict-access-through-internet-facing-endpoints.md)|Konfigurace příchozích pravidla pro NSGs doporučuje.|
|[Povolení auditování SQL serveru](security-center-enable-auditing-on-sql-servers.md)|Doporučuje, zapněte auditování Azure SQL servery (služba Azure SQL pouze; nezahrnuje SQL na virtuálních počítačích).|
|[Povolení databáze SQL auditování](security-center-enable-auditing-on-sql-databases.md)|Doporučuje, zapněte auditování pro databáze Azure SQL (služba Azure SQL pouze; nezahrnuje SQL na virtuálních počítačích).|
|[Povolit průhledné šifrování dat v SQL databáze](security-center-enable-transparent-data-encryption.md)|Doporučuje povolit šifrování pro SQL databáze (jenom služba Azure SQL).|
|[Povolení OM agenta](security-center-enable-vm-agent.md)|Slouží k zobrazení, které vyžadují VMs agenta OM. V VMs musí být nainstalované agenta OM, aby bylo možné vytvořit oprava prohledávání vyhledávání podle směrného plánu a antimalware programy. Agent OM nainstalovaný ve výchozím nastavení pro VMs, které jsou nasazeny z Azure Marketplace. V článku [rozšíření – část 2 a OM Agent](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) obsahuje informace o tom, jak nainstalovat agenta OM.|
| [Použití šifrování disku](security-center-apply-disk-encryption.md) |Doporučuje šifrování disků OM šifrovanou Azure disku (Windows a Linux VMs). Šifrování doporučujeme OS a data svazky na vaše OM.|
|[Zadejte podrobnosti o kontaktu zabezpečení](security-center-provide-security-contact-details.md) | Doporučuje poskytují zabezpečení kontaktní informace pro každou z předplatného. Kontaktní informace je e-mailovou adresu a telefonní číslo. Informace se použijí k vás kontaktovat, pokud náš tým zabezpečení zjistí, že jsou ohroženo zdroje. |
| [Aktualizovat operační systém verze](security-center-update-os-version.md) | Doporučuje aktualizace verze operačního systému (OS) pro vaše cloudové služby na nejnovější verzi k dispozici pro rodinu s operačním systémem.  Další informace o cloudové služby, najdete v článku [Přehled Cloudovým službám](../cloud-services/cloud-services-choose-me.md). |
| [Chyba hodnocení Nenainstalováno](security-center-vulnerability-assessment-recommendations.md) | Doporučuje nainstalovat řešení zhodnocení chybu na vaše OM. |
| [Nápravě chyby](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Umožňuje zobrazit chyby systému a aplikací zjistit řešení zhodnocení chybu na vaše OM nainstalovaný. |

Můžete filtrovat a zavřete doporučení.

1. Klikněte na **Filtr** na zásuvné **doporučení** . Otevře se zásuvné **Filtr** a vyberte závažnosti a stav hodnoty, které chcete vidět.

    ![Filtrování doporučení][2]

2. Pokud se rozhodnete, že doporučení neplatí, zavřete doporučení a potom filtrovat je možné ho mimo zobrazení. Chcete-li zrušit doporučení dvěma způsoby. Jedním ze způsobů je klikněte pravým tlačítkem myši položku a potom vyberte **Zavřít**. Druhý je najeďte myší na položku, klikněte na tři tečky, které se objeví vpravo a pak vyberte **Zavřít**. Klepnutí na tlačítko **Filtr**a výběrem **Dismissed**můžete zobrazit propuštěné doporučení.

    ![Zavřete doporučení][3]

### <a name="apply-recommendations"></a>Použití doporučení
Po zkontrolování všechny doporučení, rozhodnete, které jste má použít první. Doporučujeme použít závažnosti jako hlavní parametr vyhodnocení které doporučení tomu nejdřív.

V tabulce doporučení nahoře vyberte doporučení a projděte si ho jako příklad použití doporučení.

## <a name="see-also"></a>Viz taky
V tomto dokumentu byly vydané doporučení zabezpečení v Centru zabezpečení. Další informace o Centru zabezpečení, najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md) – zjistěte, jak konfigurovat zásady zabezpečení Azure předplatná a skupiny zdrojů.
- [Sledování v Centru zabezpečení Azure stavu zabezpečení](security-center-monitoring.md) – zjistěte, jak sledovat stav Azure prostředků.
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení.
- [Sledování partnerských řešení s Azure Centrum zabezpečení](security-center-partner-solutions.md) – zjistěte, jak sledovat stav partnerských řešení.
- [Časté otázky k Centru zabezpečení Azure](security-center-faq.md) – najít nejčastější dotazy k použití služby.
- [Zabezpečení Azure blog](http://blogs.msdn.com/b/azuresecurity/) – najít příspěvky Azure zabezpečení a dodržování předpisů.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
[3]: ./media/security-center-recommendations/dismiss-recommendations.png

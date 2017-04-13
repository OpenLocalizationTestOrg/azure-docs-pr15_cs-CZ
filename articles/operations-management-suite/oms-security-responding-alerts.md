<properties
   pageTitle="Sledování zpráv a reagování na výstrahy zabezpečení v operace správy sadu zabezpečení a auditování řešení | Microsoft Azure"
   description="V tomto dokumentu umožňuje použít možnost intelligence hrozbou, že k dispozici v OMS zabezpečení a auditování sledovat a reagovat na výstrahy zabezpečení."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="monitoring-and-responding-to-security-alerts-in-operations-management-suite-security-and-audit-solution"></a>Sledování zpráv a reagování na výstrahy zabezpečení v operace správy sadu zabezpečení a auditování řešení

Tento dokument pomáhá možnost hrozbou, že intelligence k dispozici v OMS zabezpečení a auditování k sledování zpráv a odpovídání na výstrahy zabezpečení.

## <a name="what-is-oms"></a>Co je OMS?

Microsoft operace správy sady (OMS) je cloudu společnosti Microsoft na základě řešení pro správu IT, který vám pomáhá spravovat a chránit vaše místních a cloudových infrastruktury. Další informace o OMS, přečtěte si článek [Operace správy sadu](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="threat-intelligence"></a>Riziko měřítka

V podnikovém prostředí kde uživatelé mají obecných přístup k síti a připojení k firemní data pomocí různých zařízeních je nezbytné aktivně sledujte zdroje a rychle odpovědět na zabezpečení incidentem. Toto je důležité z hlediska zabezpečení životního cyklu, protože některé počítačové bezpečnosti, který nemusí vysokoškolskou hrozeb upozornění příslušné nebo podezřelých aktivit, jež technické ovládacími prvky tradiční zabezpečení. 

Pomocí možnosti **Měřítka hrozbou, že** k dispozici v OMS zabezpečení a auditování správce IT a obsahuje například identifikace hrozeb zabezpečení v prostředí, můžete určit, zda určitého počítače je součástí [botnet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection). Počítače se může stát uzlů v botnet při útočníků nedovoleným způsobem instalace malware, který tajně tento počítač připojuje a příkaz Ovládací prvek. Můžete taky určit potenciální hrozeb pocházejících z kanály podzemní komunikace, třeba [darknet](https://www.microsoft.com/security/sir/story/default.aspx#!botnetsection_honeypots_darkents). 

Abyste mohli vytvořit toto riziko měřítka, OMS zabezpečení a auditování pomocí dat pocházejících z různých zdrojů aplikace Microsoft. Tato data k identifikaci potenciální hrozeb proti prostředí budou využít OMS zabezpečení a auditování.

Podokno Intelligence hrozbou, že tvoří tři hlavní možností:
- Servery s odchozí útokům
- Typy zjištěné hrozeb
- Riziko intelligence mapy

> [AZURE.NOTE] Přehled o všech možnostech přečtěte si [Začínáme s operace správy sadu zabezpečení a auditování řešení](oms-security-getting-started.md).

### <a name="responding-to-security-alerts"></a>Reagovat na výstrahy zabezpečení

Jedním z kroků procesu [incidentu zabezpečení](https://technet.microsoft.com/library/cc512623.aspx) je k identifikaci závažnosti systémy narušení zabezpečení. V této fázi má provádět následující úkoly:

- Určení povahy útok
- Určení útok bodu počátku
- Určení záměr útok. Byla útok konkrétně řízené ve vaší organizaci získat konkrétních informací nebo byla náhodné?
- Vyhledání počítačů, které byly ohroženo
- Soubory, které máte dostupné a zjistit, zda rozlišuje tyto soubory identifikovat

Můžete využít **Intelligence hrozbou, že** informace v OMS zabezpečení a auditování řešení pomoct s těmito úkoly. Postupujte podle pokynů pro přístup k této možnosti **Měřítka hrozbou, že** :

1. Na řídicím panelu hlavní **Microsoft operace správy Suite** klikněte na dlaždici **zabezpečení a auditování** .

    ![Zabezpečení a auditování](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)

2. Na řídicím panelu **zabezpečení a auditování** zobrazí se možnosti **Měřítka hrozbou, že** v pravém jak je ukázáno v následujícím příkladu:

    ![Riziko intel](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig2-ga.png)

Tyto tři dlaždice získáte základní informace o aktuálním hrozeb. Na **Server s odchozí útokům** , které budete moci určit, zda je počítač, který sledujete (uvnitř a mimo vaší síti), které odesílá útokům k Internetu. 

Dlaždice **Detected hrozbou, že typy** zobrazuje souhrn hrozeb, které jsou aktuální "přírody", pokud kliknete na této dlaždici zobrazí se další informace o těchto hrozeb jak je vidět níže:

![Typy zjištěné hrozbou](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig3.png)

Další informace o jednotlivých hrozbou, že se dají extrahovat kliknutím na něj. Další informace o Botnet ukazuje tento příklad:

![Další informace o nebezpečí](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig4.png)

Podle popisu v na začátek této části, tyto informace mohou být velmi užitečná při případu incidentu odpověď. Lze ji důležité během soudní vyšetřování, kdy je potřeba najít zdroj útok systém, který byl ohroženo a časové osy. V této sestavě můžete snadno identifikovat některé klíčové údaje o útok, jako například: zdroj útok, místní IP, který byl ohroženo a aktuální relaci stav připojení. 

**Mapa intelligence hrozbou, že** vám pomůže identifikovat aktuální umístění ve světě, které mají útokům. Existuje (příchozí) oranžová a červené (odchozí) šipky v tomto mapu označující směr přenosu, pokud kliknete na tlačítko v jednom z těchto šipky, bloku se blok zobrazí typ hrozbou, že a směr přenosu, jak je ukázáno v následujícím příkladu:

![hrozbou, že doplněk mapy](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig5.png)

> [AZURE.NOTE] Zobrazit ukázku o používání této funkce během incident odpověď procesy sledování prezentace [rizika zabezpečení datacentra Mitigate pomocí s průvodcem vyšetřování pomocí operace správy sadu](https://myignite.microsoft.com/videos/5000) dodaných v aplikaci Microsoft Ignite.

## <a name="see-also"></a>Viz taky

V tomto dokumentu se naučíte používat možnost **Intelligence hrozbou, že** v OMS zabezpečení a auditování řešení reagovat na výstrahy zabezpečení. Další informace o OMS zabezpečení, naleznete v následujících článcích:

- [Přehled správy sady (OMS) operace](operations-management-suite-overview.md)
- [Začínáme s operace správy sadu zabezpečení a auditování řešení](oms-security-getting-started.md)
- [Sledování zdrojů v operace správy sadu zabezpečení a auditování řešení](oms-security-monitoring-resources.md)

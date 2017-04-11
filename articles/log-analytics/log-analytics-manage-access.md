<properties
    pageTitle="Správa přístupu k protokolu analýzy | Microsoft Azure"
    description="Správa přístupu k protokolu analýzy pomocí různých úlohy správy na uživatele, účty, OMS pracovních prostorů a Azure účty."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="banders"/>

# <a name="manage-access-to-log-analytics"></a>Správa přístupu k protokolu analýzy

Správa přístupu k protokolu analýzy, používáte spoustu úlohy správy na uživatele, účty, OMS pracovních prostorů a Azure účty. Vytvoření nového pracovního prostoru v sadě operace Management (OMS), vyberte název pracovního prostoru přidružit k účtu, a zvolte geografické polohy. Pracovní prostor je v podstatě kontejner, který obsahuje informace o účtu a jednoduché konfigurace informace o účtu. Vy nebo jiní členové vaší organizace pomocí několika pracovní prostory OMS řídit různé sady dat, která se shromažďují ze všech nebo části infrastrukturu.

V článku [Začínáme s protokolu analýzy](log-analytics-get-started.md) se dozvíte jak rychle získat a spuštěn a ve zbývající části tohoto článku podrobněji některé akce budete potřebovat pro správu přístupu k OMS.

I když potřebujete nemusí provádět všechny úlohy správy nejdřív, ukazuje podrobný běžně používaných úkoly, které můžete použít v následujících částech:

- Určení počtu pracovní prostory, které potřebujete
- Správa účtů a uživatelů
- Přidání skupiny do existujícího pracovního prostoru
- Propojení stávajícího pracovního prostoru předplatné Azure
- Upgrade na placené datový tarif pracovního prostoru aplikace Groove
- Změna datového typu plánu
- Přidání organizaci Azure Active Directory do existujícího pracovního prostoru
- Zavřete OMS pracovního prostoru

## <a name="determine-the-number-of-workspaces-you-need"></a>Určení počtu pracovní prostory, které potřebujete

Pracovní prostor je Azure zdroje a je kontejner, kde je dat shromážděných, agregován, analyzovat a prezentovat na portálu OMS.

Je možné vytvořit více pracovní prostory OMS protokolu technologie pro analýzu a aby je uživatelé mohli mít taky přístup k jedné nebo více pracovní prostory. Obecně chcete minimalizovat počet pracovní prostory, jak je to vám umožní dotazu a sladit přes většina data. Tato část popisuje, kdy může být užitečné, jak vytvořit víc než jeden pracovní prostor.

Dnes poskytuje pracovní prostor protokolu analýzy:

- Zeměpisná místa pro uložení dat
- Rozlišení k fakturaci
- Izolace dat

Podle výše uvedených vlastností, můžete si vytvořit více pracovní prostory, pokud:

- Jste globální společnosti a budete potřebovat data uložená v konkrétní oblasti dat svrchovanost nebo dodržování předpisů důvodů.
- Používáte Azure a chcete nižším poplatkům za přenos odchozí datové tím, že pracovní prostor protokolu analýzy ve stejné oblasti jako Azure prostředky, které spravuje.
- Chcete-li přidělit náklady různá oddělení nebo skupinám firmy podle jejich použití. Při vytváření pracovního prostoru pro každé oddělení nebo skupiny firmy Azure stůl a používání údajů zobrazuje poplatky za jednotlivých pracovních prostorech samostatně.
- Jste poskytovatel služba spravovaných a potřebujete zachovat technologie pro analýzu dat protokolu pro jednotlivé zákazníky spravujete izolovaný od ostatních zákaznících.
- Správa více zákazníků a chcete každého zákazníků nebo oddělení nebo obchodní skupiny zobrazíte svá data, ale nikoli data pro ostatní zákazníci nebo oddělení nebo obchodní skupiny.

Při používání agentů sběr dat, můžete nakonfigurovat každý agent vykazování do požadované pracovního prostoru.

Pokud používáte systém Centrum Operations manager, každá skupina správy Operations Manager můžete spojené s jedinou pracovního prostoru. Můžete nainstalovat Microsoft Agent sledování počítačů spravuje Operations Manager a nechat agent sestava Operations Manager a jiného pracovního prostoru protokolu analýzy.

### <a name="workspace-information"></a>Informace o pracovním prostoru

Na portálu OMS můžete zobrazit informace o pracovním prostoru a zvolte, jestli chcete dostávat informace o společnosti Microsoft.

#### <a name="view-workspace-information"></a>Zobrazení informací o pracovním prostoru

1. V OMS klikněte na dlaždici **Nastavení** .
2. Klikněte na kartu **Klienti** .
3. Klikněte na kartu **Informace o pracovním prostoru** .  
  ![Informace o pracovním prostoru](./media/log-analytics-manage-access/workspace-information.png)

## <a name="manage-accounts-and-users"></a>Správa účtů a uživatelů

Každý pracovní prostor můžete mít víc uživatelských účtů přidružená a každý uživatelský účet (účet Microsoft nebo účet organizace) můžete mít taky přístup k několika pracovní prostory OMS.

Ve výchozím nastavení účtu Microsoft nebo účet organizace slouží k vytvoření pracovního prostoru stane se správcem pracovního prostoru. Správce můžete pozvat další účtů Microsoft nebo vyberte uživatele ze služby Azure Active Directory.

Poskytnutí přístupu lidi do pracovního prostoru OMS řízeno 2 místa:

- Řízení přístupu na základě rolí v Azure, slouží k poskytnutí přístupu k Azure předplatné a související Azure zdroje. Taky používá se pro přístup k Powershellu a rozhraní REST API.
- Na portálu OMS přístup jenom portál OMS – není přidružená Azure předplatného.

Pokud chcete uživatelům přístup k portálu OMS, ale ne k Azure předplatné, které se otevře, pak řešení dlaždice automatizaci, zálohování a obnovení webu nezobrazovat veškerá data pro uživatele při jejich přihlašovací portálu OMS.

Povolit všem uživatelům zobrazit data v tato řešení, ujistěte se, že mají alespoň **čtečka** přístup k pro automatizaci účtu trezoru zálohování a obnovení webu trezoru, která je propojena s centrem OMS.   

### <a name="managing-access-to-log-analytics-using-the-azure-portal"></a>Správa přístupu k protokolu analýzy pomocí portálu Azure

Pokud chcete uživatelům přístup do pracovního prostoru protokolu analýzy pomocí Azure oprávnění, na portálu Azure například klikněte stejné uživatelé přístup k portálu protokolu analýzy. Pokud uživatelé můžou na portálu Azure, budou přejděte na portál OMS kliknutím na **Portál OMS** úkol při prohlížení zdroje pracovního prostoru protokolu analýzy.

Některé aspekty mít na paměti o Azure portálu:

- Toto není *řízení přístupu na základě rolí*. Pokud máte *čtečka* přístupová oprávnění v portálu Azure pracovního prostoru protokolu analýzy, můžete dělat změny pomocí portálu OMS. Na portálu OMS má koncepci správce skupiny přispěvatelů a uživatel jen pro čtení. Je-li účet, který používáte podepsané přihlášení k aplikaci služby Azure Active Directory propojené do pracovního prostoru bude správce na portálu OMS, jinak bude přispěvatele.

- Když jste přihlášení k portálu OMS pomocí http://mms.microsoft.com, pak ve výchozím nastavení se zobrazí v seznamu **Vyberte pracovního prostoru aplikace Groove** . Obsahuje pouze pracovní prostory, které byly přidány pomocí portálu OMS. Zobrazíte pracovních prostorech máte přístup k s Azure předplatné a pak budete muset zadat klienta jako součást adresy URL. Příklad:

  `mms.microsoft.com/?tenant=contoso.com`Identifikátor klienta je často poslední část e-mailovou adresu, která jste přihlášení k aplikaci.

- Je-li účet se přihlaste se pomocí účtu v klientovi Azure Active Directory, která nastane obvykle Pokud nejste přihlášení jako CSP, bude *Správce* na portálu OMS. Pokud váš účet není v klientovi Azure Active Directory, bude *uživatel* v portálu OMS.

- Pokud chcete přejít přímo na portálu mít přístup k používání Azure oprávnění, budete muset zadat zdroje jako součást adresy URL. Je možné získat tato adresa URL pomocí Powershellu.

  Například `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  Adresa URL bude vypadat jako:`https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`


### <a name="managing-users-in-the-oms-portal"></a>Správa uživatelů v portálu OMS

Správa uživatelů a ve skupině **Správa uživatelů** na kartě **účty** na stránce nastavení. Tady můžete provádět úkoly v následujících částech.  

![Správa uživatelů](./media/log-analytics-manage-access/setup-workspace-manage-users.png)

#### <a name="add-a-user-to-an-existing-workspace"></a>Přidání uživatele do existujícího pracovního prostoru

Pomocí následujících kroků přidat uživatele nebo skupinu do pracovního prostoru OMS. Uživatel nebo skupina bude moct prohlížet ani chovalo na všechna upozornění, které jsou přidružené k tento pracovní prostor.

>[AZURE.NOTE] Pokud chcete přidat uživatele nebo skupinu ze svého účtu organizace Azure Active Directory, musíte nejdřív se ujistit, že jste přiřadili účtu OMS s vlastní doménou služby Active Directory. Přečtěte si článek [Přidání Azure Active Directory organizaci stávající schůzek](#add-an-azure-active-directory-organization-to-an-existing-workspace).

1. V OMS klikněte na dlaždici **Nastavení** .
2. Klikněte na kartu **Klienti** a pak klikněte na kartu **Správa uživatelů** .
3. V části **Spravovat uživatele** zvolte typ účtu přidáte: **Účet organizace**, **Účtu Microsoft**, **Podpora společnosti Microsoft**.
    - Pokud jste si vybrali Microsoft Account, zadejte e-mailovou adresu uživatele přidružené k Account Microsoft.
    - Pokud se rozhodnete účet organizace, můžete zadat část uživatele nebo skupiny jméno nebo e-mailové aliasy a zobrazí se seznam uživatelů a skupin. Vyberte uživatele nebo skupiny.
    - Použijte Microsoft Support a udělte tak aplikaci Microsoft Support zpětnou analýzu dočasný přístup do pracovního prostoru pomoci při řešení.

    >[AZURE.NOTE] Nejlepších výsledků dosáhnete výkonu, omezení počtu skupin Active Directory přidružené k účtu jednoho OMS třem – jeden pro správce, jedno pro přispěvatelé a jeden pro uživatele jen pro čtení. Použití více skupin může mít vliv na výkon protokolu analýzy.

5. Zvolte typ uživatele nebo skupiny přidat: **Správce**, **Přispěvatel**nebo **Uživatel jen pro čtení** .  
6. Klikněte na **Přidat**.

  Chcete-li přidat účet Microsoft, se pošle Pozvánka se chcete připojit pracovní prostor s e-mailem, který jste zadali. Poté, co uživatel sleduje pokynů uvedených v pozvání k připojení OMS, uživatel může zobrazit oznámení a informací o účtu pro tento účet OMS a budete moct zobrazit informace o uživateli na kartě **Klienti** na stránce **Nastavení** .
  Chcete-li přidat účet organizace, uživatel bude mít okamžitě přístup k protokolu analýzy.  
  ![e-mailové pozvánce](./media/log-analytics-manage-access/setup-workspace-invitation-email.png)

#### <a name="edit-an-existing-user-type"></a>Upravit stávající uživatelský typ

Můžete změnit roli účtů pro uživatele přidruženého k vašemu účtu OMS. Máte k dispozici následující možnosti role:

 - *Správce*: můžete Správa uživatelů, zobrazit a pracovat na všechna upozornění a přidání a odebrání serverů

 - *Přispěvatel*: můžete zobrazit a pracovat na všechna upozornění a přidání a odebrání serverů

 - *Uživatel jen pro čtení*: označený jako jen pro čtení uživatelé nebudou moct:
   1. Přidat nebo odebrat řešení. Galerie řešení skryté.
   2. Přidat nebo změnit nebo odebrat dlaždice na **Vlastní řídicí panel**.
   3. Umožňuje zobrazte stránky **Nastavení** . Stránky jsou skryté.
   4. V hledání zobrazení, PowerBI konfigurace, – uložená hledání a upozornění jsou skryté úkoly.


#### <a name="to-edit-an-account"></a>Chcete-li upravit účet

1. V OMS klikněte na dlaždici **Nastavení** .
2. Klikněte na kartu **Klienti** a pak klikněte na kartu **Správa uživatelů** .
3. Vyberte roli uživatele, který chcete změnit.
2. V potvrzovacím dialogu klikněte na **Ano**.

### <a name="remove-a-user-from-a-oms-workspace"></a>Odebrání uživatele z pracovního prostoru aplikace Groove OMS

Odebrání uživatele z pracovního prostoru OMS pomocí následujících kroků. Všimněte si, že to nezavře pracovního prostoru uživatele. Místo toho Odebere přidružení daný uživatel a pracovního prostoru. Pokud je uživatel přidružené k více pracovní prostory, tohoto uživatele nebude Přihlaste se k OMS a zobrazit další pracovní prostory.

1. V OMS klikněte na dlaždici **Nastavení** .
2. Klikněte na kartu **Klienti** a pak klikněte na kartu **Správa uživatelů** .
3. Klikněte na **Odebrat** u uživatelské jméno, které chcete odebrat.
4. V potvrzovacím dialogu klikněte na **Ano**.


### <a name="add-a-group-to-an-existing-workspace"></a>Přidání skupiny do existujícího pracovního prostoru

1.  Podle pokynů v "Chcete-li přidat uživatele do existujícího pracovního prostoru" 1 -4, výše.
2.  V části **Zvolte uživatele nebo skupinu**vyberte **skupinu**.
    ![Přidání skupiny do existujícího pracovního prostoru](./media/log-analytics-manage-access/add-group.png)
3.  Zadejte zobrazované jméno nebo e-mailovou adresu pro skupinu, kterou chcete přidat.
4.  Vyberte skupinu v seznamu výsledků a potom klikněte na **Přidat**.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Propojení stávajícího pracovního prostoru předplatné Azure

Je možné vytvořit pracovní prostor z webu [microsoft.com/oms](https://microsoft.com/oms) .  Však určitých mezí existují pro tyto pracovní prostory nejvíce výrazné právě maximálně 500MB/den ukládání dat, pokud používáte bezplatného účtu. Při změně tento pracovní prostor musíte *propojit existující pracovního prostoru na Azure předplatné*.

>[AZURE.IMPORTANT] Pokud chcete vytvořit odkaz pracovní prostor, musíte účet Azure už máte přístup do pracovního prostoru, který chcete propojit.  Jinými slovy účet, který používáte pro přístup k portálu Azure musí být **stejný** jako účet, který používáte pro přístup k OMS pracovního prostoru. Pokud to není velikost písmen, najdete v článku [Přidání uživatele do existujícího pracovního prostoru](#add-a-user-to-an-existing-workspace).

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>Pokud chcete vytvořit odkaz pracovního prostoru aplikace Groove Azure předplatného na portálu OMS

Pokud chcete vytvořit odkaz pracovního prostoru aplikace Groove Azure předplatného na portálu OMS, musíte přihlášený uživatel už máte účet Azure placené. Pracovní prostor, který používáte aktivně získá propojit Azure klientem.

1. V OMS klikněte na dlaždici **Nastavení** .
2. Klikněte na kartu **Klienti** a potom klikněte na kartu **Azure předplatné a plánování Data** .
3. Klikněte na datový tarif, které chcete použít.
4. Klikněte na **Uložit**.  
  ![plány předplatného a dat](./media/log-analytics-manage-access/subscription-tab.png)

Nový datový tarif se zobrazí na OMS portálu pásu karet v horní části webové stránky.

![Pás karet OMS](./media/log-analytics-manage-access/data-plan-changed.png)

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Pokud chcete vytvořit odkaz pracovního prostoru aplikace Groove Azure předplatného na portálu Azure

1.  Přihlaste se k [portálu Azure](http://portal.azure.com).
2.  Vyhledejte **Protokolu analýzy (OMS)** a vyberte ho.
3.  Zobrazí se seznam existujících pracovních prostorů. Klikněte na **Přidat**.  
    ![seznam pracovních prostorů](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4.  Ve skupinovém rámečku **OMS pracovního prostoru**klikněte na **videa nebo vytvoření odkazu stávající**.  
    ![propojit existující](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5.  Klikněte na **konfigurovat požadované nastavení**.  
    ![konfigurace požadovaná nastavení](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6.  Zobrazí se seznam pracovní prostory, které nejsou zatím spojeným s vaším účtem Azure. Vyberte pracovního prostoru aplikace Groove.  
    ![Vyberte pracovní prostory](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7.  V případě potřeby můžete změnit hodnoty u následujících položek:
    - Předplatné
    - Pole Skupina zdroje
    - Umístění
    - Ceny osy  
        ![Změňte hodnoty](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8.  Klikněte na **vytvořit**. Pracovní prostor je propojená s Azure účtu.

>[AZURE.NOTE] Pokud nevidíte pracovní prostor, který chcete propojit, pak předplatného Azure nemá přístup do pracovního prostoru OMS, kterou jste vytvořili pomocí OMS webu.  Bude potřeba udělit přístup k tomuto účtu z uvnitř OMS pracovního prostoru pomocí OMS webu. Postup najdete v článku [Přidání uživatele do existujícího pracovního prostoru](#add-a-user-to-an-existing-workspace).



## <a name="upgrade-a-workspace-to-a-paid-data-plan"></a>Upgrade na placené datový tarif pracovního prostoru aplikace Groove

Existují tři data pracovního prostoru plánování typy OMS: **Free** **Standardní**a **Premium**.  Pokud používáte plán *Uvolnit* , může mít klikněte na data zakončení 500 MB.  Je třeba upgradovat pracovního prostoru na ***systému průběžného financování plán*** pro shromažďování dat za tímto limitem. Kdykoli můžete převést typu plánu.  Další informace o OMS ceny najdete v článku [Ceny podrobnosti](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx).

>[AZURE.IMPORTANT] Pracovní prostor plány můžete změnit pouze v případě jsou *propojené* s Azure předplatného.  Pokud jste vytvořili pracovního prostoru v Azure nebo pokud jste *už* propojený pracovního prostoru, můžete tuto zprávu ignorovat.  Pokud jste vytvořili pracovního prostoru s [OMS webu](http://www.microsoft.com/oms), musíte postupujte podle pokynů v tématu [propojení stávající schůzek do Azure předplatného](#link-an-existing-workspace-to-an-azure-subscription).

### <a name="using-entitlements-from-the-oms-add-on-for-system-center"></a>Použití nároky z doplňku OMS pro System Center

Doplněk OMS pro System Center obsahuje nároku Premium plán analýzy protokolu OMS uvedeno na stránce [OMS ceny](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx).

Při nákupu OMS doplněk pro System Center OMS doplněk se přidá ve formě nárok na System Center smlouvy. Může být Azure předplatné, které se vytvoří v rámci této smlouvy použití oprávnění. Díky, například mít víc OMS pracovní prostory, které použití oprávnění z doplňku OMS.

K zajištění použití pracovního prostoru OMS do svého nároky z doplňku OMS, musíte:

1. Propojení pracovního prostoru OMS k předplatnému Azure, která je součástí Enterprise Agreement, která obsahuje OMS doplněk nákupní a použití Azure předplatného
2. Vyberte plán Premium pracovního prostoru

Při kontrole použití portálu Azure nebo OMS neuvidíte nároky doplněk OMS. Však uvidíte nároky na portálu Enterprise.  

Pokud potřebujete změnit Azure předplatné, které pracovního prostoru OMS otevře, můžete použít rutinu Powershellu Azure [AzureRmResource přesunout](https://msdn.microsoft.com/library/mt652516.aspx) .

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Použití Azure potvrzení z Enterprise Agreement

Pokud se rozhodnete sdělit nám samostatného ceny pro OMS součásti, bude platit pro jednotlivé součásti OMS samostatně a používání se zobrazí na Azure faktuře.

Pokud máte Azure peněžní potvrdit u enterprise přihlášení, ke kterému jsou propojené předplatné Azure, všechny použití protokolu analýzy bude automaticky debetní proti žádné zbývající peněžní potvrzení.

Pokud potřebujete změnit Azure předplatného, že je pro vás propojený OMS pracovní prostor můžete použít rutinu Powershellu Azure [AzureRmResource přesunout](https://msdn.microsoft.com/library/mt652516.aspx) .  



### <a name="to-change-a-workspace-to-a-paid-data-plan"></a>Chcete-li změnit pracovní prostor na placené datový tarif

1.  Přihlaste se k [portálu Azure](http://portal.azure.com).
2.  Vyhledejte **Protokolu analýzy (OMS)** a vyberte ho.
3.  Zobrazí se seznam existujících pracovních prostorů. Vyberte pracovního prostoru aplikace Groove.  
    ![seznam pracovních prostorů](./media/log-analytics-manage-access/manage-access-change-plan01.png)
4.  V části **Nastavení**klikněte na **ceny osy**.  
    ![ceny osy](./media/log-analytics-manage-access/manage-access-change-plan02.png)
5.  V části **ceny osy**vyberte datový tarif a potom klikněte na **Výběr**.  
    ![Vyberte plán](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6.  Když provedete aktualizaci zobrazení na portálu Azure, zobrazí se **ceny osy** aktualizace plánu, který jste vybrali.  
    ![Aktualizace ceny osy](./media/log-analytics-manage-access/manage-access-change-plan04.png)

Teď můžete shromažďovat data o za zakončení "zdarma" data.


## <a name="add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Přidání organizaci Azure Active Directory do existujícího pracovního prostoru

Přidružení pracovního prostoru protokolu analýzy (OMS) s služby Azure Active Directory. Umožňuje přidávat uživatele ze služby Active Directory přímo do pracovního prostoru OMS bez nutnosti samostatné svým účtem Microsoft.

Při vytváření pracovního prostoru z portálu Microsoft Azure nebo odkaz na odběr Azure pracovního prostoru služby Azure Active Directory propojena jako účtu vaší organizace.

Při vytváření pracovního prostoru z portálu OMS se výzva k propojení s Azure předplatné a účet organizace.

### <a name="to-add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Chcete-li přidat organizaci Azure Active Directory do existujícího pracovního prostoru

1. Na stránce nastavení v OMS klikněte na **účty** a potom klikněte na kartu **Informace o pracovním prostoru** .  
2. Přečtěte si informace o účty organizace a klikněte na **Přidat organizace**.  
    ![Přidání organizace](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Zadejte informace pro správce o identitě domain Azure Active Directory. Později zobrazí se potvrzení informacemi o tom, že pracovního prostoru je propojený s domain Azure Active Directory.
    ![potvrzení propojené pracovního prostoru](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

>[AZURE.NOTE] Jakmile váš účet je propojen účet organizace, propojení nelze odebrat nebo změnit.

## <a name="close-your-oms-workspace"></a>Zavřete OMS pracovního prostoru

Pokud ukončíte pracovního prostoru OMS všechna data týkající se pracovního prostoru se odstraní z službu OMS do 30 dnů důsledcích uzavření účtu pracovního prostoru.

Pokud jste správce a související s centrem více uživatelů, přidružení tyto uživatele a pracovní prostor se přeruší. Pokud uživatelé jsou přidružené k jiné pracovní prostory, potom budou můžete nadále používat OMS s těmito prostory. Ale pokud nejsou přidružené k jiných pracovních prostorů pak budou se muset vytvořit nový pracovní prostor můžete OMS.

### <a name="to-close-an-oms-workspace"></a>Zavřete OMS workspace

1. V OMS klikněte na dlaždici **Nastavení** .
2. Klikněte na kartu **Klienti** a pak klikněte na kartu **Informace o pracovním prostoru** .
3. Klikněte na **Zavřít pracovního prostoru**.
4. Vyberte jeden z důvodů uzavření pracovního prostoru nebo zadejte jiný důvod, proč do textového pole.
5. Klikněte na **Zavřít pracovního prostoru**.

## <a name="next-steps"></a>Další kroky

- V tématu [připojení Windows počítačů protokolu analýzy](log-analytics-windows-agents.md) shromažďování dat a přidejte agentů.
- [Přidání analýzy protokolu řešení z Galerie řešení](log-analytics-add-solutions.md) shromažďování dat a přidejte funkce.
- [Konfigurace nastavení proxy a brány firewall v protokolu analýzy](log-analytics-proxy-firewall.md) Pokud vaše organizace používá proxy server a bránu firewall, aby agentů komunikovat pomocí služby protokolu analýzy.

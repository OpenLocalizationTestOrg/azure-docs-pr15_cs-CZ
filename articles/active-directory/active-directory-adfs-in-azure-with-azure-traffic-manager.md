<properties
    pageTitle="Dostupnost křížově zeměpisné AD FS nasazení v Azure pomocí Správce přenos Azure | Microsoft Azure"
    description="V tomto dokumentu se Naučte se nasadit služby AD FS v Azure pro vysokou dostupnosti."
    keywords="Služba AD fs s Azure přenosy správce, služby AD FS s Azure přenosy správci zeměpisné, více datacentru, zeměpisné datacentrech, zeměpisné datacentrech více nasazení služby AD FS v azure, nasazení služby azure AD FS, služby azure AD FS, služby azure ad fs, nasadit služby AD FS, nasadit služby ad fs, služby AD FS v azure, nasadit služby AD FS v azure, nasazení služby AD FS v azure, azure služby AD FS, úvod do služby AD FS, Azure AD FS v Azure, iaas , Přesunutí služby AD FS, služby AD FS azure"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="anandy;billmath"/>
    
#<a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Dostupnost křížově zeměpisné AD FS nasazení v Azure pomocí Správce přenosy Azure

[Nasazení služby AD FS v Azure](active-directory-aadconnect-azure-adfs.md) poskytuje podrobné pokyny, jak nasadit jednoduché infrastruktury služby AD FS pro vaši organizaci v Azure. Tento článek obsahuje následující kroky, kterými vytvořit více zeměpisné nasazení služby AD FS v Azure pomocí [Správce dopravy Azure](../traffic-manager/traffic-manager-overview.md). Azure správce přenosy pomáhá vytvářet geograficky šíření dostupnost a infrastruktury služby AD FS výkonné pro vaši organizaci tím, že využívání oblast metody směrování přizpůsobili z infrastrukturu.

Vysoce k dispozici více zeměpisné infrastruktury služby AD FS umožňuje:

* **Odstranění z jednoho místa selhání:** S převzetí správce dopravy Azure můžete dosáhnout vysoce dostupné infrastruktury služby AD FS i v případě, že jeden datacentrech v části glóbusu přejde
* **Zvýšený výkon:** Navrhované nasazení v tomto článku umožňuje poskytovat infrastruktury služby AD FS výkonné, vám můžou pomoct uživatelé ověření rychleji. 

##<a name="design-principles"></a>Principy návrhu

![Celkový návrh](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

Základní zásady budou stejné jako v seznamu uvedený v návrhu zásady v článku nasazení služby AD FS v Azure. Výše uvedené diagram znázorňuje jednoduchého rozšíření základní nasazení pro jinou zeměpisnou oblast. Tady jsou některé body potřeba zvážit při rozšíření nasazení do nového zeměpisnou oblast

* **Virtuální sítě:** Vytvořte novou síť virtuální v zeměpisnou oblast, kterou chcete nasadit další infrastruktury služby AD FS. V diagramu nahoře vidíte Geo1 VNET a Geo2 VNET jako dvě virtuální sítě v jednotlivých zeměpisnou oblast.

* **Řadiče domény a servery služby AD FS v nové zeměpisné VNET:** Doporučuje se nasadit doménu, kterou řadiče v nové zeměpisnou oblast tak, aby servery služby AD FS v oblasti nová nemají kontaktovat řadiče domény v jiném daleko sítě dokončete ověřování a následné zvýšení výkonu.

* **Úložiště účty:** Účty úložiště jsou přidružené k oblasti. Vzhledem k tomu budete nasazovat počítačích v novou zeměpisnou oblast, budete muset vytvořit nový účet úložiště se nemusí používat v oblasti.  

* **Sítě skupiny zabezpečení:** Jak úložiště účty, skupiny zabezpečení sítě vytvořené v oblasti nelze použít v jinou zeměpisnou oblast. Proto je bude potřeba vytvořit novou síť skupiny zabezpečení podobají profilům ve první zeměpisnou oblast INT a DMZ podsítě v nové zeměpisnou oblast.

* **DNS štítků pro veřejnou IP adresy:** Azure přenosy správce může označovat koncové body jen přes popisky DNS. Proto musíte vytvořit štítky DNS pro externí zatížení vyrovnávání veřejné IP adres.

* **Azure přenosy správce:** Správce Microsoft Azure přenosy umožňuje řídit rozdělení umožnění datových přenosů uživatele do služby koncových bodů spuštěné v různých datacentrech ve světě. Azure správce přenosy funguje na úrovni DNS. Použije odpovědi DNS tak, aby směrovaly koncových uživatelů k globálně distributed koncové body. Klienti připojte k těmto koncové body přímo. S různé možnosti směrování výkonu, vážená a priority (priorita) ale můžete snadno Odsouhlasíte směrování nejlíp vyhovuje potřebám vaší organizace. 

* **V čistého na V čisté propojení mezi dvěma oblastmi:** Nemusíte k dispozici připojení mezi virtuálních sítí samotné. Vzhledem k tomu každý virtuální sítě má přístup k řadiče domény a má server služby AD FS a WAP sám o sobě, můžete pracovat bez žádné propojení mezi virtuálních sítí v různých oblastech. 

##<a name="steps-to-integrate-azure-traffic-manager"></a>Postup při integraci Azure přenosy správce

###<a name="deploy-ad-fs-in-the-new-geographical-region"></a>Nasazení služby AD FS v nové zeměpisnou oblast
Postupujte podle pokynů a pokyny v [nasazení služby AD FS v Azure](active-directory-aadconnect-azure-adfs.md) nasazení stejné topologie v nové zeměpisnou oblast.

###<a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>Popisky DNS pro veřejné adresy IP Vyrovnávání zatížení Internet protilehlé (veřejné)
Výše uvedené, správce přenosy Azure mohou pouze odkazovat na popisky DNS jako koncové body a proto je důležité vytvářet štítky DNS pro externí zatížení vyrovnávání veřejné IP adres. Následující snímek obrazovky zobrazuje, jak konfigurovat DNS štítku na veřejnou IP adresu. 

![Popisek DNS](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

###<a name="deploying-azure-traffic-manager"></a>Nasazení Azure přenos správce

Postupujte podle pokynů a vytvořte profil správce přenosy. Další informace můžete také odkazovat na [spravovat správce přenosy Azure profilu](../traffic-manager/traffic-manager-manage-profiles.md).

1. **Vytvořit profil správce dopravy:** Pojmenujte svůj profil správce přenosy jedinečný. Tento název profilu zadejte část názvu DNS a slouží jako předponu popisku název domény přenosy správce. Název / předponu přibude. trafficmanager.net k vytvoření popisu DNS pro přenosy pro vaši správce. Následující snímek obrazovky zobrazí přenos Správce DNS předpon nastavit jako mysts a výsledná popisek DNS bude mysts.trafficmanager.net. 

    ![Vytvoření profilu přenosy správce](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
 
2. **Provoz metodu směrování:** Nejsou k dispozici ve Správci přenosy tři možnosti směrování:

    * Priority (priorita) 
    * Výkon
    * Váha
    
    **Výkon** odpovídá doporučené možnost dosáhnout vysoce citlivé infrastruktury služby AD FS. Však můžete způsobem směrování nejlíp vyhovuje vašim potřebám nasazení. Funkce služby AD FS nejsou ovlivněny směrování možnost. Další informace najdete v tématu [metody směrování přenosu přenosy správce](../traffic-manager/traffic-manager-routing-methods.md) . Ve výběru vidí snímek nad je vybrané metodě **výkonu** .
   
3.  **Konfigurace koncové body:** Na stránce Správce přenosy klikněte na koncové body a klikněte na Přidat. Otevře se stránku koncový bod přidat podobně jako snímek dole
 
    ![Konfigurace koncové body](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
 
    Pro různé vstupy pokynu níže:

    **Typ:** Jak můžeme bude ukazovat Azure veřejnou IP adresu, vyberte Azure koncový bod.

    **Název:** Vytvořte název, který chcete přidružit k koncový bod. Toto není název DNS a nemá žádný vliv na záznamy DNS.

    **Obrázku typ zdroje:** Vyberte veřejnou IP adresu jako hodnota, kterou chcete této vlastnosti. 

    **Obrázku zdroje:** To vám nabídne možnost z různých štítků DNS zvolte, jestli že máte k dispozici v rámci předplatného. Zvolte DNS popisku pro.

    Přidání koncový bod pro každou zeměpisná oblast, kterou chcete správce dopravy Azure provoz směrovat na.
    Další informace a podrobné pokyny o tom, jak přidat a nakonfigurovat koncové body ve Správci přenos naleznete v příručce přidáte [zakázat, povolit nebo odstranění koncové body](../traffic-manager/traffic-manager-endpoints.md)
    
4. **Konfigurovat zkušební:** Na stránce Správce přenosy na ke konfiguraci. Na stránce konfigurace potřebujete změnit nastavení monitoru a probe na HTTP porty 80 a relativní cestu /adfs/probe

    ![Konfigurace zkušební](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 

    >[AZURE.NOTE] **Zajistěte, aby byl stav koncové body ONLINE po dokončení konfigurace**. Pokud jsou všechny koncové body v "jejich funkčnost bude omezena" stavu, bude Azure přenosy vedoucí provést nejlepší pokus o směrování přenosu za předpokladu, že je nesprávné Diagnostika a všechny koncové body jsou dostupné.

5. **Úpravy DNS záznamů pro správce dopravy Azure:** Federace služby by měl být CNAME názvu Azure správce dopravy DNS. Vytvořte záznam CNAME ve veřejných záznamech DNS tak, že ten, kdo se pokouší dosáhla službu federačního skutečně dosáhne správce přenosy Azure.

    Například tak, aby ukazovaly fs.fabidentity.com služby federace správci přenosy, by potřebujete aktualizovat záznam prostředku DNS být následující:

    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

##<a name="test-the-routing-and-ad-fs-sign-in"></a>Test směrování a přihlašovací služby AD FS   

###<a name="routing-test"></a>Směrování test

Velmi jednoduché test pro směrování by zkusit ping název federace služby DNS z počítače v jednotlivých zeměpisnou oblast. V závislosti na metodu směrování vybrali se projeví v zobrazení ping koncový bod, který je opravdu pomocí příkazu ping. Například pokud jste vybrali, směrování výkonu, pak koncový bod nejblíže oblasti klienta uplyne. Následuje snímek dvou příkazu ping ze dvou jiné oblasti klientské počítače, v oblasti EastAsia a jeden v západní USA. 

![Směrování test](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

###<a name="ad-fs-sign-in-test"></a>AD FS přihlašovací test

Nejjednodušším způsobem vyzkoušet službu AD FS je pomocí stránky IdpInitiatedSignon.aspx. Pokud chcete mít možnost dělat, že není potřeba povolit IdpInitiatedSignOn vlastností AD FS. Postupujte podle pokynů a zkontrolujte nastavení služby AD FS
 
1. Spustit pod rutina na server služby AD FS online přes, povolte. Set AdfsProperties - EnableIdPInitiatedSignonPage $true
2. Z jakékoli externí počítače přístup https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. Na stránce služby AD FS byste měli vidět jako níže:

    ![Test služby AD FS - ověřovací kód programu ověřování](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)

    a na úspěšně přihlásit, se vám poskytne zprávu jak je ukázáno v následujícím příkladu:

    ![Služby AD FS test - úspěch ověřování](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)
 
##<a name="related-links"></a>Související odkazy
* [Základní informace o nasazení služby AD FS v Azure](active-directory-aadconnect-azure-adfs.md)
* [Správce přenosy v síti Microsoft Azure](../traffic-manager/traffic-manager-overview.md)
* [Metody směrování přenosu přenos správce](../traffic-manager/traffic-manager-routing-methods.md)

##<a name="next-steps"></a>Další kroky
* [Správa profil správce přenos Azure](../traffic-manager/traffic-manager-manage-profiles.md)
* [Přidání, zakázat, povolit nebo odstranění koncové body](../traffic-manager/traffic-manager-endpoints.md) 


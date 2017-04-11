<properties
    pageTitle="Active Directory Federation Services v Azure | Microsoft Azure"
    description="V tomto dokumentu se Naučte se nasadit služby AD FS v Azure pro vysokou dostupnosti."
    keywords="nasazení služby AD FS v azure, nasazení služby azure AD FS, služby azure AD FS, služby azure ad fs, nasadit služby AD FS, nasazení služby ad fs, služby AD FS v azure, nasadit služby AD FS v azure, nasazení služby AD FS v azure, azure služby AD FS, úvod do služby AD FS, Azure AD FS v Azure, iaas, služby AD FS, přesunutí adfs azure"
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
    ms.date="10/03/2016"
    ms.author="anandy;billmath"/>

# <a name="ad-fs-deployment-in-azure"></a>Nasazení AD FS v Azure 

Služba AD FS nabízí federaci identity zjednodušené, zabezpečené a Web jednotné přihlašování (SSO) možnosti. Federaci s Azure AD nebo O365 uživatelům umožňuje ověřování pomocí přihlašovacích údajů místní a přístup k všech zdrojů v cloudu. V důsledku toho z něj stal důležité, abyste měli vysoce dostupné infrastruktury služby AD FS zajistit přístup ke zdrojům obou místních i cloudových. Nasazení služby AD FS v Azure dosáhnete dostupnost povinné s minimálními úsilí.
Existuje několik výhod zavedení služby AD FS v Azure, několik z nich jsou uvedeny níže:

* **Dostupnost** – s power Azure dostupnost sady zajistíte infrastrukturu vysoce dostupné.
* **Snadno měřítko** – třeba výkon? Snadno migrujte do výkonnější počítače tak, že můžete pouhými několika kliknutími v Azure
* **Křížové Geo redundance** – s můžete mít jistotu, že je infrastrukturu vysoce k dispozici po celém světě programem Azure Geo redundance
* **Snadno spravovat** – s možnostmi vysoce zjednodušená správa Azure portálu Správa infrastrukturu je velmi jednoduché a bezproblémové 

## <a name="design-principles"></a>Principy návrhu

![Návrh nasazení](./media/active-directory-aadconnect-azure-adfs/deployment.png)

Diagramu nahoře vidíte základní doporučit zahájíte nasazení infrastruktury služby AD FS v Azure. Principům různých komponent topologie jsou uvedeny níže:

* **Datacentrum / servery služby AD FS**: Pokud máte méně než 1 000 uživatelů můžete jednoduše nainstalujete rolí služby AD FS řadiče domény. Pokud nechcete, aby všechny vliv na výkon na řadiče domény nebo pokud máte víc než 1 000 uživatelů, nasaďte služby AD FS na samostatných serverech.
* **WAP Server** – je nutné nasazení webové aplikace Proxy servery, tak, aby uživatelé mohli kontaktovat AD FS, pokud nejsou v podnikové síti taky.
* **DMZ**: Web aplikace Proxy servery budou umístěny DMZ a přístup jenom TCP/443 je povolen mezi DMZ a vnitřních podsítí.
* **Vyrovnávání zatížení**: zajistit dostupnost servery služby AD FS a proxy serveru webové aplikace, doporučujeme vám použít vyrovnávání zatížení vnitřní servery služby AD FS a služby Vyrovnávání zatížení Azure pro webovou aplikaci Proxy servery.
* **Dostupnost sady**: K poskytnutí zálohy nasazení služby AD FS, doporučujeme seskupení dva nebo více virtuálních počítačích v dostupnost nastavení se pro podobné úloh. Konfigurace zajišťuje, že během buď plánované nebo neplánované údržbě, a alespoň jeden virtuální počítač bude k dispozici
* **Úložiště účty**: je vhodné mít dva účty úložiště. Bez účtu jednoho úložiště může vést k vytvoření z jednoho místa selhání a mohou způsobit nasazení osvobozením od není k dispozici ve scénáři pravděpodobně kde účtu úložiště havaruje. Dva účty úložiště vám pomůže přidružení jednoho účtu úložiště pro každý řádek poruch.
* **Oddělení sítě**: servery proxy serveru webové aplikace by měl být nasazené v síti samostatné DMZ. Můžete rozdělit dva podsítí jedné virtuální sítě a implementovat servery proxy serveru webové aplikace v izolovaném podsítě. Můžete jednoduše konfigurovat nastavení skupiny zabezpečení sítě pro všechny podsítě a povolit pouze požadovaná komunikace mezi dvěma podsítí. Podrobnější informace jsou uvedeny za scénáře dole

##<a name="steps-to-deploy-ad-fs-in-azure"></a>Postup pro nasazení služby AD FS v Azure

Kroky uvedené v této části popisují Průvodce pro nasazení pod vyobrazení infrastruktury služby AD FS v Azure.

### <a name="1-deploying-the-network"></a>1. nasazení síť

Podle pokynů uvedených výše, můžete můžete buď dvou podsítí v jedné sítě virtuální jinak vytvořit dvěma úplně různých virtuálních sítí (VNet). Tento článek se zaměřit na nasazení jedné sítě virtuální a rozdělit na dvě podsítí. Toto je aktuálně přístupu k jednodušší jako dvou samostatných VNets vyžadují VNet VNet bráně pro komunikaci.

**1.1 vytvořit virtuální sítě**

![Vytvořit virtuální sítě](./media/active-directory-aadconnect-azure-adfs/deploynetwork1.png)
    
Na portálu Azure vyberte virtuální sítě a nástroje můžete nasazovat virtuální sítě a jeden podsítě okamžitě s jediným kliknutím. Funkce INT podsítě taky definované a je nyní připravena k VMs které budou přidány.
Dalším krokem je přidat jiné podsítě k síti, tedy podsítě DMZ. Vytvoření podsítě DMZ jednoduše

* Vyberte nově vytvořený síť
* V vlastnosti vyberte podsítě
* V podsítě panelu klikněte na tlačítko Přidat
* Zadejte informace podsítě jména a adresy místo vytvoření podsítě

![Podsítě](./media/active-directory-aadconnect-azure-adfs/deploynetwork2.png)


![Podsítě DMZ](./media/active-directory-aadconnect-azure-adfs/deploynetwork3.png)

**1.2. vytváření sítě skupiny zabezpečení**

Skupiny zabezpečení síti (NSG) obsahuje seznam pravidel seznam řízení přístupu (ACL), která povolit nebo odepřít v síti na OM instance v síti virtuální. NSGs jde přidružit podsítí nebo jednotlivé instance OM v rámci této podsítě. Po přidružené k podsítě NSG ACL pravidla platí pro všechny instance OM v této podsítě.
Pro účely tohoto pokyny budeme dvěma NSGs vytvářet: jedno pro interní síť a DMZ. Budou bude označen NSG_INT a NSG_DMZ v tomto pořadí.

![Vytvoření NSG](./media/active-directory-aadconnect-azure-adfs/creatensg1.png)

Po vytvoření NSG bude 0 příchozí a 0 odchozího pravidla. Jakmile rolí na příslušné servery jsou nainstalované a funkční, můžete pravidla příchozí a odchozí volání podle požadovanou úroveň zabezpečení.

![Inicializace NSG](./media/active-directory-aadconnect-azure-adfs/nsgint1.png)

Po vytvoření NSGs přidružit NSG_INT podsítě INT a NSG_DMZ s podsítí DMZ. Snímek obrazovky příkladu je uveden níže:

![Konfigurace NSG](./media/active-directory-aadconnect-azure-adfs/nsgconfigure1.png)

* Klikněte na podsítí chcete otevřít panel pro podsítě
* Vyberte podsítě přidružit NSG 

Po konfiguraci panelu pro podsítě by měl vypadat níže:

![Podsítí po NSG](./media/active-directory-aadconnect-azure-adfs/nsgconfigure2.png)

**1.3. vytvořit připojení k místní**

Připojení k místní jsme bude potřebujete k nasazení řadiče domény (řadiče domény) v azure. Azure nabízí různé možnosti připojení připojení infrastrukturu místní ke infrastruktury Azure.

* Bod webu
* Virtuální sítě webu webu
* ExpressRoute

Doporučujeme používat ExpressRoute. ExpressRoute umožňuje vytvářet soukromé připojení mezi Azure datacentrech a infrastrukturu, která je ve svém místním prostředí nebo v prostředí spolu umístění. ExpressRoute připojení nepřekročí veřejné Internetu. Nabízejí více spolehlivost rychlejší rychlosti, dolní čekacích dob a vyšší zabezpečení než typický připojení přes Internet.
Když se doporučuje používat ExpressRoute, můžete zvolit způsobem připojení nejvhodnější pro vaši organizaci. Další informace o ExpressRoute a různé možnosti připojení pomocí ExpressRoute najdete v tématu [ExpressRoute technický přehled](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. Vytvořte účty úložiště

Abyste zachovali dostupnost a vyhnout závislost na účtu jednoho úložiště, můžete vytvořit dva účty úložiště. Rozdělit počítačích ve všech sadách dostupnost na dvě skupiny a přiřaďte každé skupiny účet samostatný úložiště.

![Vytvoření účtů úložiště](./media/active-directory-aadconnect-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. dostupnost sady vytvořit

Pro každou roli (Datacentrum/AD FS a WAP) vytvořte dostupnost sady obsahující 2 počítačích každý minimálně. To vám pomůže dosažení vyšší dostupnost pro každou roli. Při vytváření dostupnosti nastaví je nezbytné rozhodnout o těchto věcí:
* **Poruchy domén**: virtuálních počítačích v tu samou doménu poruch sdílení stejného power zdroje a zaměňte fyzické sítě. Doporučuje se aspoň 2 poruch domény. Výchozí hodnota je 3 a můžete ponechat je pro účely tohoto nasazení
* **Aktualizace domén**: společně restartování počítače patřící do stejné domény aktualizace během aktualizace. Chcete-li minimálně 2 aktualizace domén. Výchozí hodnota je 5 a můžete ponechat stejně jako pro účely tohoto nasazení

![Dostupnost sady](./media/active-directory-aadconnect-azure-adfs/availabilityset1.png)

Vytvoření těchto sad dostupnosti

| Nastavení dostupnosti | Role | Poruchy domén | Aktualizace domén |
|:----------------:|:----:|:-----------:|:-----------|
| contosodcset | DATACENTRUM/ADFS | 3 | 5 |
| contosowapset | WAP | 3 | 5 |

### <a name="4--deploy-virtual-machines"></a>4. nasazení virtuálních počítačích
Dalším krokem je nasazení hostujících různé role ve vaší Infrastruktura virtuálních počítačích. Ve všech sadách dostupnost doporučují se aspoň dva počítače. Vytvoření šest virtuálních počítačích základní nasazení.

| Počítač | Role | Podsítě | Nastavení dostupnosti | Úložiště účtu | IP adresa |
|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|contosodc1|DATACENTRUM/ADFS|FUNKCE INT|contosodcset|contososac1|Statická|
|contosodc2|DATACENTRUM/ADFS|FUNKCE INT|contosodcset|contososac2|Statický|
|contosowap1|WAP|DMZ|contosowapset|contososac1|Statická|
|contosowap2|WAP|DMZ|contosowapset|contososac2|Statická|

Jak budete možná jste si všimli, byla specifikována bez NSG. Je to proto azure umožňuje NSG na úrovni podsítě. Pak můžete řídit počítače v síti pomocí jednotlivé NSG přidružené buď podsítě jinak NIC objektu. Přečtěte si víc o [Co je skupina zabezpečení síti (NSG)](https://aka.ms/Azure/NSG).
Pokud spravujete DNS se doporučuje statickou IP adresu. Můžete použít Azure DNS a místo toho v záznamech DNS pro vaši doménu, podívejte se do nového počítače podle jejich Azure certifikátu.
Podokno virtuální počítač by měl vypadat pod po dokončení nasazení:

![Virtuálních počítačích nasazení](./media/active-directory-aadconnect-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. Konfigurace řadiče domény / servery služby AD FS
 Pro ověření všechny příchozí žádosti o službu AD FS bude potřeba řadiče domény. Ukládat nákladné ze služební cesty z Azure na místní Datacentrum pro ověřování, doporučujeme nasazení otevřené řadiče domény v Azure. K dosažení dostupnost, je vhodné pro vytvoření dostupnost na nejmenší 2 domény řadiče.

|Řadiče domény|Role|Úložiště účtu|
|:-----:|:-----:|:-----:|
|contosodc1|Otevřené|contososac1|
|contosodc2|Otevřené|contososac2|

* Zvýšení úrovně dva servery jako otevřené řadiče domény se službou DNS
* Nakonfigurujte servery služby AD FS instalace služby AD FS role správce serveru.

###<a name="6---deploying-internal-load-balancer-ilb"></a>6. nasazení interní řešení pro vyrovnávání zatížení (ILB)

**6.1. ILB vytvořit**

Abyste mohli nasadit ILB, vyberte Vyrovnávání zatížení Azure portálu a klepněte na Přidat (+).
>[AZURE.NOTE] Pokud se nezobrazí **Vyrovnávání zatížení** v nabídce, klikněte na **Procházet** v levém dolním rohu na portál a posuňte se zobrazila **Vyrovnávání zatížení**.  Klepněte na žlutá hvězda přidáte do nabídky. Nyní vyberte nová ikona Vyrovnávání zatížení chcete otevřít panel zahájíte konfigurace služby Vyrovnávání zatížení.

![Procházet Vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/browseloadbalancer.png)

* **Název**: zadejte vhodný název pro vyrovnávání zatížení
* **Schéma**: vzhledem k tomu tento Vyrovnávání zatížení umístí před servery služby AD FS a je určen pro interní síťových připojení jenom, vyberte "Interní"
* **Virtuální sítě**: Zvolte virtuální sítě, který nasazujete službu AD FS
* **Podsítě**: Zvolte vnitřních podsítí
* **Přiřazení IP adresy**: dynamické

![Vyrovnávání zatížení interní](./media/active-directory-aadconnect-azure-adfs/ilbdeployment1.png)
 
Po kliknutí na vytvořit a nasadit ILB, byste měli vidět v seznamu Vyrovnávání zatížení:

![Vyrovnávání zatížení po ILB](./media/active-directory-aadconnect-azure-adfs/ilbdeployment2.png)
 
Dalším krokem je třeba nakonfigurovat fondu back-end a zkušební back-end.

**6.2. fond back-end ILB konfigurace**

Vyberte nově vytvořený ILB v panelu Vyrovnávání zatížení. Otevře se panelu nastavení. 
1.  Vyberte back-end fondů z panelu nastavení
2.  V panelu Přidat back-end fondu klikněte na Přidat virtuálního počítače
3.  Zobrazí s panelem kde si můžete zvolit nastavení dostupnosti
4.  Klikněte na nastavit dostupnost služby AD FS

![Konfigurace fondu ILB back-end](./media/active-directory-aadconnect-azure-adfs/ilbdeployment3.png)
 
**6.3. zkušební konfigurace**

Na panelu nastavení ILB vyberte sond.
1.  Klepněte na Přidat
2.  Poskytování údajů pro zkušební. **Název**: Probe b název. **Protocol (protokol)**: TCP c. **Port**: d 443 (HTTPS). **Interval**: (výchozí hodnota) – to je 5 interval niž ILB probe počítačích ve fondu back-end e. **Mezní hodnota chybná**: 2 (výchozí val ue) – to je mezní zkušební po sobě jdoucí selhání po jejichž uplynutí ILB bude deklarovat počítače z fondu back-end reagovat a zastavit odesílání přenosy.

![Konfigurace ILB zkušební](./media/active-directory-aadconnect-azure-adfs/ilbdeployment4.png)
 
**6.4. Vytvořte pravidla pro vyrovnávání zatížení**

Abyste mohli efektivně rozložit přenos, by měl být ILB nakonfigurován pravidla pro vyrovnávání zatížení. K vytvoření pravidla, Vyrovnávání zatížení 
1.  Vyberte pravidlo z panelu nastavení ILB Vyrovnávání zatížení
2.  Klikněte na Přidat na panel pravidlo pro vyrovnávání zatížení
3.  V panelu pravidlo pro vyrovnávání zatížení přidat. **Název**: Zadejte název pravidla b. **Protocol (protokol)**: Vyberte TCP c. **Port**: 443 d. **Back-end portu**: 443 e. **Fond back-end**: Vyberte fondu jste vytvořili sami pro službu AD FS clusteru starší f. **Zkušební**: Vyberte zkušební dříve vytvořili servery služby AD FS

![Konfigurace služby Vyrovnávání pravidla ILB](./media/active-directory-aadconnect-azure-adfs/ilbdeployment5.png)

**6.5. aktualizovat DNS ILB**

Přejděte na váš DNS server a vytvořit záznam CNAME pro ILB. Záznamy CNAME by měl být pro službu federaci s adresou směřující k IP adrese ILB. Například pokud adresu ILB DIP 10.3.0.8 a služba federace nainstalovaný je fs.contoso.com, vytvořte záznam CNAME pro fs.contoso.com přejdete 10.3.0.8.
Zajistíte tím, že všechny komunikace týkající se fs.contoso.com ukončit nahoru v ILB a jsou směrovány řádně podporovat.

###<a name="7---configuring-the-web-application-proxy-server"></a>7. konfigurace serveru proxy serveru webové aplikace

**7.1. konfigurace webové aplikace Proxy servery kontaktovat servery služby AD FS**

K zajištění moct připojit k serverům služby AD FS za ILB servery proxy serveru webové aplikace vytvořte záznam v %systemroot%\system32\drivers\etc\hosts pro ILB. Všimněte si, že rozlišující název (DN) by měl být federace název služby, třeba fs.contoso.com. A položky IP by měl být ILB IP adresy (10.3.0.8 jako v příkladu).

**7.2. instalace role proxy serveru webové aplikace**

Po zajistíte, že budou moct připojit k serverům služby AD FS za ILB servery proxy serveru webové aplikace, pak nainstalovat servery proxy serveru webové aplikace. Webové aplikace Proxy servery není připojen k doméně. Role proxy serveru webové aplikace nainstalujte na dva servery proxy serveru webové aplikace tak, že vyberete roli vzdáleného přístupu. Správce serveru pomůže vám s dokončete instalaci WAP.
Další informace o tom, jak nasazení WAP přečtěte si [instalace a konfigurace Proxy serveru webové aplikace](https://technet.microsoft.com/library/dn383662.aspx).

###<a name="8---deploying-the-internet-facing-public-load-balancer"></a>8. internetové služby Vyrovnávání zatížení (veřejné) nasazení

**8.1. internetové služby Vyrovnávání zatížení (veřejné) vytvořte**
 
Na portálu Azure vyberte Vyrovnávání zatížení a potom klikněte na Přidat. Na panelu Vyrovnávání zatížení vytvořit zadejte tyto informace
1. **Název**: název pro vyrovnávání zatížení
2. **Schéma**: veřejné – tato možnost umožňuje Azure, že tento Vyrovnávání zatížení potřebovali veřejná adresa.
3. **IP adresy**: vytvoření nové IP adresy (dynamické)

![Internet vystaveného Vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/elbdeployment1.png)

Po zavedení Vyrovnávání zatížení zobrazí v seznamu Vyrovnávání zatížení.

![Seznam Vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/elbdeployment2.png)
 
**8.2. přiřadíte popisek DNS veřejnou IP**

Klikněte na položku vyrovnávání nově vytvořený zatížení v panelu Vyrovnávání zatížení vyvoláte panelu pro konfiguraci. Postupujte podle níže kroky pro nastavení popisek DNS pro veřejnou IP:
1.  Klikněte na veřejnou IP adresu. Otevře se v panelu pro veřejnou IP a jeho nastavení
2.  Klikněte na ke konfiguraci
3.  Zadejte popisek DNS. Tím vytvoříte veřejný popisek DNS, který se dostanete odkudkoliv, například contosofs.westus.cloudapp.azure.com. Přidejte položku v externí DNS pro službu federace (třeba fs.contoso.com), jehož výsledkem DNS popisek Vyrovnávání zatížení externí (contosofs.westus.cloudapp.azure.com).

![Konfigurace Internetové Vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/elbdeployment3.png) 

![Konfigurace Internetové Vyrovnávání zatížení (DNS)](./media/active-directory-aadconnect-azure-adfs/elbdeployment4.png)

**8.3. konfigurace back-end fondu pro Internet protilehlé (veřejné) Vyrovnávání zatížení** 

Postupujte stejně jako vytváření Vyrovnávání zatížení vnitřní, konfigurace fondu back-end pro Internet protilehlé (veřejné) služby Vyrovnávání zatížení jako dostupnost pro WAP servery. Například contosowapset.

![Konfigurace back-end fondu Internet protilehlé Vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/elbdeployment5.png)
 
**8,4. zkušební konfigurace**

Postupujte stejně jako konfigurace služby Vyrovnávání zatížení interní konfigurace zkušební fondu back-end WAP serverů.

![Konfigurace zkušební Internet protilehlé Vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/elbdeployment6.png)
 
**8.5. Vytvořte pravidla pro vyrovnávání zatížení**

Postupujte stejně jako ILB konfigurace vyrovnávání zatížení pravidlo pro TCP 443.

![Konfigurace vyrovnávání pravidla pro Internet protilehlé Vyrovnávání zatížení](./media/active-directory-aadconnect-azure-adfs/elbdeployment7.png)
 
###<a name="9---securing-the-network"></a>9. zabezpečení sítě

**9.1. vnitřní podsítě zabezpečení**

Celkově potřebovat následující pravidla pro efektivní zabezpečení interní podsítě (v pořadí podle těchto pokynů)

|Pravidlo|Popis|Tok|
|:----|:----|:------:|
|AllowHTTPSFromDMZ| Povolit komunikaci HTTPS z DMZ | Příchozí |
|DenyInternetOutbound| Přístup k Internetu | Odchozí |

![Pravidla přístupu INT (příchozí)](./media/active-directory-aadconnect-azure-adfs/nsg_int.png)

[komentář]: <> (![pravidla přístupu INT (příchozí)](./media/active-directory-aadconnect-azure-adfs/nsgintinbound.png)) [komentář]: <> (![pravidla přístupu INT (odchozí)](./media/active-directory-aadconnect-azure-adfs/nsgintoutbound.png))
 
**9.2. podsítě DMZ zabezpečení**

|Pravidlo|Popis|Tok|
|:----|:----|:------:|
|AllowHTTPSFromInternet| Povolte HTTPS z Internetu DMZ | Příchozí|
|DenyInternetOutbound|  Nic kromě blokovaných HTTPS Internetu | Odchozí |

![Pravidla přístupu linka (příchozí)](./media/active-directory-aadconnect-azure-adfs/nsg_dmz.png)

[komentář]: <> (![pravidla přístupu linka (příchozí)](./media/active-directory-aadconnect-azure-adfs/nsgdmzinbound.png)) [komentář]: <> (![pravidla přístupu linka (odchozí)](./media/active-directory-aadconnect-azure-adfs/nsgdmzoutbound.png))

>[AZURE.NOTE] Pokud uživatel klientského počítače certifikát ověřování (klientem TLS ověřování pomocí X509 uživatelské certifikáty) požaduje, pak AD FS vyžaduje TCP port 49443 povolit příchozí přístup pomocí protokolu.

###<a name="10--test-the-ad-fs-sign-in"></a>10. Otestujte přihlašovací službu AD FS

Nejjednodušší způsob je vyzkoušet službu AD FS pomocí stránky IdpInitiatedSignon.aspx. Pokud chcete mít možnost dělat, že není potřeba povolit IdpInitiatedSignOn vlastností AD FS. Postupujte podle pokynů a zkontrolujte nastavení služby AD FS
1.  Spustit pod rutina na server služby AD FS online přes, povolte.
    Nastavení AdfsProperties - EnableIdPInitiatedSignonPage $true 
2.  Z jakékoli externí počítače přístup https://adfs.thecloudadvocate.com/adfs/ls/IdpInitiatedSignon.aspx  
3.  Na stránce služby AD FS byste měli vidět jako níže:

![Test přihlašovací stránka](./media/active-directory-aadconnect-azure-adfs/test1.png)

Na úspěšně přihlásit se vám poskytne zprávu jak je ukázáno v následujícím příkladu:

![Testování úspěch](./media/active-directory-aadconnect-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Šablony pro nasazení služby AD FS v Azure

Šablona nasadí instalace 6 počítače, 2 u řadiče domény, AD FS a WAP.

[Služba AD FS v Azure nasazení šablony](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

Můžete použít existující virtuální sítě nebo vytvoření nového VNET při nasazování tuto šablonu. Různé parametrů dostupných pro přizpůsobení nasazení jsou vypsané dole s popisem použití parametru v procesu nasazení. 

| Parametr | Popis |
|:--------|:-----|
|Umístění| Oblasti, kterou chcete nasadit zdroje na východ například US. |
|StorageAccountType| Typ účtu úložiště|
|VirtualNetworkUsage| Označuje, zda se vytvoří nová virtuální síť, nebo použít existující|
|VirtualNetworkName| Název virtuální sítě vytvořit povinné na existující nebo nové použití virtuální sítě|
|VirtualNetworkResourceGroupName| Určuje název Skupina zdroje, kde jsou uložená existující virtuální sítě. Pokud chcete použít existující virtuální sítě, toto je povinný parametr nasazení našli ID existující virtuální sítě|
|VirtualNetworkAddressRange| Oblast adresu nového VNET povinná při vytváření nové virtuální sítě|
|InternalSubnetName| Název interního podsítě povinné na obou možnostech použití virtuální síť (nový nebo existující)|
|InternalSubnetAddressRange| Rozsah adres interní podsítě obsahuje servery řadiče domény a služby AD FS, povinné při vytváření nové virtuální sítě.|
|DMZSubnetAddressRange| Oblast adresu podsítě dmz, která obsahuje Windows aplikační proxy servery, povinné-li vytvořit novou virtuální síť.|
|DMZSubnetName| Název interního podsítě povinné na obou možnostech použití virtuální sítě (nový nebo existující). |
|ADDC01NICIPAddress| Vnitřní IP adresu prvního řadiče domény, tuto IP adresu bude statického přidělení řadiče domény a musí být platná ip adresu v rámci vnitřní podsítě|
|ADDC02NICIPAddress| Vnitřní IP adresu druhého řadiče domény, tuto IP adresu bude statického přidělení řadiče domény a musí být platná ip adresu v rámci vnitřních podsítí|
|ADFS01NICIPAddress| Vnitřní IP adresu první server služby AD FS, tuto IP adresu bude statického přidělení na server služby AD FS a musí být platná ip adresu v rámci vnitřní podsítě|
|ADFS02NICIPAddress| Vnitřní IP adresu druhý server služby AD FS, tuto IP adresu bude statického přidělení na server služby AD FS a musí být platná ip adresu v rámci vnitřních podsítí|
|WAP01NICIPAddress| Vnitřní IP adresu serveru první WAP tuto IP adresu bude statického přidělení WAP server a musí být platná ip adresu v rámci podsítě DMZ|
|WAP02NICIPAddress| Vnitřní IP adresu druhého serveru WAP tuto IP adresu bude statického přidělení WAP server a musí být platná ip adresu v rámci podsítě DMZ|
|ADFSLoadBalancerPrivateIPAddress| Vnitřní IP adresu Vyrovnávání zatížení služby AD FS, tuto IP adresu bude statického přidělení Vyrovnávání zatížení a musí být platná ip adresu v rámci vnitřní podsítě|
|ADDCVMNamePrefix| Virtuální počítač název předponu u řadiče domény|
|ADFSVMNamePrefix| Virtuální počítač předponu názvu pro servery služby AD FS|
|WAPVMNamePrefix| Virtuální počítač název předponu WAP serverů|
|ADDCVMSize| Velikost OM řadiče domény|
|ADFSVMSize| Velikost OM servery služby AD FS|
|WAPVMSize| Velikost OM WAP serverů|
|AdminUserName| Název místní Správce virtuálních počítačích|
|AdminPassword| Heslo místního účtu Správce virtuálních počítačích|

## <a name="additional-resources"></a>Další zdroje informací
* [Dostupnost sady](https://aka.ms/Azure/Availability ) 
* [Vyrovnávání zatížení Azure](https://aka.ms/Azure/ILB)
* [Vyrovnávání zatížení interní](https://aka.ms/Azure/ILB/Internal)
* [Vyrovnávání zatížení webových Internet](https://aka.ms/Azure/ILB/Internet)
* [Účty úložiště](https://aka.ms/Azure/Storage )
* [Azure virtuálních sítí](https://aka.ms/Azure/VNet)
* [Služba AD FS a Proxy odkazy webové aplikace](http://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Další kroky

* [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
* [Konfigurace a správy vaší AD FS pomocí Azure AD Connect](active-directory-aadconnectfed-whatis.md)
* [Dostupnost křížově zeměpisné AD FS nasazení v Azure pomocí správce dopravy Azure](active-directory-adfs-in-azure-with-azure-traffic-manager.md)





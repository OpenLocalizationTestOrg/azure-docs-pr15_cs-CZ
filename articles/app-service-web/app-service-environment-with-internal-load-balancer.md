<properties
    pageTitle="Vytváření a používání interní Vyrovnávání zatížení s prostředím služby aplikace | Microsoft Azure"
    description="Vytváření a používání ILB pomocného mechanismu řízení"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Vyrovnávání zatížení vnitřní pomocí prostředí aplikace služby #

Funkce aplikace služby Environments(ASE) je možnost služby Premium služby Azure aplikace, která poskytuje rozšířeného nastavení funkce, která není k dispozici ve více klienta razítek.  Funkce pomocného mechanismu řízení v podstatě nasadí aplikace služby Azure na vaše virtuální Network(VNet) Azure.  Pokud chcete získat větší Principy funkce nabízené služby prostředí aplikace přečíst, [Co je prostředí aplikace služby] [ WhatisASE] si přečtěte následující dokumentaci.  Pokud si nejste jisti, výhody provoz v VNet číst [Azure virtuální sítě nejčastější dotazy týkající se][virtualnetwork].  


## <a name="overview"></a>Základní informace ##


Pomocného mechanismu řízení můžete být nasazené s internet přístupných osobám s postižením koncový bod nebo do IP adresa vaší VNet.  Chcete-li nastavit IP adresu na adresu VNet budete muset nasazení vaší pomocného mechanismu řízení s interní Balancer(ILB) načíst.  Pokud vaše pomocného mechanismu řízení nakonfigurována ILB poskytnete:

- vlastní doménu nebo subdomain (subdoména).  Usnadnit práci, tento dokument předpokládá subdoménu ale ho můžete nakonfigurovat oběma způsoby.  
- certifikát používaný pro HTTPS
- Správa služby DNS pro vaše subdoménu.  


Které můžete dělat třeba tyto věci:

- Host (hostitel) intranetové aplikace jako obchodních aplikací bezpečně v cloudu, které máte přístup k webu na webu nebo ExpressRoute VPN
- Host (hostitel) aplikace v cloudu, které nejsou uvedené v veřejné servery DNS
- Vytvářejte aplikace internet samostatný back-end, které aplikace front-end bezpečně integrovat s


#### <a name="disabled-functionality"></a>Vypnutí funkce ####

Zde jsou některé věci, které není při použití pomocného mechanismu řízení ILB.  Zahrnout tyto věci:

- použití IPSSL
- přiřazení IP adres pro konkrétní aplikace
- zakoupení a pomocí aplikace prostřednictvím portálu certifikátu.  Samozřejmě pořád můžete získat certifikátů, které přímo certifikační autorita a ho použít s aplikací nestačí prostřednictvím portálu Azure.


## <a name="creating-an-ilb-ase"></a>Vytvoření pomocného mechanismu řízení ILB ##

Vytvoření pomocného mechanismu řízení ILB není mnohem liší od vytváření pomocného mechanismu řízení běžným způsobem.  Pro důkladnější informace o vytváření pomocného mechanismu řízení přečtěte si, [jak vytvořit prostředí aplikace služby][HowtoCreateASE].  Postupuje při vytváření pomocného mechanismu řízení ILB odpovídá mezi vytvoření VNet při vytváření pomocného mechanismu řízení nebo výběr stávajících VNet.  Vytvoření pomocného mechanismu řízení ILB: 

1.  Na portálu Azure vyberte **nový Web -> + Mobile -> prostředí aplikace služby**
2.  Vyberte předplatné
3.  Vyberte nebo vytvořte novou skupinu zdroje
4.  Vyberte nebo vytvořte VNet
5.  Vytvoření podsítě: Pokud vyberete VNet
6.  Vyberte **virtuální/umístění v síti-> VNet konfigurace** a nastavte typ VIP interní
7.  Zadejte název subdoménu (to bude subdoména používá k aplikacím vytvořené v tomto pomocného mechanismu řízení)
8.  Klikněte na Ok a pak vytvořte


![][1]


V rámci zásuvné virtuální sítě je možnost VNet konfigurace.  Díky tomu, které můžete vybrat mezi externí VIP nebo interní VIP.  Výchozí hodnota je externí.  Pokud je nastavena na externí vaší pomocného mechanismu řízení použije přístupných osobám s postižením VIP internet.  Pokud vyberete interní, vaše pomocného mechanismu řízení nakonfigurována ILB na IP adresu v rámci vašeho VNet.  


Po výběru interní, odebere přidávat další IP adres vaše pomocného mechanismu řízení a místo toho budete muset zadat subdoména pomocného mechanismu řízení.  V pomocného mechanismu řízení s externím VIP název pomocného mechanismu řízení slouží v subdoména aplikací vytvořené v této pomocného mechanismu řízení.  
Pokud vaše pomocného mechanismu řízení označovalo jako ***contosotest*** a aplikace v tomto označovalo jako pomocného mechanismu řízení ***test*** potom subdoména by být formát ***contosotest.p.azurewebsites.net*** a adresa URL této aplikace by být ***mytest.contosotest.p.azurewebsites.net***.  
Pokud nastavíte typ VIP interní, vaše jméno pomocného mechanismu řízení nepoužívá subdoména pro pomocného mechanismu řízení.  Určení subdoména explicitně.  Pokud vaše subdoménu byl ***contoso.corp.net*** a provedené aplikace v tomto pomocného mechanismu řízení s názvem ***timereporting*** adresa URL této aplikace by ***timereporting.contoso.corp.net***.


## <a name="apps-in-an-ilb-ase"></a>Aplikace pomocného mechanismu řízení ILB ##

Vytvoření aplikace pomocného mechanismu řízení ILB je stejná jako obvykle vytvoření aplikace v pomocného mechanismu řízení.  

1. Na portálu Azure vyberte **nový Web -> + Mobile -> Web** nebo **mobilní telefon** nebo **Rozhraní API aplikace**
2. Zadejte název aplikace
2. Vyberte předplatné
3. Vyberte nebo vytvořte pole Skupina zdroje
4. Vyberte nebo vytvořte Plan(ASP) aplikaci služby.  Vytváření nových ASP vyberte svůj pomocného mechanismu řízení jako umístění a pak vyberte fondu pracovního chcete ASP byly vytvořeny v.  Při vytváření ASP vyberete svůj pomocného mechanismu řízení umístění a fondu kolegy.  Pokud zadáte název aplikace uvidíte, že subdoména pod svým jménem app nahrazuje subdoména pro vaše pomocného mechanismu řízení.   
5. Vyberte vytvořit.  Pokud chcete, aby zobrazují na řídicí panel aplikace, by měl zaškrtněte políčko **Připnout do řídicího panelu** .  

![][2]


V části název aplikace názvu subdoménu aktualizuje tak, aby odrážely subdoménu vaší pomocného mechanismu řízení.  


## <a name="post-ilb-ase-creation-validation"></a>Příspěvek pomocného mechanismu řízení ILB vytváření ověření ##

Pomocného mechanismu řízení ILB je mírně odlišnou než bez - ILB pomocného mechanismu řízení.  Co už poznamenat, budete muset spravovat vlastní DNS a máte taky zadat vlastní certifikát pro připojení HTTPS.  


Po vytvoření vaší pomocného mechanismu řízení zjistíte, že je subdoména uvedený subdoména jste zadali a že v nabídce **Nastavení** s názvem **Certifikát ILB**nová položka.  Pomocného mechanismu řízení se vytvoří pomocí certifikátu podepsaného svým držitelem, což usnadňuje testování HTTPS.  Na portálu se dozvíte, které budete muset zadat vlastní certifikát pro HTTPS ale toto je jednotka, abyste měli certifikát, který odpovídá vaší subdoménu.  

![][3]


Pokud jednoduše zkoušíte věcí a nevíte, jak vytvořit certifikát, můžete vytvořit vlastní certifikát podepsaný aplikace konzoly MMC služby IIS.  Po vytvoření otevřela můžete exportovat jako soubor .pfx a pak ho nahrajte v uživatelském rozhraní ILB certifikát. Při přístupu k webu zabezpečená pomocí certifikátu podepsaného svým držitelem prohlížeči vám umožní upozornění, že na web, který přistupujete není zabezpečené kvůli nelze ověřit certifikát.  Chcete-li zabránit že upozornění potřebnosti správně podepsané certifikát, který odpovídá vaší subdoménu a obsahuje řetězec zabezpečení rozpoznaný prohlížeč.

![][6]

Pokud chcete vyzkoušet tok s vlastních certifikátů a otestujte HTTP a HTTPS přístup k vaší pomocného mechanismu řízení:

1.  Přejděte na uživatelské rozhraní pomocného mechanismu řízení po vytvoření pomocného mechanismu řízení **pomocného mechanismu řízení -> Nastavení -> ILB certifikáty**
2.  Nastavte ILB certifikát tak, že vyberete soubor certifikátu pfx a zadejte heslo.  Tento krok zabírá nějakou dobu zpracuje a zobrazí zprávu, která probíhá operace změny měřítka.
3.  Získat adresu ILB pro vaše pomocného mechanismu řízení (**pomocného mechanismu řízení -> Vlastnosti -> virtuální adresy IP**)
4.  Vytvoření webové aplikace v pomocného mechanismu řízení po vytvoření 
5.  Vytvoření virtuálního počítače, pokud nemáte v této VNET (ale ne v stejné podsítě jako koncem pomocného mechanismu řízení nebo nějaké položky)
6.  Nastavte DNS pro vaše subdoménu.  Můžete použít zástupné znaky se vaše subdoménu v DNS nebo pokud chcete udělat pár jednoduchých testů upravte soubor hosts OM nastavíte název webové aplikace na VIP IP adresu.  Pokud vaše pomocného mechanismu řízení měli názvu subdoménu. ilbase.com a udělali mytestapp web appu tak, že by adresovanou na mytestapp.ilbase.com pak nastavení, které v souboru hosts.  (V systému Windows souboru hosts je na C:\Windows\System32\drivers\etc\)
7.  Pomocí prohlížeče na tomto OM a přejděte k http://mytestapp.ilbase.com (nebo něco jiného, co názvu web app s vaší subdoména)
8.  Pomocí prohlížeče na této OM a přejděte na https://mytestapp.ilbase.com budete muset přijmout chybějící cenného papíru, pokud pomocí certifikátu podepsaného svým držitelem.  


IP adresa vaší ILB je uvedeno ve vlastnostech virtuální IP adresa

![][4]


## <a name="using-an-ilb-ase"></a>Použití pomocného mechanismu řízení ILB ##

#### <a name="network-security-groups"></a>Skupiny zabezpečení sítě ####

Pomocného mechanismu řízení ILB umožňuje izolace sítí pro vaše aplikace aplikace nejsou přístupné nebo dokonce známé tak, že na Internetu.  Toto je pracovníků pro hostování webů v síti intranet například obchodních aplikací.  Pokud potřebujete omezit přístup i dál pořád můžete Groups(NSGs) zabezpečení síť pro řízení přístupu na úrovni síť. 


Pokud chcete omezit přístup pomocí NSGs budete muset Ujistěte se, že přerušíte není komunikace pomocného mechanismu řízení potřebují pracovat.  I když je protokolu HTTP/HTTPS přístup pouze prostřednictvím ILB použité pomocným pomocného mechanismu řízení pořád závisí na zdroje mimo VNet.  Jaký přístup k síti je pořád potřeba vyhledejte informace v dokumentu na [Řízení příchozí umožnění datových přenosů do prostředí aplikace služby] [ ControlInbound] a dokument na [Podrobnosti konfigurace sítě pro aplikaci služby prostředí s ExpressRoute][ExpressRoute].  


Pro nastavení vaší NSGs byste měli vědět IP adresy, kterou používá Azure spravovat svůj pomocného mechanismu řízení.  Tuto IP adresu je také odchozích IP adresu z vaší pomocného mechanismu řízení, pokud má požadavky na Internetu.  Vyhledejte tento IP adresu přejděte na **Nastavení -> Vlastnosti** a **Odchozích IP adresu**.  

![][5]


#### <a name="general-ilb-ase-management"></a>Správa obecné ILB pomocného mechanismu řízení ####

Správa pomocného mechanismu řízení ILB je především shodný Správa pomocného mechanismu řízení běžným způsobem.  Potřebujete škálování pracovníka fondů hostovat další instance ASP a škálování front-end serverů případě zvýšení částky přenosy protokolu HTTP/HTTPS.  Pro obecné informace o správě konfigurace pomocný čtení dokumentu o [konfiguraci prostředí aplikace služby][ASEConfig].  


Další správy položky jsou Správa certifikátů a Správa služby DNS.  Potřebujete získat a odeslat certifikát používaný pro HTTPS po vytvoření ILB pomocného mechanismu řízení a nahraďte jej před vypršením jeho platnosti.  Protože Azure vlastní základní domény můžete nabízíme certifikáty ASEs s externím VIP.  Protože subdoména používá pomocným ILB mohou být nic, je třeba zadat vlastní certifikát pro HTTPS. 


#### <a name="dns-configuration"></a>Konfigurace DNS ####

Při použití externí VIP Azure se spravuje DNS.  Aplikaci vytvořili ve vaší pomocného mechanismu řízení automaticky přidají i do DNS Azure, což je veřejné DNS.  V pomocného mechanismu řízení ILB máte spravovat vlastní DNS.  Pro dané subdoménu například contoso.corp.net potřebujete k vytvoření DNS A záznamy, které odkazují na adresu ILB pro:

    * 
    publikování *.SCM ftp 


## <a name="getting-started"></a>Začínáme
Všechny články a jak-pro uživatele pro aplikaci služby prostředí jsou k dispozici v [souboru README pro aplikaci služby prostředí](../app-service/app-service-app-service-environments-readme.md).

Začínáme s prostředím služby aplikace v tématu [Úvod do prostředí aplikace služby][WhatisASE]

Další informace o aplikaci služby Azure platformy, najdete v článku [Aplikace služby Azure][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/

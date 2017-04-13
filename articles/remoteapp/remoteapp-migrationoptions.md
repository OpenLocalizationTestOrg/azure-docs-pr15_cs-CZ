
<properties 
    pageTitle="Možnosti pro migraci z Azure RemoteApp | Microsoft Azure" 
    description="Informace o možnostech pro migraci z Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/06/2016" 
    ms.author="elizapo" />

# <a name="options-for-migrating-out-of-azure-remoteapp"></a>Možnosti pro migraci z Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Přerušili jste pomocí Azure RemoteApp kvůli [vyřazování webů oznámení](https://go.microsoft.com/fwlink/?linkid=821148) nebo vzhledem k tomu budete mít hodnocení, budete muset migrovat z Azure RemoteApp na jinou službu aplikace. Existují dva různé přístupy pro migraci: automatické spravované (někdy označovány jako infrastruktury jako služba [IaaS]) nasazení nebo plně spravovaných (často označovaných platformu jako služba) nebo Software jako služba [PaaS/SaaS] nabízející. 

Samoobslužné IaaS je vlastními nasazení, které je spravováno spravovaný a vlastníte, přímo nasazený na virtuálních počítačích (VMs) nebo pole fyzicky systémy. Na druhou stranu plně spravované PaaS/SaaS nabízející se podobá Azure RemoteApp - partnera poskytuje vrstvě služby nad vzdálená řešení, které zpracovává provozní a údržbu, přitom vy, jako zákazník, proveďte některé obrázek a aplikace správy.

Přečtěte si další informace, včetně příklady různé možnosti hostingu.    

## <a name="self-managed-iaas-solutions"></a>Vlastní správou řešení (IaaS)

### <a name="rds-on-iaas"></a>**Služby Remote Data Service na IaaS** 
Můžete nasadit na nativní na základě relace Vzdálená plocha (v systému Windows Server) nasazení pomocí RemoteApp nebo plochy místní nebo v prostředí hostovanou (like na Azure VMs). Služby Remote Data Service na IaaS nasazení je nejlepší pro zákazníky už znáte a kterým jsou existující odborných s služby Remote Data Service nasazení. 

> [AZURE.NOTE]
> Potřebujete multilicenčního programu s Software Assurance (severní) pro služby Remote Data Service licencí pro klientský přístup tuto možnost nasazení použít.

Nasazení služby Remote Data Service na Azure VMs je jednodušší, když použijete nasazení a oprav šablony (Číst [Přehled](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) a pak [přejděte je získat](https://aka.ms/rdautomation)). Stejné pružná měřítka možnosti s Azure klasického modelu zdroje informací o nasazení (ne Model prostředků Azure prostředky) v rámci Azure RemoteApp můžete získat použitím [Automatické měřítko skript](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76)sice existují další vlastní nastavení a konfiguraci. Při nasazení služby Remote Data Service na Azure VMs podpora je k dispozici až [Podporují Azure](https://azure.microsoft.com/support/plans/), stejné pracovníci technické podpory, které podporuje Azure RemoteApp. Můžete získat nákladů odhady na základě existující využívání kontaktováním [Podpory Azure](https://azure.microsoft.com/support/plans/)nebo můžete provádět výpočty sebe pomocí brzy být uvolnění kalkulačky náklady.  Také pomocí VMs N-řady (aktuálně v soukromé verze preview) můžete přidat vGPU - dozvědět víc o přidávání vGPU a o tom, k [využijte služby Remote Data Service vylepšení v systému Windows Server 2016](https://myignite.microsoft.com/videos/2794) v naší Ignite relace.   

Máme krok za krokem nasazení příručky pro [Windows Server 2012 R2](http://aka.ms/rdsonazure) a [Windows Server 2016](http://aka.ms/rdsonazure2016) pomoci s nasazením. Podívejte se na [blogu Vzdálená plocha](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) nejnovější informace.
 
### <a name="citrix-on-iaas"></a>**Citrix na IaaS** 
Nativní Citrix nasazení bázi relací XenApp nebo XenDesktop může být nasazené místní nebo hostovanou prostředí (třeba na Azure VMs). 

Podívejte se podrobné pokyny Průvodce nasazením, [Citrix XA 7.6 na Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx)Další informace. Další informace o [Citrix na Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), včetně cen kalkulačky. Můžete zjistit taky [kontaktovat Citrix](http://citrix.com/English/contact/index.asp) diskutovat o možnostech s.

## <a name="fully-managed-paassaas-offerings"></a>Plně spravovaných nabídky (PaaS/SaaS)

### <a name="citrix-cloud"></a>**Citrix cloudu** 
[Citrix existujícího cloudové řešení](https://www.citrix.com/products/citrix-cloud/), identickými architektonicky vyjádřit Citrix XenApp. Citrix nabízí [50 % sleva propagace](https://www.citrix.com/blogs/2016/10/03/special-promotion-for-microsoft-azure-remoteapp-customers/) pro stávající zákazníky Azure RemoteApp. 

### <a name="citrix-xenapp-express-in-tech-preview"></a>**Citrix XenApp Express (ve verzi tech preview)**
[Zaregistrujte si jejich ve verzi tech preview](http://now.citrix.com/remoteapp)a sledovat své [Ignite relace](https://myignite.microsoft.com/videos/2792) (počínaje minuty 20:30). XenApp Express je architektonicky identickými Citrix cloudu kromě obsahuje zjednodušená správa uživatelského rozhraní a dalších funkcí a funkcí, které jsou podobné Azure RemoteApp. 

Další informace o [Citrix XenApp Express](http://now.citrix.com/remoteapp).   

### <a name="citrix-service-provider-program"></a>**Program poskytovatele služeb Citrix** 
Program poskytovatele služeb Citrix usnadňuje poskytovatelů při zjednodušení virtuální cloudu výpočetních SMB, jejich nabízení služby, které mají v modelu snadno, systému průběžného financování. Poskytovatelé Citrix růstu firmy jejich Microsoft SPLA a rozbalte položku jejich investice platformy služby Remote Data Service pomocí libovolného zařízení, přístup odkudkoli, nejširší podpora aplikací, bohaté prostředí, zvýšené zabezpečení a lepší škálovatelnost. Zároveň poskytovatele služeb Citrix upoutání pozornosti další účastníky, zvýšit spokojenost zákazníků a snižovat provozní náklady. [Další informace](http://www.citrix.com/products/service-providers.html) nebo [Najít partnera](https://www.citrix.com/buy/partnerlocator.html).

### <a name="microsoft-hosted-service-provider"></a>**Microsoft hostované poskytovatele služeb** 
Hostingu obvykle nabízejí plně spravované hostované počítač s Windows a aplikace služeb, která může obsahovat Správa Azure zdrojů, operačních systémů, aplikací a technickou podporu pomocí partnera společnosti licence smlouvami s Microsoft nebo jiných poskytovatelů software spolu se licenční smlouvy poskytovatele služby umožňující následný z odběratele přístup licenci SAL (). Tyto informace poskytuje podrobnosti a kontaktních informací pro určitý hostitelé, které se specializují na pomoc zákazníci s jejich Azure RemoteApp migrace. Podívejte se na [aktuální seznam poskytovatelů služeb hostovaných](http://aka.ms/rdsonazurecertified) , který jste dokončili služby Remote Data Service na IaaS výukové cesty a vyhodnocování rizik.  

#### <a name="aspex"></a>**ASPEX**
[ASPEX](http://www.aspex.be/en) specializující dodavateli softwaru přechodem do cloudu a nezávislí výrobci softwaru "hledáte, jak optimalizovat své aktuální nastavení cloudu. ASPEX nabízí celou řadu spravovaných služeb, devops a konzultační služby.  

Primární umístění: Antverpy, Belgie

Operace oblast: západní Evropě

Partnera stav: [stříbrný](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)

Poskytovatel služeb cloudu společnosti Microsoft: Ano

Nabízí řešení založená na relace RemoteApp a plocha: Ano, obojí

Azure RemoteApp migrace řešení: Ano, [Další informace](https://www.aspex.be/en/azure-remote-apps)

**Kontakt:**

- Telefon: +3232202198
- Pošta:[info@aspex.be](mailto:info@aspex.be)
- Webové funkce: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)

#### <a name="conexlink-platform-name-mycloudit"></a>**Conexlink (platformy název: MyCloudIT)**
[MyCloudIT](http://www.mycloudit.com) je platformu automatizaci IT organizacím, které zjednodušit optimalizace a měřítko migrace a poskytování vzdálené plochy, vzdálené aplikace a infrastruktury cloudu společnosti Microsoft Azure. 

Platformu MyCloudIT zkracuje dobu zavádění 95 %, Azure nákladů 30 % a přesune jejich klienta celou IT infrastrukturu do cloudu v několika několik stisknutí kláves. Partneři můžete nyní spravovat zákazníci z jednoho globální řídicího panelu, služby koncovým uživatelům ve světě jako nikdy dřív a postupně se zvětšují výnosy bez přidání další režijních nebo rozsáhlé Azure školení.  

Primární umístění: Domažlice, TX, USA

Operace oblast: Celosvětová dostupnost

Partnera stav: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)

Poskytovatel služeb cloudu společnosti Microsoft: Ano

Nabízí řešení založená na relace RemoteApp a plocha: Ano, obojí

Azure RemoteApp migrace řešení: Ano, [Další informace](https://mycloudit.com/remote-app-microsoft/)

**Kontakt:**
- Brian Garoutte náměstek vývoje Business

   Telefon: 972-218-0741

   E-mail:[brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)

#### <a name="acuutech"></a>**Acuutech**
[Acuutech](http://www.acuutech.com) specializující se poskytují hostovanou plochy řešení předvádění celé plochy a aplikací nezávislých výrobců softwaru prostředí založený na technologii globální klientovi základní z Azure a vlastní datacentrech.

Primární umístění: Londýn, Spojené království; Singapur; Houston, TX

Operace oblast: Celosvětová dostupnost

Partnera stav: Gold

Poskytovatel služeb cloudu společnosti Microsoft: Ano

Nabízí řešení založená na relace RemoteApp a plocha: Ano, obojí

Azure RemoteApp migrace řešení: Ano, [Další informace](http://www.acuutech.com/ara-migration/)

**Kontakt:**

- Spojené království:

  5/6 York Chaloupku Langston cest

  Loughton Essex IG10 3TQ
  
  Telefonní číslo: +44 (0) 20 8502 2155
 
- Singapur:

  100 Cecil ulice #09-02 
  
  Glóbus pohybuje Singapur 069532
  
  Telefonní číslo: + 65 6709 4933
 
- Severní Amerika: 

  3601 S. Sandman St.
  
  Sadu 200 Houston, TX 77098
  
  Telefon: +1 713 691 0800

## <a name="need-more-help"></a>Potřebujete další pomoc?
Dál potřebujete pomoc, zvolíte nebo máte další otázky? Použijte jeden z těchto způsobů nápovědu. 

1.  E-mailové adrese [arainfo@microsoft.com](mailto:arainfo@microsoft.com).
2.  Obraťte se na [podporu Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Začněte otevřením [Azure podporují písmena](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
3.  Zavolejte. [Najít místní číslo prodeje](https://azure.microsoft.com/overview/sales-number/).

<properties
   pageTitle="Správa identity v Azure | Microsoft Azure"
   description="Tento článek vysvětluje a srovnání různých metod k dispozici pro správu identity v systémech hybridní přesahující na místní/cloudu hranice s Azure."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/26/2016"
   ms.author="telmosampaio"/>
   
# <a name="managing-identity-in-azure"></a>Správa Identity v Azure

Ve většině systémů enterprise založený na systému Windows použijete Active Directory (AD) poskytovat identity služby Správa aplikací. AD funguje taky v místním prostředí, ale když rozšíření vaši síťovou infrastrukturu do cloudu máte některé důležité rozhodnutí o poskytnutí o tom, jak spravovat identity. Můžete rozbalit domén v místním zapracovat VMs v cloudu? Vytvořit nové domény v cloudu a pokud ano, jak? Měli implementovat vlastní doménové v cloudu nebo by měl být používat z Azure Active Directory (AAD)?

Tento článek popisuje některé běžné možnosti pro schůzky výzvy, které představují tak, že tento scénář a pomáhají určit bude vhodné řešení vlastním potřebám svým požadavkům.

## <a name="using-azure-active-directory"></a>Pomocí služby Azure Active Directory

AAD můžete použít k vytvoření doméně služby AD v cloudu a odkaz na místního AD domény. AAD umožňují konfigurovat jednotné přihlašování (SSO) pro uživatele používající aplikací prostřednictvím cloudu.

[! [0]][0]

AAD je jednoduchý způsob, jak implementovat domény zabezpečení v cloudu. Je využívána mnoho aplikací Microsoft, jako je Microsoft Office 365. 

Výhody používání AAD:

- Není nutné zachovat infrastruktury služby Active Directory v cloudu. AAD je úplně spravovaných a spravovaný společností Microsoft.

- AAD poskytuje stejné identity informace, které je k dispozici místně.

- Ověřování může dojít v Azure, a snížit množství externích aplikacích a uživatelé kontaktovat místní domény.

Ukazatel na zvážit při používání AAD:

- Služby identit se omezí na uživatelé a skupiny. Vy žádným způsobem ověření služby a účty počítačů.

- Připojení musíte nakonfigurovat s vlastní doménou místní zachovat adresáři AAD synchronizovat. 

- Zodpovídáte za publikování aplikací, které mohou uživatelé v cloudu pomocí AAD.

Podrobné informace najdete v článku [Provádění Azure Active Directory][implementing-aad].

## <a name="using-active-directory-in-the-cloud-joined-to-an-on-premises-forest"></a>Pomocí služby Active Directory v cloudu připojen k místním struktury

Můžete hostovat AD DS Directory Services (AD) místní, ale ve scénáři hybridní umístění prvky aplikace v Azure může být efektivnější replikovat prvek pro tuhle funkci a úložišti AD do cloudu. Tento přístup snížit zpoždění způsobená odesílání ověřování a požadavky na místní ověření z cloudu zpět do služby AD DS místní systém. 

[! [1]][1]

Tento postup vyžaduje vytvořit vlastní domény v cloudu a připojit ji do struktuře místní. Vytvoření VMs hostovat služby AD DS.

Výhody používání samostatnou doménu v cloudu:

- Poskytuje možnost ověřování uživatele, přihlašovacích údajů a počítač účty místních i cloudových.

- Poskytuje přístup k stejné identity informace, které je k dispozici místně.

- Není nutné spravovat strukturu samostatné AD; doména v cloudu můžete patří struktuře místní.

- Můžete použít zásad skupiny definované místní objekt zásad skupiny objektů na doménu v cloudu.

Důležité informace týkající se používání samostatnou doménu v cloudu:

- Vyžaduje vytvářet a spravovat vlastní servery služby AD DS a domény v cloudu.

- Doména může obsahovat některé latence synchronizace mezi servery domény v cloudu a místní servery.

Informace o tom, jak konfigurovat tato architektura, najdete v článku [Rozšíření Active Directory adresáře služeb (přidá) na Azure][extending-adds].

## <a name="using-active-directory-with-a-separate-forest"></a>Pomocí služby Active Directory s strukturu samostatný

Organizace, která běží služby Active Directory (AD) místní pravděpodobně strukturu zahrnující mnoho různých domén. Domény můžete použít k poskytnutí izolace mezi funkčních oblastí, které jsou průběžně samostatné, případně z bezpečnostních důvodů, ale můžete sdílet informace mezi doménami vytvořením vztahy důvěryhodnosti.

[! [2]][2]

Organizace, která používá oddělené domény můžete využívat Azure přemístíte jednu nebo víc z těchto domén do samostatných struktury v cloudu. Organizace může můžete taky chtít zachovat všechny zdroje cloudu logicky odlišné od těch uloženými místní a obsahují informace o zdrojích cloudu v svůj adresář jako součást strukturu taky uskutečňuje v cloudu.

Výhody používání strukturu samostatné v cloudu:

- Provádění místních identit a oddělte jen Azure identity.

- Je třeba replikovat z místní doménové AD na Azure snížení efekty latence sítě.

Důležité informace:

- Ověřování místních identit v cloudu provádí navíc sítě *směrování* do místní servery AD.

- Vyžaduje implementovat vlastní servery služby AD DS a struktury v cloudu a vytvořit relace příslušný vztah důvěryhodnosti mezi strukturami.

Dokument [vytváření struktuře Active Directory adresáře služby (přidá) prostředků v Azure] [ adds-forest-in-azure] popisuje, jak implementovat tento přístup podrobněji.

## <a name="using-active-directory-federation-services-adfs-with-azure"></a>Použití služby Active Directory Federation Services (služby AD FS) s Azure

Služby AD FS mohlo by umožnit spuštění místní, ale ve scénáři hybridní umístění aplikací v Azure může být efektivnější k provedení této funkce v cloudu, jak je ukázáno v následujícím příkladu.

[! [3]][3]

Tato architektura je užitečné především pro:

- Řešení, která využívají federované povolení vystavit webových aplikací do organizace partnera.

- Systémy, které podporují přístup z webové prohlížeče systém mimo bránou firewall.

- Systémy, které umožňují uživatelům přístup k webovým aplikacím připojením z oprávněnými externí zařízení například vzdálených počítačů, poznámkových bloků a další mobilní zařízení. 

Výhody používání služby AD FS s Azure:

- Můžete využít deklarací aplikace.

- Poskytuje možnost důvěřovat externími partnery pro ověřování.

- Poskytuje funkce pro kompatibilitu s velké skupiny ověřovacích protokolů.

Co zvážit při Azure pomocí služby AD FS:

- Vyžaduje implementovat vlastní servery přidá služby AD FS a proxy server služby AD FS webové aplikace v cloudu.

- Tento architektury může být složité ke konfiguraci.

Podrobné informace najdete v článku [Provádění Active Directory Federation služby služby AD FS () v Azure][adfs-in-azure].

## <a name="next-steps"></a>Další kroky

Níže uvedených zdrojích popisují, jak implementovat architektury popisované v tomto článku.

- [Provádění Azure Active Directory][implementing-aad]
- [Rozšíření adresářové služby Active Directory (přidá) na Azure][extending-adds]
- [Vytvoření struktury zdrojů Active Directory adresáře služeb (přidá) v Azure][adds-forest-in-azure]
- [Implementace v Azure Active Directory Federation Services (služby AD FS)][adfs-in-azure]

<!-- Links -->
[0]: ./media/guidance-identity/figure1.png "Architektura identity cloudu pomocí služby Azure Active Directory"
[1]: ./media/guidance-identity/figure2.png "Zabezpečené hybridní architektura sítě se službou Active Directory"
[2]: ./media/guidance-identity/figure3.png "Zabezpečené hybridní architektura sítě s samostatné AD domény a struktury"
[3]: ./media/guidance-identity/figure4.png "Zabezpečené hybridní síťové architektury pomocí služby AD FS"
[implementing-aad]: ./guidance-identity-aad.md
[extending-adds]: ./guidance-identity-adds-extend-domain.md
[adds-forest-in-azure]: ./guidance-identity-adds-resource-forest.md
[adfs-in-azure]: ./guidance-identity-adfs.md
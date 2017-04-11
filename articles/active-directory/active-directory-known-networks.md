<properties 
    pageTitle="Známé sítí | Microsoft Azure" 
    description="Nakonfigurováním známé sítě se můžete vyhnout IP adresy, které jsou vlastnictví vaší organizací součástí Sign in z různých zeměpisných a Sign in z IP adresy se sestavami podezřelé aktivity." 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"  
    editor=""/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markvi"/>

# <a name="known-networks"></a>Známé sítí


Pokud chcete získat přehled o integrita a zabezpečení adresáře vaší organizace můžete přístup Azure Active Directory a sestavy využití. Tyto informace jako správce adresáře můžete lépe zjistit, kde může být možné zabezpečení rizikové tak, aby dostatečně můžou plánovat pomoci tato rizika zmírnit.

Je možné, že zprávy "*Sign in z různých zeměpisných*" a "*Sign in z IP adres pomocí podezřelé aktivity*" nesprávně příznak IP adresy, které jsou ve skutečnosti vlastnictví vaší organizací. 

K tomu můžete, například dojít, pokud: 

- Uživatel ve vaší Boston office má vzdáleně k přihlášení datovém centru v San Francisco spustí zprávu "Podepsat doplňky, které jsou z různých zeměpisných" 

- Uživatel vaší organizace, pokusí přihlašování pomocí aktivačních událostí nesprávným heslem sestava "Podepsáno doplňky, které jsou z adresy IP pomocí podezřelé aktivity" 

Zabránit takovýchto případech generování sestav zavádějící zabezpečení, bychom přidat známé rozsahy IP adres do seznamu veřejnou IP adresu vaší organizace.    


###<a name="to-add-your-organizations-public-ip-address-ranges-perform-the-following-steps"></a>Chcete-li přidat organizace veřejný rozsahy IP adres, proveďte následující kroky: 

1.  Přihlášení k [portálu Správa Azure](https://manage.windowsazure.com).

2.  V levém podokně klikněte na **Služby Active Directory**. <br><br>![Jak funguje aplikace zjišťování cloudu](./media/active-directory-known-networks/known-netwoks-01.png)

3.  Na kartě **adresáře** vyberte adresář.

4.  V nabídce na horní klikněte na **Konfigurovat**. <br><br>![Jak funguje cloudu aplikace zjišťování](./media/active-directory-known-networks/known-netwoks-02.png)

5.  Na kartě Konfigurovat přejděte na **vaše organizace veřejný rozsahy IP adres** <br><br>![Jak funguje cloudu aplikace zjišťování](./media/active-directory-known-networks/known-netwoks-03.png)

6.  Klikněte na **Přidat rozsahy známé IP adres**.

7.  Přidání adresy oblastí v dialogovém okně, které se zobrazí a po dokončení klikněte na tlačítko Zkontrolovat. <br><br>![Jak funguje aplikace zjišťování cloudu](./media/active-directory-known-networks/known-netwoks-04.png)


**Další zdroje informací**


* [Zobrazení sestav aplikace access a použití](active-directory-view-access-usage-reports.md)
* [Sign in z IP adres pomocí podezřelé aktivity](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [Sign in z různých zeměpisných](active-directory-reporting-sign-ins-from-multiple-geographies.md)



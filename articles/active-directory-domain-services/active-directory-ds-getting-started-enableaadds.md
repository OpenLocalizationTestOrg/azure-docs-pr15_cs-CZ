<properties
    pageTitle="Azure AD Domain Services: Povolení služby Azure AD domény | Microsoft Azure"
    description="Začínáme s Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="enable-azure-ad-domain-services"></a>Povolení služby Azure AD domény

## <a name="task-3-enable-azure-ad-domain-services"></a>Krok 3: Povolení služby Azure AD domény
V tomto úkolu povolení služby Azure AD Domain pro adresáře. Proveďte následující kroky konfigurace povolení služby Azure AD Domain pro adresáře.

1. Přejděte na **portál Azure klasické** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Vyberte uzel **Služby Active Directory** v levém podokně.

3. Vyberte Azure AD klienta (adresář), pro kterou chcete povolit Azure AD Domain Services.

    ![Vyberte Azure AD adresář](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Klikněte na kartu **Konfigurovat** .

    ![Konfigurace na kartě adresáře](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Přejděte dolů k oddílu s názvem **domény služby**.

    ![Konfigurace služby domény](./media/active-directory-domain-services-getting-started/domain-services-configuration.png)

6. Zapíná a vypíná možnost s názvem **Povolit domain services pro tento adresář** na hodnotu **Ano**. Všimněte si pár další možnosti konfigurace služby Azure AD domény se zobrazí na stránce.

    ![Povolení Domain Services](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

    > [AZURE.NOTE] Po povolení služby Azure AD domén pro vašeho klienta Azure AD vygeneruje a ukládá Kerberos a NTLM hash přihlašovacích údajů, které jsou zapotřebí pro ověřování uživatelů.

7. Zadejte **název domény DNS domain Services**.

   - Výchozí název domény v adresáři (to znamená, který končí **. onmicrosoft.com** přípona domény) vybrané ve výchozím nastavení.

   - Seznam obsahuje všechny domény, které byly nakonfigurovali Azure AD adresáře – včetně ověření stejně jako neověřené domény, které můžete konfigurovat na kartě "Domény".

   - Kromě toho můžete také zadat vlastní název domény. V tomto příkladu jsme zadali do pole název vlastní domény "contoso100.com".

     > [AZURE.WARNING] Zajistěte, aby byl předponu domény zadaného (například "contoso100' v"contoso100.com"název domény) název domény méně než 15 znaků. Domain Azure AD Domain Services nelze vytvořit předponou domény delší než 15 znaků.

8. Ujistěte se, že název domény DNS, který jste vybrali u spravovaných domény ještě neexistuje v virtuální sítě. Konkrétně zaškrtněte, pokud:

   - už máte doménu se stejným názvem domény DNS virtuální síti se systémem.

   - virtuální sítě, kterou jste vybrali má připojení VPN k síti místní a máte doménu se stejným názvem domény DNS ve vaší místní síti.

   - máte existující cloudové služby s tímto názvem virtuální síti se systémem.

9. Dalším krokem je a vyberte virtuální síť, ve kterém se mají služby Azure AD domény k dispozici. Vyberte virtuální sítě a vyhrazené podsítě, kterou jste vytvořili v rozevíracím seznamu s názvem **služby domény připojit k této virtuální síti**.

   - Ujistěte se, že virtuální sítě, který jste definovali patří Azure oblast nepodporuje Azure AD Domain Services. Odkaz na stránku [služby Azure podle regionů](https://azure.microsoft.com/regions/#services/) znát Azure oblasti, ve kterých je k dispozici Azure AD Domain Services.

   - Virtuální sítě patřící do oblasti, kde není podporován Azure AD Domain Services není objeví v rozevíracím seznamu.
   
   - Použijte vyhrazené podsítě v rámci virtuální sítě pro službu Azure AD Domain Services. Zkontrolujte, zda že jste nevybrali podsítě brány. V tématu [sítě aspektech](active-directory-ds-networking.md). 

   - Virtuální sítě, které byly vytvořené pomocí Správce prostředků Azure podobně nezobrazují v rozevíracím seznamu. Správce prostředků virtuální sítích aktuálně nepodporuje Azure AD Domain Services.

10. Azure AD Domain Services, klepněte na možnost **Uložit** z podokna úloh v dolní části stránky.

11. Na stránce se zobrazí "na zjištění stavu úkolů..." Stav, při je povolování služby Domain Azure AD pro adresáře.

    ![Povolení Domain Services - nevyřízená](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

    > [AZURE.NOTE] Azure AD Domain Services zvyšuje dostupnost pro vaši doménu spravovaný. Po povolení služby Azure AD domény Všimněte si IP adresy, ve kterých jsou k dispozici v virtuální sítě zobrazují postupně Domain Services. Druhá IP adresa se zobrazí krátce, jako brzy bude k dispozici tato služba umožňuje dostupnost pro vaši doménu. Pokud dostupnost jsou nakonfigurované a aktivní pro vaši doménu, byste měli vidět dvě IP adresy v části **domény služby** karty **Konfigurovat** .

12. Po asi 20 až 30 minut zobrazit první IP adresu, pro niž je k dispozici v síti virtuální do pole **IP adresa** na stránce **Configure** Domain Services.

    ![Zřízení domény povolenými službami – první IP](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)

13. Je-li dostupnost provozní pro vaši doménu, se zobrazí dvě IP adresy zobrazené na stránce. Spravované domény je k dispozici v síti vybrané virtuální v těchto dvou IP adres. Poznamenejte si IP adresy, můžete aktualizovat nastavení DNS virtuální sítě. Tento krok umožňuje virtuálních počítačích na virtuální sítě se připojit k doméně operací, jako je připojení k doméně.

    ![Povolené domény služby – zřízení obou IP adresy](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [AZURE.NOTE] V závislosti na velikosti vašeho klienta Azure AD (počet uživatelů, skupin atd), chvíli trvá synchronizace na vaši doménu spravovaný. Tento proces synchronizace probíhá na pozadí. Pro velké klienti s desítky tisíce objekty může trvat den nebo dvě pro všechny uživatele, členství ve skupinách a přihlašovací údaje k synchronizaci.

<br>

## <a name="task-4---update-dns-settings-for-the-azure-virtual-network"></a>Úkol 4 – aktualizace nastavení DNS u Azure virtuální sítě
Je další úkol konfigurace aktualizovat [Nastavení DNS pro Azure virtuální sítě](active-directory-ds-getting-started-dns.md).

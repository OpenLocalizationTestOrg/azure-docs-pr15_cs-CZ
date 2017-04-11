<properties
    pageTitle="Služby Domain Azure AD: Nastavení aktualizace DNS pro Azure virtuální sítě | Microsoft Azure"
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
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---update-dns-settings-for-the-azure-virtual-network"></a>Služby Domain Azure AD - nastavení aktualizace DNS pro Azure virtuální sítě

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Krok 4: Aktualizace nastavení DNS u Azure virtuální sítě
V předchozích konfiguraci, úspěšně povolili jste služby Azure AD Domain pro adresáře. Je další úkol zajistit, že počítače v rámci virtuální sítě se můžete připojit a používat tyto služby. Aktualizujte nastavení serveru DNS pro vaši virtuální síť přejděte na dva IP adresy, na které je k dispozici virtuální síti se systémem Azure AD Domain Services.

> [AZURE.NOTE] Poznámka: dolů IP adresy služby Azure AD Domain zobrazené na kartě **Konfigurovat** adresáře, po povolení služby Azure AD Domain pro adresář.

Proveďte následující kroky konfigurace aktualizovat nastavení serveru DNS pro virtuální síť, ve kterém jste povolili Azure AD Domain Services.

1. Přejděte na **portál Azure klasické** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. Vyberte uzel **sítí** v levém podokně.

    ![Virtuální sítě uzel](./media/active-directory-domain-services-getting-started/virtual-network-select.png)

3. Na kartě **Virtuálních sítí** vyberte virtuální sítě, ve kterém jste povolili služby Azure AD doménu zobrazíte její vlastnosti.

4. Klikněte na kartu **Konfigurovat** .

    ![Virtuální sítě uzel](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)

5. V části **servery DNS** zadejte IP adresy služby Azure AD domény.

6. Ujistěte se, které jste zadali obou IP adresy, které se zobrazí v části **Domain Services** na kartě **Konfigurovat** adresáře.

7. Pokud chcete uložit nastavení serveru DNS pro tento virtuální sítě, klepněte na tlačítko **Uložit** v podokně úloh v dolní části stránky.

   ![Aktualizujte nastavení serveru DNS pro virtuální sítě.](./media/active-directory-domain-services-getting-started/update-dns.png)

> [AZURE.NOTE] Po aktualizaci nastavení serveru DNS pro virtuální sítě, může trvat nějakou dobu virtuálních počítačích v síti získání aktualizované konfigurace DNS. Pokud virtuálního počítače se nemůže připojit k doméně, kterou vyprázdněte mezipaměť DNS (např. "ipconfig/flushdns") v počítači virtuální. Tento příkaz vynutí aktualizace nastavení DNS na virtuální počítač.


## <a name="task-5---enable-password-synchronization-to-azure-ad-domain-services"></a>Úkol 5 – synchronizace hesel povolit Azure AD Domain Services
Je další úkol konfigurace k [Povolení synchronizace hesel Azure AD Domain Services](active-directory-ds-getting-started-password-sync.md).

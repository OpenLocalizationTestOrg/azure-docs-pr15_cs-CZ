<properties
    pageTitle="Povolení Microsoft Windows Ahoj pro firmy ve vaší organizaci | Microsoft Azure"
    description="Nasazení pokyny, jak povolit Microsoft Passport ve vaší organizaci."
    services="active-directory"
    documentationCenter=""
    keywords="Konfigurace Microsoft Passport, Microsoft Windows Ahoj firmy nasazení"
    authors="markusvi"
    manager="femila"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="markvi"/>


# <a name="enable-microsoft-windows-hello-for-business-in-your-organization"></a>Povolení Microsoft Windows Ahoj pro firmy ve vaší organizaci

Po [připojení doméně zařízení Windows 10 s Azure Active Directory](active-directory-azureadjoin-devices-group-policy.md)postupujte následujícím postupem povolíte Microsoft Windows Ahoj pro firmy ve vaší organizaci:

1. Nasazení Správce konfigurace pro System Center  

2. Konfigurace nastavení zásad

3. Konfigurace certifikátu profilu  




## <a name="deploy-system-center-configuration-manager"></a>Nasazení Správce konfigurace pro System Center 

Abyste mohli nasadit uživatele certifikáty podle Ahoj systému Windows pro firmy klíče, musíte:

- **Aktuální větev Správce konfigurace systému Centrum** – bude potřeba nainstalovat verzi 1606 nebo vyšší. Další informace najdete v tématu [si přečtěte následující dokumentaci pro Centrum Správce konfigurace pro System](https://technet.microsoft.com/library/mt346023.aspx) a [Blog týmu Centrum Správce konfigurace systému](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).
- **Veřejné klíčové infrastruktury** - povolení Microsoft Windows Ahoj pro firmy pomocí uživatelské certifikáty infrastruktury musí mít na místě. Pokud nemáte a nechcete, aby se používá pro uživatelské certifikáty nástroje můžete nasazovat nového řadiče domény, který má Windows serveru 2016 sestavení 10551 (nebo lepší) nainstalovaný. [Instalace řadiče domény otevřené v stávající domény](https://technet.microsoft.com/library/jj574134.aspx) a [instalovat nové služby Active Directory strukturu, pokud vytváříte nové prostředí](https://technet.microsoft.com/library/jj574166), postupujte podle pokynů. (ISOs jsou k dispozici ke stažení na [Exchange médií Signiant](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)


## <a name="configure-policy-settings"></a>Konfigurace nastavení zásad

Konfigurace Microsoft Windows Ahoj pro firmy nastavení zásad, máte dvě možnosti:

- Zásady skupiny ve službě Active Directory 
- Správce konfigurace pro System Center 


Pomocí zásad skupiny ve službě Active Directory doporučuje se pro nastavení Microsoft Windows Ahoj pro nastavení zásad Business. 



Pomocí Správce konfigurace pro System Center při upřednostňovaná metoda také ji použijete k nasazení certifikátů. Tento scénář:

- Zajišťuje kompatibility s novější scénáře nasazení
- Vyžaduje na straně klienta 1607 verze Windows 10 nebo novější.

### <a name="configure-microsoft-windows-hello-for-business-via-group-policy-in-active-directory"></a>Konfigurace aplikace Microsoft Windows Ahoj pro firmy prostřednictvím zásad skupiny ve službě Active Directory

 
Pomocí následujících **kroků**:

1.  Otevřete správce serveru a přejděte na **Nástroje** > **Správa zásad skupiny**.
2.  Správa zásad skupiny přejděte na uzel domény, který odpovídá domény, ve kterém chcete povolit připojení Azure AD.
3.  Klikněte pravým tlačítkem myši **Objekty zásad skupiny**a vyberte **Nový**. Pojmenujte objekt zásad skupiny, například povolit Windows Ahoj pro firmy. Klikněte na **OK**.
4.  Klikněte pravým tlačítkem na nový objekt zásad skupiny a pak vyberte **Upravit**.
5.  Přejděte na **počítač konfigurace** > **zásady** > **šablony pro správu** > **součásti systému Windows** > **Ahoj systému Windows pro firmy**.
6.  Klikněte pravým tlačítkem myši **Povolit Windows Ahoj pro firmy**a pak vyberte **Upravit**.
7.  Na přepínač **Povolit** a potom klikněte na **použít**. Klikněte na **OK**.
8.  Teď můžete propojit objekt zásad skupiny do umístění podle svého výběru. Chcete-li této zásady pro všechny doméně zařízeních s Windows 10 ve vaší organizaci, odkaz na doménu zásad skupiny. Příklad:
 - Konkrétní organizační jednotce (OU) ve službě Active Directory, které budou umístěné doméně počítačích Windows 10
 - Konkrétní skupiny zabezpečení, který obsahuje Windows 10 doméně počítačů, které se automaticky registrovaný u Azure AD


### <a name="configure-windows-hello-for-business-using-system-center-configuration-manager"></a>Konfigurace systému Windows Ahoj pro firmy pomocí Správce konfigurace pro System Center


Pomocí následujících **kroků**:


1. Otevřete **Správce konfigurace pro System Center**a pak přejděte na **prostředky a dodržování předpisů > Nastavení dodržování předpisů > přístupu k prostředkům společnosti > Windows Ahoj profilů firmy**.

    ![Konfigurace systému Windows Ahoj pro firmy](./media/active-directory-azureadjoin-passport-deployment/01.png)


2. Na panelu nástrojů na horní klikněte na **Vytvořit Windows Ahoj pro firmy profilu**.

    ![Konfigurace systému Windows Ahoj pro firmy](./media/active-directory-azureadjoin-passport-deployment/02.png)

2. V dialogovém okně **Obecné** proveďte následující kroky:

    ![Konfigurace systému Windows Ahoj pro firmy](./media/active-directory-azureadjoin-passport-deployment/03.png)

    na. Do textového pole **název** zadejte název profilu, třeba **Můj profil WHfB**.

    b. Klikněte na tlačítko **Další**.


2. V dialogovém okně **Podporované platformy** vyberte platformy, která bude zřízení s Tento Windows Ahoj firmy profilu a klikněte na tlačítko **Další**.

    ![Konfigurace systému Windows Ahoj pro firmy](./media/active-directory-azureadjoin-passport-deployment/04.png)


2. V dialogovém okně **Nastavení** proveďte následující kroky:

    ![Konfigurace systému Windows Ahoj pro firmy](./media/active-directory-azureadjoin-passport-deployment/05.png)

    na. **Konfigurace systému Windows Ahoj pro firmy**vyberte **povoleno**.

    b. **Použít důvěryhodné Platform Module (TPM)**vyberte **povinné**. 

    c. Jako **metody ověřování**vyberte **založené na certifikát**.

    d. Klikněte na tlačítko **Další**.



2. V dialogovém okně **Souhrn** klikněte na **Další**.

2. V dialogovém okně **dokončení** klepněte na tlačítko **Zavřít**.


2. Na panelu nástrojů na horní klikněte na **nasazení**.

    ![Konfigurace systému Windows Ahoj pro firmy](./media/active-directory-azureadjoin-passport-deployment/06.png)



## <a name="configure-the-certificate-profile"></a>Konfigurace certifikátu profilu 

Pokud používáte ověřování na základě certifikát pro místní ověření, potřebujete ke konfiguraci a nasazení profilu certifikátu. Tohoto úkolu vyžaduje nastavte NDES server a certifikát registrace čárky webu role ve Správci konfigurace centrum systému. Další informace najdete v [požadavcích na certifikát profily ve Správci konfigurace](https://technet.microsoft.com/library/dn261205.aspx).

1. Otevřete **Správce konfigurace pro System Center**a pak přejděte na **prostředky a dodržování předpisů > Nastavení dodržování předpisů > přístupu k prostředkům společnosti > profily certifikát**.


2. Vyberte šablonu, která obsahuje čipové karty přihlašovací použití rozšířeného klíče (EKU).

Na stránce **SCEP zápisu** profil certifikátů budete muset zvolte **nainstalovat a Passport souvislosti s prací, jinak nepovede** jako **Poskytovatele úložiště klíče**.



## <a name="next-steps"></a>Další kroky
* [Windows 10 Enterprise: způsoby použití zařízení pro práci](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozšíření možností cloudu na zařízeních s Windows 10 až Azure Active Directory připojit se ke](active-directory-azureadjoin-user-upgrade.md)
* [Ověření identity bez hesla prostřednictvím Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Další informace o použití scénáře pro Azure AD připojení](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Připojte zařízení doméně Azure AD pro Windows 10 prostředí](active-directory-azureadjoin-devices-group-policy.md)
* [Nastavení připojení Azure AD](active-directory-azureadjoin-setup.md)

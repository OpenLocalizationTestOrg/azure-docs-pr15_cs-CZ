<properties
    pageTitle="Vytvoření kolekce hybridní pro Azure RemoteApp | Microsoft Azure"
    description="Naučte se vytvářet nasazení RemoteApp, který se připojuje k vaší síti."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo"/>

# <a name="how-to-create-a-hybrid-collection-for-azure-remoteapp"></a>Vytvoření kolekce hybridní pro Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Existují dva typy kolekcí Azure RemoteApp:

- Cloudu: nachází zcela Azure. Je možné uložit všechna data v cloudu (tak, aby jen cloudu kolekci) nebo k připojení kolekci VNET a uložení dat tam.   
- Hybridní: obsahuje virtuální sítě pro místní přístup - vyžaduje použití Azure AD a prostředí místní služby Active Directory.

Nevíte, které potřebujete? Podívejte se na [Jaký druh kolekce potřebujete pro Azure RemoteApp](remoteapp-collections.md).

Tento kurz se provede proces vytváření kolekce hybridní. Je osm kroky:

1.  Rozhodněte, jaký [Obrázek](remoteapp-imageoptions.md) ve vaší kolekci. Můžete vytvořit vlastní obrázek nebo použijte jeden z Microsoft obrázky součástí vašeho předplatného.
2. Nastavení sítě virtuální. Podívejte se na informace o [plánování VNET](remoteapp-planvnet.md) a [změny velikosti](remoteapp-vnetsizing.md) .
2.  Vytvoření kolekce.
2.  Připojit se ke kolekci do vaší místní domény.
3.  Přidání obrázku šablony do kolekce.
4.  Konfigurace synchronizace adresářů. Azure RemoteApp vyžaduje integrace se službou Azure Active Directory buď 1) konfigurace Azure Active synchronizaci adresářů s parametrem synchronizaci hesel nebo 2) konfigurace Azure Active synchronizaci adresářů bez možnosti synchronizaci hesel, ale pomocí domény které je federované do služby AD FS. Podívejte se na [informace o konfiguraci služby Active Directory s RemoteApp](remoteapp-ad.md).
5.  Publikování aplikací RemoteApp.
6.  Konfigurace přístupu uživatelů.

**Než začnete**

Je potřeba udělat následující před vytvořením kolekci:

- [Registrace](https://azure.microsoft.com/services/remoteapp/) pro Azure RemoteApp.
- Vytvoření uživatelského účtu ve službě Active Directory pro použití účtu služby Azure RemoteApp. Omezte oprávnění pro tento účet tak, že ho jenom účastnit se počítačích na doménu.
- Shromážděte informace o vaší místní síti: IP adresa informace a podrobnosti o zařízení VPN.
- Nainstalujte modul [Azure Powershellu](../powershell-install-configure.md) .
- Shromážděte informace o uživatelích, které chcete udělit přístup k. Budete potřebovat hlavní Azure Active Directory uživatelské jméno (například name@contoso.com) pro každého uživatele. Ujistěte se, že mezi Azure AD shoduje s UPN a služby Active Directory.
- Zvolte obrázek šablony. Obrázek šablony Azure RemoteApp obsahuje aplikace a programy, které chcete publikovat pro uživatele. Další informace najdete v tématu [Možnosti obrázku Azure RemoteApp](remoteapp-imageoptions.md) .
- Chcete používat Office 365 ProPlus obrázek? Podívejte se na informace [v tomto poli](remoteapp-officesubscription.md).
- [Konfigurace služby Active Directory pro RemoteApp](remoteapp-ad.md).



## <a name="step-1-set-up-your-virtual-network"></a>Krok 1: Nastavení virtuální sítě
Nástroje můžete nasazovat kolekce hybridní využívající existující Azure virtuální síť nebo můžete vytvořit nový virtuální sítě. Virtuální sítě umožňuje data z Accessu uživatelé ve vaší místní síti prostřednictvím RemoteApp vzdálené zdrojů. Použití Azure virtuální sítě poskytuje vaší kolekce přímý přístup k síti a další služby Azure virtuálních počítačích používaný virtuální sítě.

Zkontrolujte, že zkontrolujte informace o [plánování VNET](remoteapp-planvnet.md) a [velikost VNET](remoteapp-vnetsizing.md) před vytvořením vaší VNET.

### <a name="create-an-azure-vnet-and-join-it-to-your-active-directory-deployment"></a>Vytvoření Azure VNET a připojovat se k zavedení služby Active Directory

Začněte tak, že vytvoříte [virtuální sítě](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). Důvodem je na kartě **síť** Azure portálu. Budete muset připojit virtuální síť zavedení služby Active Directory, která se synchronizuje do vašeho tenanta služby Azure Active Directory.

Další informace najdete v tématu [vytvoření virtuální sítě pomocí portálu Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) .

### <a name="make-sure-your-virtual-network-is-ready-for-azure-remoteapp"></a>Ujistěte se, že je připraven k Azure RemoteApp virtuální sítě
Před vytvořením kolekci Pojďme zkontrolujte hotovou nové virtuální sítě. To můžete ověřit následujícím způsobem:

1. Vytvoření Azure virtuálního počítače uvnitř podsítě virtuální sítě, který jste právě vytvořili pro RemoteApp.
2. Připojení k virtuálního počítače pomocí vzdálené plochy. (Klikněte na tlačítko **Připojit**.)
3. Připojte k stejnou zavedení služby Active Directory, která chcete použít pro RemoteApp.

Které pracoval? Virtuální sítě a podsítě připraveni Azure RemoteApp!

Další informace o vytváření Azure virtuálních počítačích a připojením k nim pomocí vzdálené plochy [tady](https://msdn.microsoft.com/library/azure/jj156003.aspx)můžete najít.

## <a name="step-2-create-an-azure-remoteapp-collection"></a>Krok 2: Vytvořte kolekci Azure RemoteApp ##



1. [Azure portál](http://manage.windowsazure.com)přejděte na stránku Azure RemoteApp.
2. Klikněte na **nové > vytvořit s VNET**.
3. Zadejte název pro svou kolekci.
4. Vyberte plán, který chcete použít – standardní nebo základní.
5. Vyberte svůj VNET rozevírací seznam a potom do vaší podsítě.
6. Vyberte připojit se pro vaši doménu.
5. Klikněte na **vytvořit RemoteApp kolekce**.

Po vytvoření kolekci Azure RemoteApp poklikejte na název kolekce. Který bude zobrazte stránku **Rychlý Start** : Toto je místo, kam dokončete konfiguraci kolekci.

Něco zmizelo špatně? Přečtěte si [informace o hybridním kolekce Poradce při potížích](remoteapp-hybridtrouble.md).

## <a name="step-3-link-your-collection-to-the-local-domain"></a>Krok 3: Propojení kolekci místní domény ##


1. Na stránce **Snadné spuštění** klikněte na **Připojit se místní domény**.
2. Přidání účtu služby Azure RemoteApp místní domény Active Directory. Budete potřebovat název domény, organizační jednotku, služby účet uživatelské jméno a heslo.

    Toto je informace, které jste shromáždili, pokud jste postupovali podle kroků v tématu [Konfigurace Active Directory Azure RemoteApp](remoteapp-ad.md).


## <a name="step-4-link-to-an-azure-remoteapp-image"></a>Krok 4: Odkaz na obrázek Azure RemoteApp ##

Obrázek šablony Azure RemoteApp obsahuje programy, které chcete sdílet s uživateli. Můžete vytvořit nový [obrázek šablony](remoteapp-imageoptions.md) , nebo odkaz na existující obrázek (jedno už importovat nebo nahrát Azure RemoteApp). Můžete také propojit s některou z Azure RemoteApp [obrázky šablon](remoteapp-images.md) , které obsahují Office 365 nebo aplikací Office 2013 (pro zkušební použití).

Pokud ukládáte nový obrázek, potřebujete zadejte název a pak vyberte umístění obrázku. Na další stránce průvodce se najdete v tématu Nastavení rutiny prostředí PowerShell - kopírovat a spusťte tyto rutiny z řádek se zvýšenými oprávněními prostředí Windows PowerShell nahrát obrázek zadaný.

Pokud se připojujete k existující obrázek šablony, stačí zadejte název obrázku, umístění a související Azure předplatného.



## <a name="step-5-configure-active-directory-directory-synchronization"></a>Krok 5: Konfigurace synchronizace adresáře služby Active Directory ##

Azure RemoteApp vyžaduje integrace se službou Azure Active Directory buď 1) konfigurace Azure Active synchronizaci adresářů s parametrem synchronizaci hesel nebo 2) konfigurace Azure Active synchronizaci adresářů bez možnosti synchronizaci hesel, ale pomocí domény které je federované do služby AD FS.

Podívejte se na [AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) – Tento článek vám pomůže nastavit integrace adresářů ve 4 krocích.

Plánování informace a podrobné pokyny najdete v článku [Přehled synchronizace adresáře](http://msdn.microsoft.com//library/azure/hh967642.aspx) .

## <a name="step-6-publish-apps"></a>Krok 6: Publikování aplikací ##

Aplikace Azure RemoteApp je aplikace nebo aplikace, která můžete uživatelům poskytnout. Je součástí obrázek šablony, které jste nahráli kolekce. Když uživatel aplikace zdá se spouštějí v místním prostředí, ale ve skutečnosti běží Azure.

Před vaši uživatelé můžete přistupovat k aplikace, budete muset zveřejníte je – tímto nástrojem uživatelům přistupovat aplikace prostřednictvím klienta vzdálené plochy.

Více aplikací můžete publikovat do kolekce. Na stránce publikování klikněte na **Publikovat** na Přidat aplikaci. Buď můžete publikovat v nabídce **Start** na obrázek šablony nebo zadáním cesty na obrázek šablony aplikace. Pokud se rozhodnete přidat z nabídky **Start** , vyberte program, který chcete přidat. Pokud se rozhodnete zadejte cestu k aplikaci, zadejte název aplikace a cestu k kde hned po instalaci používat na obrázek šablony.

## <a name="step-7-configure-user-access"></a>Krok 7: Nastavení uživatelského přístupu ##

Teď, když jste vytvořili kolekci, budete muset přidat uživatele, které chcete používat vzdálená zdroje. Poskytnutí přístupu k musí být ve službě Active Directory klientovi přidružený k předplatnému uživatele slouží k vytváření této kolekce Azure RemoteApp.

1.  Na úvodní stránce klikněte na **Konfigurovat přístupu uživatelů**.
2.  Zadejte pracovní účet (ze služby Active Directory) nebo účet Microsoft, který chcete udělit přístup.

    **Poznámky:**

    Ujistěte se, že používáte “user@domain.com” formát.

    Pokud používáte Office 365 ProPlus ve vaší kolekci, je nutné použít identit služby Active Directory pro uživatele. Díky ověřit licencování.


3.  Po ověření uživatele klikněte na **Uložit**.


## <a name="next-steps"></a>Další kroky ##
To je vše: úspěšně vytvořili a kolekci Azure RemoteApp hybridní nasazení. Dalším krokem je mít uživatelé stažení a instalace klienta vzdálené plochy. Adresa URL můžete najít pro klienta na stránce Azure RemoteApp rychlý Start. Potom mít přihlášení uživatelů na klienta a přístup k aplikace, které jste publikovali.



### <a name="help-us-help-you"></a>Pomozte nám umožňují
Věděli jste, že kromě hodnocení v tomto článku a vyjádřit dolů pod, můžete provádět změny samotný článek? Něco chybí? Něco špatně? Můžu zapsat něco, co je právě matoucí? Posunutí nahoru a klikněte na **příkaz Upravit v GitHub** ke změnám – můžou být chodily do nám k revizi a potom po jsme odhlášení k nim, uvidíte změny a vylepšení tady.

<properties 
    pageTitle="Jak vytvořit kolekci cloudu Azure RemoteApp | Microsoft Azure" 
    description="Naučte se vytvářet nasazení RemoteApp Azure, který ukládá data v Azure cloudu." 
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

# <a name="how-to-create-a-cloud-collection-of-azure-remoteapp"></a>Jak vytvořit sadu cloudu Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Existují dva typy kolekcí [Azure RemoteApp](remoteapp-collections.md): 

- Cloudu: nachází zcela Azure. Je možné uložit všechna data v cloudu (tak, aby jen cloudu kolekci) nebo k připojení kolekci VNET a uložení dat tam.   
- Hybridní: obsahuje virtuální sítě pro místní přístup - vyžaduje použití Azure AD a prostředí místní služby Active Directory.

Tento kurz vás provede proces vytváření kolekce cloudu. Existují čtyři kroky: 

1.  Vytvořte kolekci Azure RemoteApp.
2.  Pokud chcete nakonfigurujte synchronizaci adresářů. Pokud používáte Azure AD + služby Active Directory, budete muset synchronizovat uživatele, kontakty a hesel ze služby Active Directory místní ke tenantovi Azure AD.
5.  Publikování aplikací.
6.  Konfigurace přístupu uživatelů.


**Než začnete**

Je potřeba udělat následující před vytvořením kolekci:

- [Registrace](https://azure.microsoft.com/services/remoteapp/) pro Azure RemoteApp. 
- Shromážděte informace o uživatelích, které chcete udělit přístup k. To může být informace o účtu Microsoft nebo Active Directory pracovní účet informace pro uživatele.
- Tento postup předpokládá, že jste buď přejdete na použijte jeden z obrázky šablon zadané jako součást předplatného nebo že už máte nahrát obrázek šablony, kterou chcete použít. Pokud budete muset nahrát obrázek jinou šablonu, můžete to udělat na stránce obrázky šablon. Jednoduše klikněte na **Nahrát obrázek šablony** a postupujte podle pokynů v průvodci. 
- Chcete používat Office 365 ProPlus obrázek? Podívejte se na informace [v tomto poli](remoteapp-officesubscription.md).
- Chcete zadat vlastní aplikace nebo LOB programy? Vytvoření nového [obrázku](remoteapp-imageoptions.md) a používat ve vaší kolekci cloudu.
- Zjistěte, jestli budete potřebovat pro připojení k VNET. Pokud zvolíte možnost připojit k VNET, ujistěte se, splňuje [pravidla pro změnu velikosti](remoteapp-vnetsizing.md) a že se [můžou připojit k RemoteApp](remoteapp-vnet.md). Podívejte se na [článek plánování VNET ](remoteapp-planvnet.md)najdete další informace.
- Pokud používáte VNET, rozhodněte, jestli se chcete připojit k místní domény Active Directory.

## <a name="step-1-create-a-cloud-collection---with-or-without-a-vnet"></a>Krok 1: Vytvoření kolekce cloudu – pomocí hesla nebo bez VNET##


Pomocí následujících kroků můžete **vytvořit kolekci jen cloudu**:

1. Na portálu správy přejděte na stránku RemoteApp.
2. Klikněte na **nové > vytvořit**.
3. Zadejte název pro svou kolekci a vyberte svoji oblast.
4. Vyberte plán, který chcete použít – standardní nebo základní.
5. Vyberte šablonu, kterou chcete použít pro tuto kolekci. 

    **Tip:** Vaše předplatné pro RemoteApp získáváte [obrázky šablon](remoteapp-images.md) , které obsahují Office 365 nebo Office 2013 (pro zkušební použití) programy, některé publikované (jako je Word) a ostatní připraveni publikovat. Můžete taky vytvořit nový [Obrázek](remoteapp-imageoptions.md) a použít v kolekci cloudu.


1. Klikněte na **vytvořit RemoteApp kolekce**.
    
    **Důležité:** Může trvat až 30 minut zřízení kolekci.

Po vytvoření kolekci Azure RemoteApp poklikejte na název kolekce. Který bude zobrazte stránku **Rychlý Start** : Toto je místo, kam dokončete konfiguraci kolekci.

Umožňuje vytvořit **cloudu + VNET kolekce**podle těchto kroků:

1. Na portálu správy přejděte na stránku Azure RemoteApp.
2. Klikněte na **Nový** > **vytvořit s VNET**.
3. Zadejte název pro svou kolekci.
4. Vyberte plán, který chcete použít – standardní nebo základní.
5. Zvolte VNET jste již vytvořili. Nevíte, jak to udělat? Nyní jsou kroků v tématu [hybridní](remoteapp-create-hybrid-deployment.md) .
6. Rozhodněte, jestli se chcete připojit kolekci do vaší domény. Pokud ano, musíte se integrace Azure AD pomocí AD Connect a vaším prostředím služby Active Directory. Která je součástí pod v **kroku 2**.
6. Klikněte na **vytvořit RemoteApp kolekce**.


## <a name="step-2-configure-active-directory-directory-synchronization-optional"></a>Krok 2: Nakonfigurujte synchronizace adresáře služby Active Directory (volitelné) ##

Pokud chcete používat službu Active Directory Azure RemoteApp vyžaduje synchronizace adresářů mezi Azure Active Directory a na adresářová služba Active Directory synchronizovat uživatele, kontakty a hesla pro vašeho klienta služby Azure Active Directory. Naleznete v tématu [Konfigurace Active Directory Azure RemoteApp](remoteapp-ad.md) informace o plánování. Můžete taky přejít přímo do [AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) informace.

## <a name="step-3-publish-apps"></a>Krok 3: Publikování aplikací ##

Aplikace Azure RemoteApp je aplikace nebo aplikace, která můžete uživatelům poskytnout. Je součástí obrázek šablony, které jste nahráli kolekce. Když uživatel aplikace aplikace vypadá běží v místním prostředí, ale ve skutečnosti běží virtuálního počítače v Azure. 

Aby uživatelé měli přístup ke aplikací, je potřeba publikovat – publikování aplikací umožňuje uživatelům přístup k aplikace přes klienta Vzdálená plocha.
 
Na kolekci Azure RemoteApp mohli publikovat více aplikace. Na stránce publikování klikněte na **Publikovat** přidání programu. Buď můžete publikovat v nabídce **Start** na obrázek šablony nebo zadáním cesty na obrázek šablony aplikace. Pokud se rozhodnete přidat z nabídky **Start** , zvolte aplikace na publikovat. Pokud se rozhodnete zadejte cestu k aplikaci, zadejte název aplikace a cestu k kde hned po instalaci používat na obrázek šablony.

## <a name="step-4-configure-user-access"></a>Krok 4: Nastavení uživatelského přístupu ##

Teď, když jste vytvořili kolekci, budete muset přidat uživatele, které chcete používat vzdálená zdroje. Pokud používáte služby Active Directory, uživateli poskytnout přístup k musí být ve službě Active Directory klientovi přidružený k předplatnému, které jste použili k vytvoření této kolekce.

1.  Na úvodní stránce klikněte na **Konfigurovat přístupu uživatelů**. 
2.  Zadejte pracovní účet (ze služby Active Directory) nebo účet Microsoft, který chcete udělit přístup.

    **Poznámky:** 

    Ujistěte se, že používáte “user@domain.com” formát.

    Pokud používáte Office 365 ProPlus ve vaší kolekci, je nutné použít identit služby Active Directory pro uživatele. Díky ověřit licence. 

3.  Po ověření uživatele klikněte na **Uložit**.


## <a name="next-steps"></a>Další kroky ##

To je vše: úspěšně vytvoření a nasazení kolekci Azure RemoteApp cloudu. Dalším krokem je mít uživatelé stažení a instalace klienta vzdálené plochy. Adresa URL můžete najít pro klienta na stránce Azure RemoteApp rychlý Start. Potom jste přihlášení uživatelů na klienta a přístup k aplikace, které jste publikovali.

### <a name="help-us-help-you"></a>Pomozte nám vám pomůžou 
Věděli jste, že kromě hodnocení v tomto článku a vyjádřit dolů pod, můžete provádět změny samotný článek? Něco chybí? Něco špatně? Můžu zapsat něco, co je právě přehlednější? Posunutí nahoru a klikněte na **příkaz Upravit v GitHub** ke změnám – můžou být chodily do nám k revizi a potom po jsme odhlášení k nim, uvidíte změny a vylepšení tady.
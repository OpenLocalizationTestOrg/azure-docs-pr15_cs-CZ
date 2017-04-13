
<properties 
    pageTitle="Jak používat vaše předplatné Office 365 s Azure RemoteApp | Microsoft Azure"
    description="Zjistěte, jak můžete předplatné Office 365 v Azure RemoteApp sdílení aplikace Office."
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-to-use-your-office-365-subscription-with-azure-remoteapp"></a>Jak používat vaše předplatné Office 365 s Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Věděli jste, že můžete stávajícímu předplatnému Office 365 v Azure RemoteApp sdílení aplikací Office z cloudu? Přečtěte si informace o Office 365 + Azure RemoteApp možnosti, včetně odkazů na článků o Office 365, které vám pomohou maximálně využít předplatného.

## <a name="how-do-i-use-office-365-accounts-for-azure-remoteapp"></a>Používání účtů Office 365 pro Azure RemoteApp
Podívejte se na nový článek je pro všechny informace: [používání Azure RemoteApp s uživatelské účty služeb Office 365](remoteapp-o365user.md)

## <a name="can-i-use-my-office-365-subscription-to-run-office-applications-in-azure-remoteapp"></a>Můžete použít svoje předplatné Office 365 pro spuštění aplikace Office v Azure RemoteApp?

Ano! Použití předplatného Office 365 ve skutečnosti je jediný způsob, jak přenést používání aplikací Office Azure RemoteApp.

(Poznámka: Pokud nasazení služby Azure RemoteApp dodává hostingu partnera, možná budete moct k poskytování licence Office podle [Licencování smlouvách poskytovatele](http://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx))


Skvělé věc o vaše předplatné Office 365, je, že umožňuje použít stejné uživatelskou licenci na mnoha různých platformách a prostředí, včetně Azure cloudu. Při používání aplikací Office v Azure RemoteApp nemusíte zakoupit další licence nebo konfigurovat stávající licence nijak jinak. Potřebujete stačí předplatnému Office 365, které zahrnuje [Office 365 ProPlus](https://technet.microsoft.com/library/Gg702619.aspx).

Office 365 ProPlus umožňuje [Aktivace sdílenému počítači](https://technet.microsoft.com/library/Dn782860.aspx) : Tato funkce umožňuje dočasné aktivaci uživatele pro Office v virtuální a cloudu prostředí jako Azure RemoteApp (a Vzdálená plocha).

U kterých plánů Office 365 zahrnují Office 365 ProPlus? Podívejte se na tabulce [dostupnost služby v rámci každý plán](https://technet.microsoft.com/library/office-365-plan-options.aspx) . Všimněte si, že ne všech plánů zahrnuje Office 365 ProPlus (například plán Office 365 Business). Pokud váš plán nepodporuje, zvažte možnost upgradu na plán, který nemá (například Office 365 Education E3).

## <a name="ok-so-how-are-my-office-365-proplus-licenses-used-with-azure-remoteapp"></a>Dobře, a jak se můj Office 365 ProPlus licencí, které používá s Azure RemoteApp?

Každý uživatelskou licenci pro Office 365 ProPlus umožňuje jednoho uživatele aktivovat aplikace Office ve až na 5 počítačů plus tablety a telefony. Každé aktivaci máte zaregistrované v rámci uživatele, dokud deaktivaci Office na zařízení. (Uživatelé můžou určit jejich zařízení na [portálu Office 365](https://portal.office365.com/).)

S Azure RemoteApp jeden uživatel může Přihlaste se k několika počítačích na stejný den bez provádějící ho. Je proto, že služba automaticky spravuje a upraví zdrojů v cloudu, zatímco uživateli se zobrazí jenom s aplikacemi a programy, které sdílíte. Pro tento scénář Office 365 ProPlus nabízí režim aktivaci sdíleném počítači – to znamená, že tohoto uživatele není potřeba udělat všechny Správa licencí pro přístup k prostředky a aby jednotlivé počítače nepočítají limit aktivace 5 počítače.

Dokud (Správci) přiřadíte Office 365 ProPlus licence uživatelů, mohou použít Office na jejich osobních zařízení i až kolekci Azure RemoteApp.

## <a name="which-office-applications-can-i-use-with-office-365-and-azure-remoteapp"></a>Aplikace Office, které můžete používat s Office 365 a Azure RemoteApp?

Vaše předplatné Office 365 můžete použít k aktivaci a ke sdílení Office 2013 v Azure RemoteApp nasazení. Aktuálně nepodporujeme použít jiné verze Office s Azure RemoteApp. Platí to i Office 2003, Office 2007, Office 2010 a Office 2016.

## <a name="what-about-visio-pro-or-project-pro"></a>Na co dávat pozor Project nebo Visio Pro Pro?

Office 365 ProPlus obrázek součástí vašeho předplatného RemoteApp cs zahrnuje odeslání Visio Pro a Project Pro. Ale předplatného Office 365 ProPlus nelze použít k aktivaci těchto aplikacích: každý mají svoje vlastní licenci. Můžete je aktivovat na [portálu Office 365](https://portal.office365.com/). 

Nemusíte licencí těchto aplikacích, pokud nechcete, aby jejich použití. Aktivace jenom aplikace, které chcete použít – a přejděte na ostatní. Budete dál vidět na obrázku, ale nemůžete používat. 

## <a name="how-do-i-get-started-with-office-365-and-azure-remoteapp"></a>Jak můžu začít s Office 365 a Azure RemoteApp?

Nyní již znáte podrobnosti o licencování Office 365, Pojďme mohli začít pracovat s ním pracovat v Azure RemoteApp – je velmi snadné:

Při vytváření kolekci Azure RemoteApp pomocí **Office 365 ProPlus (vyžaduje předplatné)** obrázek.

![Azure RemoteApp obrázek s Office 365 Pro Plus](./media/remoteapp-officesubscription/remoteapp-officeimage.png)


Tento obrázek obsahuje nejnovější verzi systému Windows Server a Office 365 ProPlus. Po nakonfigurování kolekci (včetně publikování aplikací) vaši uživatelé jednoduše přihlásit Azure RemoteApp (pomocí svého RemoteApp klienta) a zadejte své přihlašovací údaje Office 365 pro všechny aplikace Office. Licence automaticky doručované bez nastavených nebo Správa povinné.

## <a name="can-i-create-a-custom-image-with-office-365-proplus"></a>Můžete vytvořit vlastní obrázek s Office 365 ProPlus?

Vytvoření vlastního obrázku pro vaší kolekci obsahující Office 365 ProPlus. Existuje 2 možností - použití Azure Galerie aplikace, které uvádíme nebo můžete vytvořit vlastní obrázek a nainstalovat Office 365 ProPlus tam.

### <a name="use-the-azure-gallery-image"></a>Použití Galerie Azure

Nejjednodušší způsob, jak nasazení Office 365 ProPlus do kolekce je [začít s jednou Azure Galerie obrázků](remoteapp-image-on-azurevm.md) součástí vašeho předplatného Azure RemoteApp. Zkontrolujte, že vyberete obrázek **jsou předinstalované systému Windows Server hostitele relací vzdálené plochy s Office 365 ProPlus** . Nainstalujte jiných aplikací, které chcete na tomto obrázku a jste můžete začít.

### <a name="use-a-custom-image"></a>Použít vlastní obrázek

Můžete kdykoli vytvořit vlastní obrázek – můžete vytvořit [OM Azure](remoteapp-image-on-azurevm.md) nebo [Vytvořit obrázek místně](remoteapp-create-custom-image.md) a ho nahrajte do Azure. V obou případech Ujistěte se, že musíte nainstalovat Office 365 ProPlus použití na sdíleném počítači aktivace uzel. Použijte [Nástroj pro nasazení Office](http://blogs.technet.com/b/odsupport/archive/2014/07/11/using-the-office-deployment-tool.aspx) a postupujte podle [pokynů](https://technet.microsoft.com/library/Dn782858.aspx) k instalaci.  

### <a name="disable-automatic-updates-for-office-365-proplus-in-your-custom-image---important"></a>Zákaz automatické aktualizace pro Office 365 ProPlus ve vlastní obrázek – důležité

Azure RemoteApp používá vlastní obrázek jako šablonu pro přidání další zdroje informací jako poptávka z uživatelů přirážky. Pro ochranu zpožděním a problémy s připojením, zakážete automatické aktualizace pro Office v obraze. Pokud není, pak každý zdroj vytvořené pomocí této šablony se automaticky aktualizuje při spuštění. Místo toho budete postupovat standardní Azure RemoteApp při aktualizaci vlastní obrázek. Tak aktualizace aplikací Office jednou na obrázek šablony a nechte Azure RemoteApp starat o zobrazuje aktualizace uživatelů.

Zákaz automatické aktualizace, přidejte do následující konfiguračního souboru nástroj pro nasazení Office:

        <Updates Enabled="FALSE" />

Konfigurační soubor nyní by měl obsahovat tyto řádky:
    
        <Display Level="NONE" AcceptEULA="TRUE" />
        <Property Name="SharedComputerLicensing" Value="1" />
        <Updates Enabled="FALSE" />

## <a name="so-how-can-i-update-an-image-with-office-365-proplus"></a>Takže jak můžete aktualizovat obrázek s Office 365 ProPlus?

Obrázek ve vaší kolekci mnoho důvodů. Tady je pár:

- Získání nejnovější aktualizace Windows 
- Získání nejnovějších aktualizacích pro aplikace Office 365 ProPlus
- Aktualizovat vlastní aplikace
- Změnit další nastavení pro samotný obraz

Postup začátku do konce aktualizace kolekci použít obrázek jste aktualizovali, přejděte [v tomto poli](remoteapp-update.md). Ale informace o tom, jak aktualizovat obrázek a Office 365 ProPlus, podívejte se na následující informace.

Můžete mít dvě možnosti aktualizace obrázek - nahradit obrázek úplně novou mapou nebo ručně aktualizovat existující obrázek.

### <a name="replace-your-image-with-the-latest-azure-gallery-image--add-customizations"></a>Obrázek nahradit nejnovější Azure galerie + přidat vlastní nastavení
Pomocí této možnosti můžete nechat Microsoft starat o aktualizace Windows Server a Office 365 ProPlus. Namísto aktualizace existující obrázek, vytvoříte úplně nový obrázek podle nejnovější galerie. Opakujte všechny kroky dřív se upravit obrázek – nainstalujte vlastních aplikací změnit obrázek konfigurace, atd.

Galerie obrázků pravidelně aktualizují, takže můžete umístíte snadné znalost aktuální aplikací systému Windows Server a Office 365 ProPlus. Nejvhodnější poměr je samozřejmě, že budete muset pokaždé, když se zobrazí nový obrázek použít vlastní nastavení. Můžete vytvářet skripty pro automatizaci nastavení vlastních úprav.

### <a name="manually-update-your-existing-image"></a>Ruční aktualizace existující obrázek

Pomocí této možnosti používají standardní Windows nástroje instalace aktualizací pro obrázek. Pro Office 365 ProPlus použijte nástroj pro nasazení Office si stáhněte a nainstalujte nejnovější aktualizace nebo verzích Office 365 ProPlus.

> [AZURE.IMPORTANT] Mějte na paměti: zakázat Office 365 ProPlus automatické aktualizace.

Potřebujete další informace o používání nástroje pro nasazení Office aktualizací?

- [Nasazení Klikni a spusť pro Office 365 produkty pomocí nástroje pro nasazení Office](https://technet.microsoft.com/library/JJ219423.aspx)
- [Nasazení a aktualizace Office 365 ProPlus nástrojem pro nasazení Office](https://channel9.msdn.com/Events/Ignite/2015/BRK3168) (video)
- [Konfigurace nastavení aktualizace pro Office 365 ProPlus](https://technet.microsoft.com/library/dn761708.aspx)

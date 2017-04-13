
<properties
    pageTitle="Co je nového v Azure RemoteApp? | Microsoft Azure"
    description="Další informace o změny a vylepšení Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="whats-new-in-azure-remoteapp"></a>Co je nového v Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Jednou z výhod Azure RemoteApp je, že vždy pracujeme umožnit zlepšení. Pokaždé, když v takovém jsme budete ohlaste provedené změny.

## <a name="future-updates"></a>Budoucí aktualizace
Přece - víte, že tým Azure RemoteApp publikuje měsíční aktualizacích v blogu služby Remote Data Service? Můžete najít nejenom co je změna Azure RemoteApp, ale taky Další informace o používání RDS. Podívejte se na jejich blogu [Blogu služby vzdálené plochy](https://blogs.msdn.microsoft.com/rds/)informace. Příklad několika týdny zpětně, které zveřejnil údaj o [zrušení a posunutí pracovního vytížení s Azure RemoteApp a Azure AD](https://blogs.msdn.microsoft.com/rds/2016/01/19/lift-and-shift-your-workloads-with-azure-remoteapp-and-azure-ad-domain-services/).
 
## <a name="september-2015"></a>Září 2015
- Přidané Infopath na obrázek šablony a Galerie Microsoft Office 365. Pokud chcete sdílet aplikace Infopath, nezapomeňte aktualizovat vaše kolekce nejnovější obrázek.
- Aktualizace klienta:
    - Windows Klient aktualizuje tak, aby umožnit, aby uživatelé sdílet svůj názor, zejména kolem problémy s připojením.
    - iOS klient aktualizuje Oprava chybové zprávy a problém vyřešit, kde svoje přihlašovací údaje, kterým vypršela platnost dříve, než byste čekali.
- Pracujeme na získání podpory pro Office 2016 testovat. Po dokončení, vyhledejte aktualizované obrázky.
- Publikován nový článek o [rozdíly mezi kolekce cloudu a hybridní](remoteapp-collections.md) – to vám umožní zvolte požadovaný typ kolekce je nejvhodnější pro aplikace - jen cloudu, cloudu + VNET nebo hybridní.
- Chcete sdílet QuickBooks pomocí Azure RemoteApp, ale nejste si jistí, kroky? Podívejte se na [nový článek společnosti Tomáš](remoteapp-quickbooks.md) oznámením, že přesně určit, jak dělat.

## <a name="august-2015"></a>Srpen 2015
Velký změny došlo srpen – spočívá zvýraznění:

- Teď můžete Azure VNET s kolekcí cloudu! Podívejte se na [Pokyny k vytváření cloudu](remoteapp-create-cloud-deployment.md) nové postup.
- Umožnily přidání aplikací do nabídky **Start **systému Windows RemoteApp klienta. Aplikace se objeví v seznamu aplikací a můžete je připnout k nabídce **Start **v systému Windows.
- Přidat nový obrázek do Galerie Azure OM – Windows Server hostitele relací vzdálené plochy s Microsoft Office 365 ProPlus.
- Pevná klientovi Mac tak, aby aplikace s modální windows přestanou se vám ukotvení.
- Popsána, jak používat [Office 365 ProPlus předplatného](remoteapp-officesubscription.md) se Azure RemoteApp.
- Podrobné, jak se dá [zabezpečení aplikace a dat](remoteapp-secure.md) ve vaší kolekci Azure RemoteApp.

## <a name="july-2015"></a>Červenec 2015

Červenec nastavení prostředí pro změny chodit v srpnu, tak je hodně pro řešení nyní převážně aktualizace dokumentu. Tady jsou posledních změn:

- Přidat kartu **podpory** k portálu snadněji k článek podpůrné materiály, jako je na fórech.
- Informace o řešení potíží při vytváření kolekce hybridní přepracována. Rezervování souborů [nejnovějšími největší](remoteapp-hybridtrouble.md) tipy pro odstraňování potíží, například, jak identifikovat správné porty pro nastavení pro svůj VNET.
- Popsána jak vytvořili a uložili v Azure RemoteApp [uživatelská data](remoteapp-upd.md) .
- Popsány jak [uzamčení aplikace](remoteapp-secure.md).
- Publikovat [Azure RemoteApp rutiny](https://msdn.microsoft.com/library/mt428031.aspx).
- A nakonec můžeme začít konverzaci někteří uživatelé Azure RemoteApp o terminologie. Přehled změn způsob, jakým se označují různé kolekce možnosti.

## <a name="june-2015"></a>Června 2015

Tolik změny! Zjištění velmi zaneprázdnění v červnu týmu:

- Přepracované Azure RemoteApp [úvodní stránka](https://www.remoteapp.windowsazure.com/) – podívejte se na!
- Aktualizace softwaru ve všech obrázků k dispozici v rámci vašeho předplatného.
- Volání vylepšeních hybridní kolekce, včetně vynuceného tunneling podpory a kontrola IP adres podsítí velikosti před pokusem o vytvořit kolekci.
- Zjištění, které * zástupných nefunguje k webové kamery. Místo toho budete muset zadat instance ID nebo GUID. Jsme budete se aktualizuje informace o přesměrování, aby odrážela.
- Provedení, můžete přidat vlastní antivirový software má obrázek při vytváření obrázku šablonu z Galerie Azure.

Zde máme další změny zavádění v červenec, proto jsme bude brzy zpět se jiná aktualizace.

## <a name="may-2015"></a>Květen 2015

Až květen byly celou řadu doplňky (a měsíců) od jsme nově vytvořený v tomto tématu, abyste tento seznam cheats trochu a od začátku březen je. Podívejte se na tyto nové funkce:

- Automatizace všechno - Azure RemoteApp má nyní [rutin v modulu Azure Powershellu](remoteapp-tutorial-arawithpowershell.md).
- [Vytvořit obrázek Azure RemoteApp z Azure virtuálního počítače](remoteapp-image-on-azurevm.md). Uložení vlastní obrázek do Azure mnohem rychlejší něco v ní.
- Připojení vaší podnikové síti prostředky k Azure pomocí Azure VNET místo RemoteApp VNET. Jsme jste aktualizovali [hybridní kolekce pokyny](remoteapp-create-hybrid-deployment.md) pro vás provede jednotlivými vytváření Azure VNET (je kroku 1).
- Mluvíte o VNETs, podívejte se na [nové pokyny](remoteapp-vnetsizing.md) kolem omezení a limity velikosti VNET.
- A mluvit limitů – stačí co jsou [služby limity a výchozích hodnot](../azure-subscription-service-limits.md)?

Chcete se dozvědět víc o Azure RemoteApp? Skupina RemoteApp nebyla se platnými Ignite několik týdny zpětně. Podívejte se na Tomáš video, [týkajících se základů Microsoft Azure RemoteApp správy a správa](http://channel9.msdn.com/Events/Ignite/2015/BRK3868).

Potřebují vidět Azure RemoteApp v reálném světě? Podívejte se na kurz [Spustit aplikaci na libovolném zařízení kdekoliv](remoteapp-anyapp.md) : se dozvíte, jak sdílet s jinými uživateli, včetně sdílení souborů databáze aplikace Access. Kurz máme také na [Díky Office 365](remoteapp-tutorial-o365anywhere.md) spuštěn stejná na libovolném zařízení.

Děkujeme za důvodu s abychom - zpět následujícím měsíci s další aktualizace.


### <a name="help-us-help-you"></a>Pomozte nám vám pomůžou
Věděli jste, že kromě hodnocení v tomto článku a vyjádřit dolů pod, můžete provádět změny samotný článek? Něco chybí? Něco špatně? Můžu zapsat něco, co je právě matoucí? Posunutí nahoru a klikněte na **příkaz Upravit v GitHub** ke změnám – můžou být chodily do nám k revizi a potom po jsme odhlášení k nim, uvidíte změny a vylepšení tady.

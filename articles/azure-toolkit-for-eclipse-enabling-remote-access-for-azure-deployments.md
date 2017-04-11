<properties
    pageTitle="Povolení vzdáleného přístupu k Azure nasazení v zatmění"
    description="Informace o povolení vzdáleného přístupu Azure nasazení pomocí nástrojů Azure pro Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->

# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a>Povolení vzdáleného přístupu k Azure nasazení v zatmění

Můžou pomoct odstranit zavedení, můžete povolit a připojení k virtuální počítač hostingu nasazení pomocí vzdáleného přístupu. Funkce vzdálený přístup závisí na vzdálené plochy RDP (Protocol). Vzdálený přístup můžete nakonfigurovat pro nasazení po publikovaly na Azure nebo pokud používáte zatmění operačním systému Windows, můžete nakonfigurovat vzdáleného přístupu před publikováním na Azure. Všimněte si, že budete potřebovat klienta vzdálené plochy, která je kompatibilní s operačním systémem budete moct připojit k vaší nasazení virtuálního počítače v Azure.

## <a name="how-to-enable-remote-access-before-you-deploy-to-azure"></a>Povolení vzdáleného přístupu před nasazením Azure

> [AZURE.NOTE] Povolení vzdáleného přístupu před nasazením aplikaci Azure, musíte být zatmění v systému Windows.

Následující obrázek znázorňuje vlastnosti **Vzdáleného přístupu** k povolení vzdáleného přístupu použít dialogové okno.

![][ic719494]

Chcete-li zobrazit dialogové okno Vlastnosti **Vzdáleného přístupu** dvěma způsoby:

* Klikněte na odkaz **Upřesnit** v části **Vzdáleného přístupu** dialogové okno **Publikovat Azure** .
* Otevřete dialogové okno **Vlastnosti** Azure projektu.

Při vytváření nového projektu Azure nasazení projektu nemá vzdáleného přístupu standardně zapnutá. Však můžete snadno povolení vzdáleného přístupu zadáním uživatelské jméno a heslo v dialogovém okně **Publikovat Azure** . Heslo vzdáleného přístupu je šifrovaná pomocí certifikáty X.509. Pokud nepoužíváte zadat vlastní certifikát šifrování vychází certifikátu podepsaného svým držitelem součástí Azure modul plug-in pro Eclipse. Certifikát podepsaný svým držitelem se ve složce **certifikátu** Azure projektu, uložená jako soubor veřejné certifikátu (SampleRemoteAccessPublic.cer) i soubor certifikátu osobní informace Exchange (PFX) (SampleRemoteAccessPrivate.pfx). Ten obsahuje privátním klíčem certifikátu a má výchozí heslo **hesla1**. Protože toto heslo je veřejně známými, výchozí certifikát by měl použít pouze pro účely, ne pro nasazení výrobní výuku. Takže jiné než pro výuku účely, když budete chtít povolené relacích nasazení najdete by měl kliknete na odkaz **Upřesnit** v dialogovém okně **Publikovat Azure** zadat vlastní certifikát. Všimněte si, že budete muset nahrát PFX verzi certifikátu hostovanou služby v portálu pro správu Azure tak tento Azure může dešifrovat heslo uživatele.

Zbytek kurzu se dozvíte, jak povolení vzdáleného přístupu pro Azure nasazení projektu, který byl původně vytvořený pomocí vzdáleného přístupu zakázané. Pro účely tohoto kurzu vytvoříte nový certifikát podepsaný svým držitelem a jeho soubor .pfx bude mít heslo podle svého výběru. Máte taky možnost pomocí certifikátu vydán renomovanou certifikační autoritou.

## <a name="how-to-enable-remote-access-after-you-have-deployed-to-azure"></a>Povolení vzdáleného přístupu po nasazení na Azure

Povolení vzdáleného přístupu po nasazení k Azure, použijte následující kroky:

1. Přihlaste se k portálu Správa Azure pomocí účtu Azure
1. V seznamu **Cloudové služby**vyberte publikovaném cloudové služby
1. Na webové stránce cloudové služby klikněte na **Konfigurovat** odkaz
1. V dolní části na stránce konfigurace klikněte na **vzdálené** odkaz
1. Pokud se zobrazí dialogové okno automaticky otevírané:
    * Zadat roli, u kterého chcete povolení vzdáleného přístupu
    * Zaškrtněte políčko **Povolit vzdálená plocha**
    * Zadejte uživatelské jméno a heslo, které chcete použít pro vzdálený přístup
    * Vyberte certifikát, který chcete použít
1. Klikněte na tlačítko **OK** 

Zobrazí se zpráva oznamující, že změny konfigurace probíhá, které může trvat několik minut. Po dokončení konfigurace změnit postupujte podle kroků uvedených v části **přihlášení vzdáleně** dál v tomto článku.
    
## <a name="how-to-enable-remote-access-in-your-package"></a>Povolení vzdáleného přístupu do balíčku

1. V podokně Průzkumník projektu na zatmění Azure projektu klikněte pravým tlačítkem myši a klikněte na **Vlastnosti**.

1. V dialogovém okně **Vlastnosti** rozbalte **Azure** v levém podokně a klikněte na **Vzdáleného přístupu**.

1. V dialogovém okně **Vzdáleného přístupu** zajistěte, že je zaškrtnuté políčko **Povolit všechny role přijmout připojení ke vzdálené ploše s tyto přihlašovací údaje** .

1. Zadejte uživatelské jméno pro připojení ke vzdálené ploše.

1. Zadejte a potvrďte heslo pro uživatele. Uživatelské jméno a heslo hodnoty v tomto dialogovém okně nastavení se použije při připojení ke vzdálené ploše. (Vezměte v úvahu, že se jedná samostatné heslo z hesla PFX.)

1. Zadejte datum vypršení platnosti uživatelského účtu.

1. Klikněte na **Nový** vytvořit certifikát podepsaný svým držitelem nové. (Můžete taky může z vašeho pracovního prostoru nebo systém souborů pomocí tlačítek **pracovního prostoru** nebo **systému souborů** vyberte certifikát v tomto pořadí, ale pro účely tohoto kurzu vytvoříte nový certifikát.)

    * V dialogovém okně **Nový certifikát** zadejte a potvrďte heslo, které budete používat pro soubor PFX.

    * Přijměte zadaná hodnota pro **Název (CN)**nebo použít vlastní název.

    * Zadejte cestu a název uložení nový certifikát v podobě .cer. V tomto kroku a dalším krokem můžete použít složku **certifikátu** Azure projektu, ale můžete zadarmo vyberte jiné umístění. Pro účely tohoto kurzu budeme používat **c:\mycert\mycert.cer**. (Vytvořte složku **c:\mycert** před pokračováním nebo použít existující složku, pokud budete chtít.)

    * Zadejte cestu a název kde nový certifikát a jeho privátním klíčem formou .pfx, budou uloženy. Pro účely tohoto kurzu budeme používat **c:\mycert\mycert.pfx**. Dialogové okno **Nový certifikát** by měla vypadat přibližně takto (Aktualizovat cesty složky, pokud jste nepoužili **c:\mycert**):

        ![][ic712275]

    * Klikněte na **OK** zavřete dialogové okno **Nový certifikát** .

1. Dialogové okno **Vzdáleného přístupu** by měla vypadat přibližně takto:</p>

    ![][ic719495]

1. Klikněte na **OK** zavřete dialogové okno **Vzdáleného přístupu** .
    
Vytvořit aplikaci znovu pomocí sestavení nastavení pro nasazení do cloudu.

## <a name="to-log-in-remotely"></a>Chcete-li vzdálenému

Po instanci rolí je připravená, můžete vzdáleně přihlášení do virtuálního počítače, který je hostitelem vaší aplikace.

* Pokud používají zatmění na Windows a jste vybrali možnost **Start Vzdálená plocha na nasazení** během nasazení služby Azure, zobrazí se přihlašovací obrazovka připojení ke vzdálené ploše při spuštění nasazení. Když se zobrazí výzva pro uživatelské jméno a heslo, zadejte hodnoty, které jste zadali vzdáleného uživatele a budou moct přihlásit.

* Další způsob, jak vzdáleně přihlásit se pomocí <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Portálu Správa Azure</a>:

    * V zobrazení **Cloudové služby** na portálu Správa Azure klikněte cloudové služby, klikněte na tlačítko **instance**, klikněte na konkrétní instanci a potom klikněte na tlačítko **Připojit** . Tlačítko **Připojit** se zobrazí jako v panelu s příkazy následující kroky:

        ![][ic659273]

    * Po kliknutí na tlačítko **Připojit** , zobrazí se výzva k otevření souboru RDP. Otevřete soubor a postupujte podle pokynů. (Se může taky tento soubor uložit do místního počítače a spusťte soubor poklepáním na remote Log in to přihlásit virtuálního počítače aniž by bylo nutné nejdřív přejděte na portál správy.)

    * Když se zobrazí výzva pro uživatelské jméno a heslo, zadejte hodnoty, které jste zadali vzdáleného uživatele a budou moct přihlásit.

> [AZURE.NOTE] Pokud používáte operační systém není Windows, musíte používat vzdálené plochy klienta, který je kompatibilní s operačním systémem a postupujte podle pokynů ke konfiguraci klientských s nastavením v RDP souboru, který jste stáhli.

## <a name="see-also"></a>Viz taky

[Azure sada nástrojů pro Eclipse][]

[Vytvoření prezentace aplikace Ahoj pro Azure v zatmění][]

[Instalace Azure sada nástrojů pro Eclipse][] 

Další informace o používání Azure pomocí jazyka Java naleznete v článku [Středisko pro vývojáře Java Azure][].

<!-- URL List -->

[Středisko pro vývojáře Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Vytvoření prezentace aplikace Ahoj pro Azure v zatmění]: http://go.microsoft.com/fwlink/?LinkID=699533
[Instalace Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png

<properties
    pageTitle="Vytvoření Ahoj světě Cloudová služba pro Azure v zatmění"
    description="Zjistěte, jak vytvořit jednoduchý Vítáme aplikace pomocí nástrojů Azure pro Eclipse."
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

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690944.aspx -->

# <a name="create-a-hello-world-cloud-service-for-azure-in-eclipse"></a>Vytvoření Ahoj světě Cloudová služba pro Azure v zatmění #

Podle těchto kroků vidíte, jak vytvářet a nasazení aplikace základní JSP Azure pomocí nástrojů Azure pro Eclipse. Příklad JSP zobrazený pro zjednodušení, ale velmi podobným způsobem vhodné pro Java servlet jde Azure nasazení.

Aplikace bude vypadat podobně jako tento:

![][ic600360]

## <a name="prerequisites"></a>Zjistit předpoklady pro ##

* Java vývojář Kit (JDK), v 1.7 nebo novější.
* -Zatmění integrovaném vývojovém prostředí pro vývojáře í Java džínovinu nebo novější. Můžete stáhnout z <http://www.eclipse.org/downloads/>.
* Rozdělení na základě Java webový server nebo aplikační server, například Apache Tomcat, GlassFish, JBoss aplikační Server, molo nebo IBM® WebSphere® aplikace Server Svoboda Core.
* Azure předplatné, které lze získat z <http://azure.microsoft.com/pricing/purchase-options/>.
* Azure sada nástrojů pro Eclipse. Další informace najdete v tématu [instalace Azure sada nástrojů pro Eclipse][].

## <a name="to-create-a-hello-world-application"></a>Vytvoření aplikace Vítáme ##

Nejdřív budete Začneme s vytvářením Java projektu.

*  Zahájení zatmění, v nabídce klikněte na **soubor**, klikněte na **Nový**a klikněte na **Dynamické Project Web**. (Pokud se nezobrazuje **Dynamické Project Web** uvedené jako projektového plánu dostupné po kliknutí na **soubor**, **Nový**, proveďte následující kroky: klikněte na **soubor**, klikněte na **Nový**, klikněte na **projekt...**, rozbalte **Web**, klikněte na **Dynamické Project Web**a klikněte na tlačítko **Další**.)
*  Pro účely tohoto kurzu zadejte název projektu **MyHelloWorld**. (Zajistit použijete tento název, následující kroky v tomto kurzu očekávat souboru WAR se jménem MyHelloWorld). Obrazovce bude vypadat takto: ![][ic589576]
* Klikněte na **Dokončit**.
* V zobrazení na zatmění Průzkumník projektu rozbalte **MyHelloWorld**. Klikněte pravým tlačítkem na **obsah webu**, klikněte na **Nový**a potom klikněte na **Soubor JSP**.
* V dialogovém okně **Nový soubor JSP** zadejte název souboru **index.jsp**. Zachovat nadřazenou složku jako **MyHelloWorld/obsah webu**, jak je vidět v následujících tématech:  ![][ic659262]
* V dialogovém okně **Vybrat šablonu JSP** pro účely tohoto kurzu vyberte **Nový soubor JSP (html)** a klikněte na tlačítko **Dokončit**.
* Při otevření souboru index.jsp v zatmění přidejte v dynamicky zobrazený text **Vítáme!** ve stávající `<body>` prvek. Vaše aktualizované `<body>` obsah objevit jako následující:
```
    <body>
    <b><% out.println("Hello World!"); %></b>
    </body>
```
* Uložení index.jsp.

## <a name="to-deploy-your-application-to-azure-the-quick-and-simple-way"></a>Nasazení aplikace Azure, rychlý a jednoduchý způsob ##

Jakmile jste ještě webovou aplikaci Java li otestovat, můžete následující zkratku te102824457 na vašem přímo na Azure cloudu.

1. V prvku zatmění Průzkumník projektu klikněte na **MyHelloWorld**.
1. Na panelu nástrojů zatmění klikněte na **Publikovat** rozevírací tlačítko a potom klikněte na **Publikovat jako Azure cloudové služby**
    ![][publishDropdownButton]
1. Pokud jste nevytvořili Azure nasazení projektu pro tuto aplikaci před publikujete tuto aplikaci Azure poprvé, projekt Azure nasazení se vytvoří automaticky. Měli byste vidět na následující dotaz, které je taky uvedené balíček JDK a aplikační server, který má být nasazené automaticky ke spuštění aplikace.
    ![][ic789598]

    Tento přístup místní umožňuje rychlý a snadný způsob otestovat aplikace v Azure, aniž byste museli konfigurace konkrétní server nebo JDK, která se liší od výchozího nastavení. Pokud jste s výchozím nastavením spokojení, klikněte na tlačítko **OK** pokračujte pomocí následujícího postupu.
    Ale pokud chcete změnit JDK nebo aplikační server pro účely aplikace, můžete to udělat později úpravou Azure nasazení projektu, který se automaticky vytvoří, nebo klepněte na tlačítko **Storno** a přečtěte si **o Azure nasazení projekty části** tohoto kurzu.
1. V dialogovém okně **Publikovat Azure** :
    1. Pokud jsou bez předplatných vyberte v seznamu **předplatné** ještě, importovat informace o předplatném takto:
        1. Klikněte na **Importovat ze souboru nastavení publikování**.
        1. V dialogovém okně **Informace o předplatném importovat** klikněte na **Stáhnout soubor nastavení publikování**. Pokud si nejste ještě přihlášení k účtu Azure, zobrazí se výzva k přihlášení. Pak budete vyzváni k uložení Azure publikování souboru nastavení. Si ji uložte do místního počítače.
        1. V dialogovém okně **Informace o předplatném Import** klikněte na tlačítko **Procházet** , vyberte soubor nastavení publikování, kterou jste uložili v předchozím kroku místně a klepněte na tlačítko **Otevřít**. Na obrazovce by měla vypadat přibližně takto: ![][ic644267]
        1. Klikněte na **OK**.
    1. **Předplatné**vyberte předplatné, které chcete použít pro nasazení.
    1. **Úložiště účet**vyberte úložiště účet, který chcete použít nebo klikněte na **Nový** vytvořte nový účet úložiště.
    1. **Název služby**vyberte cloudové služby, který chcete použít nebo klikněte na **Nový** k vytvoření nové cloudové služby.
    1. U **Cílové OS**vyberte verze operačního systému, který chcete použít pro nasazení.
    1. U **cílové prostředí**pro účely tohoto kurzu, vyberte **pracovní**. (Pokud budete chtít nasadit na web výrobní, změníte takto **výroby**.)
    1. Volitelné: Ujistěte se, že **přepíše předchozí nasazení** je zaškrtnuté políčko požadovaná nové nasazení automaticky přepsat předchozí nasazení. Pokud povolíte tuto možnost, není prostředí "409 konflikt" problémy při publikování na stejné místo.
        Všimněte si, že dialogové okno **Publikovat Azure** obsahuje oddíl pro **Vzdáleného přístupu**. Ve výchozím nastavení není povolený vzdáleného přístupu a společnost Microsoft nepovolí v tomto příkladu. Povolení vzdáleného přístupu, který byste zadali uživatelské jméno a heslo pro použití při vzdáleně přihlášení. Další informace o vzdáleného přístupu najdete v tématu [Povolení vzdáleného přístupu k nasazení Azure v zatmění][].
        Dialogové okno **Publikovat Azure** zobrazí podobná této: ![][ic719488]
1. Klepnutí na tlačítko **Publikovat** do pracovním prostředí.
    Po zobrazení výzvy k provedení úplné Tvůrce dotazů, klikněte na **Ano**. To může trvat několik minut, než první Tvůrce dotazů.
    **Protokol činnosti Azure** se zobrazí v části zobrazení zatmění na záložkách.
    ![][ic719489]
   Můžete tento protokol, stejně jako zobrazení **konzoly** průběh nasazení se zobrazí. Další možností je a přihlaste se k [Portálu Správa Azure][], část **Cloudovým službám** slouží ke sledování stavu.
1. Po úspěšném nasazení nasazení **Protokolu činnosti Azure** se zobrazí stav **Publikováno**. Klepněte na možnost **Publikováno**, jak je znázorněno na následujícím obrázku a prohlížeči se otevře instanci nasazení.
    ![][ic719490]

Protože to byl nasazení pracovní prostředí, název DNS bude ve tvaru http://&lt;*guid*&gt;. cloudapp.net a adresa URL bude obsahovat název DNS a UPN pro aplikace. Například http://447564652c20426f6220526f636b7321.cloudapp.net/MyHelloWorld. (Rozlišuje části **MyHelloWorld** .) Můžete taky zobrazí název DNS Pokud kliknete na název nasazení v portálu pro správu platformy Azure (v části Cloudovým službám portálu pro správu).

I když tento walk-through byl nasazení pracovní prostředí, nasazení výroby spočívá stejným způsobem, kromě možnosti v dialogovém okně **Publikovat Azure** , vyberte **výrobní** místo **přípravu** **cílové prostředí**. Nasazení výrobní výsledky v adrese URL podle názvu DNS podle svého výběru, místo GUID používané pro pracovní.

>[AZURE.WARNING] V tomto okamžiku nasadili Azure aplikace do cloudu. Však před pokračováním uvědomíte, že nasazeném aplikace, i když není spuštěná, budou pro metodu nabíhání nákladů času pro vaše předplatné k fakturaci. Proto je velice důležité odstranit nežádoucí nasazení z Azure předplatného.

## <a name="about-azure-deployment-projects"></a>O projektech Azure nasazení ##

Abyste mohli nasadit jeden nebo více aplikací Java na Azure, je potřeba projektu nasazení Azure. Přehrání role "balíček", že aplikace nutné omotané do k publikování v Azure.

Kromě informace o používání aplikací projekt Azure nasazení obsahuje také informace o dalších klíčové součásti nasazení, především: kontejneru aplikace serveru spustit webovou aplikaci v a modulu runtime Java ho spusťte. Azure podporuje velké výběr Java runtimes a Java aplikace serverech, mezi nimiž můžete vybírat.

I když výrazně zjednodušit příklad zde použitý pro větší názornost projekt Azure nasazení mohou také obsahovat další důležité informace o konfiguraci, který umožňuje vytvořit téměř libovolně složité scalable, vysoce dostupné, vícevrstvého cloudovým službám s aplikacemi. Můžete povolit **relace spřažení ("rychlé relace")**, **rychlé ukládání do mezipaměti**, **vzdálené ladění**, **Odstranění SSL**, **portu brány firewall/směrování**, **vzdálený přístup**a řadu dalších výkonných funkcí.

Pokud jste nedokončili předchozím oddíle tohoto kurzu ("k aplikaci nasadit Azure, rychlý a jednoduchý způsob"), zobrazí nový projekt Azure nasazení Prohlížeč projektu za vás automaticky generované a s názvem "**MyHelloWorld_onAzure**".

Může taky začnete tento kurz nejdřív vytvořením projekt prázdné Azure nasazení sami a přidání aplikací na něj. Je delší proces, ale umožňuje větší kontrolu nad nastavením počáteční od začátku.

Vytvoření úplně nového projektu Azure nasazení, klikněte na tlačítko **Nový projekt nasazení Azure** ![][ic710876].

Bez ohledu na to, zda práce s už existujícím Azure nasazení projektu, nebo vytvořit od začátku, si můžete změnit jeho nastavení a součásti, například JDK nebo aplikační server rovnoměrně z vnější kdykoli jednoduše.

Chcete-li změnit JDK, nebo aplikační server nebo v seznamu aplikací v existujícím projektu Azure nasazení:

1. Rozbalte uzel projektu (například **MyHelloWorld_onAzure**) v okně Průzkumník projektu
2. Klikněte pravým tlačítkem myši **WorkerRole1**
3. Rozbalení podnabídky **Azure** v místní nabídce
4. Klikněte na tlačítko **Konfigurace serveru**

Bez ohledu na tom, jestli je vytvořená tyto kroky konfigurace serveru úpravou existujícího projektu Azure nasazení uvedené výše nebo vytvoření nového od začátku, zobrazí se stejného typu umožňuje konfigurace JDK, serveru a aplikace součásti dialogová okna. Další informace o nastavení v těchto dialogových oknech, například změnit JDK aplikační server a přidání nebo odebrání aplikací v nasazení, najdete v článku [Konfigurace vlastností serveru][] .

## <a name="windows-only-to-deploy-your-application-to-the-compute-emulator"></a>Pouze ve Windows: nasazení aplikace emulátoru výpočetním ##

>[AZURE.NOTE] Azure emulátoru je k dispozici pouze v systému Windows. Tuto část přeskočte, pokud používáte operační systém než Windows.

Pokud jste vytvořili nový projekt Azure nasazení kroků popsaných výše, tedy implicitně publikováním aplikaci Azure, JDK a aplikace servery nakonfigurovány cloudu, ale ne místní emulace. Příprava projektu pro účely testování v místním emulátoru, postupujte takto:

1. V prvku zatmění Průzkumník projektu klikněte na **MyHelloWorld_onAzure**.
1. Klikněte pravým tlačítkem myši na **WorkerRole1**.
1. Rozbalte podnabídky **Azure** v místní nabídce.
1. Klikněte na **Nastavení serveru**.
1. Na kartě **JDK** zaškrtněte, pokud tato sada předem nakonfiguroval výchozí místní JDK za vás. Pokud není nebo pokud chcete změnit předpokládaný výchozí nastavení, ujistěte se, že je zaškrtnuté políčko **použít JDK z cesta k souboru pro účely testování místně** a není zadán umístění instalace JDK, který chcete použít. Pokud chcete změnit, klikněte na tlačítko **Procházet** a pomocí ovládacího prvku procházet, vyberte umístění adresáře JDK používat.
1. Klikněte na kartu **Server** .
1. Do textového pole **Cesta místního serveru** v dolní části dialogového okna zadejte cestu místně nainstalované serveru, která odpovídá typu a hlavní číslo verze serveru vybraný v horní části dialogu klikněte v části zaškrtávací políčko **nasazení serveru tohoto typu** . Pokud chcete použít jiný typ nebo hlavní verzi aplikační server, výběr v části tohoto zaškrtávacího políčka nejdřív změňte.
1. Klikněte na **OK**.
1. Na panelu nástrojů zatmění klikněte na tlačítko **Spustit v Azure emulátoru** ![][ic710879]. Tlačítko **Spustit v Azure emulátoru** není povolená, vyberte **MyHelloWorld_onAzure** v prvku zatmění Průzkumník projektu a zajistit, aby Průzkumník projektu na zatmění fokus jako aktuálním okně. Nejdřív to spustí úplné sestavení projektu a potom spuštění webové aplikace Java ve výpočetním emulátoru. (Poznámka: v závislosti na vašem počítači výkonu vlastnostech první sestavení může trvat mezi několik sekund, než několik minut, že následující vytvoří bude rychlejší.) Po dokončení cílem prvního kroku sestavení zobrazí se výzva tak, že Windows účtu řízení Uživatelských účtů povolit tento příkaz provádět změny se svým počítačem. Klikněte na **Ano**.

>[AZURE.IMPORTANT] Pokud se nezobrazí výzva nástroje Řízení uživatelských účtů, zkontrolujte na hlavním panelu Windows na ikonu nástroje Řízení uživatelských účtů a klikněte na ni nejdřív. Někdy Uživatelských výzva nezobrazuje jako horní okno, ale je viditelné pouze jako ikona na hlavním panelu.

1. Prohlédněte si výstup emulátoru výpočetním uživatelského rozhraní pro zjistit, zda jsou problémy s projektem. V závislosti na obsah nasazení může trvat několik minut aplikace jej lze spustit plně ve výpočetním emulátoru.
1. Spusťte prohlížeč a použijte adresu URL `http://localhost:8080/MyHelloWorld` jako adresa ( `MyHelloWorld` část adresy URL, je velká a malá písmena). Měli byste vidět aplikace MyHelloWorld (výstup index.jsp), vidíte na následujícím obrázku: ![][ic589579]

Když budete chtít přestat aplikace v aplikaci výpočetním emulátoru na panelu nástrojů zatmění klikněte na tlačítko **Obnovit emulátoru Azure** ![][ic710880].

## <a name="to-delete-your-deployment"></a>Chcete-li odstranit nasazení ##

Odstranit nasazením v Azure sada nástrojů pro Eclipse, vyberte **MyHelloWorld_onAzure** v prvku zatmění Průzkumník projektu, a zajistit, aby Průzkumník projektu zatmění má aktuálním okně přizpůsobte a potom klikněte na tlačítko **Zrušit publikování** ![][ic710883], na panelu nástrojů zatmění. (Může provedete stejnou operaci tak, že kliknete pravým tlačítkem myši **MyHelloWorld_onAzure** v prvku zatmění Průzkumník projektu, kliknutím **Azure** a potom na **Undeploy z cloudu Azure**.) Zobrazí se dialogové okno **Zrušit publikování Azure projekt** .

![][ic719491]

Vyberte předplatné a cloudu službu, která obsahuje nasazení, vyberte nasazení, který chcete odstranit a klikněte na **Zrušit publikování**.

(Alternativa k použití této sady odstranit nasazení, je použít části **Cloudové služby** na portálu Správa Azure: přejděte na nasazení, vyberte ho a klikněte na tlačítko **Odstranit** . To zastavte a odstraňte nasazení. Pokud pouze nechcete nasazení a neodstraníte ho, klikněte na tlačítko **Zastavit** místo na tlačítko **Odstranit** , ale výše uvedené, pokud neodstraníte nasazení, fakturaci poplatky zůstanou metodu nabíhání nákladů pro nasazení, i když je zastaveno.)

## <a name="see-also"></a>Viz taky ##

[Azure sada nástrojů pro Eclipse][]

[Instalace Azure sada nástrojů pro Eclipse][] 

[Co je nového v Azure sada nástrojů pro Eclipse][]

Další informace o používání Azure pomocí jazyka Java naleznete v článku [Středisko pro vývojáře Java Azure][].

<!-- URL List -->

[Středisko pro vývojáře Java Azure]: http://go.microsoft.com/fwlink/?LinkID=699547
[Portál Správa Azure]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Role Properties]: http://go.microsoft.com/fwlink/?LinkID=699525
[Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Povolení vzdáleného přístupu k Azure nasazení v zatmění]: http://go.microsoft.com/fwlink/?LinkID=699538
[Instalace Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Vlastnosti konfigurace serveru]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[Co je nového v Azure sada nástrojů pro Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic589576]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589576.png
[ic589579]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589579.png
[ic600360]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic600360.png
[ic644267]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic644267.png
[ic659262]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic659262.png
[ic710876]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710876.png
[ic710879]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710880.png
[ic710882]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710883.png
[ic719488]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719488.png
[ic719489]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719489.png
[ic719490]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719490.png
[ic719491]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719491.png
[ic789598]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic789598.png
[publishDropdownButton]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/publishDropdownButton.png

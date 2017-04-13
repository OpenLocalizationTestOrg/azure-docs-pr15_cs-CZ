<properties
   pageTitle="Publikování do cloudové služby používat nástroje Azure | Microsoft Azure"
   description="Informace o tom, jak publikovat projekty služby Azure cloudu pomocí aplikace Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publishing-a-cloud-service-using-the-azure-tools"></a>Publikování do cloudové služby používat nástroje Azure

Pomocí nástroje Azure pro Microsoft Visual Studio můžete publikovat Azure aplikace přímo z aplikace Visual Studio. Visual Studio podporuje integrované publikování pracovní nebo provozním prostředí do cloudové služby.

Musíte před publikováním aplikace Azure, musíte mít předplatné Azure. Cloudové služby a úložiště účet musí taky nastavit pro použití v aplikaci. Tyto můžete nastavit v [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885).

>[AZURE.IMPORTANT] Při publikování, můžete vybrat prostředí nasazení cloudové službě. Je taky musíte vybrat úložiště účet, který se používá k ukládání balíčku aplikace pro nasazení. Po nasazení balíčku aplikace zmizí z účtu úložiště.

Když vývoji a testování Azure aplikace můžete nasazení webu publikování změn postupně pro váš web role. Po publikování aplikace pro nasazení prostředí nasazení webu můžete nasadit změny přímo do virtuálního počítače, na kterém běží roli web. Nemáte balíček a publikovat celé Azure aplikace pokaždé, když chcete aktualizovat svoji roli web otestovat změny. Tento přístup můžete mít změny role webu k dispozici v cloudu pro testování bez nutnosti čekání mít aplikace publikované na prostředí nasazení.

Publikovat Azure aplikace a k aktualizaci roli web pomocí nasazení webu, postupujte následovně:

- Publikování nebo balíček Azure aplikace Visual Studio

- Aktualizovat webové roli v rámci vývoje a testování obrázku

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Publikování nebo balíček aplikace Azure z aplikace Visual Studio

Při publikování Azure aplikace můžete udělat jednu z následujících úloh:

- Vytvoření balíčku služby: můžete balíček a konfiguračního souboru služby publikování aplikace pro nasazení prostředí z [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885).

- Publikování Azure projektu z aplikace Visual Studio: publikování aplikace přímo do Azure, použijte Průvodce Publikovat. Informace najdete v tématu [Publikování průvodce aplikací Azure](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-create-a-service-package-from-visual-studio"></a>Vytvoření balíčku služby z aplikace Visual Studio

1. Jakmile budete připraveni publikovat aplikace, otevřete Průzkumníka řešení, otevřete místní nabídky pro Azure projekt, který obsahuje vaše role a vyberte publikovat.

1. Pokud chcete vytvořit sadu služby, postupujte takto:  

  1. V místní nabídce Azure projektu vyberte **balíček**.

  1. V dialogovém okně **Balíček aplikace Azure** zvolte konfiguraci služby, u kterého chcete vytvořit balíček a zvolte konfiguraci sestavení.

  1. (volitelné) Zapnout vzdálené plochy ke cloudové službě po publikování, zaškrtněte políčko **Povolit ke vzdálené ploše pro všechny role** a potom vyberte **Nastavení** a konfigurace vzdálené plochy. Pokud chcete po publikování ladění cloudové služby, zapněte možnost vzdálené ladění tak, že vyberete **Povolit vzdálené ladění pro všechny role**.

      Další informace najdete v tématu [Pomocí vzdálené plochy s rolemi Azure](vs-azure-tools-remote-desktop-roles.md).

  1. Pokud chcete vytvořit balíček, zvolte odkaz **balíčku** .

      Průzkumník souborů zobrazuje umístění souboru nově vytvořený soubor. Toto umístění můžete zkopírovat tak, že můžete z [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885).

  1. Publikovat balíček pro nasazení prostředí, je nutné použít toto umístění jako umístění balíčku při vytváření Cloudová služba a tento balíček prostředí s [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Volitelné) Chcete-li zrušit procesu nasazení v místní nabídce pro položku řádku protokol aktivity v vyberte **zrušit a odebrat**. Ukončení procesu nasazení a odstraní prostředí nasazení z Azure.

    >[AZURE.NOTE] Po byly nasazeny odebrat toto prostředí nasazení, je nutné použít [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Volitelné) Po role instance jste zahájili, Visual Studio automaticky se zobrazuje nasazení prostředí v Průzkumníku serveru uzlu **Cloudovým službám** . Tady můžete zobrazit stav instancí jednotlivé role. V tématu [Správa Azure zdroje v Průzkumníkovi cloudu](vs-azure-tools-resources-managing-with-cloud-explorer.md). Následující obrázek ukazuje role instance době, kdy je stále ve stavu Probíhá inicializace:

    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-the-development-and-testing-cycle"></a>Aktualizovat webové roli v rámci vývoje a testování obrázku

Pokud je vaše aplikace back-end infrastruktura stabilní, ale role web třeba více často aktualizovat, můžete nasazení webu aktualizovat jenom roli web projektu. To je užitečné, pokud nechcete znovu vytvořit a přeinstalujte role pracovníka back-end nebo pokud máte víc role web a chcete aktualizovat jenom jedna z role web.

### <a name="requirements"></a>Požadavky

Tady jsou požadavky na používání nasazení webu aktualizovat svoji roli web:

- **Pouze pro vývoj a testování účely:** Změnám přímo k počítači virtuální kterém běží roli web. Pokud tento virtuální počítač má být z koše, změny budou ztraceny, protože původní balíček, který jste publikovali slouží k znovu vytvořit virtuální počítač rolí. Musíte znovu publikujte aplikaci zobrazíte posledním změnám pro roli web.

- **Lze aktualizovat jenom web role:** Role pracovníka nelze aktualizovat. Kromě toho nelze aktualizovat RoleEntryPoint v web role.cs.

- **Podporuje pouze jeden výskyt roli web:** Nesmí obsahovat více instancí libovolný web role v prostředí pro nasazení. Ale podporuje více web rolí každý s jedinou instance.

- **Musíte povolit připojení ke vzdálené ploše:** To je nutné, aby nasazení webu mohli používat uživatele a heslo pro připojení k virtuální počítač nasadit změny na server, na kterém běží služba (internetové). Kromě toho budete muset připojit k přidání důvěryhodnou certifikační služby IIS na tomto počítači virtuální virtuální počítač. (Zajistíte, že je připojení ke vzdálené služby IIS používanou nasazení webu bezpečné.)

Následující postup předpokládá, že používáte průvodce **Publikování aplikací Azure** .

### <a name="to-enable-web-deploy-when-you-publish-your-application"></a>Chcete-li povolit nasazení webu při publikování aplikace

1. Povolit **Povolit nasazení webu** web role políčko Vše, musíte nejdřív nakonfigurovat připojení ke vzdálené ploše. Vyberte **Povolit ke vzdálené ploše** pro všechny role a potom zadejte přihlašovací údaje, které se použije k propojení vzdáleně v zobrazeném dialogovém okně **Konfigurace vzdálené plochy** . Další informace najdete v článku [Pomocí vzdálené plochy s rolemi Azure](vs-azure-tools-remote-desktop-roles.md) .

1. Pokud chcete povolit nasazení webu pro všechny role webové aplikace, zaškrtněte **Povolit nasazení webu pro všechny role web**.

    Zobrazí se žlutým trojúhelníkovým upozorněním. Nasazení web používá nedůvěryhodných, podepsaného certifikát ve výchozím nastavení se nedoporučuje nahrávání citlivá data. Pokud potřebujete zabezpečené tohoto postupu pro citlivá data, můžete přidat certifikát SSL pro nasazení webu připojení. Certifikát musí být důvěryhodný certifikát. Informace o tom, jak to udělat naleznete v části **Chcete provést Web nasazení zabezpečené** dál v tomto tématu.

1. Zvolte **Další** zobrazíte obrazovce **Souhrn** a klikněte na **Publikovat** nasazení cloudovou službu.

    Cloudové služby je publikován. Virtuální počítač, který se má vzdáleného připojení pro službu IIS tak, aby nasazení webu slouží k aktualizaci webové rolí znovu publikovat je.

    >[AZURE.NOTE] Pokud máte víc než jednou instancí nakonfigurován pro web roli, zobrazí se zpráva s upozorněním, informacemi o tom, aby jednotlivé role web se omezí na jedna instance pouze v okně balíček, která se vytvoří publikovat aplikace. Pokračujte výběrem **OK** . Jak je uvedeno v oddílu požadavky, může mít více než jednu webovou roli ale jenom jedna instance každou roli.

### <a name="to-update-your-web-role-by-using-web-deploy"></a>Aktualizovat svoji roli Web pomocí nasazení webu

1. Můžete nasadit Web měnit kódu projectu pro některý web role ve Visual Studiu, který chcete publikovat a potom klikněte pravým tlačítkem myši tohoto projektu v řešení a přejděte na příkaz **Publikovat**. Zobrazí se dialogové okno **Publikovat Web** .

1. (Volitelné) Pokud jste přidali důvěryhodných certifikát SSL pro připojení ke vzdálené služby IIS, můžete zrušte zaškrtnutí políčka **Povolit nedůvěryhodných certifikát** . Informace o tom, jak přidat certifikát pro nasazení webu zabezpečení naleznete v části **Na Vytvořit Web nasazení zabezpečené** dál v tomto tématu.

1. Můžete nasadit Web publikování mechanismus potřebuje uživatelské jméno a heslo, které jste nastavili pro vaše připojení ke vzdálené ploše při prvním publikování balíčku.

  1. Do pole **uživatelské jméno**zadejte uživatelské jméno.

  1. Do pole **heslo**zadejte heslo.

  1. (Volitelné) Pokud chcete uložit toto heslo v tomto profilu, vyberte **Uložit heslo**.

1. Publikování změn vaše role web, vyberte **Publikovat**.

    Stavový řádek zobrazuje **začít publikovat**. Po dokončení publikování **úspěšně publikovat** se zobrazí. Změny již byly nasazeny teď k roli web ve vašem počítači virtuální. Nyní můžete začít Azure aplikace v Azure prostředí otestovat provedené změny.

### <a name="to-make-web-deploy-secure"></a>Chcete-li zabezpečit Web nasazení

1. Nasazení web používá nedůvěryhodných, podepsaného certifikát ve výchozím nastavení se nedoporučuje nahrávání citlivá data. Pokud potřebujete zabezpečené tohoto postupu pro citlivá data, můžete přidat certifikát SSL pro nasazení webu připojení. Certifikát musí být důvěryhodný certifikát, kterou můžete získat od certifikační autorita (CA).

    Pro nasazení webu zabezpečení pro každý počítač virtuální pro každý web role musí nahrát důvěryhodných certifikát, který chcete použít pro web nasazení [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885). Zajistíte tím, že certifikát je přidán do virtuálního počítače, která se vytvoří web rolí při publikování aplikace.

1. Přidání důvěryhodného certifikát SSL služby IIS pro účely vzdáleného připojení, postupujte takto:

  1. Připojení k virtuálního počítače, na kterém běží roli web, vyberte instanci systému roli webu v **Průzkumníkovi cloudu** nebo **Průzkumník serveru**a klikněte na příkaz **Připojit pomocí vzdálené plochy** . Podrobnosti o tom, jak připojit k počítači, virtuální najdete v tématu [Pomocí vzdálené plochy s rolemi Azure](vs-azure-tools-remote-desktop-roles.md).

      Prohlížeče zobrazí výzvu ke stažení. Soubor RDP.

  1. Pokud chcete přidat certifikát SSL, otevřete službu pro správu ve Správci služby IIS. Ve Správci služby IIS povolte SSL otevřením propojení **vazeb** v podokně **akcí** . Zobrazí se dialogové okno **Přidat vazbu webu** . Zvolte **Přidat**a pak zvolte HTTPS v rozevíracím seznamu **Typ** . V seznamu **certifikát SSL** vyberte certifikát SSL, aby obsahoval podepsáno certifikační Autority a nahrát na [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885). Další informace najdete v tématu [Konfigurace nastavení připojení pro službu pro správu](http://go.microsoft.com/fwlink/?LinkId=215824).

      >[AZURE.NOTE] Pokud přidáte důvěryhodných certifikát SSL žlutým trojúhelníkovým upozorněním již nezobrazoval v **Průvodce publikování**.

## <a name="include-files-in-the-service-package"></a>Zahrnout soubory balíčku služby

Možná budete muset zahrnout konkrétní soubory do balíčku služby tak, aby byly k dispozici v počítači virtuální, která se vytvoří pro roli. Můžete například přidat .exe nebo soubor .msi, který se používá skriptem spuštění balíčku služby. Nebo zřejmě nutné přidat sestavení vyžadovaného projekt webové role nebo pracovníka role. Zahrnout souborů musí být přidán do řešení Azure aplikace.

### <a name="to-include-files-in-the-service-package"></a>Zahrnout souborů do balíčku služby

1. Přidání sestavení do balíčku služby, pomocí následujících kroků:

  1. V **Okně Průzkumník** rozbalte uzel projektu pro projekt, který chybí odkazované sestavení.

  1. Sestavení do projektu přidáte otevřete místní nabídky pro složku **odkazy** a pak zvolte **Přidat odkaz**. Zobrazí se dialogové okno Přidat odkaz.

  1. Vyberte odkaz, který chcete přidat a potom klikněte na tlačítko **OK** .

      Odkaz se přidá do seznamu ve složce **odkazy** .

  1. Otevření místní nabídky pro sestavení, které jste přidali a zvolte **Vlastnosti**. Zobrazí se okno **Vlastnosti** .

      Zahrnout toto sestavení v balíčku služby v **místní kopii seznamu** vyberte **hodnotu True**.

1. V **Okně Průzkumník** rozbalte uzel projektu pro projekt, který chybí odkazované sestavení.

1. Sestavení do projektu přidáte otevřete místní nabídky pro složku **odkazy** a pak zvolte **Přidat odkaz**. Zobrazí se dialogové okno **Přidat odkaz** .

1. Vyberte odkaz, který chcete přidat a potom klikněte na tlačítko **OK** .

    Odkaz se přidá do seznamu ve složce **odkazy** .

1. Otevření místní nabídky pro sestavení, které jste přidali a zvolte **Vlastnosti**. Zobrazí se okno Vlastnosti.

1. Zahrnout toto sestavení balíček služby v seznamu **Místní kopie** vyberte **hodnotu True**.

1. Zahrnout soubory služby balíček, který jste přidali do projektu role web, otevřete v místní nabídce souboru a klikněte na **Vlastnosti**. V okně **Vlastnosti** vyberte **obsahu** v seznamu **Akce sestavení** .

1. Zahrnout balíček služby, který jste přidali do projektu role pracovní soubory, otevřete v místní nabídce souboru a klikněte na **Vlastnosti**. V okně **Vlastnosti** vyberte **kopírování: když novější** ze seznamu **Kopírovat do výstupní adresář** .

## <a name="next-steps"></a>Další kroky

Další informace o publikování Azure z aplikace Visual Studio najdete v tématu [Publikování průvodce aplikací Azure](vs-azure-tools-publish-azure-application-wizard.md).

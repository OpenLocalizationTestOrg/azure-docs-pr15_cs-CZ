<properties
   pageTitle="Jak spravovat konfigurace služeb a profily | Microsoft Azure"
   description="Informace o práci se soubory služby konfigurace a profily konfigurace | které uložení nastavení pro nasazení prostředí a nastavení publikování pro cloudovým službám."
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

# <a name="how-to-manage-service-configurations-and-profiles"></a>Jak spravovat konfigurace služeb a profily

## <a name="overview"></a>Základní informace

Při publikování do cloudové služby Visual Studio ukládá informace o konfiguraci dva typy souborů konfigurace: služby konfigurace a profily. Konfigurace služeb (.cscfg soubory) obsahují nastavení prostředí nasazení služby Azure cloudu. Azure tyto soubory konfigurace používá při správě cloudovým službám. Nastavení služby cloudu publikování na druhé straně úložiště profily (soubory .azurePubxml). Toto nastavení je záznam co si zvolíte při použití Průvodce Publikovat a používají místně Visual Studio. Toto téma vysvětluje, jak pracovat s obou typů souborů konfigurace.

## <a name="service-configurations"></a>Konfigurace služeb

Můžete vytvořit více konfigurace služeb můžete u všech prostředí nasazení. Můžete například vytvořit konfiguraci služby pro místním prostředí, které používáte pro spustit a otestovat Azure aplikace a další konfiguraci služby pro provozním prostředí.

Můžete přidat, odstranit, přejmenovat a změnit tyto konfigurace služeb svým požadavkům. Můžete spravovat tyto konfigurace služeb z aplikace Visual Studio, jak je znázorněno na následujícím obrázku.

![Správa konfigurace služeb](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-service-config.png)

Dialogové okno **Spravovat konfigurace** můžete otevřít taky ze stránky vlastností role. Otevřete dialogové okno Vlastnosti pro roli v Azure projektu, otevřete místní nabídku pro tuto roli a klikněte na **Vlastnosti**. Na kartě **Nastavení** rozbalte seznam **Konfiguraci služby** a potom vyberte **Spravovat** otevřete dialogové okno **Spravovat konfigurace** .

### <a name="to-add-a-service-configuration"></a>Chcete-li přidat konfiguraci služby

1. V okně Průzkumník otevření místní nabídky pro Azure projektu a potom vyberte **Spravovat konfigurace**.

    Zobrazí se dialogové okno **Spravovat konfigurace služeb** .

1. Přidat konfiguraci služby, musíte vytvořit kopii existující konfigurace. K tomuto účelu vyberte konfigurace, který chcete zkopírovat ze seznamu název a pak vyberte **vytvořit kopii**.

1. (Volitelné) Chcete, aby konfiguraci služby jiným názvem, zvolte nové konfiguraci služby ze seznamu název a pak vyberte **Přejmenovat**. Do textového pole **název** zadejte název, který chcete použít pro tuto konfiguraci služby a pak vyberte **OK**.

    Nový služby konfigurační soubor s názvem ServiceConfiguration. [Název] .cscfg se přidá do Azure projektu v Průzkumníku řešení.


### <a name="to-delete-a-service-configuration"></a>Chcete-li odstranit konfiguraci služby

1. V Průzkumníku otevřete místní nabídky pro Azure projektu a potom vyberte **Spravovat konfigurace**.

    Zobrazí se dialogové okno **Spravovat konfigurace služeb** .

1. Pro smazání konfiguraci služby vyberte konfiguraci, kterou chcete odstranit ze seznamu **název** a potom vyberte **Odstranit**. Zobrazí se dialogové okno pro ověření, že chcete odstranit tuto konfiguraci.

1. Vyberte **Odstranit**.

     Konfigurační soubor služby odebrali Azure projekt v Průzkumníku řešení.


### <a name="to-rename-a-service-configuration"></a>Chcete-li přejmenovat konfiguraci služby

1. V Průzkumníku otevřete místní nabídky pro Azure projektu a potom vyberte **Spravovat konfigurace**.

    Zobrazí se dialogové okno **Spravovat konfigurace služeb** .

1. Konfigurace služby přejmenovat, zvolte nové konfiguraci služby ze seznamu **název** a pak vyberte **Přejmenovat**. Do pole **název** zadejte název, který chcete použít pro tuto konfiguraci služby a pak vyberte **OK**.

    Název souboru konfigurace služby dojde ke změně v Azure projektu v Průzkumníku řešení.

### <a name="to-change-a-service-configuration"></a>Chcete-li změnit konfiguraci služby

- Pokud chcete změnit konfiguraci služby, otevřete místní nabídky pro konkrétní rolí, kterou chcete změnit v Azure projektu a potom vyberte **Vlastnosti**. V tématu [jak: Konfigurace role pro službu Azure cloudu pomocí aplikace Visual Studio](https://msdn.microsoft.com/library/azure/hh369931.aspx) Další informace.

## <a name="make-different-setting-combinations-by-using-profiles"></a>Zkontrolujte nastavení jiné kombinace pomocí profilů

Pomocí profilu můžete automaticky vyplnit **Průvodce publikováním** s různé kombinace nastavení pro jiné účely. Například máte pro ladění profilů a jiné pro verzi vytvoří. V takovém případě profilu **ladění** by měla **IntelliTrace** povolené a konfigurace **ladění** vybraná a profilu **vydání** by měla **IntelliTrace** zakázáno a konfigurace **vydání** vybraná. Můžete použít taky různé profily nasazení službu používáte účet jiné úložiště.

Když poprvé spustíte Průvodce se vytvoří výchozí profil. Visual Studio ukládá profilu soubor s příponou .azurePubXml, který je přidán do Azure projektu ve složce **profily** . Pokud ručně zadáte různé možnosti při spuštění Průvodce později, soubor automaticky aktualizuje. Před spuštěním následujícího postupu můžete by měl mít publikované cloudové služby aspoň jednou.

### <a name="to-add-a-profile"></a>Chcete-li přidat profilu

1. Otevření místní nabídky pro Azure projektu a vyberte **Publikovat**.

1. Vedle seznamu **cílový profil** klikněte na tlačítko **Uložit profil** , jak je znázorněno následujícím obrázku. Tím vytvoříte profilu za vás.

    ![Vytvoření nového profilu](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/create-new-profile.png)

1. Po vytvoření profil vyberte **< spravovat >** v seznamu na **cílový profil** .

    Zobrazí se dialogové okno **Spravovat profily** , jak ukazuje následující obrázek.

    ![Správa profilů dialogového okna](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-profiles.png)

1. V seznamu **název** zvolte profil a pak vyberte **Vytvořit kopii**.

1. Klikněte na tlačítko **Zavřít** .

    Nový profil se zobrazí v cílovém seznamu profilu.

1. V seznamu **cílový profil** vyberte profil, který jste právě vytvořili. Nastavení publikování průvodce se vyplní pomocí možností na profil, který jste vybrali.

1. Výběr tlačítka **předchozí** a **Další** zobrazíte každé stránce Průvodce publikováním a upravte nastavení profilu. Informace najdete v tématu [Publikování průvodce aplikací Azure](http://go.microsoft.com/fwlink/p/?LinkID=623085) .

1. Po dokončení úprav nastavení klikněte na tlačítko **Další** se vrátíte na stránku nastavení. Profil se uloží při publikování službu pomocí těchto nastavení nebo vyberete **Uložit** vedle seznamu profilů.

### <a name="to-rename-or-delete-a-profile"></a>Přejmenování nebo odstranění profilu

1. Otevření místní nabídky pro Azure projektu a vyberte **Publikovat**.

1. V seznamu **cílový profil** vyberte **Spravovat**.

1. V dialogovém okně **Spravovat profily** vyberte profil, který chcete odstranit a klikněte na **Odebrat**.

1. V potvrzovacím dialogu, která se zobrazí klikněte na **OK**.

1. Vyberte **Zavřít**.

### <a name="to-change-a-profile"></a>Chcete-li změnit profilu

1. Otevření místní nabídky pro Azure projektu a vyberte **Publikovat**.

1. V seznamu **cílový profil** vyberte profil, který chcete změnit.

1. Výběr tlačítka **předchozí** a **Další** zobrazíte každé stránce Průvodce publikování a změňte požadované nastavení. Informace najdete v tématu [Publikování průvodce aplikací Azure](http://go.microsoft.com/fwlink/p/?LinkID=623085) .

1. Po provedení změn nastavení klepnutím na tlačítko **Další** přejděte zpátky na stránku **Nastavení** .

1. (Volitelné) vyberte **Publikovat** publikovat cloudovou službu s použitím nové nastavení. Pokud nechcete, aby publikovat cloudové služby v současné době a zavřete průvodce publikováním, Visual Studio zeptá, jestli chcete uložit změny profilu.

## <a name="next-steps"></a>Další kroky

Další informace o konfiguraci jiné části Azure projektu z aplikace Visual Studio najdete v tématu [Konfigurace projekt Azure](http://go.microsoft.com/fwlink/p/?LinkID=623075)

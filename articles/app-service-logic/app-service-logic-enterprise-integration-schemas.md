<properties
    pageTitle="Základní informace o schémata a Enterprise integrace Pack | Microsoft Azure"
    description="Naučte se používat schémata s aplikacemi Enterprise integrace Pack a logika"
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="msftman"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="deonhe"/>

# <a name="learn-about-schemas-and-the-enterprise-integration-pack"></a>Další informace o schémata a Enterprise integrace Pack  

## <a name="why-use-a-schema"></a>Proč používat schéma?
Potvrďte, že jsou platné, s očekávaná data ve formátu předdefinované dokumentů XML, které dostáváte pomocí schémat. Schémata umožňují ověření zpráv, které jsou vyměňovat ve scénáři B2B.

## <a name="add-a-schema"></a>Přidání schématu
Z portálu Microsoft Azure:  

1. Vyberte **Další služby**.  
![Snímek obrazovky Azure portál se zvýrazněným "Další služby"](./media/app-service-logic-enterprise-integration-overview/overview-11.png)    

2. Do pole Hledat filtru zadejte **Integrace**a vyberte **Účty integrace** ze seznamu výsledků.     
![Snímek obrazovky vyhledávacího pole Filtr](./media/app-service-logic-enterprise-integration-overview/overview-21.png)  
3. Vyberte **účet integrace** , do kterého můžete přidávat schématu.    
![Snímek obrazovky s seznam účtů integrace](./media/app-service-logic-enterprise-integration-overview/overview-31.png)  

4. Vyberte dlaždici **schémat** .  
![Snímek obrazovky s IEP integrace účet s "Schémata" zvýrazněným](./media/app-service-logic-enterprise-integration-schemas/schema-11.png)  

### <a name="add-a-schema-file-less-than-2-mb"></a>Přidání souboru schématu menší než 2 MB  

1. V zásuvné **schémata** , která se otevře (předchozích kroků) vyberte **Přidat**.  
![Snímek obrazovky schémata zásuvné se zvýrazněným tlačítkem "Přidat"](./media/app-service-logic-enterprise-integration-schemas/schema-21.png)  

2. Zadejte název pro vaše schéma. Nahrát soubor schématu, klikněte na ikonu složky vedle **schématu** textové pole. Po dokončení procesu nahrát, klepněte na tlačítko **OK**.    
![Snímek obrazovky s "Přidat schéma", "Small souborem" zvýrazněným](./media/app-service-logic-enterprise-integration-schemas/schema-31.png)  

### <a name="add-a-schema-file-larger-than-2-mb-up-to-a-maximum-of-8-mb"></a>Přidání souboru schématu větších než 2 MB (až do velikosti 8 MB)  

Postup pro tuto závisí na úroveň přístupu objektů blob kontejner: **veřejné** nebo **žádný anonymní přístup**. K určení tato úroveň přístupu, v **Azure úložiště Explorer**pod **Objektů Blob kontejnerů**, vyberte kontejner objektů blob, který chcete. Vyberte **zabezpečení**a vyberte kartu **Úroveň přístupu** .

1. Pokud objektů blob úroveň přístupu je **veřejné**, postupujte takto:  
  ![Snímek obrazovky s Azure úložiště Průzkumníkem, s "Objektů Blob kontejnery", "Zabezpečení" a "Veřejné" zvýrazněným](./media/app-service-logic-enterprise-integration-schemas/blob-public.png)  

    na. Nahrání schématu k základnímu úložišti a zkopírujte URI.  
    ![Snímek obrazovky s účtem úložiště, se zvýrazněným identifikátor URI](./media/app-service-logic-enterprise-integration-schemas/schema-blob.png)  

    b. V **Přidat schéma**vyberte **velké soubory**a poskytují URI v textovém poli **Obsahu URI** .  
    ![Snímek obrazovky s schémata s tlačítkem "Přidat" a "Velké soubory" zvýrazněným](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

2. Pokud objektů blob úroveň přístupu je **anonymní přístup**, postupujte takto:  
  ![Snímek obrazovky s Azure úložiště Průzkumníkem, s "Objektů Blob kontejnery", "Zabezpečení" a "Bez anonymního přístupu" zvýrazněným](./media/app-service-logic-enterprise-integration-schemas/blob-1.png)  

    na. Nahrajte schématu k základnímu úložišti.  
    ![Snímek obrazovky s účtu úložiště](./media/app-service-logic-enterprise-integration-schemas/blob-3.png)

    b. Generování podpisu sdílený přístup pro schéma.  
    ![Snímek obrazovky s sccount úložiště, s kartou podpisy sdílený přístup zvýrazněným](./media/app-service-logic-enterprise-integration-schemas/blob-2.png)

    c. V **Přidat schéma**vyberte **velké soubory**a poskytují podpis sdílený přístup URI v textovém poli **Obsahu URI** .  
    ![Snímek obrazovky s schémata s tlačítkem "Přidat" a "Velké soubory" zvýrazněným](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

3. V zásuvné **schémata** účtu integrace EIP teď byste měli vidět schématu nově přidaný.  
![Snímek obrazovky s EIP integrace účet, s "Schémata" a nové schéma zvýrazněným](./media/app-service-logic-enterprise-integration-schemas/schema-41.png)
  

## <a name="edit-schemas"></a>Úprava schémata
1. Vyberte dlaždici **schémat** .  
2. Vyberte schéma, které chcete upravit, která se otevře zásuvné **schémata** .
3. Na zásuvné **schémata** vyberte **Upravit**.  
![Snímek obrazovky schémata zásuvné](./media/app-service-logic-enterprise-integration-schemas/edit-12.png)    
4. Vyberte požadovaný upravovat pomocí dialogového okna Výběr souboru, které se otevře soubor schématu.
5. Vyberte **Otevřít** v ovládacím prvku pro výběr souboru.  
![Snímek obrazovky pro výběr souboru](./media/app-service-logic-enterprise-integration-schemas/edit-31.png)  
6. Zobrazí se upozornění, která označuje, že nahrávání byl úspěšný.  

## <a name="delete-schemas"></a>Odstranění schémata
1. Vyberte dlaždici **schémat** .  
2. Vyberte schéma, které chcete odstranit z zásuvné **schémata** , která se otevře.  
3. Na zásuvné **schémata** vyberte **Odstranit**.
![Snímek obrazovky schémata zásuvné](./media/app-service-logic-enterprise-integration-schemas/delete-12.png)  

4. Výběr potvrďte výběrem **Ano**.  
![Snímek obrazovky s "Odstranit schématu" potvrzovací zprávy](./media/app-service-logic-enterprise-integration-schemas/delete-21.png)  
5. Všimněte si, že aktualizuje seznam schémat v zásuvné **schémata** a už není uvedená schéma, které jste odstranili.  
![Snímek obrazovky s EIP integrace účet s "Schémata" zvýrazněným](./media/app-service-logic-enterprise-integration-schemas/delete-31.png)    

## <a name="next-steps"></a>Další kroky

- [Další informace o Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Přečtěte si víc o pack podnikové integrace").  

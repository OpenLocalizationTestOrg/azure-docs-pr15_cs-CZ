
<properties
    pageTitle="Certifikáty pomocí podnikové integrace Pack | Microsoft Azure"
    description="Naučte se používat certifikáty s Enterprise integrace Pack a logika aplikace"
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
    ms.date="09/06/2016"
    ms.author="deonhe"/>

# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>Další informace o certifikáty a Enterprise integrace Pack

## <a name="overview"></a>Základní informace
Podnikové integrace používá k zabezpečení komunikace B2B certifikáty. V aplikacích integrace vaší organizace můžete použít dva typy certifikátů:

- Veřejné certifikátech, které je třeba zakoupit od certifikační autority (CA).
- Soukromé certifikátech, které můžete sami problémů. Tato poukázky jsou někdy označovány jako podepsaný certifikáty.


## <a name="what-are-certificates"></a>Co jsou certifikáty?
Certifikáty jsou digitálních dokumentů, ověřit identitu všech účastníků v elektronické komunikace a který taky zabezpečit elektronické komunikace.

## <a name="why-use-certificates"></a>Proč používat certifikáty?
Někdy B2B komunikaci musí být důvěrné. Podnikové integrace používá certifikátů k zabezpečení komunikace těmito dvěma způsoby:

- Protože šifruje obsah zprávy
- V digitálně podepisovat zprávy  

## <a name="how-do-you-upload-certificates"></a>Jak nahrajete certifikátů

### <a name="public-certificates"></a>Veřejné certifikátů
Použít *certifikát veřejného* v logických aplikací s možnostmi B2B, musíte nejdřív nahrajte certifikátu do vašeho účtu integrace. Použití certifikátu *podepsaného svým držitelem*na druhé straně nejdřív uložte ho [Azure klíč trezoru](../key-vault/key-vault-get-started.md "Další informace o klíč trezoru").

Po odeslání certifikát je k dispozici týkající se zabezpečení B2B zprávy při definování jejich vlastnosti v [smlouvami](./app-service-logic-enterprise-integration-agreements.md) , kterou vytvoříte.  

Tady jsou podrobný postup pro odeslání certifikátů veřejných ke svému účtu integrace po přihlášení k portálu Azure:

1. Vyberte **Procházet**.  
    ![Vyberte Procházet](./media/app-service-logic-enterprise-integration-overview/overview-1.png)  

2. **Integrace** zadejte do vyhledávacího pole filtru a ze seznamu výsledků klikněte na příkaz **Integrace účty** .     
    ![Vyberte účty integrace](./media/app-service-logic-enterprise-integration-overview/overview-2.png)

3. Vyberte integrace účet, ke kterému chcete přidat certifikát.  
    ![Vyberte integrace účet, ke kterému chcete přidat certifikátu](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4.  Vyberte dlaždici **certifikáty** .  
    ![Vyberte dlaždici certifikátů](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)

5. V zásuvné **certifikáty** , které se otevře klikněte na tlačítko **Přidat** .
    ![Klikněte na tlačítko Přidat](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. Zadejte **název** vašeho certifikátu a potom vyberte typ certifikátu. (V tomto příkladu jsme použili typ certifikátu veřejného.) Klikněte na ikonu složky na pravé straně textového pole **certifikát** .

7. Až se otevře okno pro výběr souboru, vyhledejte a vyberte soubor certifikát, který chcete nahrát ke svému účtu integrace.

8. Vyberte certifikát a potom vyberte **OK** v okně pro výběr souboru. Ověří a odešle certifikát ke svému účtu integrace.

8. Konečně zpátky na zásuvné **Přidat certifikát** klikněte na tlačítko **OK** .  
    ![Kliknutím na tlačítko OK](./media/app-service-logic-enterprise-integration-certificates/certificate-3.png)  

9. V asi minutu, zobrazí se upozornění, které označuje dokončení nahrávání certifikát.

10. Vyberte dlaždici **certifikáty** . Měli byste vidět nově přidaný certifikát.  
    ![V tématu nový certifikát.](./media/app-service-logic-enterprise-integration-certificates/certificate-4.png)  

### <a name="private-certificates"></a>Soukromé certifikátů
Soukromé certifikáty můžete nahrávat do vašeho účtu integrace tak, že provedením následujících kroků:  

1. [Nahrání privátním klíčem do trezoru klíč] (../key-vault/key-vault-get-started.md "Další informace o klíčových trezoru").  

    > [AZURE.TIP] Musí povolit funkci logiky aplikace služby Azure aplikace provádět operace s klíč trezoru. Pomocí následujícího příkazu Powershellu můžete udělit přístup jistinu služeb aplikací použití logických operátorů:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  

2. Vytvořte certifikát soukromé.  

3. Nahrajte soukromé certifikát ke svému účtu integrace.

Po přijetí předchozích kroků, můžete vytvořit smlouvy soukromé certifikát.

Podrobný postup pro odeslání soukromé certifikáty ke svému účtu integrace po přihlášení k portálu Azure jsou následující:  

1. Vyberte **Procházet**.  
    ![Nahrání soukromé certifikáty ke svému účtu integrace](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    

2. **Integrace** zadejte do vyhledávacího pole filtru a ze seznamu výsledků klikněte na příkaz **Integrace účty** .     
    ![Vyberte účty integrace](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  

3. Vyberte integrace účet, ke kterému chcete přidat certifikát.  
    ![Vyberte integrace účet, ke kterému chcete přidat certifikátu](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4. Vyberte dlaždici **certifikáty** .  
    ![Vyberte dlaždici certifikátů](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)  

5. V zásuvné **certifikáty** , které se otevře klikněte na tlačítko **Přidat** .
    ![Klikněte na tlačítko Přidat](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. Zadejte **název** vašeho certifikátu a potom vyberte typ certifikátu. (V tomto příkladu jsme použili typ veřejné certifikátu.) Klikněte na ikonu složky na pravé straně textového pole **certifikát** .

7. Až se otevře okno pro výběr souboru, vyhledejte a vyberte soubor certifikát, který chcete nahrát ke svému účtu integrace.

8. Poté, co jste vybrali certifikát, vyberte **OK** v okně pro výběr souboru. Tato akce ověří certifikát a odešle ke svému účtu integrace.

9. Konečně zpátky na zásuvné **Přidat certifikát** klikněte na tlačítko **OK** .  
    ![Přidání certifikátu](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-1.png)  

10. V asi minutu, zobrazí se upozornění, které označuje dokončení nahrávání certifikát.

11. Vyberte dlaždici **certifikáty** . Měli byste vidět nově přidaný certifikát.
    ![V tématu nový certifikát.](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-2.png)  

Po odeslání certifikát je k dispozici týkající se zabezpečení B2B zprávy při definování jejich vlastnosti [dohod](./app-service-logic-enterprise-integration-agreements.md).  

## <a name="next-steps"></a>Další kroky
- [Vytvoření aplikace logiku využívající funkcemi B2B](./app-service-logic-enterprise-integration-b2b.md)  
- [Vytvoření B2B smlouva](./app-service-logic-enterprise-integration-agreements.md)  
- [Další informace o trezoru klíč] (../key-vault/key-vault-get-started.md "Další informace o klíčových trezoru")  

<properties 
    pageTitle="Základní informace o integration účty a Enterprise integrace Pack | Služba Microsoft Azure aplikací | Microsoft Azure" 
    description="Zjistěte, všechny informace o účtech integrace, Enterprise integrace Pack a logika aplikace" 
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
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="overview-of-integration-accounts"></a>Základní informace o integration účty

## <a name="what-is-an-integration-account"></a>Co je účet integrace?
Integrace účet je Azure účtu, který umožňuje integraci podnikových aplikací ke správě artefakty včetně schémata mapy, certifikáty, partnery a smlouvy. Vytvořenou aplikaci integrace potřebovat účet integrace za účelem přístupu ke schématu, mapy nebo certifikátu, například.

## <a name="create-an-integration-account"></a>Vytvořte účet integrace 
1. Vyberte **Procházet**   
![](./media/app-service-logic-enterprise-integration-accounts/account-1.png)  
2. **Integrace** zadejte do vyhledávacího pole filtru a vyberte **Účty integrace** ze seznamu výsledků     
 ![](./media/app-service-logic-enterprise-integration-accounts/account-2.png)  
3. V nabídce v horní části stránky vyberte tlačítko *Přidat*      
![](./media/app-service-logic-enterprise-integration-accounts/account-3.png)  
4. Zadejte **název**, vyberte **předplatné** , které chcete použít, buď vytvoření nové **skupiny prostředků** nebo vyberte existující skupinu zdroje, vyberte **umístění** , kde účtu integrace se hostované, vyberte **ceny osy**a potom klikněte na tlačítko **vytvořit** .   

  V tomto okamžiku bude v umístění, do kterého jste vybrali zřízení účtu integrace. Dokončení by měl do 1 minuty.    
![](./media/app-service-logic-enterprise-integration-accounts/account-4.png)  
5. Po aktualizaci stránky. Zobrazí se vašemu novému účtu integrace uvedené. Blahopřejeme!  
![](./media/app-service-logic-enterprise-integration-accounts/account-5.png) 

## <a name="how-to-link-an-integration-account-to-a-logic-app"></a>Jak propojit účet integrace logiku aplikace
Aby logiku aplikace pro přístup k mapám, schémat, smlouvami a jiných artefakty, které máte uložené ve vašem účtu integrace je nutné nejprve propojit integrace účtu do aplikace logiku.

### <a name="here-are-the-steps-to-link-an-integration-account-to-a-logic-app"></a>Tady najdete postup, jak propojení klienta integrace logiku aplikace 

#### <a name="prerequisites"></a>Zjistit předpoklady pro
- Integrace účet
- V aplikaci logiku

>[AZURE.NOTE]Zajistit, že váš účet integrace a logika aplikace ve **stejném umístění Azure** než začnete

1. Vyberte odkaz **Nastavení** z nabídky aplikace pro použití logických operátorů  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-1.png)   
2. Vyberte položku **Integration účtu** z nastavení zásuvné  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-2.png)   
3. Vyberte integrace účet, který chcete propojit do logiky aplikace **Vyberte účet integrace** rozevírací seznam  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-3.png)   
4. Uložení prezentace  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-4.png)   
5. Zobrazí se upozornění, které označuje, že byla účtu integrace propojený s logiky aplikace a všechny artefakty ve vašem účtu integrace se teď k dispozici pro použití logických operátorů aplikace.  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-5.png)   

Teď účtu integrace propojen aplikace pro použití logických operátorů, můžete přejít do aplikace použití logických operátorů a B2B spojnic například ověření XML, plochému souboru kódovat/dekódovat nebo transformace vytváření aplikací s funkcemi B2B.  
    
## <a name="how-to-delete-an-integration-account"></a>Jak odstranit integrace účet?
1. Vyberte **Procházet**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. **Integrace** zadejte do vyhledávacího pole filtru a vyberte **Účty integrace** ze seznamu výsledků     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Vyberte **Integrace účtu** , který chcete odstranit  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Vyberte **Odstranit** odkaz, který je umístěn v nabídce   
![](./media/app-service-logic-enterprise-integration-accounts/delete.png)  
5. Potvrzení výběru    

## <a name="how-to-move-an-integration-account"></a>Jak přesunout integrace účet?
Integrace účtu můžete jednoduše přesunout do nové předplatné a nové skupiny prostředků. Pokud budete muset přesunout integrace účtu, postupujte takto:

>[AZURE.IMPORTANT] Bude potřeba aktualizovat všechny požadované skripty nový zdroj ID po přesunutí účet integrace.

1. Vyberte **Procházet**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. **Integrace** zadejte do vyhledávacího pole filtru a vyberte **Účty integrace** ze seznamu výsledků     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Vyberte **Integrace účtu** , který chcete odstranit  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Vyberte **Přesunout** odkaz, který je umístěn v nabídce   
![](./media/app-service-logic-enterprise-integration-accounts/move.png)  
5. Potvrzení výběru    

## <a name="next-steps"></a>Další kroky
- [Další informace o smlouvami] (./app-service-logic-enterprise-integration-agreements.md "Přečtěte si víc o podnikové integrace smlouvami")  


 
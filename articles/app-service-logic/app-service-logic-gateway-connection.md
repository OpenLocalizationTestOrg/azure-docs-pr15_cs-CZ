<properties
   pageTitle="Použití logických operátorů aplikace místní datové připojení brány | Microsoft Azure"
   description="Informace o tom, jak vytvořit připojení k bráně místních dat pomocí funkčně logiky aplikace."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="connect-to-the-on-premises-data-gateway-for-logic-apps"></a>Připojení k bráně místních dat pro použití logických operátorů aplikace

Použití logických operátorů podporovaných aplikací spojnic umožňují konfigurovat připojení místních dat přístup přes bránu místní data.  Podle těchto kroků vás provede jednotlivými jak nainstalujte a nakonfigurujte bránu místních dat pro práci s aplikací logiku.

## <a name="prerequisites"></a>Zjistit předpoklady pro

* Musí být pomocí pracovní nebo školní e-mailovou adresu v Azure přidružíte místní brána dat pomocí svého účtu (účet Azure Active Directory na základě)
    * Pokud používáte Account Microsoft (například @outlook.com, @live.com) vytvořit pracovní nebo školní e-mailovou adresu podle [pokynů uvedených v tomto poli](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-classic-portal) můžete používat účet Azure

> [AZURE.WARNING] Není aktuálně omezení, který místních brány instalace dokončí jenom při použití účtu, který registrované s Power BI.  Zaregistrujte mezitím žádného účtu se "Power BI zdarma" úspěšné dokončení instalace.

* Může mít místní data brány [na místním počítači nainstalovaný](app-service-logic-gateway-install.md).
* Brána nesmí požadovaným jiné brána Azure místních dat ([převzetí se stane po vytvoření kroku 2 níže](#2-create-an-azure-on-premises-data-gateway-resource)): instalace lze přidružit pouze jednu bránu zdroji.

## <a name="installing-and-configuring-the-connection"></a>Instalace a konfigurace připojení

### <a name="1-install-the-on-premises-data-gateway"></a>1. nainstalujte bránu místních dat

Informace o instalaci místní brána dat najdete [v tomto článku](app-service-logic-gateway-install.md).  Brány musí být nainstalovaný v počítači místní před pokračováním zbývající kroky.

### <a name="2-create-an-azure-on-premises-data-gateway-resource"></a>2. Vytvořte zdroj Azure místních dat brány

Po instalaci můžete předplatné Azure přidružit brány místní data.

1. Přihlaste se k Azure pomocí stejného pracovní nebo školní e-mailovou adresu, která byla použita při instalaci brány
1. Klikněte na tlačítko **Nový** prostředek
1. Najděte a vyberte **místní brána dat**
1. Zadejte informace chcete přidružit k účtu – včetně výběru příslušný **Název instalace** brány

    ![Místní Data brány připojení][1]
1. Klikněte na tlačítko **vytvořit** vytvořit zdroj

### <a name="3-create-a-logic-app-connection-in-the-designer"></a>3. připojení logiky aplikace v Návrháři

Teď Azure předplatné je přidružená k instanci místní brána dat, můžete vytvořit připojení k ní z v rámci logiky aplikace.

1. Otevřete aplikaci logiky a zvolte spojnici, který podporuje místní připojení (od tohoto psaní SQL Server)
1. Zaškrtněte políčko pro **připojení prostřednictvím místní brána dat**

    ![Vytvoření brány aplikace Návrhář logiky][2]
1. Vyberte **brány** k připojení a dokončování jakékoli další informace o připojení povinné
1. Kliknutím na **vytvořit** vytvoříte připojení

Připojení by nyní úspěšně nakonfigurován pro použití v aplikaci použití logických operátorů.  

## <a name="next-steps"></a>Další kroky
- [Běžné příklady a scénáře použití logických operátorů aplikace](app-service-logic-examples-and-scenarios.md)
- [Funkce integrace Enterprise](app-service-logic-enterprise-integration-overview.md)

<!-- Image references -->
[1]: ./media/app-service-logic-gateway-connection/createblade.PNG
[2]: ./media/app-service-logic-gateway-connection/blankconnection.PNG
[3]: ./media/app-service-logic-gateway-connection/checkbox.PNG
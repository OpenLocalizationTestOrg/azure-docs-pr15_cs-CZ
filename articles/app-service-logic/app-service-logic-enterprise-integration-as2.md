<properties 
    pageTitle="Naučte se vytvořit dohodu AS2 pro Enterprise integrace Pack" 
    description="Naučte se vytvořit dohodu AS2 pro Enterprise integrace Pack | Aplikace služby Microsoft Azure" 
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
    ms.date="06/29/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-as2"></a>Podnikové integrace se službou AS2

## <a name="create-an-as2-agreement"></a>Vytvoření dohodu AS2
Abyste mohli používat funkce organizace v aplikacích pro použití logických operátorů, je nutné nejprve vytvořit smlouvy. 

### <a name="heres-what-you-need-before-you-get-started"></a>Tady je budete potřebovat než začnete
- [Integrace účet](./app-service-logic-enterprise-integration-accounts.md) podle předplatného Azure  
- Aspoň dva [partnery](./app-service-logic-enterprise-integration-partners.md) už určených ve vašem účtu integrace  

>[AZURE.NOTE]Při vytváření dohodu obsahu v souboru smlouvy se musí shodovat typ smlouvy.    


Poté, co jste [účet integrace vytvořené](./app-service-logic-enterprise-integration-accounts.md) a [přidané partnerů](./app-service-logic-enterprise-integration-partners.md), můžete vytvořit dohodu provedením těchto kroků:  

### <a name="from-the-azure-portal-home-page"></a>Z Azure domovskou stránku portálu

Po přihlášení do [Azure portál](http://portal.azure.com "Azure portálu"):  
1. **Přejděte** v nabídce vyberte na levé straně.  

>[AZURE.TIP]Pokud nevidíte odkaz **Procházet** , budete muset nejdřív rozbalte nabídku. To udělat tak, že vyberete **Zobrazit nabídku** odkaz, který je umístěn v levém horním rohu sbalené nabídky.  

![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. *Integrace* zadejte do vyhledávacího pole Filtr a pak vyberte **Účty integrace** ze seznamu výsledků.       
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. V zásuvné **Integrace účty** , které se objeví vyberte integrace účtu, na kterém můžete vytvořit smlouvu. Pokud nevidíte veškerá integrace účty seznamy, [vytvořit první](./app-service-logic-enterprise-integration-accounts.md "All about integration accounts").  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4.  Vyberte dlaždici **smlouvami** . Pokud se nezobrazuje dlaždice smlouvami nejdřív přidat.   
![](./media/app-service-logic-enterprise-integration-agreements/agreement-1.png)   
5. Klikněte na tlačítko **Přidat** v zásuvné smlouvami, která se otevře.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-2.png)  
6. Zadejte **název** pro souhlas potom vyberte **Hostitele partnera**, **Hostitele Identity**, **Hosta partnera**, **Hosta Identity**v zásuvné smlouvami, která se otevře.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-3.png)  

Tady je pár podrobností, které můžete narazit na užitečné při konfiguraci nastavení vašeho smlouva: 
  
|Vlastnost|Popis|
|----|----|
|Host (hostitel) partnera|Dohodu musí hostitele i hosta partnera. Host (hostitel) partnera představuje organizace, která konfiguruje smlouvu.|
|Host (hostitel) Identity|Identifikátor partnera Host (hostitel). |
|Host partnera|Dohodu musí hostitele i hosta partnera. Host partnera představuje organizace, která je obchodní s partnerem Host (hostitel).|
|Host Identity|Identifikátor pro hosta partnera.|
|Nastavení příjímání|Tyto vlastnosti platí pro všechny zprávy přijaté dohodu|
|Nastavení odesílání|Tyto vlastnosti platí pro všechny zprávy od dohodu|  
Pojďme pokračovat:  
7. Vyberte **Nastavení přijímání** můžete nakonfigurovat, jak mají být zpracování zprávy přijmete prostřednictvím této smlouvy.  
 
 - V případě potřeby můžete změnit vlastnosti příchozí zprávy. K tomuto účelu zaškrtněte políčko **přepsat vlastností zprávy** .
  - Pokud chcete vyžadovat podepsání všech příchozích zpráv, zaškrtněte políčko **zprávy je třeba podepsat** . Pokud vyberete tuto možnost, bude taky muset vyberte **certifikát** , který se použijí k ověření podpis do zprávy.
  - Pokud chcete můžete požádat zprávy šifrované také. K tomuto účelu zaškrtněte políčko **Zašifrovat zprávu** . Musíte potom vyberte **certifikát** , který se bude používat pro dekódovat příchozích zpráv.
  - Můžete taky nastavíte, že zprávy na komprimovat. K tomuto účelu zaškrtněte políčko **zprávy by měly komprimovány** .  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-4.png)  

Pokud chcete další informace o jaké příjem povolovalo, viz následující tabulka.  

|Vlastnost|Popis|
|----|----|
|Přepsat vlastností zprávy|Výběrem této možnosti označuje, že můžete přepsat vlastností v přijatých zprávách |
|Je třeba podepsat zprávu|Povolení to vyžadovat digitálně podepsaných zpráv|
|Šifrování zprávy|Povolte tento postup vyžaduje zprávy šifrování. Zprávy bez šifrované bude odmítnuto.|
|Zprávy by měly komprimovány.|Povolte tento postup vyžaduje zpráv na komprimovat. Bez komprimovaných zprávy bude odmítnutá.|
|MDN textu|Toto je výchozí MDN odesílateli zprávy|
|Odeslání MDN|Povolení to chcete povolit MDNs k odeslání.|
|Odeslání podepsaného MDN|Povolte tento postup vyžaduje MDNs určený k podpisu.|
|Algoritmus Mikrofon||
|Odeslání asynchronní MDN|Povolte tento postup vyžaduje asynchronní odesílání zpráv.|
|ADRESA URL|Toto je adresa URL, do kterého budou odeslané zprávy.|
Teď Pojďme pokračovat:  
8. Vyberte **Nastavení pro odesílání** a nakonfigurovat, jak mají být zpracování zprávám odeslaným pomocí této smlouvy.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-5.png)  

Pokud chcete další informace o jaké odeslat povolovalo, viz následující tabulka.  

|Vlastnost|Popis|
|----|----|
|Povolit podepisování zprávy|Zaškrtnutím tohoto políčka umožníte všechny zprávy od smlouva určený k podpisu.|
|Algoritmus Mikrofon|Vyberte algoritmus používat podepisování zprávy|
|Certifikát|Vyberte certifikát, který chcete použít podepisování zprávy|
|Povolení šifrování zpráv|Zaškrtnutím tohoto políčka šifrování všechny zprávy posílané z této smlouvy.|
|Algoritmus šifrování|Vyberte algoritmu šifrování používat v šifrování zpráv|
|Projevovat záhlaví HTTP|Zaškrtnutím tohoto políčka projevovat záhlaví HTTP typu obsahu do jednoho řádku.|
|Žádost o MDN|Toto zaškrtávací políčko požádat o MDN pro všechny zprávy posílané z této smlouvy povolit|
|Žádost o přihlášení MDN|Povolení požádat o přihlášeni všechny MDNs poslané na této smlouvy|
|Žádost o asynchronní MDN|Povolit asynchronní MDN zasílané do této smlouvy požádat o|
|ADRESA URL|Adresu URL, do kterého budou odesílány MDNs|
|Povolení NRR|Zaškrtnutím tohoto políčka umožníte Nepopiratelnost odpovědnosti přijetí|
Jsme téměř Hotovo.  
9. Vyberte dlaždici **smlouvami** na zásuvné integrace účet a zobrazí se nově přidaný smlouva uvedené.  
![](./media/app-service-logic-enterprise-integration-agreements/agreement-6.png)


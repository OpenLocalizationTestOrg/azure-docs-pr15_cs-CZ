<properties 
    pageTitle="Vytvoření a Správa připojení hybridní | Microsoft Azure" 
    description="Naučte se vytvořit hybridní připojení, spravovat připojení a nainstalujte Connection Manager hybridní. MABS WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="ccompy"/>


# <a name="create-and-manage-hybrid-connections"></a>Vytvoření a Správa připojení hybridní


## <a name="overview-of-the-steps"></a>Přehled kroků
1. Vytvořte si připojení hybridní tak, že zadáte **název hostitele** nebo **plně kvalifikovaný název domény** místního zdroje v síti soukromé.
2. Odkaz Azure webových aplikací nebo Azure mobilních aplikací na hybridní připojení.
3. Instalace Connection Manager hybridní na místní zdroje a připojte k konkrétní hybridní připojení. Portál Azure poskytuje možnosti jediným klepnutím nainstalovat a připojit.
4. Správa hybridní připojení a legendou připojení.

Toto téma obsahuje seznam těchto kroků. 

> [AZURE.IMPORTANT] Je možné nastavit koncový bod hybridní připojení k IP adrese. Pokud používáte k IP adrese, může nebo nemusí dosáhla místní zdroj, v závislosti na svému klientovi. Připojení hybridní závisí na straně klienta způsobem vyhledání DNS. Ve většině případů __klienta__ je kód aplikace. Pokud klienta neprovádí vyhledávání DNS (ho není pokusíte vyřešit IP adresa, jako kdyby to byl název domény (x.x.x.x)), potom přenosy neodešle prostřednictvím hybridní připojení.
>
> Například (pseudokódu) definujete **10.4.5.6** jako hostitele místní:
> 
> **Funguje následujících situacích:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **Následující scénář nefunguje:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`


## <a name="CreateHybridConnection"></a>Vytvoření hybridní připojení

Na portálu Azure pomocí webové aplikace **nebo** služby BizTalk lze vytvořit hybridní připojení. 

**Vytvoření hybridní připojení pomocí webové aplikace**, najdete v článku [Připojení Azure webových aplikací Web Apps na místní zdroji](../app-service-web/web-sites-hybrid-connection-get-started.md). Můžete taky nainstalujete hybridní připojení správce (HCM) z vaší webové aplikace, což je upřednostňovaná metoda. 

**Vytvoření hybridní připojení ve službách BizTalk**:

1. Přihlaste se k [Azure klasické portálu](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. V levém navigačním podokně vyberte **BizTalk služby** a potom vyberte BizTalk službu. 

    Pokud nemáte stávající služba BizTalk, můžete [vytvořit BizTalk službu](biztalk-provision-services.md).
3. Vyberte kartu **Hybridní připojení** :  
![Kartu hybridní připojení][HybridConnectionTab]

4. Vyberte **vytvořit hybridní připojení** nebo klikněte na tlačítko **Přidat** do hlavního panelu. Zadejte následující údaje:

    Vlastnost | Popis
--- | ---
Jméno | Název připojení hybridní musí být jedinečný a nesmí být stejný název jako služba BizTalk. Můžete zadat libovolný název ale je konkrétní s její účel. Jako příklad lze uvést:<br/><br/>Mzdy*SQL Server*<br/>SupplyList*SharepointServer*<br/>Zákazníci*OracleServer*
Název hostitele | Zadejte název hostitele plně kvalifikovaný název hostitele nebo adresy IPv4 místního zdroje. Jako příklad lze uvést:<br/><br/>mySQLServer<br/>*mySQLServer*. *Doména*. Corporation*Společnost*.com<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*. .com *Společnost*<br/>10.100.10.10<br/><br/>Pokud použijete adresu IPv4, mějte na paměti, že kód klienta nebo aplikace nemusí vyřešit IP adresu. Zobrazit důležitá poznámka v horní části tohoto tématu.
Port | Zadejte číslo portu místního zdroje. Například, pokud používáte webové aplikace, zadejte port 80 nebo port 443. Pokud používáte SQL Server, zadejte port 1433.

5. Vyberte značku zaškrtnutí dokončete nastavení. 

#### <a name="additional"></a>Další

- Lze vytvořit více hybridní připojení. Zobrazit [BizTalk služby: edice grafu](biztalk-editions-feature-chart.md) pro počet připojení povolené. 
- U každého připojení hybridní se vytvoří pomocí dvojice připojovací řetězec: aplikace klíče této odeslat a místní klíče, které POSLECH. Každou dvojici má primární a sekundární klíč. 


## <a name="LinkWebSite"></a>Propojení Azure aplikaci služby v prohlížeči nebo mobilní aplikaci

Pokud chcete propojit existující hybridní připojení v prohlížeči nebo mobilní aplikaci služby Azure aplikace, vyberte **použít existující připojení hybridní** v zásuvné hybridní připojení. V tématu [přístup místních zdrojů pomocí hybridní připojení aplikace služby Azure](../app-service-web/web-sites-hybrid-connection-get-started.md).

## <a name="InstallHCM"></a>Instalace hybridní Connection Manager místní

Po vytvoření připojení hybridní nainstalujte Connection Manager hybridní místního zdroje. Je můžete stáhnout z Azure webových aplikací nebo z BizTalk služby. Služby BizTalk pomocí následujících kroků: 

1. Přihlaste se k [Azure klasické portálu](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. V levém navigačním podokně vyberte **BizTalk služby** a potom vyberte BizTalk službu. 
3. Vyberte kartu **Hybridní připojení** :  
![Kartu hybridní připojení][HybridConnectionTab]
4. Na hlavním panelu klikněte na **Místní nastavení**:  
![Místní nastavení][HCOnPremSetup]
5. Vyberte **instalace a konfigurace** spustit nebo stahování Connection Manager hybridní na místní systém. 
6. Vyberte na značku zaškrtnutí, čímž zahájíte instalaci. 

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Další
- Hybridní Connection Manager možné nainstalovat na následující operační systémy:

    - Windows Server 2008 R2 (.NET Framework 4.5 + a Windows Management Framework 4.0 + povinné)
    - Windows Server 2012 (Windows Management Framework 4.0 + povinné)
    - Windows serveru 2012 R2


- Po instalaci Connection Manager hybridní mít tyto důsledky: 

    - Připojení hybridní hostitelem Azure nakonfigurovaný automaticky používat primární připojovací řetězec aplikace. 
    - Zdrojů na pracovišti nakonfigurovaný automaticky používat primární připojovací řetězec v místním nasazení.

- Hybridní Connection Manager používají platné místního připojovací řetězec pro se tak mohli ověřovat. Azure Web Apps nebo mobilní aplikace musíte použít připojovací řetězec platné žádosti o povolení.
- Hybridní připojení můžete zobrazit jako instalací jiné instance správce hybridní připojení na jiném serveru. Konfigurace místního posluchače jako první posluchače místní použít stejnou adresu. V takovém případě přenosu mezi posluchače aktivní místního je náhodně distribuované (s každým). 


## <a name="ManageHybridConnection"></a>Správa hybridní připojení
Ke správě hybridní připojení, máte tyto možnosti:

- Pomocí portálu Azure a přejděte do služby BizTalk. 
- Pomocí [rozhraní REST API](http://msdn.microsoft.com/library/azure/dn232347.aspx).

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Kopírovat/znovu vygenerovat řetězce hybridní připojení

1. Přihlaste se k [Azure klasické portálu](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. V levém navigačním podokně vyberte **BizTalk služby** a potom vyberte BizTalk službu. 
3. Vyberte kartu **Hybridní připojení** :  
![Kartu hybridní připojení][HybridConnectionTab]
4. Vyberte připojení hybridní. V hlavním panelu vyberte **Spravovat připojení**:  
![Správa možností][HCManageConnection]

    **Správa připojení** seznam aplikací a místní připojení řetězce. Můžete zkopírovat připojovací řetězce nebo obnovit přístupová klávesa použité v připojovacím řetězci. 

    **Pokud vyberete možnost znovu vygenerovat**, sdílené přístupová klávesa použít v připojovacím řetězci se změní. Postupujte takto:
    - Na portálu Azure klasická vyberte **Synchronizovat klíčů** v aplikaci Azure.
    - Opětovným spuštěním **Místní nastavení**. Když znovu spusťte instalaci na pracovišti, místní zdroje je automaticky nakonfigurované používají aktualizované primární připojovací řetězec.


#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Použití zásad skupiny pro řízení zdrojů místní hybridní připojení používá

1. Stažení [šablony pro správu hybridní Connection Manager](http://www.microsoft.com/download/details.aspx?id=42963).
2. Extrahujte soubory.
3. Na počítači, který změní zásad skupiny postupujte takto:  

    - Kopírovat. Soubory ADMX ke složce *%WINROOT%\PolicyDefinitions* .
    - Kopírovat. ADML soubory do složky *%WINROOT%\PolicyDefinitions\en-us* .

Po zkopírování, můžete změnit zásady Editor zásad skupiny.




## <a name="next"></a>Další

[Připojení k místní zdroje Azure Web Apps](../app-service-web/web-sites-hybrid-connection-get-started.md)  
[Připojení ke místního serveru SQL Azure Web Apps](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)   
[Přehled hybridní připojení](integration-hybrid-connection-overview.md)


## <a name="see-also"></a>Viz taky

[Rozhraní REST API pro správu služeb BizTalk na Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk služby: Graf edice](biztalk-editions-feature-chart.md)  
[Vytvoření BizTalk služby Azure klasické portálu](biztalk-provision-services.md)  
[BizTalk služby: Řídicí panel, sledování a měřítko karty](biztalk-dashboard-monitor-scale-tabs.md)


[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 

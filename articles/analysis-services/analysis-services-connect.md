<properties
   pageTitle="Načtení dat z Azure Analysis Services | Microsoft Azure"
   description="Zjistěte, jak se připojit k a načtení dat ze služby Analysis Services serveru v Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="get-data-from-azure-analysis-services"></a>Načtení dat z Azure Analysis Services
Jakmile jste vytvořili serveru v Azure a používaný tabulkového modelu, je připraven k připojení a začít se zkoumáním dat uživatelé ve vaší organizaci.

Azure služby Analysis Services podporuje připojení klientů pomocí [objektové modely aktualizován](#client-libraries); Petr, AMO, Adomd.Net nebo MSOLAP, připojení prostřednictvím xmla na server. Například Power BI, Power BI Desktop, Excel nebo libovolné jiných výrobců klientské aplikace, který podporuje objektové modely.

## <a name="server-name"></a>Název serveru
Při vytváření serveru služby Analysis Services v Azure zadejte jedinečný název a oblasti, kde má být vytvořen serveru. Při zadávání názvu serveru v připojení, je schéma názvového serveru:
```
<protocol>://<region>/<servername>
```
 Protokol řetězec **asazure**, oblasti je Uri oblasti, které byl vytvořen serveru (například západní USA westus.asazure.windows.net) a webu název serveru je název serveru jedinečný v rámci oblasti.

## <a name="get-the-server-name"></a>Zjištění názvu serveru
Před připojením, budete muset zjištění názvu serveru. **Azure** portálu > serveru > **Přehled** > **název serveru**, zkopírujte název celý server. Pokud ostatní uživatelé ve vaší organizaci připojení k serveru příliš, je vhodné s nimi nesdílíte tento název serveru. Při zadávání názvu serveru, musí být použita celá cesta.

![Zjištění názvu serveru v Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)


## <a name="connect-in-power-bi-desktop"></a>Připojení v Power BI Desktop

> [AZURE.NOTE] Tato funkce je náhled.

1. V [Power BI Desktop](https://powerbi.microsoft.com/desktop/), klikněte na **Načíst Data** > **databází** > **Azure Analysis Services**.

2. Na **serveru**vložte název serveru ze schránky.

3. V **databázi**Pokud znáte název tabulkového modelu databáze nebo perspektivu, kterou chcete připojit, vložte ho tady. Jinak můžete, ponechejte toto pole prázdné. Můžete vybrat databázi nebo pohledu na další obrazovce.

4. Opustit výchozí možnost **Připojit live** vybraná a potom stiskněte **Připojit**. Pokud se zobrazí výzva k zadání účet, zadejte účtu vaší organizace.

5. V **tabulce dat**rozbalte server, a pak vyberte model nebo perspektivu, kterou chcete připojit k a potom klikněte na **Připojit**. Jedním klepnutím na modelu nebo perspektivy zobrazuje všechny objekty pro toto zobrazení.


## <a name="connect-in-power-bi"></a>Připojení v Power BI
1. Vytvořte Power BI Desktop soubor, který byl aktivní připojení k modelu na serveru.

2. V [Power BI](https://powerbi.microsoft.com), klikněte na **Načíst Data** > **soubory**. Najděte a vyberte soubor.


## <a name="connect-in-excel"></a>Připojení v Excelu
Připojení k serveru služby Azure Analysis Services v aplikaci Excel je podporována pomocí načíst Data v Excelu 2016 nebo Power Query v dřívějších verzích. Vyžaduje se [MSOLAP.7 poskytovatele](https://aka.ms/msolap) . Připojení pomocí Průvodce importem tabulky v doplňku Power Pivot není podporováno.

1. V Excelu 2016, na pásu karet **Data** klikněte na **Načíst externí Data** > **Z jiných zdrojů** > **Ze služby Analysis Services**.

2. V Průvodci datovým připojením vložte do pole **název serveru**, název serveru ze schránky. V **přihlašovací údaje**, vyberte **použít následující uživatelské jméno a heslo**a potom zadejte organizace uživatelské jméno, například nancy@adventureworks.com, a heslo.

    ![Připojení v Excelu přihlášení](./media/analysis-services-connect/aas-connect-excel-logon.png)

4. V, **Vyberte databázi a tabulku**vyberte databázi a modelu nebo perspektivu a klikněte na tlačítko **Dokončit**.

    ![Připojení v Excelu vyberte model](./media/analysis-services-connect/aas-connect-excel-select.png)

## <a name="connection-string"></a>Připojovací řetězec
Při připojení ke službě Analysis Azure pomocí tabulkového modelu objektu, použijte tyto formáty řetězec připojení:

###### <a name="integrated-azure-active-directory-authentication"></a>Integrované ověřování služby Azure Active Directory
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;"
```
Integrované ověřování vyzvedne mezipaměti Azure Active Directory přihlašovacích údajů, pokud je k dispozici. V opačném případě se zobrazí okno Azure přihlášení.

###### <a name="azure-active-directory-authentication-with-username-and-password"></a>Azure Active Directory authentication pomocí jména a hesla
```
"Provider=MSOLAP;Data Source=<Azure AS instance name>;User ID=<user name>;Password=<password>;Persist Security Info=True; Impersonation Level=Impersonate;";
```

## <a name="client-libraries"></a>Knihoven klienta
Při připojování ke Azure Analysis Services z Excelu nebo jiná rozhraní například TOMA AsCmd ADOMD.NET, budete muset nainstalovat nejnovější knihoven klienta poskytovatele. Nejnovější informace:  

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>



## <a name="next-steps"></a>Další kroky
[Správa serveru](analysis-services-manage.md)

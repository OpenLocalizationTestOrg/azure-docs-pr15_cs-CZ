<properties
   pageTitle="Získat další informace z centra zabezpečení Azure dat s Power BI | Microsoft Azure"
   description="Obsahu pack Azure zabezpečení Centrum Power BI snadné vyhledání výstrah zabezpečení, doporučení, útoku zdroje a trendů, založené na datovou sadu, který byl vytvořený pro vašeho sestav."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="get-insights-from-azure-security-center-data-with-power-bi"></a>Získat další informace z centra zabezpečení Azure dat s Power BI
[Řídicího panelu Power BI](http://aka.ms/azure-security-center-power-bi) pro Centrum zabezpečení Azure umožňuje vizualizovat, analýza a filtrace doporučení a výstrahy zabezpečení odkudkoli, včetně mobilního zařízení. Řídicího panelu Power BI umožňuje zobrazit trendy a útoku vzorky – zobrazení upozornění zabezpečení zdroje nebo Zdrojová IP adresa a unaddressed zabezpečení rizikové zdrojů nebo věk. 

Se můžete taky zpracování Centrum zabezpečení doporučení a výstrahy zabezpečení s jinými daty zajímavými způsoby, například využívání dat z [Azure protokolů auditování](https://powerbi.microsoft.com/blog/monitor-azure-audit-logs-with-power-bi/) a [Auditování databáze SQL Azure](https://powerbi.microsoft.com/blog/monitor-your-azure-sql-database-auditing-activity-with-power-bi/). Nabízejí řídicí panely Power BI a tato data můžete taky exportovat do Excelu pro snadné hlášení o stavu zabezpečení cloudové zdroje.

##<a name="using-azure-security-center-dashboard-to-access-power-bi"></a>Přístup k Power BI pomocí řídicí panel centra zabezpečení Azure
Řídicí panel centra zabezpečení Azure můžete taky přístup k sestavám Power BI. Postupujte podle pokynů k provedení tohoto úkolu: 

1. Na řídicím panelu **Azure Centrum zabezpečení** klikněte na tlačítko **prozkoumání v Power BI** .

    ![Připojení k Centrum zabezpečení Azure pomocí Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new10.png) 

2. Zásuvné **prozkoumat v Power BI** otevře na pravé straně, jak je vidět na následující obrazovce:

    ![Připojení k Centrum zabezpečení Azure pomocí Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new2.png)

3. Při vytváření řídicího panelu Power BI poprvé, můžete vybrat jednu z následujících možností v zásuvné **prozkoumat v Power BI** : 

    - **Řídicí panel přehledy zabezpečení**: Vyberte tuto možnost, pokud chcete vytvořit řídicí panel, který obsahuje stav zabezpečení, podprocesů a zjišťování. Tato možnost je další společný pro DevOps roli, která odpovídá pro analýzu svůj stav ochrany a zjištění upozornění u předplatných.
    - **Řídicí panel pro správu zásad**: Vyberte tuto možnost, pokud chcete prozkoumat zásady správy a prosazování.  Tato možnost je další společný pro centrální kdo další věnovaných zásady správného řízení. Získat další informace o dodržování zásady zabezpečení ve své organizaci jim umožní anonymní tohoto řídicího panelu.
    - Pokud už máte řídicího panelu Power BI, klikněte na **Přejít na aktuální řídicího panelu Power BI**.

4. V tomto příkladu vyberte možnost **zabezpečení přehledy řídicího panelu** . Pokud se jedná poprvé, při vytváření řídicího panelu na Power BI pro Centrum zabezpečení se zobrazí výzva k instalaci sady obsahu. Jak je vidět na následující obrazovce, klikněte na tlačítko **získat** v okně **obsahu sady k Power BI** :

    ![Řídicí panel Azure přehledy zabezpečení Centrum zabezpečení](./media/security-center-powerbi/security-center-powerbi-fig1-new3.png)

5. Objeví se okno **Připojit interpretace zabezpečení Centrum zabezpečení Azure** . Ujistěte se, metody **ověřování** **oAuth2** jak je ukázáno v následujícím příkladu je a klikněte na tlačítko **přihlásit** .
    
    ![Ověřování](./media/security-center-powerbi/security-center-powerbi-fig1-new4.png)

6. Můžete být vyzváni k ověření znovu pomocí Azure přihlašovacích údajů. Po ověření řídicího panelu se vytvoří. Po vytvoření řídicího panelu zobrazí zprávu s podobným strukturu jak je vidět na následující obrazovce:

    ![Řídicího panelu Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new5.png)


> [AZURE.NOTE] Aktualizace sestavy je naplánovaný na místě denně. V případě, že dochází k selhání na tato aktualizace, přečtěte si [Aktualizace problémům s Power BI Centrum zabezpečení Azure](https://blogs.msdn.microsoft.com/azuresecurity/2016/04/07/azure-security-center-power-bi-refresh-fails/)a další informace o řešení potíží.

Zde se zobrazí počet výstrah zabezpečení a doporučení i počet VMs, Azure SQL databáze a síťových prostředků sledován Azure Centrum zabezpečení.

Odkaz na Centrum zabezpečení Azure vás přesměruje na portál Azure. Grafy to snadný způsob vizualizace informace o doporučení týkající se zabezpečení a upozornění, včetně:

- Stav zabezpečení zdroje
- Pole Čekání na doporučení
- Doporučení OM
- Upozornění v čase
- Napaden zdroje
- Napaden IP adresy

Za všechny grafy jsou další přehledy. Vyberte dlaždici zobrazíte další informace. Například dlaždici **Stav zabezpečení zdroje** uvidíte další podrobnosti o čeká na vyřízení doporučení zdroji jak je vidět na následující obrazovce:

![Doporučení](./media/security-center-powerbi/security-center-powerbi-fig1-new6.png)

Pokud kliknete na každém řádku tento graf, ostatní se chystáte šedě a soustředit se jenom na tu, kterou jste vybrali. Pokud chcete vrátit do řídicího panelu, klikněte na **Centrum zabezpečení Azure** ve skupinovém rámečku Možnosti **řídicího panelu** v levém podokně na této stránce.

> [AZURE.NOTE] Pokud chcete přizpůsobit sestav přidáním dalších polí nebo měnit stávající vizuálních prvků, můžete upravovat sestavy. Další informace v tématu [interakce s sestavu v zobrazení pro úpravy v Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-interact-with-a-report-in-editing-view/) .

Dlaždice **upozornění s časem útoku zdroje** a **IP adresy útočník** bude mít podobně jako výstup, po kliknutí na každý z nich je. To je způsobeno sestavu sloučí informace týkající se tyto tři proměnné a volá **zdroje napadení** jak je vidět na následující obrazovce:

![Zdroje napadení](./media/security-center-powerbi/security-center-powerbi-fig1-new7.png)

V tomto okamžiku můžete taky uložení kopie této zprávy, tisku nebo publikovat na webu pomocí možností v nabídce **soubor** .

![Nabídka Soubor](./media/security-center-powerbi/security-center-powerbi-fig8.png)

## <a name="exploring-your-azure-security-center-data-with-power-bi-services"></a>Prozkoumání dat centra zabezpečení Azure pomocí služby Power BI

Připojení k [Power BI obsahu Pack služby](https://msit.powerbi.com/groups/me/getdata/services) Power BI a provést následující kroky:

1. V okně **Obsahu Pack pro Power BI** uvidíte dvě možnosti, jak je ukázáno v následujícím příkladu.

    ![Obsahu pack pro Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new.png)

    >[AZURE.NOTE] Pokud už provedena první část tohoto článku uvidíte jenom jednou z možností, což je Správa zásad Centrum zabezpečení Azure.

2. Pro účely uvedeném příkladu klikněte na **získat** dlaždici **Azure Správa zásad Centrum zabezpečení** .

3. V okně **připojit k Správa zásad Centrum zabezpečení Azure** zkontrolujte vyberte **oAuth2** v části **Metody ověřování** rozevírací jak je znázorněno níže a klikněte na tlačítko **přihlásit** .

    ![V okně Správa zásad](./media/security-center-powerbi/security-center-powerbi-fig1-new8.png)

4. Budete přesměrováni na stránku ověřování, kde je třeba zadat přihlašovací údaje, které používáte pro připojení k Azure Centrum zabezpečení. Po dokončení procesu ověření Power BI začnou importu dat za účelem vytvoření sestav. Během této doby může zobrazit tato zpráva v pravém dolním rohu prohlížeče:

    ![Připojení k Centrum zabezpečení Azure pomocí Power BI](./media/security-center-powerbi/security-center-powerbi-fig4.png)

    >[AZURE.NOTE] Při vytváření řídicího panelu poprvé může trvat déle než obvykle, zejména pro situace, ve které máte víc předplatných. 

5. Po dokončení procesu načíst bude řídícího Azure zabezpečení Centrum Power BI s **Správa zásad** sestava podobná té ukázáno v následujícím příkladu:

    ![Řídicí panel pro správu zásad](./media/security-center-powerbi/security-center-powerbi-fig1-new9.png)

## <a name="see-also"></a>Viz taky
V tomto dokumentu se naučíte používat Power BI v Centru zabezpečení Azure. Další informace o Centru zabezpečení Azure, najdete v těchto článcích:

- [Plánování Centrum zabezpečení Azure a příručka](security-center-planning-and-operations-guide.md) – zjistěte, jak plánovat zavádění Azure Centrum zabezpečení.
- [Nastavení zásad zabezpečení v Centru zabezpečení Azure](security-center-policies.md) – zjistěte, jak konfigurovat nastavení zabezpečení v Centru zabezpečení Azure
- [Správa a reagovat na výstrahy zabezpečení v Centru zabezpečení Azure](security-center-managing-and-responding-alerts.md) – zjistěte, jak reagovat na výstrahy zabezpečení
- [Časté otázky k Centru zabezpečení Azure](security-center-faq.md) – najít časté otázky k používání služby
- [Azure zabezpečení Blog](http://blogs.msdn.com/b/azuresecurity/) – najít příspěvky Azure zabezpečení a dodržování předpisů

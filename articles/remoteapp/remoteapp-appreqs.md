
<properties
    pageTitle="Požadavky aplikace pro Azure RemoteApp | Microsoft Azure"
    description="Další informace o požadavcích pro aplikace, které chcete použít v Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="app-requirements"></a>Požadavky aplikace

> [AZURE.IMPORTANT]
> Azure RemoteApp je právě ukončené. Přečtěte si [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.

Azure RemoteApp podporuje streamování 32bitová verze nebo 64bitová verze aplikace pro systém Windows z Windows serveru 2012 R2 obrázku. Většina existující 32bitová verze nebo 64bitová verze aplikace pro systém Windows pracovat "je" v Azure RemoteApp (Vzdálená plocha nebo dřív známý jako Terminálová služba) prostředí. Však je rozdíl mezi systémem, který dobře – některé aplikace fungovat správně a provádět, zatímco ostatní co nedělat. Tyto informace obsahuje pokyny k vývoji aplikací v prostředí Vzdálená plocha a testování k zajištění kompatibility.

Tip: Pracujeme týkající se vytváření příklady pracovní aplikací za vás. Zobrazí se nové témata, která diskutovat o použití aplikace Microsoft Access, QuickBooks a V aplikaci v RemoteApp.

## <a name="requirements"></a>Požadavky
Tyto tři požadavky, pokud postupovali, pomoct aplikace běží dobře v RemoteApp:

1.  Aplikace, které všechny [Certifikační požadavky pro aplikace klasické pracovní plochy Windows](https://msdn.microsoft.com/library/windows/desktop/hh749939.aspx) a řídit [vzdálené plochy programování pokyny](https://msdn.microsoft.com/library/aa383490.aspx) bude mít dokončení kompatibility s RemoteApp.
2.  Aplikace by nikdy ukládají data místně na obrázek nebo RemoteApp instance, které může dojít ke ztrátě.  Po vytvoření kolekce RemoteApp instance jsou klonovat jsou příslušnosti a by měl obsahovat pouze aplikací. Ukládání dat v externím zdrojem nebo přímo v profilu uživatele.
3.  Vlastní obrázky by nikdy daty, která může dojít ke ztrátě.  

## <a name="testing-your-apps"></a>Testování aplikace
Pomocí těchto kroků k testování aplikací:

1.  Instalace systému Windows Server 2012 R2 a aplikace
2.  Povolení Vzdálená plocha
3.  Vytvoření dvou uživatelských účtů, Uživatel_a a Uživateli_b přidání obou uživatelských účtů do skupiny zabezpečení Vzdálená plocha.
4.  Zkontrolujte kompatibilitu více relací vytvořením dvou souběžné relaci na počítač při spuštění aplikace.
5.  Ověření chování aplikace

## <a name="application-development-guidelines"></a>Pokyny k vývoji aplikací
K vývoji aplikací pro RemoteApp se řiďte následujícími pokyny.

### <a name="multiple-users"></a>Víc uživatelů

- Instalace [aplikace pro jednoho uživatele ](https://msdn.microsoft.com/library/aa380661.aspx)můžete vytvořit problémů v prostředí.
- Aplikace by měl [obsahují informace specifické pro uživatele](https://msdn.microsoft.com/library/aa383452.aspx) do umístění specifické pro uživatele, odděleně od globálních informací, které platí pro všechny uživatele.
- RemoteApp používá více [názvů objektů jádra](https://msdn.microsoft.com/library/aa382954.aspx); globální obor názvů používanou primárně služby v aplikacích klient server.
- Není bezpečný předpokládají, že název počítače nebo [IP adresy](https://msdn.microsoft.com/library/aa382942.aspx) přiřazené k počítači, jsou přidružené k jednoho uživatele protože více uživatelů můžete být současně přihlášeni k serveru vzdálené plochy relace hostitele (RD relací).

### <a name="performance"></a>Výkon
- Před přidáním aplikace RemoteApp zakážete [grafické efekty](https://msdn.microsoft.com/library/aa380822.aspx) .
- Maximalizovat procesoru dostupnost pro všechny uživatele, zakažte [úlohy na pozadí](https://msdn.microsoft.com/library/aa380665.aspx) nebo vytvořte efektivně pozadí úkoly, které nejsou náročné.
- Měli byste ladění a zůstatek aplikace [vlákna použití](https://msdn.microsoft.com/library/aa383520.aspx) více uživatelů, procesory prostředí.
- Optimalizace výkonu, je vhodné pro aplikacím [zjistit,](https://msdn.microsoft.com/library/aa380798.aspx) zda jsou spuštěné v relaci klienta.

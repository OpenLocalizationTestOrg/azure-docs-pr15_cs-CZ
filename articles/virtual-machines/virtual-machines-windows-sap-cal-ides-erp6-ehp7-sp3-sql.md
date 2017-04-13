<properties 
pageTitle="Zavedení SAP IDES EHP7 SP3 pro SAP ERP 6.0 na Microsoft Azure | Microsoft Azure" 
description="Nasazení SAP IDES EHP7 s aktualizací SP3 pro SAP ERP 6.0 na Microsoft Azure" 
services="virtual-machines-windows" 
documentationCenter="" 
authors="hermanndms" 
manager="timlt" 
editor="" 
tags="azure-resource-manager" 
keywords=""/> 
<tags 
ms.service="virtual-machines-windows" 
ms.devlang="na" 
ms.topic="article" 
ms.tgt_pltfrm="vm-windows" 
ms.workload="infrastructure-services" 
ms.date="09/16/2016" 
ms.author="hermannd"/> 


# <a name="deploying-sap-ides-ehp7-sp3-for-sap-erp-60-on-microsoft-azure"></a>Nasazení SAP IDES EHP7 s aktualizací SP3 pro SAP ERP 6.0 na Microsoft Azure 

Tento článek popisuje, jak nasazení SAP IDES spustit SQL Server a operačního systému Windows na Microsoft Azure prostřednictvím SAP cloudu zařízení knihovny 3.0. Snímky obrazovek zobrazení procesu krok za krokem. Nasazení další řešení, v seznamu funguje stejným způsobem z hlediska obrázku. Jedna obsahuje jenom zvolit jiné řešení.

Začínat SAP cloudu zařízení knihovny (SAP CAL) přejděte [sem](https://cal.sap.com/). Existuje blogu ze SAP o nové [SAP cloudu zařízení knihovny 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience). 


Následující snímky obrazovek předvedení podrobné nasazení SAP IDES na Microsoft Azure. Tento proces funguje stejným způsobem jako další řešení.


![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic1.jpg)

Na první z nich zobrazuje všechny řešení, které jsou k dispozici na Microsoft Azure. Zvýrazněnou serveru s Windows SAP IDES řešení, které je dostupný pouze na Azure byla vybrána přejdete procesem.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic2.jpg)

Nejdřív nový účet SAP CAL má vytvořit. Dvě možnosti Azure – standardní Azure a Azure v Číně kontinentální, který provozuje 21Vianet partnera aktuálně existuje.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic3.jpg)

Potom je nutné zadat ID Azure předplatné, které se nachází na portálu Azure - viz také dál dolů jak ho získat. Navíc Azure správy certifikát musí ke stažení.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic6.jpg)

V nové Azure portálu jeden najde položku "Předplatná" na levé straně. Klikněte na Zobrazit všechny aktivní předplatné pro vaše uživatele.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic7.jpg)

Výběrem jedné z předplatných a zadáním požadavku, že "Správa certifikátů" vysvětluje, který tam je nový koncept pomocí "služby objekty" pro nový model správce prostředků Azure.
SAP CAL není zatím upravený pro tento nový model a pořád vyžaduje "klasické" modelu a bývalého Azure portálu pro práci s osvědčení o řízení.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic4.jpg)

Tady jednu vidí portálu bývalého Azure. Nahrání certifikát správy vám bude radit SAP CAL potřebná oprávnění k vytváření virtuálních počítačích v rámci předplatného zákazníka. V části "PŘEDPLATNÁ" můžete najít kartu jednu ID předplatného, který má být zadán v portálu SAP Volejte.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic5.jpg)

Na kartě druhý bude ji možné odeslat certifikát správy staženou z SAP CAL před.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic8.jpg)

Malé dialogové okno překryvné okno Vybrat soubor stažený certifikát.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic9.jpg)

Po nahrání certifikát připojení mezi Azure předplatného zákazníka a SAP CAL můžete otestuje v rámci SAP volejte. Malé zprávy by měly objevovat který říká, že je platný připojení.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic10.jpg)

Po nastavení účtu je nutné vybrat řešení, které by měl být nasazené a vytvoření instance.
"Základní" režim je skutečně důvodu. Zadejte název instance, zvolte Azure oblast a definovat hlavní heslo pro řešení.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic11.jpg)

Po určité době v závislosti na velikosti a složitosti má toto řešení (odhad je dán SAP CAL) se zobrazuje jako "aktivní" a je připraven k použití. Je velmi jednoduché.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic12.jpg)

Prohlížíte některé podrobnosti řešení, jednu můžete najdete v článku jaký druh VMs byly nasazeny. V tomto případě je jeden jednoho OM Azure velikosti D12 vytvořený pomocí SAP Volejte.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic13.jpg)

Na portálu Azure virtuálního počítače najdete počínaje stejný název instance uvedenou v SAP Volejte.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic14.jpg)

Nyní je možné připojit k řešení tlačítkem připojit na portálu SAP Volejte. Malé dialogové okno s odkazem na uživatelské příručce, která popisuje výchozí pověření pro práci s řešení.
[Tady](https://caldocs.hana.ondemand.com/caldocs/help/Getting_Started_Guide_IDES607MSSQL.pdf) je odkaz na příručku pro IDES řešení.

![](./media/virtual-machines-windows-sap-cal-ides-erp6-ehp7-sp3-sql/ides-pic15.jpg)

Další možností je přihlášení k Windows OM a začít třeba předkonfigurovaná grafické SAP.






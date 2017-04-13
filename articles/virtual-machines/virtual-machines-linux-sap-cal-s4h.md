<properties 
pageTitle="Nasazení S/4 HANA nebo pásma/4 HANA na Azure OM | Microsoft Azure" 
description="Nasazení S/4 HANA nebo HANA pásma/4 na Azure OM" 
services="virtual-machines-linux" 
documentationCenter="" 
authors="hermanndms" 
manager="timlt" 
editor="" 
tags="azure-resource-manager" 
  keywords=""/> 
<tags 
  ms.service="virtual-machines-linux" 
  ms.devlang="na" 
  ms.topic="article" 
  ms.tgt_pltfrm="vm-linux" 
  ms.workload="infrastructure-services" 
  ms.date="09/15/2016" 
  ms.author="hermannd"/> 


# <a name="deploying-s4-hana-or-bw4-hana-on-microsoft-azure"></a>Nasazení S/4 HANA nebo HANA pásma/4 na Microsoft Azure 

Tento článek popisuje, jak nasazení S/4 HANA na Microsoft Azure prostřednictvím SAP cloudu zařízení knihovny 3.0.
Snímky obrazovek zobrazení procesu krok za krokem. Nasazení další řešení založená na SAP HANA jako pásma/4 HANA funguje stejným způsobem z hlediska obrázku. Jedna obsahuje jenom zvolit jiné řešení.

Začínat SAP cloudu zařízení knihovny (SAP CAL) přejděte [sem](https://cal.sap.com/). Existuje blogu ze SAP o nové [SAP cloudu zařízení knihovny 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience). 


Následující snímky obrazovek předvedení podrobné nasazení S/4 HANA na Microsoft Azure. Tento proces funguje stejným způsobem jako pro ostatní řešení likeBW/4 HANA.


![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-1b.jpg)

Na první z nich zobrazuje všechny řešení založená na SAP CAL HANA, které jsou dostupné na Microsoft Azure.
Exemplarily "SAP S/4 HANA místní edici" (řešení dole v snímek) byla rozhodli absolvovat procesu.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-2.jpg)

Nejdřív nový účet SAP CAL má vytvořit. Dvě možnosti Azure – standardní Azure a Azure v Číně kontinentální, který provozuje 21Vianet partnera aktuálně existuje.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic3b.jpg)

Potom je nutné zadat ID Azure předplatné, které se nachází na portálu Azure - viz také dál dolů jak ho získat. Navíc Azure správy certifikát musí ke stažení.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic6b.jpg)

V nové Azure portálu jeden najde položku "Předplatná" na levé straně. Klikněte na Zobrazit všechny aktivní předplatné pro vaše uživatele.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic7b.jpg)

Výběrem jedné z předplatných a zadáním požadavku, že "Správa certifikátů" vysvětluje, který tam je nový koncept pomocí "služby objekty" pro nový model správce prostředků Azure.
SAP CAL není zatím upravený pro tento nový model a pořád vyžaduje "klasické" modelu a bývalého Azure portálu pro práci s osvědčení o řízení.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic4b.jpg)

Tady jednu vidí portálu bývalého Azure. Nahrání certifikát správy vám bude radit SAP CAL potřebná oprávnění k vytváření virtuálních počítačích v rámci předplatného zákazníka. V části "PŘEDPLATNÁ" můžete najít kartu jednu ID předplatného, který má být zadán v portálu SAP Volejte.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic5.jpg)

Na kartě druhý bude ji možné odeslat certifikát správy staženou z SAP CAL před.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic8.jpg)

Malé dialogové okno překryvné okno Vybrat soubor stažený certifikát.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic9.jpg)

Po nahrání certifikát připojení mezi Azure předplatného zákazníka a SAP CAL můžete otestuje v rámci SAP volejte. Malé zprávy by měly objevovat který říká, že je platný připojení.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic10.jpg)

Po nastavení účtu je nutné vybrat řešení, které by měl být nasazené a vytvoření instance.
"Základní" režim je skutečně důvodu. Zadejte název instance, zvolte Azure oblast a definovat hlavní heslo pro řešení.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic11.jpg)

Po určité době v závislosti na velikosti a složitosti má toto řešení (odhad je dán SAP CAL) se zobrazuje jako "aktivní" a je připraven k použití. Je velmi jednoduché.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic12.jpg)

Prohlížíte některé podrobnosti řešení, jednu můžete najdete v článku jaký druh VMs byly nasazeny. V tomto případě byly vytvořeny tři VMs Azure různě a účel.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic13.jpg)

Na portálu Azure virtuálních počítačích najdete počínaje stejný název instance uvedenou v SAP Volejte.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic14b.jpg)

Nyní je možné připojit k řešení tlačítkem připojit na portálu SAP Volejte. Malé dialogové okno s odkazem na uživatelské příručce, která popisuje výchozí pověření pro práci s řešení.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic15.jpg)

Další možností je přihlášení ke klientovi OM Windows a začít třeba předkonfigurovaná grafické SAP.








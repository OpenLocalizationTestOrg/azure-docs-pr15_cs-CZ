<properties
   pageTitle="Přístup k Azure virtuálních počítačích z Průzkumníka serveru | Microsoft Azure"
   description="Získat základní informace o tom, jak zobrazit vytváření a správa virtuálních počítačích Azure (VMs) v okně Průzkumník serveru ve Visual Studiu."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>Přístup k Azure virtuálních počítačích z Průzkumníka serveru

Pomocí Průzkumníka serveru ve Visual Studiu, můžete zobrazit informace o vaší virtuálních počítačích hostitelem Azure.

## <a name="accessing-virtual-machines-in-server-explorer"></a>Přístup k virtuálních počítačích v Průzkumníku serveru

Pokud máte virtuálních počítačích hostitelem Azure, dostanete se k nim v Průzkumníku serveru. Musíte prvním přihlášení k předplatnému Azure zobrazíte mobilních služeb. Pokud chcete přihlásit, otevřete místní nabídky pro Azure uzel v Průzkumníku serveru a vyberte **připojit k Microsoft Azure**.

### <a name="to-get-information-about-your-virtual-machines"></a>Chcete-li získat informace o virtuálních počítačích

1. V okně Průzkumník serveru zvolte virtuálního počítače a zvolte klávesy F4 zobrazte jeho okno Vlastnosti.

    Následující tabulka ukazuje, jaké vlastnosti jsou k dispozici, ale jsou všechny určené jen pro čtení. Změnit, použijte [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885).

  	|Vlastnost|Popis|
  	|---|---|
  	|Název DNS|Adresu URL s Internetovou adresu virtuálního počítače.|
  	|Prostředí|Virtuálního počítače tato vlastnost hodnotu vždy výroby.|
  	|Jméno|Název virtuálního počítače.|
  	|Velikost|Velikost virtuální stroje, které odpovídá počtu paměti a místa na disku, který je k dispozici. Další informace najdete v tématu Jak: Konfigurace velikosti virtuálního počítače.|
  	|Stav|Hodnoty jsou počáteční, Začínáme, zastavení, zastaveno a načítání stavu. Pokud se zobrazí stav načítání, aktuální stav neznámý. Hodnoty pro tuto vlastnost se liší od hodnoty, které se používají na [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885).|
  	|SubscriptionID|ID předplatného pro váš účet Azure. Zobrazení vlastností pro předplatné, můžete zobrazit tyto informace na [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885) .|

1. Vyberte uzel koncového bodu a zobrazte okno **Vlastnosti** .

1. Následující tabulka popisuje dostupné vlastnosti koncové body, ale jsou určeny jen pro čtení. Přidání nebo úprava koncové body virtuálního počítače, můžete [Azure klasické portálu](http://go.microsoft.com/fwlink/?LinkID=213885). 

  	|Vlastnost|Popis|
  	|---|---|
  	|Jméno|Identifikátor koncového bodu.|
  	|Soukromé Port|Port pro interní aplikaci přístup k síti.|
  	|Protocol (protokol)|Protokol, který používá vrstva pro tento koncový bod TCP a UDP.|
  	|Veřejný Port|Číslo portu, který se používá pro přístup k veřejným aplikaci.|

## <a name="next-steps"></a>Další kroky

Další informace o používání Azure role ve Visual Studiu najdete v tématu [Pomocí vzdálené plochy s rolemi Azure](vs-azure-tools-remote-desktop-roles.md).

<properties
   pageTitle="Přístup k OM ID"
   description="Popisuje zobrazení a použití Azure OM jedinečné ID"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="kmouss"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="02/08/2016"
   ms.author="kmouss"/>
   
# <a name="accessing-and-using-azure-vm-unique-id"></a>Zobrazení a použití Azure OM jedinečné ID

Azure OM jedinečné ID je identifikátor 128 mi kódování a uložené ve všech Azure IaaS OM společnosti SMBIOS a aktuálně jde přečíst pomocí příkazů BIOS platformu.

Azure OM jedinečné ID je vlastnost jen pro čtení. Azure jedinečné ID OM nemění při vypnutí restartování počítače (buď plánované pro neplánované), spustit a zastavit zrušit přidělení, služby retušování nebo obnovit na místě. Pokud OM je snímek a zkopírují do vytvářet nové instance, je nakonfigurovaný nové ID OM Azure.

> [AZURE.NOTE] Pokud máte starší VMs vytvořené a od tato nová funkce máte chránění (18 září 2014), přejděte prosím restart OM automaticky získat Azure jedinečné ID.


Přístup k Azure jedinečné OM ID z v OM:


## <a name="create-a-vm"></a>Vytvoření virtuálního počítače
 

Další informace najdete v tématu [Vytvoření virtuálního počítače](virtual-machines-linux-creation-choices.md)


## <a name="connect-to-the-vm"></a>Připojení k OM
 

Další informace najdete v tématu [SSH z Linux](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="query-vm-unique-id"></a>Jedinečné ID OM dotazu

Příkaz (příklad používá **se systémem Ubuntu**):

    sudo dmidecode | grep UUID
    
Příklad očekávané výsledky:

    UUID: 090556DA-D4FA-764F-A9F1-63614EDA019A
    
Z důvodu velké Endian bit řazení skutečné jedinečné ID OM v tomto případě bude:

    DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
    
    
Azure OM jedinečné ID lze použít v různých scénářích, zda OM běží na Azure nebo místní a pomůže vašim požadavkům sledování licencování, vytváření sestav nebo Obecné, které by mohly být na Azure IaaS nasazení. Počet nezávislých prodejců softwaru vytváření aplikací a potvrzuje na Azure může vyžadovat k identifikaci Azure OM během svého životního cyklu a zjistit, jestli je spuštěný OM na Azure, místní nebo na jiných poskytovatelů cloudu. Tento identifikátor platformy můžou pomoct například zjistit, pokud je software řádně licencován nebo nápovědu ke koordinaci žádná data OM ke zdroji, jako je třeba pro pomoc týkající se nastavení správné metriky pro správné platformy a ke sledování a sladit tyto metriky mezi další možnosti využití.

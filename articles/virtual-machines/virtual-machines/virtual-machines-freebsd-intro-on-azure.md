<properties
   pageTitle="Úvod k FreeBSD na Azure | Microsoft Azure"
   description="Zjistěte, jak používat FreeBSD virtuálních počítačích na Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="KylieLiang"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/27/2016"
   ms.author="kyliel"/>

# <a name="introduction-to-freebsd-on-azure"></a>Úvod k FreeBSD na Azure
Toto téma obsahuje přehled o spuštění virtuálního počítače FreeBSD v Azure.

## <a name="overview"></a>Základní informace
FreeBSD pro Microsoft Azure je operační systém pokročilým používá k power moderní servery, stolní počítače a vložené platformách. Obrázek FreeBSD 10.3 poskytuje společnost Microsoft Corporation a je dostupná v Azure. Je založeno na FreeBSD 10.3 vydání a instalaci [2.1.4](https://github.com/Azure/WALinuxAgent/releases/tag/v2.1.4) Azure agenta hosta OM. Agent je zodpovědný za komunikaci mezi OM FreeBSD a Azure struktury operací, jako je zřizování OM při prvním použití (uživatelské jméno, heslo, název hostitele atd.) a povolení funkce pro výběrové OM rozšíření.
Strategie pro budoucí verze FreeBSD, je mít nejnovější informace a zpřístupnit nejnovější verzí brzy po jejich vydání týmem FreeBSD vydání technickým. Nadcházející verzi je [FreeBSD 11](https://www.freebsd.org/releases/11.0R/schedule.html).

## <a name="deploying-a-freebsd-virtual-machine"></a>Nasazení FreeBSD virtuálního počítače
Nasazení FreeBSD virtuálního počítače je jednoduchý proces použít obrázek z [Webu Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/).

## <a name="vm-extensions-for-freebsd"></a>OM příponu FreeBSD
Tady jsou podporované OM rozšíření v FreeBSD.

### <a name="vmaccess"></a>VMAccess

Rozšíření [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) tyto možnosti:

- Resetování hesla původní sudo uživatele.
- Vytvoření nového uživatele sudo s zadané heslo.
- Nastavte klíč veřejné hostitele s klávesou dostali.
- Obnovte klíči veřejné hostitele poskytovaných během OM zřizování Pokud není zadán klíč hostitele.
- Otevřít SSH port (22) a obnovit sshd_config Pokud reset_ssh nastavena na hodnotu true.
- Odebrání stávajícího uživatele.
- Zaškrtněte políčko discích.
- Opravte přidané disku.

### <a name="customscript"></a>CustomScript

Rozšíření [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) tyto možnosti:

- Pokud k dispozici, stáhněte si vlastní skripty z úložiště Azure nebo externí veřejné úložiště (třeba GitHub).
- Skript čárky položku spusťte.
- Podpora vložené příkazy.
- Převedení styl systému Windows nový řádek v kostra domu a skripty Python automaticky.
- Odeberte Kusovníku kostra domu a skripty Python automaticky.
- Chrání citlivá data v CommandToExecute.

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Ověření: uživatelská jména hesla a SSH klíče
Když vytváříte FreeBSD virtuálního počítače pomocí portálu Azure, je nutné zadat uživatelské jméno, heslo nebo SSH veřejným klíčem.
Uživatelská jména pro nasazení FreeBSD virtuálního počítače na Azure nesmí podle názvů systémový účet (UID < 100) již v virtuálního počítače ("root," například).
V současnosti je podporována pouze RSA SSH klávesu. Víceřádkové klíč SSH musí začínat "---POČÁTEČNÍHO SSH2 VEŘEJNÝM klíčem---" a končí "---SSH2 veřejné klávesy END---".

## <a name="obtaining-superuser-privileges"></a>Získání superuser oprávnění
Uživatelský účet, který není zadán během nasazení instance virtuálního počítače na Azure je privilegovaných účet. Balíček sudo byl nainstalovaný publikované FreeBSD obrázku.
Když jste přihlášení pomocí tohoto uživatelský účet, můžete použít příkazy jako kořenovou pomocí syntaxe příkazu.

    # sudo <COMMAND>

Volitelně můžete získat kořenové prostředí pomocí sudo -ne.

## <a name="next-steps"></a>Další kroky
- Přejděte na vytvoření OM FreeBSD [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/) .
- Pokud chcete přenést vlastní FreeBSD Azure, v nápovědě k [Vytvoření a odeslání FreeBSD virtuální pevný disk na Azure](../virtual-machines-linux-classic-freebsd-create-upload-vhd.md).

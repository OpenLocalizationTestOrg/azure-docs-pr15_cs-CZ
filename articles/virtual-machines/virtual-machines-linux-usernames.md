<properties 
    pageTitle="Výběrem jména uživatelů Linux | Microsoft Azure" 
    description="Naučte se v Azure vyberte jména uživatelů pro virtuální počítač Linux." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>



#<a name="selecting-user-names-for-linux-on-azure"></a>Výběrem jména uživatelů Linux na Azure#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Když zřizujete Linux virtuálního počítače na Azure zadejte jméno uživatele jiné kořenové, které později je možné použít k přihlášení do OM. Můžete zvolit název nového uživatele nebo pokud zřizování prostřednictvím portálu Azure klasické můžete chvíli přijmout výchozí název "azureuser".

Ve většině případů tento uživatel neexistují na základní obrázek a bude vytvořen během procesu zřizovací. Pokud uživatel vyskytuje na ikonu základní OM, pak agent Azure Linux jednoduše nastaví heslo nebo SSH klíč pro daného uživatele na základě informací, které jste zadali při vytváření OM.

**Ale**Linux definuje sadu jména uživatelů, které není určená k použití. Zřizovací proces se **nezdaří** při pokusu o zřízení Linux OM pomocí existujícího uživatele systému, který je definován jako uživatel s UID 0 99. Typický příklad je `root` uživatele, který má UID 0.

 - Viz taky: [oblasti Linux Standard základu - ID uživatele](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)

Následuje seznam běžných předdefinovaný systémový uživatelů pro CentOS a systémem Ubuntu, že byste neměli používat při zřizování Linux virtuálního počítače na Azure. Tady je právě příklad, najdete v dokumentaci k rozdělení zajistit, že uživatelské jméno, které zvolíte není v konfliktu s existujícím uživatelům systému.


## <a name="centos"></a>CentOS
- abrt
- ADM
- zvuk
- Koš
- CD-ROM
- cgred
- Démon
- dbus
- síti službou
- DIP
- disk
- disketa
- FTP
- FTP
- her
- Gopher
- haldaemon
- zastavení
- kmem
- Zámek
- LP
- pošta
- Muž
- paměti
- nfsnobody
- nikdo
- NTP
- operátor
- oprofile
- postdrop
- přípona
- qpidd
- kořenové
- vzdálené volání procedur
- rpcuser
- saslauth
- vypnutí
- slocate
- sshd
- stapdev
- stapusr
- synchronizace
- systémových
- páskou
- Test
- tcpdump
- TTY
- Uživatelé
- utempter
- utmp
- UUCP
- vcsa
- Video
- kolo


## <a name="ubuntu"></a>Se systémem Ubuntu
- ADM
- Správce
- zvuk
- zálohování
- Koš
- CD-ROM
- crontab
- Démon
- síti službou
- DIP
- disk
- faxu
- disketa
- Sloučit
- her
- gnats
- IRC
- kmem
- orientace na šířku
- libuuid
- seznam
- LP
- pošta
- Muž
- MessageBus
- mlocate
- netdev
- příspěvky
- nikdo
- "nogroup"
- operátor
- plugdev
- proxy serveru
- kořenové
- SASL
- stín
- src
- SSH
- sshd
- zaměstnanců
- sudo
- synchronizace
- systémových
- Syslog
- páskou
- TTY
- Uživatelé
- utmp
- UUCP
- Video
- hlasové
- whoopsie
- WWW dat

 
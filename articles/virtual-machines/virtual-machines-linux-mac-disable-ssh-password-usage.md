<properties
    pageTitle="Zakázání SSH hesla na Linux OM nakonfigurováním SSHD | Microsoft Azure"
    description="Zabezpečené Linux OM na Azure zakázáním přihlášení heslo pro SSH."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="v-livech"/>

# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a>Zakázání SSH hesla na Linux OM nakonfigurováním SSHD

Tento článek se zaměřuje na tom, jak uzamknout přihlášení zabezpečení OM Linux.  Jakmile je SSH port 22 otevřený na světě roboti začátek pokusu o přihlášení pomocí uhodnout hesla.  Co jsme udělá v tomto článku je zakázat přihlášení heslo nad SSH.  Úplně odebráním možnost používání hesel chráníme z tohoto typu přímým útokům OM Linux.  Přidané odměnu je že jsme nastaví její konfiguraci a Linux SSHD umožňuje pouze přihlášení pomocí SSH veřejné a privátní klíče mnohem nejbezpečnější způsob, jak přihlášení k Linux.  Možné kombinace vyžadují uhádnout privátním klíčem je velmi významně pomoci a proto odrazuje od roboti z i bothering zkusit hrubou silou SSH klíče.


## <a name="goals"></a>Cíle

- Konfigurace SSHD disallow:
  - Heslo přihlášení
  - Přihlášení uživatele k kořenové
  - Odpovědi na výzvu ověřování
- Konfigurace SSHD umožňuje:
  - pouze SSH klíčové přihlášení
- Restartujte SSHD přihlaste dál
- Vyzkoušejte nové konfiguraci SSHD

## <a name="introduction"></a>Úvod

[SSH definované](https://en.wikipedia.org/wiki/Secure_Shell)

SSHD je SSH serveru, na kterém běží na OM Linux.  SSH je klienta, který běží v prostředí, kde k počítači, Macbooku nebo Linux.  SSH je také protokol sloužící k zabezpečení a šifrovat komunikaci mezi počítači a OM Linux.

Tento článek, který používá nezapomeňte jedno přihlášení k OM Linux otevření pro celý procházení.  Z tohoto důvodu jsme se otevře dvou terminály a SSH OM Linux z obou z nich.  Použijeme první terminálu SSHDs konfiguračního souboru proveďte požadované změny a restartujte službu SSHD.  Chcete-li otestovat provedené změny až po restartování služby použijeme druhý terminálu.  Protože jsme zakázání SSH hesla a program může sporná, i když SSH klíčů, pokud nejsou správné SSH klíče a zavřít připojením bude v angličtině, OM budou trvale uzamčené a nikdo bude moct přihlášení k němu vyžadující ho k odstranění a obnovení.

## <a name="prerequisites"></a>Zjistit předpoklady pro

- [Vytvoření SSH klíče na Mac a Linux pro Linux VMs v Azure](virtual-machines-linux-mac-create-ssh-keys.md)
- Účet Azure
  - [Bezplatná zkušební registrace](https://azure.microsoft.com/pricing/free-trial/)
  - [Azure portálu](http://portal.azure.com)
- Linux OM provozu v azure
- SSH veřejné a privátní klíče pár v`~/.ssh/`
- Veřejný klíč SSH `~/.ssh/authorized_keys` na Linux OM
- V OM Sudo oprávnění
- Port 22 otevřít

## <a name="quick-commands"></a>Rychlé příkazy

_Zkušený Linux správce, kteří jenom zajímají verze TLDR, začněte tady.  Pro všechny ostatní chce tuto část přeskočit podrobné vysvětlení a procházení._

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes

# Change PermitRootLogin to this:
PermitRootLogin no

# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no

# On the Debian Family
username@macbook$ sudo service ssh restart

# On the RedHat Family
username@macbook$ sudo service sshd restart
```

## <a name="detailed-walk-through"></a>Podrobné procházení

Přihlaste se ke OM Linux na terminál 1 (T1).  Přihlaste se ke OM Linux terminálu 2 (T2).

Na T2 nyní klikněte na tlačítko Upravit konfiguračního souboru SSHD.  

```
username@macbook$ sudo vim /etc/ssh/sshd_config
```

Tady jsme upravit jenom nastavení hesla zakázání a povolení SSH klíčové přihlášení.  Existuje mnoho možnosti v tento soubor, který by měl výzkum a změňte zabezpečit Linux & SSH jako podle potřeby.

#### <a name="disable-password-logins"></a>Zakázání přihlášení heslo

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a>Povolit ověřování veřejných klíčů

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a>Zakázání kořenové přihlášení

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PermitRootLogin to this:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a>Zakázání odpovědi na výzvu ověřování

```
# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a>Restartujte SSHD

V prostředí, kde T1 ověřte, jestli jste pořád přihlášeni.  Toto je důležité, tak, aby se není zamknuté mimo vaší OM Pokud SSH klíče nejsou správné, protože teď jsou zakázány hesla.  Pokud je nastavení jednotlivých nesprávná na vaše Linux OM T1 slouží k řešení sshd_config a budete se pořád Zaprotokolují SSH zachová připojení aktivní při poskytování služby SSHD restartujte.

Z T2 spuštění:

##### <a name="on-the-debian-family"></a>V Debian řady

```
username@macbook$ sudo service ssh restart
```

##### <a name="on-the-redhat-family"></a>V RedHat řady

```
username@macbook$ sudo service sshd restart
```

Hesla zakázány nyní u svého OM chrání před pokus o přihlášení hrubou platnost hesla.  S pouze SSH klávesami povoleno, že budete moci přihlásit rychleji a mnohem lepší zabezpečení.

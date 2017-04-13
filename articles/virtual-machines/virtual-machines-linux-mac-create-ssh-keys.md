<properties
    pageTitle="Vytvořit SSH klíče na Linux a Mac | Microsoft Azure"
    description="Generovat a klávesami se SSH na Mac a Linux pro správce zdrojů a klasické nasazení modelech na Azure."
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="v-livech"/>

# <a name="create-ssh-keys-on-linux-and-mac-for-linux-vms-in-azure"></a>Vytvoření SSH klíče na Mac a Linux pro Linux VMs v Azure

Pomocí SSH keypair můžete vytvořit virtuálních počítačích na Azure, ověřování, pomocí kláves SSH ve výchozím nastavení odstraňování nutnosti hesla k přihlášení.  Hesla můžete uhodnout a otevřete VMs až houževnatým hrubou silou pokusí uhodnout, svoje heslo. VMs vytvořené pomocí šablon Azure nebo `azure-cli` můžete zahrnout veřejný klíč SSH jako součást nasazení odebrání konfiguraci nasazení příspěvek.  Pokud se připojujete k OM Linux z Windows, přečtěte si článek [Tento dokument.](virtual-machines-linux-ssh-from-windows.md)

V článku vyžaduje:

- Azure účet ([získat bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/)).

- přihlášení k lyncu se [Rozhraní příkazového řádku Azure](../xplat-cli-install.md)`azure login`

- režim správce prostředků Azure _musí být v_ Azure rozhraní příkazového řádku`azure config mode arm`

## <a name="quick-commands"></a>Rychlé příkazy

V následujících příkazů nahraďte příklady vlastních voleb.

SSH klíče jsou ve výchozím nastavení k dispozici `.ssh` adresář.  

```bash
cd ~/.ssh/
```

Pokud nemáte `~/.ssh` adresáře `ssh-keygen` příkaz ho pro vytvoření s správná oprávnění.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

Zadejte název souboru, který je uložený v `~/.ssh/` directory:

```bash
id_rsa
```

Zadejte heslo pro id_rsa:

```bash
correct horse battery staple
```

Teď je `id_rsa` a `id_rsa.pub` SSH klíčové pár v `~/.ssh` adresář.

```bash
ls -al ~/.ssh
```

Přidání klávesy nově vytvořený `ssh-agent` na Linux a Mac (také přidána do řetězce klíčů OSX):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

Zkopírujte veřejným klíčem SSH Linux server:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub myusername@myserver
```

Otestujte přihlášení pomocí klávesy místo hesla:

```bash
ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes -i ~/.ssh/id_rsa myusername@myserver
Last login: Tue April 12 07:07:09 2016 from 66.215.22.201
$
```

## <a name="detailed-walkthrough"></a>Podrobné informace

Použití SSH veřejné a privátní klíče je nejjednodušší přihlásit k serverům Linux. [Veřejný klíč kryptografický](https://en.wikipedia.org/wiki/Public-key_cryptography) umožňuje mnohem lepší zabezpečení se přihlásit k Linux nebo BSD OM v Azure než hesla, které mohou být vynucená hrubou daleko snadněji. Veřejný klíč možné sdílet s kýmkoli; ale jenom vy (nebo infrastrukturu místního zabezpečení) mít privátním klíčem.  Soukromý klíč SSH by měla být [velmi zabezpečené heslo](https://www.xkcd.com/936/) (zdroj:[xkcd.com](https://xkcd.com)) k ochraně ho.  Toto heslo, je právě přístup k privátním klíčem SSH a **není** hesla uživatelského účtu.  Když přidejte heslo pro kód SSH šifruje privátním klíčem tak, aby privátním klíčem nelze bez heslo k odemknutí.  Pokud se zlými úmysly stole privátním klíčem a tuto klávesu neobsahoval hesla, bude moct používat soukromý klíč k přihlášení k serverům, které mají odpovídající veřejný klíč.  Pokud privátním klíčem je chráněný heslem nelze použít uživatelem, které zlými úmysly poskytují další úroveň zabezpečení pro infrastrukturu na Azure.

Tento článek vytváří *ssh-rsa* formátované klíčové soubory, které jsou vám doporučené pro nasazení na správce prostředků.  klíče *SSH-rsa* vyžadovaných na [portál](https://portal.azure.com) klasické a správce prostředků nasazení.

## <a name="create-the-ssh-keys"></a>Vytvořit SSH klíče

Azure vyžaduje aspoň 2048bitový, ssh-rsa formátovat veřejné a privátní klíče. Vytvoření použití klávesy `ssh-keygen`, která zobrazí řadu otázek a zapíše privátním klíčem a odpovídající veřejným klíčem. Po vytvoření Azure OM veřejným klíčem se zkopírují do `~/.ssh/authorized_keys`.  Klíče SSH `~/.ssh/authorized_keys` slouží opravný klienta podle odpovídající privátním klíčem připojení SSH přihlášení.

## <a name="using-ssh-keygen"></a>Použití ssh-keygen

Tento příkaz vytvoří hesla zabezpečená (šifrované) SSH Keypair pomocí 2048bitový RSA a je komentář snadno identifikovat.  

Změňte adresáře, a začněte tak, aby všechny vaše ssh klíče vytvořené v adresáři.

```bash
cd ~/.ssh
```

Pokud nemáte `~/.ssh` adresáře `ssh-keygen` příkaz ho pro vytvoření s správná oprávnění.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

_Příkaz vysvětlení_

`ssh-keygen`= program použitá k vytvoření klíče

`-t rsa`= Typ klávesy vytvořte, což je [RSA formát](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048`= bitů klíče

`-C "myusername@myserver"`= Komentář konci souborem veřejného klíče snadno identifikovat.  Za normálních okolností e-mailu používá jako komentář ale můžete použít něco jiného, co je nejvhodnější pro infrastrukturu.

### <a name="using-pem-keys"></a>Pomocí kláves se PEM

Pokud používáte klasickou nasazení modelu (klasické portál Azure nebo rozhraní příkazového řádku Azure služby správy `asm`), možná budete muset používat PEM formátované SSH kláves pro přístup k Linux VMs.  Tady je postup, jak vytvořit z existující SSH veřejný klíč a existující x509 PEM klíč certifikát.

Vytvoření PEM formátované klíč z existující SSH veřejný klíč:

```bash
ssh-keygen -f ~/.ssh/id_rsa.pub -e > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Příklad ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 myusername@myserver
The key's randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

Ukládání souborů klíče:

`Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa`

Klíč název tohoto článku.  Máte pár klíče s názvem **id_rsa** je výchozí hodnota a některé nástroje může očekávají, že název souboru privátní klíče **id_rsa** tak, aby mají jedno dobré. V adresáři `~/.ssh/` je výchozí umístění pro dvojice klíčů SSH a SSH konfiguračního souboru.

```bash
ls -al ~/.ssh
-rw------- 1 myusername staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 myusername staff   410 Aug 25 18:04 rsa.pub
```
Seznam `~/.ssh` adresář. `ssh-keygen`vytvoří `~/.ssh` adresář, pokud není k dispozici a také nastaví správné režimy vlastnictví a soubor.

Heslo klíče:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen`odkazuje na hesla jako "přístupové heslo."  *Důrazně* doporučujeme přidáním hesla páry klíčů je. Bez hesla ochrana pár klíčů každý, kdo má soubor soukromého klíče slouží k přihlášení k serveru, který má odpovídající veřejný klíč. Přidání hesla nabízí další ochranu v případě, uživatel nebude moct získat přístup k souboru privátní klíče jste je dostali času na změnu klávesy lze ověřit.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>Použití ssh agenta uložit heslo privátní klíče

Chcete-li předejít zadáním hesla privátní klíče soubor s přihlášením každé SSH, můžete použít `ssh-agent` do mezipaměti hesla privátní klíče soubor. Pokud používáte Mac, řetězce klíčů OSX bezpečně ukládá privátní klíče hesla, pokud vyvoláte `ssh-agent`.

Ověřte, že nejdřív `ssh-agent` běží

```bash
eval "$(ssh-agent -s)"
```

Teď přidejte soukromý klíč `ssh-agent` pomocí příkazu `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

Privátní klíče heslo teď uložený ve `ssh-agent`.

## <a name="create-and-configure-an-ssh-config-file"></a>Vytváření a konfigurace SSH konfiguračního souboru

Je doporučený osvědčený postup vytvoření a konfigurace `~/.ssh/config` soubor k dosažení vyššího protokolu ins a pro optimalizaci chování klienta SSH.

Následující příklad ukazuje standardní konfiguraci.

### <a name="create-the-file"></a>Vytvoření souboru

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Upravte tento soubor a chcete-li přidat novou konfiguraci SSH:

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>Příklad `~/.ssh/config` souboru:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User myusername
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

Konfigurace tento SSH vám oddíly pro každý server umožnit oběma mít vlastní vyhrazené pár klíče. Výchozí nastavení (`Host *`) jsou hosts, které neodpovídají určité hostitelů vyšší nahoru v souboru konfigurace.


### <a name="config-file-explained"></a>Konfigurační soubor vysvětlení

`Host`= název hostitele volání na terminálu.  `ssh fedora22`říká `SSH` používat hodnoty v nastavení blokování označeného `Host fedora22` Poznámka: může to být jakýkoli popisek, který je logické pro používání a ne představuje skutečný název hostitele libovolného serveru.

`Hostname 102.160.203.241`= IP adresa nebo název DNS server přistupuje.

`User myusername`= vzdálené uživatelského účtu pro použití při přihlášení k serveru.

`PubKeyAuthentication yes`= říká SSH, který chcete použít SSH klíč k přihlášení.

`IdentityFile /home/myusername/.ssh/id_id_rsa`= privátním klíčem SSH a odpovídající veřejný klíč pro účely ověření.


## <a name="ssh-into-linux-without-a-password"></a>SSH do Linux bez hesla

Teď, když máte dvojici klíč SSH a nakonfigurovanou SSH konfiguračního souboru, je možné k přihlášení k Linux OM rychle a bezpečné. Při prvním přihlášení k serveru pomocí klávesy SSH příkazový řádek se pro heslo pro tento soubor klíče.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Příkaz vysvětlení

Když `ssh fedora22` probíhá SSH nejdřív najde a načte všechna nastavení z `Host fedora22` bloku a načte všechny zbývající nastavení z poslední blok `Host *`.

## <a name="next-steps"></a>Další kroky

Další nahoru je vytvořit VMs Linux Azure pomocí nového SSH veřejným klíčem.  Azure VMs, které jsou vytvořené pomocí SSH veřejným klíčem jako přihlášení se lépe bezpečná jako VMs vytvořená pomocí výchozí metoda přihlašovacího hesla.  Azure VMs vytvořené pomocí klávesy SSH jsou ve výchozím nastavení nakonfigurována hesla zakázané, chybovým vynucená hrubou hádání pokusy.

- [Vytvoření zabezpečené OM Linux pomocí Azure šablony](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Vytvoření zabezpečené OM Linux pomocí portálu Azure](virtual-machines-linux-quick-create-portal.md)
- [Vytvoření zabezpečené OM Linux pomocí rozhraní příkazového řádku Azure](virtual-machines-linux-quick-create-cli.md)

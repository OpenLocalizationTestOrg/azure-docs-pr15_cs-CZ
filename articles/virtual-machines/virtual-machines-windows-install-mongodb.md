<properties
    pageTitle="Instalace MongoDB na Windows OM | Microsoft Azure"
    description="Zjistěte, jak nainstalovat MongoDB Azure OM systémem Windows Server 2012 R2 vytvořené pomocí modelu nasazení Správce prostředků."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a>Nainstalujte a nakonfigurujte MongoDB na Windows OM v Azure
[MongoDB](http://www.mongodb.org) je Oblíbené zdroje otevřít, výkonné NoSQL databázi. Tento článek vás provede instalace a konfigurace MongoDB počítače Windows serveru 2012 R2 virtuální (OM) v Azure. Můžete taky [nainstalovat MongoDB na Linux OM v Azure](virtual-machines-linux-install-mongodb.md).


## <a name="prerequisites"></a>Zjistit předpoklady pro

Před instalací a konfigurace MongoDB, musíte vytvořit virtuálního počítače a v ideálním případě přidat disk dat do ní. Naleznete v následujících článcích vytvoření virtuálního počítače a přidání disk dat:

- [Vytvoření OM serveru Windows na portálu Azure](virtual-machines-windows-hero-tutorial.md) nebo [vytvořte OM serveru Windows pomocí prostředí PowerShell Azure](virtual-machines-windows-ps-create.md)
- [Připojení dat disku, který chcete OM serveru Windows pomocí portálu Azure](virtual-machines-windows-attach-disk-portal.md) nebo [připojení dat disku, který chcete OM serveru Windows Azure PowerShell](https://msdn.microsoft.com/library/mt603673.aspx)
    
Chcete-li začít instalace a konfigurace MongoDB, [Přihlaste se k vaší OM systému Windows Server](virtual-machines-windows-connect-logon.md) pomocí vzdálené plochy.


## <a name="install-mongodb"></a>Instalace MongoDB

> [AZURE.IMPORTANT] Ve výchozím nastavení nejsou povolené funkce zabezpečení MongoDB, jako je ověřování a IP adresy vazby. Funkce zabezpečení lze povolit před nasazením MongoDB provozním prostředí. Další informace najdete v tématu [MongoDB zabezpečení a ověření](http://www.mongodb.org/display/DOCS/Security+and+Authentication).

1. Po připojení vašeho v angličtině pomocí vzdálené plochy, spusťte aplikaci Internet Explorer v nabídce **Start** OM.

2. Vyberte **použít doporučené zabezpečení, ochrana osobních údajů a nastavení kompatibility** při prvním otevření aplikace Internet Explorer a klikněte na **OK**.

3. Konfigurace rozšířené zabezpečení aplikace Internet Explorer aktivované ve výchozím nastavení. Přidání MongoDB webu do seznamu povolených webů:

    - V pravém horním rohu vyberte ikonu **Nástroje** .
    - V dialogovém okně **Možnosti Internetu**vyberte kartu **zabezpečení** a potom vyberte ikonu **Důvěryhodné servery** .
    - Klikněte na tlačítko **weby** . Přidání _https://\*. mongodb.org_ do seznamu důvěryhodných webů a potom zavřete dialogové okno.

    ![Konfigurace nastavení zabezpečení Internet Explorer](./media/virtual-machines-windows-install-mongodb/configure-internet-explorer-security.png)

4. Přejděte na stránky [souborů ke stažení MongoDB -](http://www.mongodb.org/downloads) (http://www.mongodb.org/downloads).

5. Ve výchozím nastavení měli vybrat **Server komunity** edition a nejnovější stabilní ální pro Windows Server 2008 R2 64-bit a novější. Pokud si Pokud chcete stáhnout instalační program, klikněte na **DOWNLOAD (msi)**.

    ![Stáhnout instalační program MongoDB](./media/virtual-machines-windows-install-mongodb/download-mongodb.png)

    Spusťte instalační program po dokončení stahování.

6. Přečtěte si a přijměte podmínky licenční smlouvy. Pokud se zobrazí výzva, vyberte **Dokončit** instalaci.

7. Na poslední obrazovce klikněte na **instalovat**.


## <a name="configure-the-vm-and-mongodb"></a>Konfigurace OM a MongoDB

1. Proměnné cesty se neaktualizují MongoDB instalačního programu. Bez MongoDB `bin` umístění v proměnné path, budete muset zadat úplnou cestu pokaždé, když použijete MongoDB spustitelný soubor. Chcete-li přidat umístění proměnná path:

    - Klikněte pravým tlačítkem myši na nabídku **Start** a vyberte **systém**.
    - Klikněte na **Upřesnit nastavení systému**a potom klikněte na **Proměnné**.
    - V části **systém proměnných**vyberte **cestu**a potom klikněte na **Upravit**.

    ![Konfigurace proměnné cesty](./media/virtual-machines-windows-install-mongodb/configure-path-variables.png)

    Přidat cestu k vaší MongoDB `bin` složky. MongoDB nainstalovaný obvykle v `C:\Program Files\MongoDB`. Ověřte Instalační cesta na vaše OM. Následující příklad přidává výchozí MongoDB instalace umístění `PATH` proměnnou:

    ```
    ;C:\Program Files\MongoDB\Server\3.2\bin
    ```

    > [AZURE.NOTE] Je potřeba přidat úvodní středník (`;`) označíte, že přidáváte umístění pro vaše `PATH` proměnné.

2. Vytvořte MongoDB dat a protokolů adresáře na disku data. V nabídce **Start** vyberte **Příkazový řádek**. Následující příklady na jednotce F: vytvořte adresáře

    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```

3. Spusťte instanci MongoDB pomocí následujícího příkazu úpravu cestu k datům a přihlášení adresářů příslušným způsobem:

    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```

    Může trvat několik minut MongoDB přidělit soubory deníku a spusťte naslouchají připojením. Všechny zprávy protokolu přesměrováni *F:\MongoLogs\mongolog.log* soubor jako `mongod.exe` serveru spustí a přidělí soubory deníku.

    > [AZURE.NOTE] Příkazový řádek zůstane soustředit na tento úkol je spuštěná MongoDB instance. Ponechání otevřeného pokračujte MongoDB okna příkazového řádku. Nebo instalace MongoDB jako služba, jak je uvedeno v dalším kroku.

4. Robustnější MongoDB setkat i v případě, nainstalujte `mongod.exe` jako služba. Vytvoření služby znamená, že nepotřebujete a zavřete okno příkazového řádku s pokaždé, když chcete použít MongoDB. Vytvoření službu následujícím způsobem příslušným způsobem nastavení cestu k adresářům dat a protokolů:

    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log `
        --logappend  --install
    ```

    Příkaz předchozí vytvoří službu s názvem MongoDB, s popisem "Mongo DB". Následujících parametrů jsou také jazyku:

    - `--dbpath` Možnost určuje umístění datového adresáře.
    - `--logpath` Možnost musíte použít k určení soubor protokolu, protože pracovního služba nemá okno příkazového řádku pro zobrazení výstupu.
    - `--logappend` Možnost určuje, že restartovat službu způsobí, že výstup pro připojení k existující soubor protokolu.

  Spusťte službu MongoDB, spusťte tento příkaz:

    ```
    net start MongoDB
    ```

    Další informace o vytváření službu MongoDB najdete v článku [Konfigurace služby systému Windows pro MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service).

## <a name="test-the-mongodb-instance"></a>Testování instancí MongoDB

MongoDB systém jako jedna instance nebo nainstalovaný jako služba teď zahájení vytváření a používání vašich databází. Prostředí pro správu MongoDB zahájíte otevřete jiné okno příkazového řádku z nabídky **Start** a zadejte tento příkaz:

```
mongo  
```

Můžete vytvořit seznam s databází `db` příkaz. Vložení některá data následujícím způsobem:

```
db.foo.insert( { a : 1 } )
```

Chcete data hledat následujícím způsobem:

```
db.foo.find()
```

Výstup je podobný jako v následujícím příkladu:

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

Ukončení `mongo` konzoly následujícím způsobem:

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a>Konfigurace brány firewall a pravidla skupiny zabezpečení sítě
Teď MongoDB je nainstalovanou a spuštěnou, otevřete tak, aby se můžete vzdáleně připojovat k MongoDB port v bráně Firewall systému Windows. Pokud chcete vytvořit nové příchozí pravidlo umožňuje TCP port 27017, otevřete pro správu příkazovém řádku prostředí PowerShell a zadejte tento příkaz:

```powerShell
New-NetFirewallRule -DisplayName "Allow MongoDB" -Direction Inbound `
    -Protocol TCP -LocalPort 27017 -Action Allow
```

Pomocí nástroje pro správu grafické **Brána Windows Firewall s pokročilým zabezpečením** můžete taky vytvořit pravidlo. Vytvoření nového příchozí pravidla umožňuje TCP port 27017.

V případě potřeby vytvořte pravidlo skupiny zabezpečení sítě přístupu k MongoDB z mimo existující podsítě Azure virtuální sítě. Vytvoření pravidla skupiny zabezpečení sítě pomocí [Azure portál](virtual-machines-windows-nsg-quickstart-portal.md) nebo [Azure Powershellu](virtual-machines-windows-nsg-quickstart-powershell.md). Pomocí pravidel brány Windows Firewall umožnit port TCP 27017 rozhraní virtuální sítě MongoDB OM.

> [AZURE.NOTE] TCP 27017 je výchozí port používaný MongoDB. Tento port můžete změnit pomocí `--port` parametr při spuštění `mongod.exe` ručně nebo od služby. Pokud změníte číslo portu, nezapomeňte aktualizovat Brána Firewall systému Windows a skupiny zabezpečení sítě pravidla v předchozích krocích.


## <a name="next-steps"></a>Další kroky
V tomto kurzu se naučíte nainstalujte a nakonfigurujte MongoDB na OM Windows. Nyní máte přístup MongoDB na vaše OM Windows pomocí následujících Pokročilá témata v [si přečtěte následující dokumentaci MongoDB](https://docs.mongodb.com/manual/).

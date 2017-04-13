<properties
   pageTitle="Interakce s služby struktury clusterů pomocí rozhraní příkazového řádku | Microsoft Azure"
   description="Postup slouží k interakci s clusteru struktury služby Azure rozhraní příkazového řádku"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="using-the-azure-cli-to-interact-with-a-service-fabric-cluster"></a>Pomocí rozhraní příkazového řádku Azure Pokud chcete provést interakci s clusteru struktury služby

Můžete pracovat s služby struktury obrázku z počítače Linux pomocí rozhraní příkazového řádku Azure na Linux.

Cílem prvního kroku je získání nejnovější verzi rozhraní příkazového řádku z rep libovolná a nastavit ve vaší cestu pomocí následujících příkazů:

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

U každého příkazu pro podporuje, můžete zadat název příkazu získání nápovědy pro tento příkaz. Automatické dokončování je podporovaný pro příkazy. Například následující příkaz vám nápovědy pro všechny příkazy aplikace. 

```sh
 azure servicefabric application 
```

Nápovědu k příkazu konkrétní můžete dál filtrovat jako v následujícím příkladu:

```sh
 azure servicefabric application  create
```

Povolení automatického dokončování v rozhraní příkazového řádku, spusťte následující příkazy:

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

Následující příkazy připojte se k němu a zobrazit uzlů v clusteru:

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

Použití pojmenovaných parametrů a najít, co jsou, můžete zadat – nápovědy po provedení příkazu. Příklad:

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

Při připojování k více počítačů obrázku z počítače **tedy není součástí clusteru**, použijte tento příkaz:

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

Nahraďte značku PublicIPorFQDN skutečné IP nebo plně kvalifikovaný název domény podle potřeby. Při připojování k více počítačů obrázku z počítače **tedy součástí clusteru**, použijte tento příkaz:

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

Pomocí prostředí PowerShell nebo rozhraní příkazového řádku chcete provést interakci s Cluster Linux služby struktury vytvořený prostřednictvím portálu Azure. 

**Upozornění:** Těchto clusterů nejsou zabezpečené, tedy se může být otevření si svůj seznam jedné přidáním veřejnou IP adresu na manifestu obrázku.



## <a name="using-the-azure-cli-to-connect-to-a-service-fabric-cluster"></a>Připojení k obrázku struktury služby pomocí rozhraní příkazového řádku Azure

Následující příkazy Azure rozhraní příkazového řádku se dočtete, jak se připojit k zabezpečené obrázku. Podrobnosti o certifikátu se musí shodovat certifikát uzlech clusteru.

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```
 
Pokud je váš certifikát certifikační autority (CA), musíte přidat – ca certifikátu cesta parametru jako v následujícím příkladu: 

```
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```
Pokud máte víc čas, použijte jako oddělovače čárku.
 
Pokud do běžné název certifikátu neodpovídá připojení koncového bodu, můžete použít parametr `--strict-ssl` obejít ověření, jak ukazuje tento příkaz: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl false 
```
 
Pokud chcete přeskočit ověření CA, můžete přidat – neoprávněné odmítnout parametr jak to znázorňuje tento příkaz: 

```
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized false 
```
 
Po připojení, je třeba spustit další příkazy rozhraní příkazového řádku chcete provést interakci s clusteru. 

## <a name="deploying-your-service-fabric-application"></a>Nasazení aplikace služby struktury

Provést následující příkazy Kopírovat, zaregistrovat a spusťte aplikaci struktury služby:

```
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```


## <a name="upgrading-your-application"></a>Upgrade aplikace

Proces je podobný [proces ve Windows](service-fabric-application-upgrade-tutorial-powershell.md)).

Vytvoření, kopírování, zaregistrovat a vytvářet aplikace z kořenového adresáře projektu. Pokud se instance aplikace jmenuje struktury: / MySFApp a typ MySFApp, příkazy by vypadal takto:

```
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

Udělejte potřebné změny do aplikace a znovu vytvořit službu změněné.  Aktualizujte seznamu soubor změněné služby (ServiceManifest.xml) aktualizované verze pro službu (a kód nebo Config nebo Data podle potřeby). Také změnit manifestu aplikace (ApplicationManifest.xml) s číslem aktualizovanou verzi aplikace a změny služby.  Teď zkopírujte a zaregistrujte aktualizované aplikace pomocí následujících příkazů:

```
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

Upgrade aplikace teď můžete začít s Tento příkaz:

```
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0  --rolling-upgrade-mode UnmonitoredAuto
```

Teď můžete sledovat pomocí SFX upgrade aplikace. Za několik okamžiků aplikace by byly aktualizovány.  Můžete taky zkusit aktualizované aplikaci, zobrazí se chybová zpráva a zkontrolovat funkci automatického vrácení změn v struktury služby.

## <a name="troubleshooting"></a>Řešení potíží

### <a name="copying-of-the-application-package-does-not-succeed"></a>Kopírování balíčku aplikace neproběhne úspěšně

Zaškrtněte, pokud `openssh` je nainstalovaný. Ve výchozím nastavení se systémem Ubuntu plochy není ji máte nainstalovanou. Nainstalujte ji pomocí následujícího příkazu:

```
 sudo apt-get install openssh-server openssh-client**
```

Pokud potíže potrvají, zkuste zakázat PAM pro ssh změnou **sshd_config** soubor pomocí následujících příkazů:

```sh
 sudo vi /etc/ssh/sshd_config
#Change the line with UsePAM to the following: UsePAM no
 sudo service sshd reload
```

Pokud problém přesto trvá, zvětšete počet ssh relace provedením následujících příkazů:

```sh
 sudo vi /etc/ssh/sshd\_config
# Add the following to lines:
# MaxSessions 500
# MaxStartups 300:30:500
 sudo service sshd reload
```
Pomocí kláves ssh ověřování (namísto hesla) není zatím podporované (protože platformu slouží ssh ke kopírování balíčků), takže použijte ověřování hesla.


## <a name="next-steps"></a>Další kroky

Nastavte si vývojové prostředí a nasazení aplikace služby struktury clusteru Linux.

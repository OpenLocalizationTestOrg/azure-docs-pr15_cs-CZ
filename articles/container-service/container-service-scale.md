<properties
   pageTitle="Změna velikosti obrázku ACS s Azure rozhraní příkazového řádku | Microsoft Azure"
   description="Jak zobrazit svůj cluster kontejneru služby Azure pomocí rozhraní příkazového řádku Azure."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, kontejnerů, Micro služby, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/03/2016"
   ms.author="timlt"/>

# <a name="scale-an-azure-container-service"></a>Změna velikosti služby Azure kontejneru

Je možné přizpůsobit si počtu uzlů služby Azure kontejneru (ACS) obsahuje pomocí nástroje Azure rozhraní příkazového řádku. Při použití rozhraní příkazového řádku Azure zobrazit nástroj vrátí nový konfigurační soubor představující změna projevila na kontejner.

## <a name="about-the-command"></a>Příkaz

Rozhraní příkazového řádku Azure musí být v režimu Azure správce prostředků pro kterou chcete provést interakci s Azure kontejnery. Můžete přepnout do režimu správce prostředků tak, že zavoláte `azure config mode arm`. `acs` Příkaz má příkazu podřízené s názvem `scale` , který nemá všechny operace měřítko pro službu kontejner. Můžete ji získat informace o různých parametry používané v příkazu měřítko systém `azure acs scale --help`, které výstupy něco podobného:

```azurecli
azure acs scale --help

help:    The operation to scale a container service.
help:
help:    Usage: acs scale [options] <resource-group> <name> <new-agent-count>
help:
help:    Options:
help:      -h, --help                               output usage information
help:      -v, --verbose                            use verbose output
help:      -vv                                      more verbose with debug output
help:      --json                                   use json output
help:      -g, --resource-group <resource-group>    resource-group
help:      -n, --name <name>                        name
help:      -o, --new-agent-count <new-agent-count>  New agent count
help:      -s, --subscription <subscription>        The subscription identifier
help:
help:    Current Mode: arm (Azure Resource Management)
```

## <a name="use-the-command-to-scale"></a>Použití příkazu Zobrazit

Zobrazit kontejner služby, musíte nejdřív znát **pole Skupina zdroje** a **jméno Container služby Azure (ACS)**a také zadat nový počet agenti. Pomocí menší nebo větší množství měřítko dolů nebo nahoru v tomto pořadí.

Je vhodné jaké aktuální počet agenti služby kontejneru vědět, než je měřítko. Použití `azure acs show <resource group> <ACS name>` příkazu, který vrátí konfigurace ACS. Všimněte si <mark>počtu</mark> výsledků.

#### <a name="see-current-count"></a>Zobrazit aktuální počet

```azurecli
azure acs show containers-test containerservice-containers-test

info:    Executing command acs show
data:
data:     Id                 : /subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test
data:     Name               : containerservice-containers-test
data:     Type               : Microsoft.ContainerService/ContainerServices
data:     Location           : westus
data:     ProvisioningState  : Succeeded
data:     OrchestratorProfile
data:       OrchestratorType : DCOS
data:     MasterProfile
data:       Count            : 1
data:       DnsPrefix        : myprefixmgmt
data:       Fqdn             : myprefixmgmt.westus.cloudapp.azure.com
data:     AgentPoolProfiles
data:       #0
data:         Name           : agentpools
data:         <mark>Count          : 1</mark>
data:         VmSize         : Standard_D2
data:         DnsPrefix      : myprefixagents
data:         Fqdn           : myprefixagents.westus.cloudapp.azure.com
data:     LinuxProfile
data:       AdminUsername    : azureuser
data:       Ssh
data:         PublicKeys
data:           #0
data:             KeyData    : ssh-rsa <ENCODED VALUE>
data:     DiagnosticsProfile
data:       VmDiagnostics
data:         Enabled        : true
data:         StorageUri     : https://<storageid>.blob.core.windows.net/
```  

#### <a name="scale-to-new-count"></a>Změna velikosti nový počet

Je to již pravděpodobně samozřejmé, můžete změnit měřítko služba kontejneru tak, že zavoláte `azure acs scale` a poskytování **pole Skupina zdroje**, **ACS název**a **počtu agent**. Při měřítka služba kontejneru rozhraní příkazového řádku Azure vrátí řetězec JSON představující nového konfiguraci služby kontejneru, včetně nový počet agent.

```azurecli
azure acs scale containers-test containerservice-containers-test 10

info:    Executing command acs scale
data:    {
data:        id: '/subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test',
data:        name: 'containerservice-containers-test',
data:        type: 'Microsoft.ContainerService/ContainerServices',
data:        location: 'westus',
data:        provisioningState: 'Succeeded',
data:        orchestratorProfile: { orchestratorType: 'DCOS' },
data:        masterProfile: {
data:            count: 1,
data:            dnsPrefix: 'myprefixmgmt',
data:            fqdn: 'myprefixmgmt.westus.cloudapp.azure.com'
data:        },
data:        agentPoolProfiles: [
data:            {
data:                name: 'agentpools',
data:                <mark>count: 10</mark>,
data:                vmSize: 'Standard_D2',
data:                dnsPrefix: 'myprefixagents',
data:                fqdn: 'myprefixagents.westus.cloudapp.azure.com'
data:            }
data:        ],
data:        linuxProfile: {
data:            adminUsername: 'azureuser',
data:            ssh: {
data:                publicKeys: [
data:                    { keyData: 'ssh-rsa <ENCODED VALUE>' }
data:                ]
data:            }
data:        },
data:        diagnosticsProfile: {
data:            vmDiagnostics: { enabled: true, storageUri: 'https://<storageid>.blob.core.windows.net/' }
data:        }
data:    }
info:    acs scale command OK
``` 

## <a name="next-steps"></a>Další kroky

- [Nasazení clusteru](container-service-deployment.md)
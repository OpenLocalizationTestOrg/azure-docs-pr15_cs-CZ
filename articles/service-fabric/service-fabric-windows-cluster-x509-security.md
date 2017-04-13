<properties
   pageTitle="Připojení k zabezpečené soukromé clusteru | Microsoft Azure"
   description="Tento článek popisuje, jak k zabezpečení komunikace do samostatného nebo soukromé obrázku a také mezi klienty a clusteru."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/08/2016"
   ms.author="dkshir"/>

# <a name="secure-a-standalone-cluster-on-windows-using-x509-certificates"></a>Zabezpečené samostatného obrázku v systému Windows pomocí X.509 certifikátů

Tento článek popisuje k zabezpečení komunikace mezi různými uzly svůj cluster Windows samostatného taky jak ověřit připojení klientů k tomuto clusteru pomocí X.509 certifikáty. Zajistíte, že můžete jenom uživatelé přístup ke clusteru nasazeném aplikací a provádět úlohy správy.  Certifikát zabezpečení je třeba povolit u clusteru při vytvoření clusteru.  

Další informace o zabezpečení clusteru například mezi uzly zabezpečení, uzel klient zabezpečení a řízení přístupu na základě rolí v tématu [scénáře zabezpečení obrázku](service-fabric-cluster-security.md).

## <a name="which-certificates-will-you-need"></a>Které certifikáty budete potřebovat?

Chcete-li začít, [Stáhnout balíček samostatného obrázku](service-fabric-cluster-creation-for-windows-server.md#downloadpackage) do jednoho z uzlů v clusteru. V okně stažené balíček bude hledat **ClusterConfig.X509.MultiMachine.json** souboru. Otevřete soubor a přečtěte si část **zabezpečení** v části **Vlastnosti** :

    "security": {
        "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        "CertificateInformation": {
            "ClusterCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ServerCertificate": {
                "Thumbprint": "[Thumbprint]",
                "ThumbprintSecondary": "[Thumbprint]",
                "X509StoreName": "My"
            },
            "ClientCertificateThumbprints": [{
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            }, {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }],
            "ClientCertificateCommonNames": [{
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint" : "[Thumbprint]",
                "IsAdmin": true
            }]
            "HttpApplicationGatewayCertificate":{
                "Thumbprint": "[Thumbprint]",
                "X509StoreName": "My"
            }
        }
    }

Tato část popisuje certifikáty, které potřebujete k zabezpečení svůj cluster Windows samostatně. Povolit na základě certifikátu zabezpečení nastavení hodnoty **ClusterCredentialType** a **ServerCredentialType** na *X509*.

>[AZURE.NOTE] [Miniatura](https://en.wikipedia.org/wiki/Public_key_fingerprint) je primární identitu certifikát. Přečtěte si, [Jak získat Miniatura certifikátu](https://msdn.microsoft.com/library/ms734695.aspx) zobrazíte otisk certifikáty, které vytvoříte.

V následující tabulce jsou uvedeny certifikáty, které budete potřebovat na vzhled obrázku:

|**Nastavení CertificateInformation**|**Popis**|
|-----------------------|--------------------------|
|ClusterCertificate|K zabezpečení komunikace mezi uzly clusteru je potřeba certifikát. Dva různé certifikáty, primárních a sekundárních můžete použít pro upgrade. V části **Miniatura** a že sekundární proměnných **ThumbprintSecondary** nastavte Miniatura primární certifikát.|
|ServerCertificate|Certifikát jsou uvedeny klientovi při pokusu o připojení k tomuto clusteru. Pro usnadnění můžete používat stejný certifikát pro *ClusterCertificate* a *ServerCertificate*. Dva různé serveru certifikáty, primárních a sekundárních můžete použít pro upgrade. V části **Miniatura** a že sekundární proměnných **ThumbprintSecondary** nastavte Miniatura primární certifikát. |
|ClientCertificateThumbprints|To je sada certifikáty, které chcete nainstalovat ověřené klienty. Můžete mít několik různých certifikátů na počítačích, které chcete povolit přístup ke clusteru nainstalovaný. Nastavte Miniatura každý certifikát proměnné **Miniatura certifikátu** . Pokud **IsAdmin** nastavena na *hodnotu true*, pak klientovi certifikát nainstalovaným udělat správce činnosti správy na clusteru. Pokud **IsAdmin** vyhodnotí jako *Nepravda*, klientovi certifikát můžete provádět pouze akce povolené pro přístupová práva, obvykle určené jen pro čtení. Další informace o rolích v článku [řízení přístupu (RBAC) na základě rolí](service-fabric-cluster-security.md/#role-based-access-control-rbac)  |
|ClientCertificateCommonNames|Nastavte běžné název první certifikát klienta pro **CertificateCommonName**. **CertificateIssuerThumbprint** je Miniatura Vystavitel certifikátu. Přečtěte si [práci s certifikáty](https://msdn.microsoft.com/library/ms731899.aspx) chcete dozvědět víc o běžné názvy a Vystavitel.|
|HttpApplicationGatewayCertificate|Toto je nepovinný certifikát, který může být zadán, chcete-li zabezpečené brány Http aplikace. Zkontrolujte, že reverseProxyEndpointPort je nastavená v nodeTypes, pokud používáte certifikát.|

Tady je příklad clusteru konfigurace místo, kam jste obdrželi clusteru, serveru a klientských certifikáty.

 ```
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
      "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
      "iPAddress": "10.7.0.6",
            "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        }
        "security": {
            "metadata": "The Credential type X509 indicates this is cluster is secured using X509 Certificates. The thumbprint format is - d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ServerCertificate": {
                    "Thumbprint": "a8 13 67 58 f4 ab 89 62 af 2b f3 f2 79 21 be 1d f6 7f 43 26",
                    "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpoint": "19001",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="aquire-the-x509-certificates"></a>Médií X.509 certifikátů
K zabezpečení komunikace v rámci clusteru, Nejdřív musíte získat certifikáty X.509 pro uzly clusteru. Kromě toho autora připojení k tomuto clusteru uživatelům oprávnění počítačích /, musíte získat a nainstalovat certifikáty pro klientské počítače.

U clusterů, které běží výrobní zatížení, byste měli použít [Certifikační autorita (CA)](https://en.wikipedia.org/wiki/Certificate_authority) podepsané X.509 certifikátu za účelem zabezpečené obrázku. Informace o získání tato poukázky [jak: Získejte certifikát](http://msdn.microsoft.com/library/aa702761.aspx).

V rámci clusterů, které používáte pro účely testování můžete pomocí certifikátu podepsaného svým držitelem.

## <a name="optional-create-a-self-signed-certificate"></a>Volitelné: Vytvoření certifikátu podepsaného svým držitelem
Jeden způsob, jak vytvořit podepsaný svým držitelem certifikát, který může být zabezpečený správně, je pomocí skriptu *CertSetup.ps1* ve složce služby struktury SDK v adresáři *C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure*. Upravit tento soubor a slouží k vytvoření certifikátu s požadovaný název.

Teď exportujte certifikátu do souboru PFX chráněné heslem. Je třeba nejprve zobrazíte otisk certifikátu. Spusťte aplikaci certmgr.exe. Přejděte do složky **Místní počítač\Osobní** a vyhledejte certifikát, který jste právě vytvořili. Poklikejte na certifikát, který chcete otevřít, vyberte kartu *Podrobnosti* a posuňte se dolů k oblasti *Miniatura* . Zkopírujte hodnotu Miniatura do příkazu Powershellu pod, odebrání mezery.  Změňte hodnotu *$pswd* vhodnosti zabezpečené hesla k ochraně a spuštění prostředí PowerShell:

```   
$pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
```

Prohlédněte si podrobnosti certifikát v počítači nainstalován spuštěním následujícího příkazu Powershellu:

```
$cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
Write-Host $cert.ToString($true)
```

Můžete taky Pokud máte předplatné Azure, postupujte podle části [certifikáty přidat do trezoru klíč](service-fabric-cluster-creation-via-arm.md#add-certificate-to-key-vault).

## <a name="install-the-certificates"></a>Nainstalovat certifikáty
Až budete mít certifikáty, můžete nainstalovat na uzlů. Uzly musí mít nejnovější prostředí Windows PowerShell 3.x nainstalovanou na nich. Je třeba opakujte tyto kroky v jednotlivých uzlech obrázku i serveru certifikáty a vedlejší certifikátů.

1. Zkopírujte soubor .pfx na uzel.
2. Otevřete okno prostředí PowerShell jako správce a zadejte následující příkazy. Nahraďte *$pswd* heslo, které jste použili k vytvoření certifikát. Nahraďte *$PfxFilePath* úplnou cestu .pfx zkopírují do tohoto uzlu.

    ```
    $pswd = "1234"
    $PfcFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```

3. Dále je třeba nastavit řízení přístupu na certifikát tak, aby služby struktury proces, který běží pod účtem služby sítě, můžete ho použít spuštěním následujícího skriptu. Zadejte Miniatura certifikát a "Síť služby" pro účet služby. Můžete zkontrolovat, že seznamů ACL certifikát správnost nástrojem certmgr.exe a prohlížíte Správa privátních klíčů v certifikátu.

    ```
    param
    (
        [Parameter(Position=1, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$pfxThumbPrint,

        [Parameter(Position=2, Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$serviceAccount
        )

        $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; };

        # Specify the user, the permissions and the permission type
        $permission = "$($serviceAccount)","FullControl","Allow"
        $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission;

        # Location of the machine related keys
        $keyPath = $env:ProgramData + "\Microsoft\Crypto\RSA\MachineKeys\";
        $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName;
        $keyFullPath = $keyPath + $keyName;

        # Get the current acl of the private key
        $acl = (Get-Item $keyFullPath).GetAccessControl('Access')

        # Add the new ace to the acl of the private key
        $acl.SetAccessRule($accessRule);

        # Write back the new acl
        Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop

        #Observe the access rights currently assigned to this certificate.
        get-acl $keyFullPath| fl
        ```

4. Repeat the steps above for each server certificate. You can also use these steps to install the client certificates on the machines that you want to allow access to the cluster.

## Create the secure cluster
After configuring the **security** section of the **ClusterConfig.X509.MultiMachine.json** file, you can proceed to [Create your cluster](service-fabric-cluster-creation-for-windows-server.md#createcluster) section to configure the nodes and create the standalone cluster. Remember to use the **ClusterConfig.X509.MultiMachine.json** file while creating the cluster. For example, your command might look like the following:

```
.\CreateServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab - AcceptEULA $true
```

Once you have the secure standalone Windows cluster successfully running, and have setup the authenticated clients to connect to it, follow the section [Connect to a secure cluster using PowerShell](service-fabric-connect-to-secure-cluster.md#connectsecurecluster) to connect to it. For example:

```
Připojení ServiceFabricCluster - ConnectionEndpoint 10.7.0.4:19000 - KeepAliveIntervalInSec 10 – X509Credential - ServerCertThumbprint 057b9544a6f2733e0c8d3a60013a58948213f551 - FindType FindByThumbprint - FindValue 057b9544a6f2733e0c8d3a60013a58948213f551 - StoreLocation CurrentUser - položku StoreName Moje
```

If you are logged on to one of the machines in the cluster, since this already has the certificate installed locally you can simply run the Powershell command to connect to the cluster and show a list of nodes:

```
Připojení ServiceFabricCluster Get-ServiceFabricNode
```
To remove the cluster call the following command:

```
.\RemoveServiceFabricCluster.ps1 - ClusterConfigFilePath.\ClusterConfig.X509.MultiMachine.json - MicrosoftServiceFabricCabFilePath.\MicrosoftAzureServiceFabric.cab
```

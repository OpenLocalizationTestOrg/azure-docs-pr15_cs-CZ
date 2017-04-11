<properties 
    pageTitle="Cloud Services a správa certifikátů | Microsoft Azure" 
    description="Naučte se vytvářet a používat certifikáty s Microsoft Azure" 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="certificates-overview-for-azure-cloud-services"></a>Přehled certifikátů pro službu Azure Cloud Services
Certifikáty se používají v Azure cloud services ([služby certifikáty](#what-are-service-certificates)) a k rozhraní API (při použití Azure klasické portálem a ne ARM[Správa certifikátů](#what-are-management-certificates) ) pro správu ověřování. Toto téma obsahuje obecné základní informace o obou typů certifikátů postupem [Vytvoření](#create) a [nasazení](#deploy) do Azure.

Certifikáty v Azure jsou x.509 v3 certifikáty a můžete podepsáno jiné důvěryhodnou certifikační nebo může být podepsaný svým držitelem. Certifikát podepsaný svým držitelem podepsaná vlastní poznámkové bloky pro školy a proto, nejsou za důvěryhodné. Většině prohlížečů můžete ignorovat tím. Podepsaný certifikáty lze používat pouze za vás po vývoj a testování cloudovým službám. 

Certifikáty používané Azure může obsahovat osobní nebo veřejným klíčem. Certifikáty mají Miniatura poskytující prostředky k identifikaci jednoznačné způsobem. Tento Miniatura slouží v Azure [konfiguračního souboru](cloud-services-configure-ssl-certificate.md) k identifikaci který certifikát do cloudové služby, používejte. 

## <a name="what-are-service-certificates"></a>Co jsou služby certifikáty?
Služba certifikáty jsou připojené k cloudových služeb a povolení zabezpečená komunikace chcete a od služby. Například pokud jste nainstalovali roli web, je vhodné zadat certifikát, který může ověřovat vystaveného koncový bod HTTPS. Služba certifikáty podle svého definice služby jsou automaticky nasazeny virtuálního počítače, na kterém běží instance vaše role. 

Můžete nahrát certifikáty služby Azure klasické portál buď na portálu Azure klasické nebo pomocí rozhraní API služby správy. Certifikáty Service jsou spojené s konkrétní Cloudová služba a přiřadí nasazení v souboru definice služby.

Certifikáty služby dá se ovládat samostatně z vašich služeb a lze spravovat pomocí různých osob. Vývojář může například uložení balíčku služby, které odkazuje na certifikát, který má správce IT jste předtím nahráli na Azure. Správce IT můžete spravovat a tento certifikát změna konfigurace služby, aniž by musel uložení nového balíčku služby obnovit. Je to možné, protože název logické certifikát a jeho název úložiště a umístění jsou uvedeny v souboru definice služby během miniaturu certifikátu není zadán v souboru konfigurace služby. Aktualizovat certifikát, je pouze nutné nahrát nový certifikát a změňte hodnotu Miniatura v souboru konfigurace služby.

## <a name="what-are-management-certificates"></a>Jaké jsou osvědčení o řízení?
Osvědčení o řízení umožňují ověřování pomocí rozhraní API služby správy poskytovanou Azure klasické. Mnoho programy a nástroje (například Visual Studio nebo Azure SDK) bude používat tyto certifikáty automatické konfigurace a nasazení různé služby Azure. Tyto netýkají skutečně cloudových služeb. 

>[AZURE.WARNING] Dej si pozor! Tyto typy certifikátů povolit každému, kdo ověří s nimi ke správě předplatné, které jsou přidružené. 

### <a name="limitations"></a>Omezení
Existuje omezení 100 Správa certifikátů na jedno předplatné. Je také maximálně 100 certifikáty správy pro všechna předplatná klikněte v části uživatelského ID pro konkrétní službu správce. Pokud je potřeba více certifikátů ID uživatele pro účet správce už byla použita k přidání 100 Správa certifikátů, můžete přidat vedlejšího správce pro přidání dalších certifikátů. 

Před přidáním víc než 100 certifikáty, najdete v článku když můžete znovu použít existující certifikát. Pomocí dalších správců přidá potenciálně nepotřebné složitost proces správy certifikát.


<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Vytvoření nového podepsaného svým držitelem
Můžete použít libovolný nástroj k vytvoření certifikátu podepsaného svým držitelem, dokud dodržovat toto nastavení:

* Certifikát X.509.
* Obsahuje privátním klíčem.
* Vytvoří klíčové exchange (soubor .pfx).
* Název subjektu se musí shodovat doménu použitou pro přístup ke cloudové službě. 
    > Nelze získat doménu SSL certifikátů cloudapp.net (nebo libovolný Azure související); Název subjektu certifikát musí odpovídat vlastní název domény použít pro přístup k aplikaci. Například, **contoso.net** **contoso.cloudapp.net**.
* Minimální 2048bitový šifrování.
* **Služba certifikát pouze**: klientských certifikát musí nacházet v úložišti *osobních* certifikátů.

Existují dva jednoduchých způsobů, jak vytvořit certifikát v systému Windows, s `makecert.exe` nástroj nebo IIS.

### <a name="makecertexe"></a>MakeCert.exe

Tento nástroj byly změněny a už jsou zde uvedeny. Najdete v [tomto článku MSDN](https://msdn.microsoft.com/library/windows/desktop/aa386968) pro další informace.

### <a name="powershell"></a>Prostředí PowerShell

```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

>[AZURE.NOTE] Pokud chcete použít certifikát s adresou IP místo domény, použijte v parametru - Název_dns IP adresu.


Pokud budete chtít použít tento [certifikát pomocí portálu pro správu](../azure-api-management-certs.md), exportovat do souboru **.cer** :

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Internetové informační služby (IIS)

Existuje více stránek na Internetu, které titulních školá s Internetové informační služby. [Tady](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) je skvělý mám, že myslím, že dobře vám to vysvětlí. 

### <a name="java"></a>Java
Java umožňuje [vytvořit certifikát](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate).

### <a name="linux"></a>Linux
[Tento](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) článek popisuje, jak vytvořit certifikáty s SSH.

## <a name="next-steps"></a>Další kroky

[Nahrání certifikát služby Azure klasické portál](cloud-services-configure-ssl-certificate.md) (nebo [Azure portál](cloud-services-configure-ssl-certificate-portal.md)).

Na portálu Azure klasické nahrajte [správy rozhraní API certifikát](../azure-api-management-certs.md) .

>[AZURE.NOTE] Portál Azure nepoužívá správa certifikátů pro přístup k rozhraní API ale raději používá uživatelských účtů.

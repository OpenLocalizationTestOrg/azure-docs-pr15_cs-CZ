<properties 
    pageTitle="Jak vytvořit ILB pomocného mechanismu řízení pomocí Azure šablon správce prostředků | Microsoft Azure" 
    description="Naučte se vytvářet interní zatížení vyrovnávání pomocného mechanismu řízení pomocí Správce prostředků Azure šablon." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-create-an-ilb-ase-using-azure-resource-manager-templates"></a>Jak vytvořit ILB pomocného mechanismu řízení pomocí Azure správce prostředků šablon

## <a name="overview"></a>Základní informace ##
Aplikace služby prostředí lze vytvořit pomocí virtuální sítě interní adresa místo veřejného VIP.  Tato vnitřní adresa není uvedený Azure složkou s názvem Vyrovnávání zatížení interní (ILB).  Pomocného mechanismu řízení ILB lze vytvářet pomocí portálu Azure.  Také vytvořené pomocí automatizace pomocí Správce prostředků Azure šablony.  Tento článek provede kroky a syntaxe potřebné k vytvoření pomocného mechanismu řízení ILB šablonami správce prostředků Azure.

Zahrnuje tři kroky v tématu Vytvoření pomocného mechanismu řízení ILB automatizace:
1. Nejdřív základní pomocného mechanismu řízení vytvořené v síti virtuální zadáním adresy vyrovnávání interní zatížení místo veřejného VIP.  V rámci tohoto kroku název kořenové domény přiřazená pomocného mechanismu ILB řízení.
2. Po vytvoření pomocného mechanismu řízení ILB certifikát SSL nahrané.  
3. Nahrané certifikát SSL explicitně přiřazen pomocného mechanismu řízení ILB jako jeho "Výchozí" certifikát SSL.  Certifikát SSL se použije pro umožnění datových přenosů SSL do aplikace na pomocného mechanismu řízení ILB při aplikace adresami pomocí běžné kořenovou doménu přiřazená pomocného mechanismu řízení (například https://someapp.mycustomrootcomain.com)

## <a name="creating-the-base-ilb-ase"></a>Vytvoření základní ILB pomocného mechanismu řízení ##
Správce prostředků Azure šablonu příklad a související parametry souboru, jsou dostupné na GitHub [tady][quickstartilbasecreate].

Většina parametrů v souboru *azuredeploy.parameters.json* jsou společná pro vytváření ILB ASEs, jak ASEs vázaný na veřejné VIP.  V seznamu pod hovory mimo parametry zvláštní poznámky nebo které jsou jedinečné, při vytváření pomocného mechanismu řízení ILB:


- *interalLoadBalancingMode*: ve většině případů bude vázat nastavení této možnosti se 3, což znamená přenosy protokolu HTTP/HTTPS na porty 80 a 443 i ovládací prvek/data porty channel data službou FTP v pomocný ILB přidělit interní adresy virtuální sítě.  Pokud tato vlastnost je místo toho nastavená na hodnotu 2, pak pouze službu FTP související porty (ovládací prvek a data kanály) bude vázat adresu ILB během přenosy protokolu HTTP/HTTPS zůstane na veřejné VIP.
-  *dnsSuffix*: Tento parametr určuje kořenovou doménu výchozí přiřazené k pomocného mechanismu řízení.  Ve veřejných variantu aplikaci služby Azure kořenová doména výchozí pro všechny webové aplikace je *azurewebsites.net*.  Ale od pomocného mechanismu řízení ILB je interní virtuální sítě zákazníků, nemá smysl používat veřejné služby výchozí kořenovou doménu.  Místo toho pomocného mechanismu řízení ILB má výchozí kořenové domény, který dává smysl pro použití v rámci společnosti interní virtuální sítě.  Hypotetické společnosti Contoso třeba mohly využít kořenovou doménu výchozí *interní* contoso.com ve aplikace, které mají být pouze možností vyřešení přístupných osobám s postižením ve společnosti Contoso virtuální síti. 
-  *ipSslAddressCount*: Tento parametr se automaticky převezme hodnotu 0 v souboru *azuredeploy.json* , protože ILB ASEs k dispozici pouze jeden ILB adresu.  Nejsou žádné explicitní IP SSL adresy pro pomocný ILB a tedy fondu IP SSL adres pro pomocného mechanismu řízení ILB musí být nastavený na nulu, jinak dojde k chybě zřizovací. 

Jakmile soubor *azuredeploy.parameters.json* bylo vyplněno pro pomocného mechanismu řízení ILB, pomocného mechanismu řízení ILB bude možné vytvořit pomocí následující fragment kódu Powershellu.  Změna souboru cesty podle umístění soubory šablon Azure správce ve vašem počítači.  Nezapomeňte také zadat vlastní hodnoty pro správce prostředků Azure nasazení název a název skupiny prostředků.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Po správce prostředků Azure šablony se odešle, že bude trvat několik hodin pomocného mechanismu řízení ILB vytvořit.  Po dokončení vytváření pomocného mechanismu řízení ILB se objeví v portálu činnosti koncového uživatele v seznamu aplikací služby prostředí pro předplatné, který spustil nasazení.

## <a name="uploading-and-configuring-the-default-ssl-certificate"></a>Nahrávání a konfigurace certifikátu SSL "Výchozí" ##

Po vytvoření pomocného mechanismu řízení ILB jako "Výchozí" certifikát SSL použít pro vytváření připojení SSL k aplikacím by měl být přidružené k pomocného mechanismu řízení certifikát SSL.  Pokračováním hypotetické společnosti Contoso, aby příklad, pokud pomocného mechanismu řízení výchozí DNS je přípona *vnitřní contoso.com*, pak připojení k *https://some-random-app.internal-contoso.com* vyžaduje certifikát SSL, který je platný pro **.internal contoso.com*. 

Existuje několik způsobů, jak získat platný certifikát SSL včetně interních čas zakoupení u externích Vystavitel certifikátu a pomocí certifikátu podepsaného svým držitelem.  Bez ohledu na zdroj certifikát SSL následující atributy certifikát třeba správně nakonfigurovat:

- *Předmět*: Tenhle atribut musí být nastavena na **.your kořenovou doménu here.com*
- *Alternativní název předmětu*: Tenhle atribut musí obsahovat obě * *.your kořenovou doménu here.com*a * *.scm.your-kořenové-domény-here.com*.  Důvod druhá položka je, že připojení SSL na stránku Správce služeb nebo Kudu přidružený k každé aplikaci budou použitím adresy formuláři *your-app-name.scm.your-root-domain-here.com*.

S platný certifikát SSL ruku je třeba dvou další přípravných kroků.  Certifikát SSL musí být převeden/uloží jako soubor .pfx.  Mějte na paměti, že soubor .pfx je potřeba zahrnout všechny intermediate a root certificates a také musí být zabezpečený pomocí hesla.

Potom soubor .pfx výsledné musí převedou na řetězec ve formátu Base 64, protože certifikát SSL budou odeslány pomocí šablony správce prostředků Azure.  Od správce prostředků Azure šablony se soubory ve formátu RTF, soubor .pfx musí být převedený řetězec ve formátu Base 64, může být zahrnuta jako parametr šablony.

Fragment kódu prostředí Powershell dole vidíte příklad generování certifikátu podepsaného svým držitelem, Export certifikátu jako soubor .pfx převod soubor .pfx ve formátu Base 64 zakódovaný řetězce a následným uložením ve formátu Base 64 zakódovaný řetězec do samostatného souboru.  Kód prostředí Powershell kódování base64 byl upraven z [Blogu skriptů Powershellu][examplebase64encoding].

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     
    
    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")
    
Jakmile certifikát SSL byl úspěšně generovaného a převedeny na base64 zakódovaný řetězec, například správce prostředků Azure šablonu na GitHub [Konfigurace certifikátu SSL výchozí] [ configuringDefaultSSLCertificate] lze použít.

Parametry v souboru *azuredeploy.parameters.json* jsou uvedeny níže:

- *appServiceEnvironmentName*: název ILB pomocného mechanismu řízení konfiguruje.
- *existingAseLocation*: textový řetězec obsahující Azure oblasti, kde pomocného mechanismu řízení ILB nasazené.  Příklad: "Jih centrální nám".
- *pfxBlobString*: based64 zakódovaný řetězec představující soubor .pfx.  Použití kódu uvedené výše, by řetězec obsažený v "exportedcert.pfx.b64" zkopírovat a vložit ho do jako hodnotu atributu *pfxBlobString* .
- *heslo*: heslo používá k zabezpečení soubor .pfx.
- *Miniatura certifikátu*: Miniatura certifikátu.  Pokud tuto hodnotu načtete z prostředí Powershell (například *$certificate. Miniatura* z předchozích fragment kódu), můžete použít hodnotu jako-je.  Ale pokud zkopírujte hodnotu z dialogové okno certifikát Windows nezapomeňte odstranit nadbytečné mezery.  *Miniatura certifikátu* by měl vypadat nějak: AF3143EB61D43F6727842115BB7F17BBCECAECAE
- *certificateName*: identifikátor popisný řetězec vlastní používá pro identitu certifikátu.  Název se použije jako součást jedinečný identifikátor správce prostředků Azure entity *Microsoft.Web/certificates* představující certifikát SSL.  Název **musí** konci následujících přípon: \_yourASENameHere_InternalLoadBalancingASE.  Tato přípona slouží portálem jako indikátor, že certifikát se používá k zabezpečení podporou ILB pomocného mechanismu řízení.


Příklad zkrácený *azuredeploy.parameters.json* je uveden níže:


    {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
              "appServiceEnvironmentName": {
                   "value": "yourASENameHere"
              },
              "existingAseLocation": {
                   "value": "East US 2"
              },
              "pfxBlobString": {
                   "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
              },
              "password": {
                   "value": "PASSWORDGOESHERE"
              },
              "certificateThumbprint": {
                   "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
              },
              "certificateName": {
                   "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
              }
         }
    }

Jakmile vyplněná souboru *azuredeploy.parameters.json* certifikát SSL výchozí je možné konfigurovat pomocí následující fragment kódu Powershellu.  Změna souboru cesty podle umístění soubory šablon Azure správce ve vašem počítači.  Nezapomeňte také zadat vlastní hodnoty pro správce prostředků Azure nasazení název a název skupiny prostředků.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Po odeslání šablony správce prostředků Azure, že ho bude trvat zhruba 40 minut za pomocného mechanismu řízení front-end změnu potvrďte.  Například s výchozí velikosti pomocný pomocí dvou front-end šablona bude trvat zhruba jednu hodinu a dvaceti minut dokončení.  Je spuštěná šabloně pomocného mechanismu řízení nebude možné měnit jejich velikost.  

Po dokončení šablony aplikací na pomocného mechanismu řízení ILB můžete k nim získat přístup prostřednictvím HTTPS a připojení se zabezpečit pomocí výchozího certifikátu SSL.  Certifikát SSL výchozím nastavení se použije při aplikací na pomocného mechanismu řízení ILB adresami pomocí kombinace název aplikace a výchozí název hostitele.  Příklad *https://mycustomapp.internal-contoso.com* byste měli použít výchozí certifikát SSL, pro **.internal contoso.com*.

Však stejně jako aplikace spuštěna veřejné služby více klienta vývojáři můžete taky nakonfigurovat vlastní hostitele pro jednotlivé aplikace a potom i konfiguraci jedinečné vazby certifikátu SNI SSL pro jednotlivé aplikace.  


## <a name="getting-started"></a>Začínáme

Začínáme s prostředím služby aplikace, najdete v článku [Úvod do prostředí aplikace služby](app-service-app-service-environment-intro.md)

Všechny články a jak-pro uživatele pro aplikaci služby prostředí jsou k dispozici v [souboru README pro aplikaci služby prostředí](../app-service/app-service-app-service-environments-readme.md).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 
 

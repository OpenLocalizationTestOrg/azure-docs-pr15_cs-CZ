<properties 
    pageTitle="Přidání certifikátu do úložiště Java CA | Microsoft Azure" 
    description="Naučte se přidávat certifikát certifikační autorita (CA) Java (cacerts) úložiště certifikátů pro službu Twilio nebo Bus služby Azure." 
    services="" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="adding-a-certificate-to-the-java-ca-certificates-store"></a>Přidání certifikátu do úložiště certifikátů CA Java
Podle těchto kroků ukazují, jak přidáte úložiště certifikátů (cacerts) Java certifikát certifikační autorita (CA). V příkladu používají určen certifikační požadované službou Twilio. Informace v tématu popisuje, jak nainstalovat certifikát certifikační Autority pro Bus služby Azure. 

Keytool slouží k přidání certifikační před kompresi vaší JDK a pak ho přidejte do složky **approot** Azure projektu, nebo můžete spustit Azure počáteční úkol, který používá keytool k přidání certifikátu. V tomto příkladu předpokládá, že přidáte certifikační před JDK je ZIP. Také určitý certifikát CA se použije v příkladu, ale kroky pro získání různých certifikační a tohoto souboru importujete do úložiště cacerts by být stejný.

## <a name="to-add-a-certificate-to-the-cacerts-store"></a>Přidání certifikátu do úložiště cacerts

1. Na příkazovém řádku, která je nastavená jako vaše JDK **jdk\jre\lib\security** složky spusťte podle následujících pokynů zjištění, jaké certifikáty jsou nainstalovány:

    `keytool -list -keystore cacerts`

    Zobrazí se výzva k zadání hesla úložiště přihlašovacích údajů. Výchozí heslo je **changeit**. (Pokud chcete změnit heslo, najdete v dokumentaci keytool na <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.) Tento příklad předpokládá, že certifikát s MD5 identifikovat 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 nenajdete a, který chcete importovat (určitý certifikát je nutný pro službu Twilio rozhraní API).
2. Získejte certifikát ze seznamu certifikátů uvedený v části [GeoTrust kořenových certifikátů](http://www.geotrust.com/resources/root-certificates/). Klikněte pravým tlačítkem na odkaz na certifikát s 35:DE:F4:CF pořadové číslo a uložte ho do složky **jdk\jre\lib\security** . Pro účely v tomto příkladě se jste ho uložili do souboru s názvem **Equifax\_zabezpečené\_certifikát\_Authority.cer**.
3. Importujte certifikátu prostřednictvím tento příkaz:

    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`

    Po zobrazení výzvy k zabezpečení certifikát, pokud certifikát má MD5 otisku 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 odpovědět zadáním **y**.
4. Spusťte tento příkaz zajistěte, aby že certifikát byl úspěšně importován:

    `keytool -list -keystore cacerts`

5. ZIP JDK a přidejte ji do složky **approot** Azure projektu.

Informace o keytool najdete v tématu <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.

## <a name="azure-root-certificates"></a>Azure kořenových certifikátů

Aplikace, které používají Azure služby (například Bus služby Azure) nutné s informacemi o důvěryhodnosti Baltimore CyberTrust kořenového certifikátu. (Na začátku 15 duben 2013 Azure zahájení migrace z GTE CyberTrust globální Root Baltimore CyberTrust Root. Tento migrace pořídili pomocí několik měsíců dokončete.)

Baltimore certifikát může být už nainstalovaný v úložišti cacerts to vzpomenout spustit **keytool – seznam** příkaz nejdřív, čímž se zobrazí, pokud již existuje.

Pokud potřebujete přidat Baltimore CyberTrust Root, má 02:00:00:b9 pořadové číslo a SHA1 otisku d4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74. Je můžete stáhnout z <https://cacert.omniroot.com/bc2025.crt>, uloží do místní soubor s příponou **.cer**a importu pomocí **keytool** uvedené výše.

## <a name="next-steps"></a>Další kroky

Další informace o kořenových certifikátů používaný Azure najdete v článku [Azure kořenového certifikátu migrace](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).

Další informace o Java naleznete v článku [Středisko pro vývojáře Java](/develop/java/).

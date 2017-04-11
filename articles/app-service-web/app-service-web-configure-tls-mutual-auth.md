<properties 
    pageTitle="Postup při konfiguraci TLS vzájemné ověřování pro Web App" 
    description="Informace o konfiguraci webové aplikace pomocí ověřování klientských certifikátů na TLS." 
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/08/2016" 
    ms.author="naziml"/>    

# <a name="how-to-configure-tls-mutual-authentication-for-web-app"></a>Postup při konfiguraci TLS vzájemné ověřování pro Web App

## <a name="overview"></a>Základní informace ##
Omezit přístup k Azure webovou aplikaci povolením různých typů ověřování pro něj. Jedním ze způsobů tak je ověření pomocí certifikátu klienta po zadání TLS/SSL. Tento postup se nazývá vzájemné ověřování TLS nebo certifikát klienta, že je ověřování a v tomto článku se podrobností jak nastavit webové aplikace pomocí ověřování certifikátu klienta.

> **Poznámka:** Pokud máte přístup k webu přes HTTP a ne HTTPS nebudou každý klientský certifikát. Tak pokud aplikace vyžaduje certifikátů neměli povolte žádosti o aplikaci přes HTTP.


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="configure-web-app-for-client-certificate-authentication"></a>Konfigurace webové aplikace pro ověřování klientských certifikátů ##
Chcete-li nastavit webovou aplikaci vyžadují certifikáty klienta, abyste mohli přidat nastavení clientCertEnabled webů pro web app a nastavena na hodnotu true. Toto nastavení není momentálně dostupné prostřednictvím možnostem správy na portálu a rozhraní REST API bude nutné použít k tomu.

[Nástroj ARMClient](https://github.com/projectkudu/ARMClient) můžete snadno vytvořit volání rozhraní REST API. Po přihlášení pomocí nástroje pro musíte tento příkaz:

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose
    
nahrazení všechno na {} informace pro web app a vytvoření souboru s názvem enableclientcert.json s následující JSON obsahu:

> {"umístění": "Moje webové aplikace umístění"   
>   "vlastnosti": {  
>     "clientCertEnabled": PRAVDA}}  

Zkontrolujte, že tam, kde je umístěn webovou aplikaci změňte hodnotu "umístění" například severní centrální nás nebo západní USA atd.

> **Poznámka:** Pokud spustíte ARMClient z prostředí Powershell, budete muset uniknout @ symbol JSON souboru s zpět popisky ".

## <a name="accessing-the-client-certificate-from-your-web-app"></a>Přístup k certifikát klienta z Web Appu ##
Pokud používáte ASP.NET a konfigurace aplikace pomocí ověřování certifikátu klienta, budou dostupné prostřednictvím vlastnost **HttpRequest.ClientCertificate** certifikát. Pro jiné aplikace štosech bude k dispozici v aplikaci prostřednictvím hodnotu ve formátu Base 64 zakódovaný v záhlaví "X ARR ClientCert" žádost o certifikátu klienta. Aplikaci můžete vytvořit certifikát od této hodnoty a potom ji použijte pro účely ověření a povolení aplikace.

## <a name="special-considerations-for-certificate-validation"></a>Důležité informace k ověření certifikátů ##
Klient certifikát, který se odešle žádost nebudou procházet jakékoli ověřování tak, že platformu Azure Web Apps. Ověření certifikát zodpovídá ve web appu. Tady je ukázka ASP.NET kód, který ověří vlastnosti certifikátů pro účely ověření.

    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;
                
                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;
                
                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                // If you also want to test if the certificate chains to a Trusted Root Authority you can uncomment the code below
                //
                //X509Chain certChain = new X509Chain();
                //certChain.Build(certificate);
                //bool isValidCertChain = true;
                //foreach (X509ChainElement chElement in certChain.ChainElements)
                //{
                //    if (!chElement.Certificate.Verify())
                //    {
                //        isValidCertChain = false;
                //        break;
                //    }
                //}
                //if (!isValidCertChain) return false;

                return true;
            }
        }
    }

<properties
    pageTitle="Poradce při potížích s sníženou kvalitu stavu na Azure přenosy správce"
    description="Jak řešit problémy s profily přenosy správce, když se zobrazí jako sníženou kvalitu stav."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Poradce při potížích s sníženou kvalitu stavu na Azure přenosy správce

Tento článek popisuje, jak řešit problémy s Azure přenosy správce profil, který je zobrazený jejich funkčnost bude omezena stav. V tomto scénáři vzít v úvahu, že jste nakonfigurovali profil správce přenosy přejdete na některé z vašich služeb cloudapp.net hostované. Při kontrole stavu Správce přenosy uvidíte, že má sníženou kvalitu stav.

![jejich funkčnost bude omezena stavu](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degraded.png)

Pokud přejdete na kartu koncové body tohoto profilu, zobrazí jednu nebo více koncové body se stavem Offline:

![v režimu offline](./media/traffic-manager-troubleshooting-degraded/traffic-manager-offline.png)

## <a name="understanding-traffic-manager-probes"></a>Vysvětlení přenosy správce sond

- Přenosy správce byly použity koncový bod pouze v případě, že zkušební přijímání odpověď HTTP 200 zpět zkušební cesta být ONLINE. Jiné než 200 odpovědi je selhání.
- Přesměrování 30 x nepovede, i když přesměrování adresy URL vrátí 200.
- Pro HTTPs sond chyb certifikátu ignorovány.
- Jeho obsah dráhy zkušební nezáleží, dokud je vrácena 200. Zjišťování adresy URL některé statický obsah jako "/ favicon.ico" je běžné postup. Dynamický obsah, například stránky ASP nemusí vždy vrátí 200, i když máte správný aplikace.
- Doporučený postup je cestu nastavte na zkušební něco, co má dost logiku zjistíte, že je webu nahoru nebo dolů. V předchozím příkladu, nastavením cestu "/ favicon.ico", jsou pouze testování této w3wp.exe odpovídá. Tento zkušební nemusí, který označuje, že webové aplikace správný. Lepší možnosti bude pro nastavit cesty něco jako "/ Probe.aspx" s cílem určit stavu webu. Můžete třeba použít výkonnosti k využití procesoru nebo změřit počet neúspěšných požadavků. Nebo může pokusí o přístup k databázi zdrojů nebo stav relace a ujistěte se, zda je funkční webové aplikace.
- Pokud jsou sníženou kvalitu. všechny koncové body v profilu, pak přenosy správce zpracovávat všechny koncové body jako správný a trasy přenosy na všechny koncové body. Toto chování zajišťuje, že problémy s zjišťování mechanismus za následek dokončení výpadku služby.

## <a name="troubleshooting"></a>Řešení potíží

Poradce při potížích s selhání zkušební, je nutné nástroj, který se zobrazí stavový kód HTTP zpáteční z adresy URL zkušební. Jsou k dispozici mnoho nástroje, které zobrazit jako nezpracovaná odpověď HTTP.

* [Fiddler](http://www.telerik.com/fiddler)
* [Otočení](https://curl.haxx.se/)
* [wget](http://gnuwin32.sourceforge.net/packages/wget.htm)

Také můžete kartu síť nástroje ladění F12 v Internet Exploreru zobrazit odpovědi HTTP.

V tomto příkladu jsme chcete vidět odpověď adresa URL zkušební: http://watestsdp2008r2.cloudapp.net:80/zkušební. Následující příklad prostředí PowerShell ukazuje problém.

```powershell
    Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Příklad výstupu:

```text
    StatusCode StatusDescription
    ---------- -----------------
            301 Moved Permanently
```

Všimněte si, že jsme přijata odpověď na přesměrování. Jak je uvedeno všechny StatusCode dříve, než 200 je považován za se nepovede. Přenosy správce stav koncový bod se změní na Offline. Problém vyřešíte Kontrola konfigurace webu zajistit, že správné StatusCode může být vrácen z zkušební cesty. Změna konfigurace zkušební provoz správce tak, aby ukazovaly na cestu, která vrací 200.

Pokud vaše zkušební používá protokol HTTPS, budete muset zakázat ověřování vyhnout se chybám SSL/TLS vaší při zkoušce certifikátů. Následující příkazy Powershellu zakázat ověřovací certifikát pro aktuální relaci Powershellu:

```powershell
    add-type @"
    using System.Net;
    using System.Security.Cryptography.X509Certificates;
    public class TrustAllCertsPolicy : ICertificatePolicy {
        public bool CheckValidationResult(
        ServicePoint srvPoint, X509Certificate certificate,
        WebRequest request, int certificateProblem) {
        return true;
        }
    }
    "@
    [System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy
```

## <a name="next-steps"></a>Další kroky

[O směrování metod přenosy přenosy správce](traffic-manager-routing-methods.md)

[Co je přenosy správce](traffic-manager-overview.md)

[Cloud Services](http://go.microsoft.com/fwlink/?LinkId=314074)

[Azure Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/)

[Operace na přenosy správce (REST API odkazy)](http://go.microsoft.com/fwlink/?LinkId=313584)

[Azure rutin přenosy správce][1]

[1]: https://msdn.microsoft.com/library/mt125941(v=azure.200).aspx

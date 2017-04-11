<properties
   pageTitle="Vytvoření vlastních záznamů DNS pro web app | Microsoft Azure  "
   description="Jak vytvořit vlastní domény pro web app pomocí služby Azure DNS záznamy DNS."
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a>Vytvoření záznamů DNS pro web app ve vlastní doméně

Azure DNS můžete hostovat vlastní domény pro váš web apps. Například vytváříte Azure webovou aplikaci a mají uživatelé přístup k některým pomocí contoso.com nebo www.contoso.com jako úplný název domény.

K tomuto účelu budete muset vytvořte dva záznamy:

- Záznam root "A" ukazujícím na contoso.com
- Záznam "CNAME" www název odkazující na záznam

Mějte na paměti, že pokud vytváříte záznamu a pro web app v Azure záznam musí být ručně aktualizovat Pokud podkladového IP adres pro změny web app.

## <a name="before-you-begin"></a>Než začnete

Než začnete, musíte nejdřív vytvořit zóny DNS v Azure DNS a delegovat zóny registrátora vaší Azure DNS.

1. Pokud chcete vytvořit zóny DNS, postupujte podle pokynů v tématu [vytvoření zóny DNS](dns-getstarted-create-dnszone.md).
2. Delegování DNS Azure DNS, postupujte podle pokynů v [delegování DNS domény](dns-domain-delegation.md).

Po vytvoření zóně a delegování Azure DNS, můžete pak vytvoříte záznamy pro vaši vlastní doménu.


## <a name="1-create-an-a-record-for-your-custom-domain"></a>1. Vytvořte záznam pro vlastní doménu

Záznam se používá k mapování názvů na IP adresu. V následujícím příkladu jsme přiřadit @ jako záznam a, kterým adresy IPv4:

### <a name="step-1"></a>Krok 1

Vytvořte záznam a přiřadit proměnné $rs

    $rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600

### <a name="step-2"></a>Krok 2

Přidání hodnotu IPv4 dříve vytvořené sadě záznamů "@" pomocí $rs proměnné přiřazené. Hodnota IPv4 přiřazené bude na IP adresu pro web app.

Zjistěte IP adresu pro web app, postupujte podle pokynů v [článku konfigurace vaší vlastní doménou v aplikaci služby Azure](../web-sites-custom-domain-name.md#Find-the-virtual-IP-address).

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address <your web app IP address>

### <a name="step-3"></a>Krok 3

Potvrďte změny provedené v sadě záznamů. Použití `Set-AzureRMDnsRecordSet` nahrajete změny sady Azure DNS záznamů:

    Set-AzureRMDnsRecordSet -RecordSet $rs

## <a name="2-create-a-cname-record-for-your-custom-domain"></a>2. Vytvořte záznam CNAME pro vlastní doménu

Pokud svoji doménu se už spravuje Azure DNS (viz [delegování DNS domény](dns-domain-delegation.md), můžete použít následující příklad vytvořit záznam CNAME pro contoso.azurewebsites.net.

### <a name="step-1"></a>Krok 1

Otevřete PowerShell a vytvořte novou sadu záznam CNAME a přiřadit proměnné $rs. Tento příklad vytvoří typu Sada záznamů CNAME s "time to live" 600 sekund v zóně DNS s názvem "contoso.com".

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Krok 2

Po vytvoření sady záznamů CNAME potřebujete k vytvoření aliasu hodnotu, která bude přejděte na web appu.

Pomocí přiřazeného proměnné "$rs" můžete příkazu Powershellu vytvořit alias pro web app contoso.azurewebsites.net.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Krok 3

Potvrďte změny pomocí `Set-AzureRMDnsRecordSet` rutinu:

    Set-AzureRMDnsRecordSet -RecordSet $rs

Ověření záznamu vytvořil správně dotazování "www.contoso.com" pomocí nástroje nslookup, jak je ukázáno v následujícím příkladu:

    PS C:\> nslookup
    Default Server:  Default
    Address:  192.168.0.1

    > www.contoso.com
    Server:  default server
    Address:  192.168.0.1

    Non-authoritative answer:
    Name:    <instance of web app service>.cloudapp.net
    Address:  <ip of web app service>
    Aliases:  www.contoso.com
    contoso.azurewebsites.net
    <instance of web app service>.vip.azurewebsites.windows.net

## <a name="create-an-awverify-record-for-web-apps"></a>Vytvoření záznamu "awverify" pro webové aplikace


Pokud se rozhodnete používat záznamu a pro webovou aplikaci, musíte přejít do proces ověření zajistit, že doménu opravdu vlastníte. Tento krok ověření probíhá vytvořením zvláštní záznam CNAME s názvem "awverify". Tato část se týká jenom záznamy.


### <a name="step-1"></a>Krok 1

Vytvořte záznam "awverify". V následujícím příkladu budeme vytvářet záznam "aweverify" pro contoso.com k ověření vlastnictví pro vlastní doménu.

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "awverify" -RecordType "CNAME" -Ttl 600

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Krok 2

Po vytvoření sada záznamů "awverify" přiřadíte záznam CNAME alias nastavení. V následujícím příkladu jsme přiřadí záznam CNAMe alias nastavit awverify.contoso.azurewebsites.net.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Krok 3

Potvrďte změny pomocí `Set-AzureRMDnsRecordSet cmdlet`, jak je vidět na následující odkaz.

    Set-AzureRMDnsRecordSet -RecordSet $rs



## <a name="next-steps"></a>Další kroky

Postupujte podle kroků v tématu [Konfigurace vlastního názvu domény pro aplikaci služby](../app-service-web/web-sites-custom-domain-name.md) pro nastavení web appu používat vlastní doménu.









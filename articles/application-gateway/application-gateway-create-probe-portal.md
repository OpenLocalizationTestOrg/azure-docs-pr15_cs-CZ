<properties
   pageTitle="Vytvoření vlastní zkušební aplikace brány pomocí portálu | Microsoft Azure"
   description="Naučte se vytvářet vlastní zkušební aplikace brány tak, že na portálu"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-application-gateway-by-using-the-portal"></a>Vytvoření vlastní zkušební pro bráně aplikace pomocí portálu

> [AZURE.SELECTOR]
- [Azure portálu](application-gateway-create-probe-portal.md)
- [Azure prostředí PowerShell správce prostředků](application-gateway-create-probe-ps.md)
- [Azure klasické prostředí PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

## <a name="scenario"></a>Scénář

Následující scénář prochází vytváření vlastních stavu zkušební v existující brány aplikace.
Scénář předpokládá, že už jste postupovali podle kroků k [Vytvoření bráně aplikace](application-gateway-create-gateway-portal.md).

## <a name="createprobe"></a>Vytvoření zkušební

Sond nakonfigurovaná ve dvou krocích prostřednictvím portálu. Cílem prvního kroku je vytvořit zkušební, potom přidat zkušební back-end http nastavení brány aplikace.

### <a name="step-1"></a>Krok 1

Přejděte na http://portal.azure.com a vyberte existující bráně aplikace.

![Přehled bráně aplikace][1]

### <a name="step-2"></a>Krok 2

Klikněte na **sond** a klikněte na tlačítko **Přidat** přidat nové zkušební.

![Přidání zkušební zásuvné s informacemi o vyplněnými][2]

### <a name="step-3"></a>Krok 3

Vyplňte požadované informace pro zkušební a až budete hotovi, klikněte na tlačítko **OK**.

- **Název** - popisný název pro zkušební přístupný na portálu.
- **Host (hostitel)** – to je název hostitele, který se používá k zkušební. Použít pouze v případě více webů konfigurované ve bráně aplikace, jinak používat "127.0.0.1". Tím se liší od OM název hostitele.
- **Cesta** - zbytek úplnou adresu url pro vlastní zkušební. Platnou cestu začíná na "/"
- **Interval (sekundy)** – jak často zkušební spustit Pokud chcete zkontrolovat stav. Se nedoporučuje, pokud chcete nastavit nižší než 30 sekund.
- **Časový limit (sekundy)** - dobu zkušební čekání před vypršení časového limitu. Časový limit je třeba dostatečně vysoké, pozvání http lze zajistit že stránka back-end stavu je k dispozici.
- **Chybná prahová** - počet neúspěšných pokusů považuje za nebezpečné. Mezní hodnota 0 znamená, že pokud zdravotní políčko dojde k chybě back-end závisí chybná okamžitě.

> [AZURE.IMPORTANT] název hostitele není názvu serveru. To je název virtuální hostiteli na aplikační server. Zkušební odeslaný do http://(host name):(port from httpsetting)/urlPath

![nastavení konfigurace zkušební][3]

## <a name="add-probe-to-the-gateway"></a>Zkušební opatřovat brány

Teď, když zkušební byl vytvořen, je čas na Přidat do brány. Zkušební nastavení na nastavení http back-end bráně aplikace.

### <a name="step-1"></a>Krok 1

Klikněte na možnost **Nastavení HTTP** brány aplikace a potom klikněte na aktuální back-end http nastavení v okně vyvoláte zásuvné konfigurace.

![protokol HTTPS v okně Nastavení][4]

### <a name="step-2"></a>Krok 2

Na zásuvné **appGatewayBackEndHttp** nastavení klikněte na **použít vlastní zkušební** a zvolte zkušební vytvořené v části [Vytvoření zkušební](#createprobe) .
Až budete hotovi, klikněte na tlačítko **OK** a použijí se nastavení.

![nastavení zásuvné appgatewaybackend][5]

Výchozí zkušební zkontroluje výchozí přístup k webové aplikace. Teď vytvořil vlastní zkušební brány aplikace používá vlastní cesty definované sledování stavu pro back-end vybrané. Na základě kritérií, která byla definována, bráně aplikace zkontroluje souboru zadaného v zkušební. Pokud volání Host (hostitel): Port / cestu nevrátí odpověď Stav Http 200 OK, server, se považuje mimo otočení v prostoru, po dosažení chybná prahová hodnota. Zjišťování pokračuje na chybná instance zjistit, kdy z něj stal správný znovu. Jakmile instance přibude zpět do fondu server v pořádku začíná přenos znovu pokračuje a zjišťování instanci pokračuje na určeného časového intervalu uživatele jako normálně.


## <a name="next-steps"></a>Další kroky

Naučíte se konfigurovat SSL odstranění Azure aplikace brána najdete v článku [Konfigurace převzít SSL](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png
[3]: ./media/application-gateway-create-probe-portal/figure3.png
[4]: ./media/application-gateway-create-probe-portal/figure4.png
[5]: ./media/application-gateway-create-probe-portal/figure5.png
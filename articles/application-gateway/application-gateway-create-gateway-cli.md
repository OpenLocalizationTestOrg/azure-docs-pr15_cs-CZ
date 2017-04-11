<properties
   pageTitle="Vytvoření brány aplikační pomocí rozhraní příkazového řádku Azure správce prostředků | Microsoft Azure"
   description="Naučte se vytvářet aplikace brány pomocí rozhraní příkazového řádku Azure ve Správci zdrojů"
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

# <a name="create-an-application-gateway-by-using-the-azure-cli"></a>Vytvoření brány aplikační pomocí rozhraní příkazového řádku Azure

> [AZURE.SELECTOR]
- [Azure portálu](application-gateway-create-gateway-portal.md)
- [Azure prostředí PowerShell správce prostředků](application-gateway-create-gateway-arm.md)
- [Azure klasické prostředí PowerShell](application-gateway-create-gateway.md)
- [Azure správce prostředků šablony](application-gateway-create-gateway-arm-template.md)
- [Azure rozhraní příkazového řádku](application-gateway-create-gateway-cli.md)

Azure aplikace brána je Vyrovnávání zatížení vrstvy-7. Poskytuje selhání směrování výkonu požadavků HTTP mezi jiné servery, ať už jde o v cloudu a místní. Aplikace brány má následující funkce aplikace doručení: HTTP načtení vyrovnávání spřažení na základě souborů cookie relace a převedení Secure (Sockets Layer SSL) sond vlastní stavu a podporu více webu.

## <a name="prerequisite-install-the-azure-cli"></a>Předpoklady: Instalace Azure rozhraní příkazového řádku

Abyste mohli provést kroky v tomto článku, budete potřebovat k [instalaci rozhraní Azure příkazového řádku pro Mac a Linux, systému Windows (Azure rozhraní příkazového řádku)](../xplat-cli-install.md) a budete muset [přihlásit ke Azure](../xplat-cli-connect.md). 

> [AZURE.NOTE] Pokud nemáte účet Azure, musíte jednu. Přejděte znaménko [tady bezplatnou zkušební verzi](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Scénář

V tomto scénáři je Naučte se vytvářet aplikace brány pomocí portálu Azure.

Tento scénář udělejte toto:

- Vytvoření brány pro střední aplikace s dvě instance.
- Vytvořte virtuální sítě s názvem AdatumAppGatewayVNET s rezervovaná CIDR blokem 10.0.0.0/16.
- Vytvoření podsítě s názvem Appgatewaysubnet používající 10.0.0.0/28 jako jeho blokování CIDR.
- Konfigurace certifikátu SSL převedení.

![Příklad scénáře][scenario]

>[AZURE.NOTE] Další konfiguraci brány aplikace, včetně vlastní stavu sond, back-end fondu adresy a další pravidla nakonfigurované po konfiguraci brány aplikace a nikoli během počáteční nasazení.

## <a name="before-you-begin"></a>Než začnete

Azure brány aplikace vyžadují vlastní podsítě. Při vytváření virtuální síť, zajistěte ponechejte dostatek místa adresu mít několik podsítí. Po nasazení aplikace brány do podsítě pouze další aplikace bran se přidají do podsítě.

## <a name="log-in-to-azure"></a>Přihlaste se k Azure

Otevřete **Microsoft Azure příkazový řádek**a přihlaste se. 

    azure login

Když zadáte v předchozím příkladu, je k dispozici kódu. Přejděte na https://aka.ms/devicelogin v prohlížeči pokračujte proces přihlášení.

![cmd zobrazující zařízení přihlášení][1]

V prohlížeči zadejte kód, který jste dostali. Budete přesměrováni na přihlašovací stránku.

![prohlížeče zadejte kód][2]

Jakmile zadá kód jste přihlášení, zavřete prohlížeč pokračujte na scénáře.

![úspěšně přihlášení][3]

## <a name="switch-to-resource-manager-mode"></a>Přepněte do režimu správce prostředků

    azure config mode arm

## <a name="create-the-resource-group"></a>Vytvoření skupiny zdrojů

Před vytvořením aplikace brány, skupina zdroje je vytvořeno má být aplikace brány Na následujícím obrázku je příkaz.

    azure group create -n AdatumAppGatewayRG -l eastus

## <a name="create-a-virtual-network"></a>Vytvořit virtuální sítě

Po vytvoření skupiny zdrojů virtuální sítě se vytvoří brány aplikace.  V následujícím příkladu byl adresní prostor jako 10.0.0.0/16 podle předchozího scénář poznámky.

    azure network vnet create -n AdatumAppGatewayVNET -a 10.0.0.0/16 -g AdatumAppGatewayRG -l eastus

## <a name="create-a-subnet"></a>Vytvoření podsítě

Po vytvoření virtuální sítě podsítě přibude brány aplikace.  Pokud chcete používat bránu aplikací pomocí webové aplikace hostované ve stejné síti virtuální jako brány aplikace, ujistěte se, že jste ponechte dostatek místa pro jiné podsítě.

    azure network vnet subnet create -g AdatumAppGatewayRG -n Appgatewaysubnet -v AdatumAppGatewayVNET -a 10.0.0.0/28 

## <a name="create-the-application-gateway"></a>Vytvoření brány pro aplikace

Po vytvoření virtuální sítě a podsítě dokončeny předpoklady brány aplikace. Dále jsou potřeba pro následující krok dříve exportovaného .pfx certifikát a heslo pro potvrzení: IP adresy používané pro back-end jsou IP adresy back-end serveru. Tyto hodnoty může být privátní IP adresy v virtuální sítě, veřejnou IP adresy nebo plně kvalifikované názvy domén back-end serverů.

    azure network application-gateway create -n AdatumAppGateway -l eastus -g AdatumAppGatewayRG -e AdatumAppGatewayVNET -m Appgatewaysubnet -r 134.170.185.46,134.170.188.221,134.170.185.50 -y c:\AdatumAppGateway\adatumcert.pfx -x P@ssw0rd -z 2 -a Standard_Medium -w Basic -j 443 -f Enabled -o 80 -i http -b https -u Standard

> [AZURE.NOTE] Seznam parametrů, které lze zadat při vytváření spusťte tento příkaz: **aplikace brána azure sítě vytvořit – Nápověda**.

Tento příklad vytvoří brány základní aplikace s výchozím nastavením pro posluchače, back-end fondu, nastavení protokolu http back-end a pravidla. Konfiguruje taky převedení SSL. Můžete změnit nastavení tak, aby vyhovovala nasazení po úspěšném zřizování.
Pokud už máte web aplikace tato pole definovány fondu back-end v předchozích krocích po vytvoření Vyrovnávání zatížení, nebude zahájen.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak se dá vytvořit vlastní stav sond návštěva [vytvořit vlastní stav zkušební](application-gateway-create-probe-portal.md)

Zjistěte, jak konfigurovat odstranění SSL a udělejte nákladné dešifrování SSL vypnout webových serverů tím, že návštěva [Konfigurace převzít SSL](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png
<properties
  pageTitle="Použití služby Azure DNS s jinými službami Azure | Microsoft Azure"
  description="Principy používání Azure DNS řešení název pro další služby Azure"
  services="dns"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure dns"
/>
<tags
  ms.service="dns"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="09/21/2016"
  ms.author="sewhee"
/>

# <a name="using-azure-dns-with-other-azure-services"></a>Použití služby Azure DNS s jinými službami Azure

Je to Azure DNS hostovanou službu DNS management a název rozlišení. Umožňuje vytvořit veřejné názvů DNS pro jiné aplikace a služby, které jste nainstalovali v Azure. Vytvoření názvu pro službu Azure ve vaší vlastní domény je jednoduchá – stačí přidání záznamu správný typ službě.

* Pro dynamické přidělené IP adresy musíte vytvořit záznam DNS CNAME, který odpovídá názvu DNS vytvořené pro službu Azure. Organizace pro standardy DNS můžete zabránit pomocí záznam CNAME pro vrcholu pásma.
* Pro staticky přidělené IP adresy můžete vytvořit záznam DNS A pomocí názvů, včetně názvu _otevřeným domény_ na vrcholu zóny.

Následující tabulka popisuje typů podporovaných záznamů, které se dá použít pro různé služby Azure. Jak můžete vidět z této tabulky, podporuje Azure DNS pouze záznamy DNS pro internetové síťové prostředky. Azure DNS není možné použít pro překlad interní soukromé adres.

| Služby Azure | Rozhraní sítě | Popis |
|---------------|-------------------|-------------|
| Aplikace brány | Front-end veřejnou IP | Můžete vytvořit záznam DNS A nebo CNAME. |
| Vyrovnávání zatížení | Front-end veřejnou IP | Můžete vytvořit záznam DNS A nebo CNAME. Vyrovnávání zatížení může mít IPv6 veřejnou IP adresu, která se přiřadí dynamické. Proto musíte vytvořit záznam CNAME pro adresy IPv6. |
| Přenosy správce | Veřejné jméno | Pouze můžete vytvořit záznam CNAME, který odpovídá názvu trafficmanager.net přiřazená svůj profil správce přenos. Další informace najdete v tématu [Jak přenosy správce](../traffic-manager/traffic-manager-how-traffic-manager-works.md#traffic-manager-example). |
| Cloudová služba | Veřejné IP | Pro staticky přidělené IP adresy můžete vytvořit záznam DNS A. Pro dynamické přidělené IP adresy musíte vytvořit záznam CNAME, který odpovídá názvu _cloudapp.net_ . Pokud chcete toto pravidlo se týká VMs vytvořit na portálu klasické, protože jsou nasazeny jako do cloudové služby. Další informace najdete v tématu [konfigurace vaší vlastní doménou ve službě Cloud Services](../cloud-services/cloud-services-custom-domain-name-portal.md). |
| Aplikace služby | Externí IP | Pro vnější IP adresy můžete vytvořit záznam DNS A. Jinak musíte vytvořit záznam CNAME, který odpovídá názvu azurewebsites.net. Další informace najdete v tématu [Mapovat vlastní název domény do aplikace Azure](../app-service-web/web-sites-custom-domain-name.md) |
| Správce prostředků VMs | Veřejné IP | Správce prostředků VMs může mít veřejnou IP adres. OM s veřejnou IP adresu možná by vás za vyrovnávání zatížení. Můžete vytvořit záznam DNS A nebo CNAME pro možnost Adresa veřejného. Tato vlastní název lze obejít VIP na Vyrovnávání zatížení. |
| Klasický VMs | Veřejné IP | Klasický VMs vytvořené pomocí prostředí PowerShell nebo rozhraní příkazového řádku možné konfigurovat pomocí dynamického nebo statického (rezervovaná) virtuální adresu. Vytvoříte záznam CNAME DNS (záznam) v tomto pořadí. |

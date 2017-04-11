

<properties
   pageTitle="Stav sledování přehled Azure aplikace brány | Microsoft Azure"
   description="Další informace o možnosti sledování v bráně aplikace Azure"
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

# <a name="application-gateway-health-monitoring-overview"></a>Přehled sledování stavu aplikace brány

Azure brány aplikace ve výchozím nastavení sleduje stav všech zdrojů v jeho fondu back-end a automaticky odstraní všechny zdroje z fondu považované za nebezpečné. Aplikace brány nadále sledovat chybná instance a přidává je zpět do fondu správný back-end po jsou k dispozici a odpovědět na sond stavu. Aplikace brány odešle že stavu sond s stejné číslo portu, která je definovaná v části Nastavení protokolu HTTP back-end. Zajistíte tak, že zkušební testuje stejný port, který by se připojujete k back-end pomocí zákazníci.

![Příklad zkušební brány aplikace][1]

Kromě použití sledování zkušební výchozího stavu, můžete upravovat taky zkušební stavu podle požadavků na vaše aplikace. V tomto článku jsou uvedeny výchozí a vlastní stavu sond.

## <a name="default-health-probe"></a>Výchozí stav zkušební

Brány aplikační automaticky nakonfiguruje výchozího stavu zkušební, když není nastavíte veškeré vlastní zkušební konfigurace. Sledování chová tím, že žádost HTTP k IP adrese nakonfigurovány fondu back-end. Výchozí sond Pokud k back-end http nakonfigurování nastavení pro HTTPS, zkušební použije https i k testování stavu back-end.

Příklad: Konfigurace brány aplikace chcete používat k přijímání HTTP v síti na portu 80 servery back-end A, B a C. Sledování stavu výchozí testuje tři servery každých 30 sekund správný HTTP odpovědi. Správný odpověď HTTP má [stavový kód](https://msdn.microsoft.com/library/aa287675.aspx) 200 až 399.

Neúspěšné kontrola zkušební výchozí server A brána aplikace ji odebere z fondu jeho back-end a pokračuje tento server přestane v síti. Výchozí zkušební přetrvává se mají zjišťovat server každých 30 sekund. Pokud server A odpoví na jednu žádost o z zkušební stavu výchozí úspěšně, je vrstva přidána zpět správný fondu back-end a přenosy spustí toku na server znovu.

### <a name="default-health-probe-settings"></a>Výchozí nastavení zkušební stavu

|Probe vlastnost | Hodnota | Popis|
|---|---|---|
| Probe adresy URL| http://127.0.0.1:\<port\>/ | Cesta URL |
| Interval | 30 | Probe interval v sekundách |
| Časový limit  | 30 | Probe časového limitu v sekundách |
| Chybná prahová hodnota | 3 | Probe počet opakování. Databázový server je označen po počet chyb po sobě jdoucí zkušební dosáhne chybná prahová hodnota. |

> [AZURE.NOTE] Číslo portu vždycky stejný port jako nastavení HTTP back-end.

Výchozí zkušební sleduje pouze http://127.0.0.1:\<port\> k určení stavu. Pokud potřebujete ke konfiguraci zkušební stavu chcete vrátit na vlastní adresy URL nebo změnit další nastavení, musíte použít vlastní sond podle následujících kroků.

## <a name="custom-health-probe"></a>Zkušební vlastní stavu

Vlastní sond umožňuje mít přesnější kontrolu nad sledování stavu. Pokud chcete použít vlastní sond, můžete nakonfigurovat interval zkušební, adresu URL a cesta k testování a počet neúspěšných odpovědí přijmout před označením instance back-end fondu jako chybné.

### <a name="custom-health-probe-settings"></a>Nastavení zkušební vlastní stavu

Následující tabulka obsahuje definice vlastností zkušební vlastní stavu.

|Probe vlastnost| Popis|
|---|---|
| Jméno | Název zkušební. Tento název se používá odkázat na zkušební v části Nastavení protokolu HTTP back-end. |
| Protocol (protokol) | Protokol sloužící k odeslání zkušební. Zkušební použije protokolu HTTP nastavení back-end |
| Host (hostitel) |  Název hostitele odeslat zkušební. Použít pouze v případě více webů je nakonfigurovaný na použití brány, jinak použít "127.0.0.1". Tím se liší od OM název hostitele. |
| Cesta | Relativní cestu zkušební. Začíná platnou cestu "/". |
| Interval | Probe interval v sekundách. Toto je časový interval mezi dvěma po sobě jdoucí sond.|
| Časový limit | Probe časového limitu v sekundách. Zkušební je označen jako se nepodařilo není-li platnou odpověď přijata v rámci tohoto časového období. |
| Chybná prahová hodnota | Probe počet opakování. Databázový server je označen po počet chyb po sobě jdoucí zkušební dosáhne chybná prahová hodnota. |

> [AZURE.IMPORTANT] Pokud aplikace bránu pro jednoho webu, ve výchozím nastavení hostitele je nutné zadat název jako "127.0.0.1", pokud jinak konfigurovat vlastní zkušební.
Kdykoliv při ruce se neodesílají vlastní zkušební \<protokol\>://\<hostitele\>:\<port\>\<cestu\>. Port používaný budou stejný port podle nastavení HTTP back-end.

## <a name="next-steps"></a>Další kroky

Po získání informací o sledování stavu aplikace brány, můžete nakonfigurovat [vlastní stavu zkušební](application-gateway-create-probe-portal.md) v portálu Azure nebo [vlastní stavu zkušební](application-gateway-create-probe-ps.md) pomocí prostředí PowerShell a nasazení modelu správce prostředků Azure.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png
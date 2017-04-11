<properties
   pageTitle="Import a export souboru zóny domain Azure DNS pomocí rozhraní příkazového řádku | Microsoft Azure"
   description="Zjistěte, jak import a export souboru zóny DNS na server DNS Azure pomocí rozhraní příkazového řádku Azure"
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

# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli"></a>Import a export souboru zóny DNS pomocí rozhraní příkazového řádku Azure


Tento článek vás provede jednotlivými způsob importu a exportu souborů zóny DNS pro službu Azure DNS pomocí rozhraní příkazového řádku Azure.

## <a name="introduction-to-dns-zone-migration"></a>Úvod k migraci zóny DNS

Soubor zóny DNS je textový soubor, který obsahuje podrobnosti o všech systému DNS (Domain Name) záznamů v zóně. Následuje standardní formát, díky vhodné pro přenos mezi systémy DNS záznamy DNS. Pomocí souboru zóny představuje snadné, spolehlivé a pohodlný způsob přenášet zóny DNS nebo stmívání Azure DNS.

Azure DNS podporuje importu a exportu souborů zóny pomocí rozhraní Azure příkazového řádku (rozhraní příkazového řádku). Rozhraní příkazového řádku Azure je různé platformy nástroj příkazového řádku pro správu služby Azure. Je k dispozici pro Windows, Mac a Linux platformy z [Azure stahování stránky](https://azure.microsoft.com/downloads/). Různé platformy podpory totiž velmi důležitým pro importu a exportu souborů zóny nejběžnější serverový software název [VAZBA](https://www.isc.org/downloads/bind/), obvykle běží na Linux.

## <a name="obtain-your-existing-dns-zone-file"></a>Získání existující soubor zóny DNS

Než budete do Azure DNS importovat soubor zóny DNS, musíte získat kopii souboru zóny. Zdrojový soubor závisí na které je právě hostovaný zóny DNS.

- Pokud váš DNS zone je hostitelem služby partnera (například doménovému registrátorovi, vyhrazené poskytovatele hostingu DNS nebo alternativní cloudu poskytovatelem), službu by měl obsahovat možnost Stáhnout soubor zóny DNS.

- Pokud DNS zónu je hostovaný ve Windows DNS, je výchozí složka pro soubory zóny **%systemroot%\system32\dns**. Úplnou cestu k souboru každé zóny se zobrazí také na kartě **Obecné** konzolu Správa služby DNS.

- Pokud DNS zónu hostuje pomocí VAZBU, umístění souboru zóny pro každou zónu zadané v souboru konfigurace VAZBU **named.conf**.

**Práce se soubory zóny registrátora GoDaddy**<BR>
Zóny soubory stažené ze GoDaddy s formátem mírně nevhodných. Potřebujete tento problém můžete vyřešit před importem tyto soubory zóny do Azure DNS. Názvy DNS v Data_záznamu každý záznam DNS jsou určeny jako plně kvalifikované názvy, ale tito uživatelé nemají ukončení "." To znamená, že jsou interpretovaný jinými DNS systémy jako relativní názvy. Potřebujete upravit soubor zóny přidání ukončení "." na názvy před importem do Azure DNS.

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Import souboru zóny DNS do Azure DNS


Import souboru zóny vytvoří novou zónu v Azure DNS Pokud ještě neexistuje. Pokud zóna už existuje, sady záznamů v souboru zóny je nutno sloučit s existující sady záznamů.

### <a name="merge-behavior"></a>Sloučení chování

- Ve výchozím nastavení jsou sloučena stávajících, tak i nové sady záznamů. Stejné záznamy v rámci sady sloučené záznamů jsou odstranit duplicitní.

- Můžete taky tak, že zadá `--force` možnosti importu, nahradí existující sady záznamů s nové sady záznamů. Existující sady záznamů, které nemají odpovídající záznam nastavení v souboru importovaného zóny nebudou odebrány.

- Při sloučení sady záznamů, bude použita time to live (TTL) z existující sady záznamů. Když `--force` je použili, se používá TTL nové sadě záznamů.

- Zahájení autorita (SOA) parametrů (kromě `host`) jsou vždy pořízené ze souboru importovaného zóny, bez ohledu na to, zda `--force` se používá. Podobně pro záznam názvového serveru rozmístěné po vrcholu zóna, hodnota TTL vždy odebírá z souboru importovaného zóny.

- Importovaný záznam CNAME nenahradí stávajícího záznamu CNAME se stejným názvem Pokud `--force` zadaný parametr.

- Když dojde ke konfliktu mezi záznam CNAME a dalším záznamem stejný název, ale jiný typ (bez ohledu na to, které je existující nebo nové), se zachovají existující záznam. Toto je nezávislý využívání `--force`.

### <a name="additional-information-about-importing"></a>Další informace o importu

Následující poznámky poskytují další technické podrobnosti o zónu import obrázku.

- `$TTL` Vynechán směrnice a tuto možnost podporuje. Pokud ne `$TTL` směrnice je uveden, záznamů bez explicitní TTL bude importovaných nastavit na stav výchozí TTL 3600 sekund. Když dva záznamy v sadě záznamů stejné zadat jinou ignorují, bude použita nižší hodnota.

- `$ORIGIN` Směrnice vynechán, a tuto možnost podporuje. Pokud ne `$ORIGIN` je nastavena použita výchozí hodnota název zóny jako zadaného do příkazového řádku (plus ukončení ".").

- `$INCLUDE` a `$GENERATE` směrnice nejsou podporované.

- Tyto typy záznamů jsou podporované: A, AAAA, CNAME, MX, NS, SOA, SRV a TXT.

- Záznam SOA automaticky se vytvoří službou Azure DNS při vytvoření zóny. Při importu souboru zóny všechny parametry SOA jsou odebrány zónu soubor *s výjimkou* `host` parametr. Tento parametr používá zadaná hodnota službou Azure DNS. Důvodem je tento parametr musí odkazovat na primární názvový server poskytovanou Azure DNS.

- Záznam názvového serveru na vrcholu zóny je také vytvářeny automaticky službami Azure DNS při vytvoření zóny. Pouze TTL záznamu nastavenou naimportují. Tyto záznamy obsahovat názvy serverů název poskytovanou Azure DNS. Datový záznam není přepsat hodnoty obsažené v souboru importovaného zóny.

- Během Public Preview Azure DNS podporuje pouze záznamy TXT jednoho řetězce. Nahrazován TXT záznamy budou spojeny a zkrácen na 255 znaků.

### <a name="cli-format-and-values"></a>Formát rozhraní příkazového řádku a hodnoty


Formát příkazu Azure rozhraní příkazového řádku pro import zóny DNS je:<BR>`azure network dns zone import [options] <resource group> <zone name> <zone file name>`

Hodnoty:

- `<resource group>`stejný název pole Skupina zdroje pro zónu v Azure DNS.
- `<zone name>`stejný název zóny.
- `<zone file name>`je cestu a název souboru zóny na import.

Pokud zóna s tímto názvem neexistuje ve skupině prostředků, vytvoří se za vás. Pokud zóna už existuje, importovaný sady záznamů sloučeny se existující sady záznamů. Přepsat existující sady záznamů, můžete `--force` možnost.

Ověření formátu souboru zóny bez skutečně importu, použít `--parse-only` možnost.

### <a name="step-1-import-a-zone-file"></a>Krok 1. Import souboru zóny

K importu souboru zóny pro zónu **contoso.com**.

1. Přihlaste se k předplatnému Azure pomocí rozhraní příkazového řádku Azure.

        azure login

2. Vyberte předplatné, kde chcete vytvořit nové zóny DNS.

        azure account set <subscription name>

3. Azure je to DNS Azure jen správce služby, takže rozhraní příkazového řádku Azure musí být přepnutí správce prostředků.

        azure config mode arm

4. Před použitím služby Azure DNS, musíte zaregistrovat předplatného na používání poskytovatele Microsoft.Network zdroje. (Toto je jednorázové operace pro každého předplatného.)

        azure provider register Microsoft.Network

5. Pokud nemáte už, musíte taky vytvořit skupinu zdrojů správce prostředků.

        azure group create myresourcegroup westeurope

6. Naimportujte zóna **contoso.com** ze souboru **contoso.com.txt** zónu DNS nové skupiny prostředků **myresourcegroup**, příkaz `azure network dns zone import`.<BR>Tento příkaz načtení souboru zóny a analyzovat ho. Příkaz bude spuštěn posloupnost příkazů na služby Azure DNS k vytvoření zóny a všechny záznamu sady v zóně. Příkaz také vykazovat průběh v okně konzoly spolu s chyb a upozornění. Protože sady záznamů jsou vytvářeny v řadě, může trvat několik minut k importu souboru velké zóny.

        azure network dns zone import myresourcegroup contoso.com contoso.com.txt



### <a name="step-2-verify-the-zone"></a>Krok 2. Ověření zóny

Pro ověření DNS zónu po importu souboru, můžete použít některou z těchto způsobů:

- Seznam záznamy pomocí následujícího příkazu Azure rozhraní příkazového řádku.

        azure network dns record-set list myresourcegroup contoso.com

- Seznam záznamů pomocí rutiny prostředí PowerShell `Get-AzureRmDnsRecordSet`.

- Můžete použít `nslookup` ověřit překlad záznamy. Protože zónu není zatím delegované, bude nutné explicitně zadat správné názvové servery Azure DNS. Následující příklad ukazuje, jak načíst názvy serverů jméno přiřazené k zóně. IT také ukazuje, jak k vytvoření dotazu na záznam "www" pomocí `nslookup`.

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/…/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a>Krok 3. Aktualizace DNS delegování

Po ověření správně importu zónu musíte aktualizovat delegování DNS tak, aby ukazovaly na názvové servery Azure DNS. Další informace naleznete v článku [aktualizace delegování DNS](dns-domain-delegation.md).

## <a name="export-a-dns-zone-file-from-azure-dns"></a>Export souboru zóny DNS z Azure DNS

Formát příkazu Azure rozhraní příkazového řádku pro import zóny DNS je:<BR>`azure network dns zone export [options] <resource group> <zone name> <zone file name>`

Hodnoty:

- `<resource group>`stejný název pole Skupina zdroje pro zónu v Azure DNS.
- `<zone name>`stejný název zóny.
- `<zone file name>`cestu a název souboru zóny a exportovat je.

Jako importu zóny se nejprve potřebujete přihlásit, vyberte předplatné a konfigurace Azure rozhraní příkazového řádku pro použití režimu správce prostředků.

### <a name="to-export-a-zone-file"></a>Chcete-li exportovat souboru zóny


1. Přihlaste se k předplatnému Azure pomocí rozhraní příkazového řádku Azure.

        azure login

2. Vyberte předplatné, kde chcete vytvořit nové zóny DNS.

        azure account set <subscription name>

3. Je to Azure DNS služby Azure jen správce prostředků. Rozhraní příkazového řádku Azure musí přepnout do režimu správce prostředků.

        azure config mode arm

4. Pokud chcete existující Azure DNS zone **contoso.com** ve skupině zdroje **myresourcegroup** exportovat do souboru **contoso.com.txt** (v aktuální složce), spusťte `azure network dns zone export`. Tento příkaz zavolá služby Azure DNS vytvořte záznam množiny v zóně a exportovat výsledky do souboru zóny kompatibilní s prohlížečem VAZBU.

        azure network dns zone export myresourcegroup contoso.com contoso.com.txt


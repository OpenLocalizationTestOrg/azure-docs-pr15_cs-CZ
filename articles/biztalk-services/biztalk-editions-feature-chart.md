<properties
    pageTitle="Další informace o funkcích v edicích BizTalk služby | Microsoft Azure"
    description="Porovnání funkcí edice BizTalk služby: uvolnit, karta Vývojář, Basic, Standard a Premium. MABS, WABS."
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="mandia"/>


# <a name="biztalk-services-editions-chart"></a>BizTalk služby: Graf edice

Služby Azure BizTalk nabízí několik edice. Pomocí tohoto článku zjistíte, jakou verzi je vyhovuje vašim potřebám scénář pro podnikatele.


## <a name="compare-the-editions"></a>Porovnat verze

**Uvolnit (verze Preview)**

Můžete vytvořit a spravovat hybridní připojení. Připojení hybridní je snadný způsob, jak se připojit k místní systém SQL Server Azure webu.

**Karta Vývojář**

Obsahuje hybridní připojení, EAI a upravit zpracování zprávy s e-použití obchodní portálu pro správu partnerů a podpora pro běžné upravit schémata a bohaté zpracování upravit nad X12 a AS2. Můžete vytvořit obvyklé scénáře EAI připojením služby v cloudu pomocí žádné protokolu HTTP/S REST, FTP, WCF a SFTP protokoly pro čtení a zápis zprávy.  Připojení k místní systémy LOB využít s adaptéry SAP Oracle eBusiness, databáze Oracle, Siebel a SQL Server připravených k použití. Použijte na střed prostředí developer pomocí nástrojů Visual Studio snadno rozvoje a nasazení. Pouze pro účely vývoj a testování jenom s žádné služby úroveň dohodu (SLA).

**Základní**

V hybridním EAI mosty, upravit smlouvami, připojení a BizTalk adaptér Pack připojení obsahuje většina funkcí vývojář zvětšuje. Nabízí taky dostupnost a možnost zobrazit se smlouva úrovni služeb (SLA).

**Standardní**

Zahrnuje všechny základní funkce zvětšuje hybridní EAI mosty, upravit smlouvami, připojení a BizTalk adaptér Pack připojení. Nabízí taky dostupnost a možnost zobrazit se smlouva úrovni služeb (SLA).

**Premium**

Zahrnuje všechny standardní funkce zvětšuje hybridní EAI mosty, upravit smlouvami, připojení a BizTalk adaptér Pack připojení. Obsahuje taky archivace dostupnost a možnost zobrazit se smlouva úrovni služeb (SLA).


## <a name="editions-chart"></a>Edice grafu
V následující tabulce jsou uvedeny rozdíly.

<table border="1">
<tr bgcolor="FAF9F9">
        <th></th>
        <th>Uvolnit (verze Preview)</th>
        <th>Karta Vývojář</th>
        <th>Základní</th>
        <th>Standardní</th>
        <th>Premium</th>
</tr>

<tr>
<td><strong>Spuštění cena</strong></td>
<td colspan="5"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011"> Služby Azure BizTalk ceny</a> <br/><br/> <a HREF="http://azure.microsoft.com/pricing/calculator/?scenario=full">Azure ceny Kalkulačka</a></td>
</tr>
<tr>
<td><strong>Výchozí minimální konfigurace</strong></td>
<td>1 bezplatného jednotka</td>
<td>1 vývojář jednotky</td>
<td>1 základní jednotka</td>
<td>1 standardní jednotka</td>
<td>1 premium jednotky</td>
</tr>
<tr>
<td><strong>Stupnice</strong></td>
<td>Bez měřítka</td>
<td>Bez měřítka</td>
<td>Ano, v částech 1 základní jednotka</td>
<td>Ano, v částech 1 standardní jednotky</td>
<td>Ano, v částech 1 jednotka Premium</td>
</tr>
<tr>
<td><strong>Maximální povolenou škálu</strong></td>
<td>Bez měřítka</td>
<td>Bez měřítka</td>
<td>Až 8 jednotek</td>
<td>Až 8 jednotek</td>
<td>Až 8 jednotek</td>
</tr>
<tr>
<td><strong>EAI mosty za jednotku</strong></td>
<td>Nezahrnuté</td>
<td>25</td>
<td>25</td>
<td>125</td>
<td>500</td>
</tr>
<tr>
<td><strong>UPRAVIT, AS2</strong>
<br/><br/>
Obsahuje TPM smlouvami</td>
<td>Nezahrnuté</td>
<td>Zahrnout. 10 smlouvami za jednotku.</td>
<td>Zahrnout. 50 smlouvami za jednotku.</td>
<td>Zahrnout. 250 smlouvami za jednotku.</td>
<td>Zahrnout. 1 000 smlouvami za jednotku.</td>
</tr>
<tr>
<td><strong>Hybridní připojení za jednotku</strong></td>
<td>5</td>
<td>5</td>
<td>10</td>
<td>50</td>
<td>100</td>
</tr>
<tr>
<td><strong>Přenos dat připojení (GB) hybridní za jednotku</strong></td>
<td>5</td>
<td>5</td>
<td>50</td>
<td>250</td>
<td>500</td>
</tr>
<tr>
<td><strong>Služba adaptér BizTalk připojení k místní LOB systémy</strong></td>
<td>Nezahrnuté</td>
<td>1 připojení</td>
<td>2 připojení</td>
<td>5 připojení</td>
<td>25 připojení</td>
</tr>
<tr>
<td align="left"><strong>Podporované protokoly/systémy:</strong>
<ul>
<li>NASTAVIT INFORMACE HTTP</li>
<li>PROTOKOL HTTPS</li>
<li>FTP</li>
<li>SFTP</li>
<li>WCF</li>
<li>Služba Bus (SB)</li>
<li>Objektů Blob Azure</li>
<li>Rozhraní REST API</li>
</ul>
</td>
<td>Nezahrnuté</td>
<td>Součástí</td>
<td>Součástí</td>
<td>Součástí</td>
<td>Součástí</td>
</tr>
<tr>
<td><strong>Dostupnost</strong>
<br/><br/>
Pro službu úroveň dohodu (SLA), najdete v článku <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=304011">Ceny služby Azure BizTalk</a>.
</td>
<td>Nezahrnuté</td>
<td>Nezahrnuté</td>
<td>Součástí</td>
<td>Součástí</td>
<td>Součástí</td>
</tr>
<tr>
<td><strong>Zálohování a obnovení</strong></td>
<td>Nezahrnuté</td>
<td>Součástí</td>
<td>Součástí</td>
<td>Součástí</td>
<td>Součástí</td>
</tr>
<tr>
<td><strong>Sledování</strong></td>
<td>Nezahrnuté</td>
<td>Součástí</td>
<td>Součástí</td>
<td>Součástí</td>
<td>Součástí</td>
</tr>
<tr>
<td><strong>Archivace</strong><br/><br/>
Obsahuje Nepopiratelnost odpovědnosti potvrzení (NRR) a stahování sledované zprávy</td>
<td>Nezahrnuté</td>
<td>Součástí</td>
<td>Nezahrnuté</td>
<td>Nezahrnuté</td>
<td>Součástí</td>
</tr>
<tr>
<td><strong>Použití vlastního kódu</strong></td>
<td>Nezahrnuté</td>
<td>Součástí</td>
<td>Součástí</td>
<td>Součástí</td>
<td>Součástí</td>
</tr>
<tr>
<td><strong>Použití transformací, včetně vlastních transformací XSLT.</strong></td>
<td>Nezahrnuté</td>
<td>Součástí</td>
<td>Součástí</td>
<td>Součástí</td>
<td>Součástí</td>
</tr>
</table>

> [AZURE.NOTE] Pro odolnost proti chybám hardwaru dostupnost znamená s více VMs v rámci jedné BizTalk jednotky.


## <a name="faqs"></a>Nejčastější dotazy

#### <a name="what-is-a-biztalk-unit"></a>Co je jednotka BizTalk?
"Celku" je atomová úroveň nasazení služby Azure BizTalk. Každý edition získáváte jednotku, která má kapacita různých výpočetním a paměti. Například základní jednotka obsahuje více výpočetních než vývojář, standardní obsahuje více výpočetních než základní atd. Při změně měřítka služba BizTalk, změně velikosti z hlediska jednotky.

#### <a name="what-is-the-difference-between-biztalk-services-and-azure-biztalk-vm"></a>Jaký je rozdíl mezi službami BizTalk a OM BizTalk Azure?
Služby BizTalk poskytuje true architekturu platformy jako-Service (PaaS) pro vytváření integrace řešení v cloudu. S modelem PaaS úplně zaměřit na logiky aplikace a nechejte všechny správy infrastruktury společnosti Microsoft, včetně:

- Není nutné spravovat nebo oprava virtuálních počítačích.
- Společnost Microsoft zaručuje dostupnosti.
- Můžete řídit měřítko na vyžádání vyžádat jednoduše více nebo méně kapacity portálu Azure.

BizTalk Server na virtuálních počítačích Azure poskytuje architekturu infrastruktury jako-Service (IaaS). Vytváření virtuálních počítačích a nastavovat stejně jako místním prostředím a snadněji spustit existující aplikace v cloudu pomocí žádné změny kódu. S IaaS jste zodpovědní pořád konfigurace virtuálních počítačích, řízení virtuálních počítačích (například instalaci softwaru a opravy OS) a architecting žádost o dostupnost.

Pokud se díváte zdokonalené integrace minimalizovat vaše plánování řízené úsilí správu infrastruktury, BizTalk služby použijte. Pokud chcete rychle přenesení existujících řešení BizTalk nebo hledáte prostředí na vyžádání vyvíjet a otestovat Admin.systému aplikací, pomocí Admin.systému na Azure virtuálního počítače.

#### <a name="what-is-the-difference-between-biztalk-adapter-service-and-hybrid-connections"></a>Jaký je rozdíl mezi službou adaptér BizTalk a hybridní připojení?
Služba adaptér BizTalk používá služby Azure BizTalk. Připojení k systému místní řádek Business (LOB) využívá služba adaptér BizTalk BizTalk adaptér Pack. Připojení hybridní poskytuje jednoduché a pohodlný způsob připojení Azure aplikací, jako je funkce Web Apps v Azure aplikaci služby a služby Azure Mobile, místní zdroji.

#### <a name="what-does-hybrid-connection-data-transfer-gb-per-unit-mean-is-this-per-minutehourdayweekmonth-what-happens-when-the-limit-is-reached"></a>Co znamená "Hybridní připojení přenos dat (GB) za jednotku"? Je to za minutu/hod/den/týden/měsíc? Co se stane, když je limitu?

Jednotková cena hybridní připojení závisí na BizTalk Services edition. Jednoduše řečeno, náklady, závisí na množství zpracovávaných dat je převést. Třeba přenos dat 10 GB denně nákladů, menší než 100 GB přenos denně. Pomocí [Kalkulačky ceny](https://azure.microsoft.com/pricing/calculator/?scenario=full) služby BizTalk zjistíte specifické náklady. Limity se obvykle nevynucují denně. Pokud přesáhne limit jakékoliv nadsazení bude účtováno sazbou $1 za GB.

#### <a name="when-i-create-an-agreement-in-biztalk-services-why-does-the-number-of-bridges-go-up-by-two-instead-of-just-one"></a>Při vytváření dohodu služby BizTalk, proč počet mosty zvládli, dvě místo jen jeden?

Každý smlouvy se skládá ze dvou různých mosty: Most odeslat strany komunikace a most přijímání strany komunikace.

####  <a name="what-happens-when-i-hit-the-quota-limit-on-the-number-of-bridges-or-agreements"></a>Co se stane, když přístupů kvótu počtu mosty nebo dohod?

Nejste moct nasazení nové mosty ani vytvářet nové smlouvy. Abyste mohli nasadit informace, budete muset škálování k více jednotkám služby BizTalk nebo upgradovat na novější verzi.

#### <a name="how-do-i-migrate-from-one-tier-of-biztalk-services-to-another"></a>Jak se můžu do jiného migrovat z jednoho osy BizTalk služeb?

Bezplatné edition nelze poštovním nebo "diagramů s měřítky" k jiné vrstvě a nelze zálohování a obnovení k jiné vrstvě. Pokud potřebujete jiný osy, vytvořte nové služby BizTalk pomocí nové osy. Všechny artefakty vytvořené pomocí bezplatné vydání, včetně hybridní připojení, je nutné znovu vytvořit nové služby BizTalk. 

Pro zbývající edice použijte zálohování a obnovení pro migrace vaše artefakty z jedné úrovně. Například zálohování artefakty ve standardní osy a obnovit je vrstva Premium. [BizTalk služby: zálohování a obnovení](biztalk-backup-restore.md) popisuje cesty podporované migrace a seznamy, jaké artefakty zálohují. Všimněte si, že nejsou zálohovala hybridní připojení. Po zálohování a obnovování nové osy, je pak znovu vytvořit připojení hybridní.  


#### <a name="is-the-biztalk-adapter-service-included-in-the-service-how-do-i-receive-the-software"></a>Je služba adaptér BizTalk zahrnuty ve službě? Jak dostávat software?

Ano, služba adaptér BizTalk s aktualizací Service Pack adaptér BizTalk jsou součástí SDK služby Azure BizTalk [Stáhnout](http://www.microsoft.com/download/details.aspx?id=39087).

## <a name="next-steps"></a>Další kroky

Chcete-li vytvořit Azure BizTalk služby na portálu Azure, přejděte na [BizTalk služby: zřizování pomocí portálu Azure](biztalk-provision-services.md). Zahájení vytváření aplikací, přejděte na [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="additional-resources"></a>Další zdroje informací
- [BizTalk služby: Pomocí portálu Azure zřizování](biztalk-provision-services.md)<br/>
- [BizTalk služby: Diagram stavu zřizování](biztalk-service-state-chart.md)<br/>
- [BizTalk služby: Řídicí panel, sledování a měřítko karty](biztalk-dashboard-monitor-scale-tabs.md)<br/>
- [BizTalk služby: Zálohování a obnovení](biztalk-backup-restore.md)<br/>
- [Služby BizTalk: omezení](biztalk-throttling-thresholds.md)<br/>
- [BizTalk služby: Název vydavatele a Vystavitel klíč](biztalk-issuer-name-issuer-key.md)<br/>
- [Jak můžu začít používat služby SDK BizTalk Azure](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>

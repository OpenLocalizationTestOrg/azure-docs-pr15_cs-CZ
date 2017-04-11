<properties 
    pageTitle="Vytvoření a obnovení záložní služby BizTalk | Microsoft Azure" 
    description="Služby BizTalk obsahuje zálohování a obnovení. Naučte se vytvářet a obnovení záložní a zjistit, co se zálohovala. MABS WABS" 
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
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>


# <a name="biztalk-services-backup-and-restore"></a>BizTalk služby: Zálohování a obnovení

Služby Azure BizTalk zahrnuje funkce, zálohování a obnovení. Toto téma popisuje, jak zálohování a obnovení BizTalk služby na portálu Azure klasické.

Můžete taky obecnějším údajům BizTalk služby pomocí [Rozhraní REST API BizTalk služby](http://go.microsoft.com/fwlink/p/?LinkID=325584). 

> [AZURE.NOTE] Hybridní připojení není zálohují, bez ohledu na verzi. Je nutné znovu vytvořit hybridní připojení.

## <a name="before-you-begin"></a>Než začnete

- Zálohování a obnovení nemusí být k dispozici pro všechny verze. V tématu [BizTalk služby: edice grafu](biztalk-editions-feature-chart.md).

- Na portálu Azure klasické, můžete vytvořit zálohu na vyžádání nebo vytvoření naplánované zálohy. 

- Zálohování obsah obnovit je může do stejné služby BizTalk nebo nové služby BizTalk. Obnovení služba BizTalk bloku stejný název, musí být stávající služba BizTalk odstraněny a název musí mít k dispozici. Po odstranění služba BizTalk, může to trvat déle než chtěli pro stejný název, aby byl k dispozici. Pokud nemůžete počkat stejný název, aby byl k dispozici, obnovte novou službu BizTalk.

- Služby BizTalk jde obnovit na stejné edition nebo vyšší verzi. Obnovování služby BizTalk na nižší verzi, ze kdy vytvoření zálohy, není podporovaná.

    Zálohování pomocí základní edici například jde obnovit na Premium Edition. Zálohování pomocí Premium Edition nelze obnovit na standardní vydání.

- Ovládací prvek Upravit čísla zálohují zachovat kontinuitu kontrolních čísel. Pokud za poslední záložní kopii zpracování zpráv, obnovení záložní obsahu mohou způsobit duplicitních čísly.

- Pokud dávky aktivní zprávy, zpracovat dávku **před** spuštěním zálohování. Při vytváření zálohy (jako potřebné nebo plánované), jsou uloženy nikdy zpráv v dávkách. 

    **Pokud zálohy pořízené aktivní zpráv v listu, tyto zprávy nejsou zálohovat a proto jsou ztraceny.**

- Volitelné: V portálu služby BizTalk vypnout č. všechny operace správy.


## <a name="create-a-backup"></a>Vytvoření zálohy

Zálohy můžete kdykoli pořízené a je úplně spravujete. V této části jsou uvedeny kroky k vytváření záloh na portálu Azure klasické, včetně:

[Na vyžádání zálohování](#backupnow)

[Naplánování zálohy](#backupschedule)

#### <a name="backupnow"></a>Na vyžádání zálohování
1. Na portálu Azure klasické vyberte **BizTalk služby**a potom vyberte BizTalk službu chcete zálohovat.
2. Na kartě **řídicího panelu** vyberte **obecnějším údajům** v dolní části stránky.
3. Zadejte název zálohy. Zadejte například *myBizTalkService*BU*Datum*.
4. Vyberte účet úložiště objektů blob a zaškrtněte políčko Spustit zálohování.

Po dokončení zálohování, vytvoří se ve účtu úložiště kontejneru s název zálohy, kterou zadáte. Tento kontejner obsahuje konfigurace zálohování BizTalk služby.

#### <a name="backupschedule"></a>Naplánování zálohování

1. Na portálu Azure klasické vyberte **BizTalk služeb**, vyberte název BizTalk služby chcete naplánovat zálohování a pak vyberte kartu **Konfigurovat** .
2. **Automaticky**nastavte **Stav zálohování** . 
3. Vyberte **Účet úložiště** zálohu uložit, zadejte **Četnost** k vytváření záloh a jak dlouho mají zachovat zálohy (**Dnů uchovávání informací**):

    ![][AutomaticBU]

    **Poznámky**   
    - V **Dnů uchovávání informací**musí být větší než záložní počet_plateb doba uchovávání informací.
    - Vyberte **vždy mít nejméně jeden zálohování**, i když je dřívější doba uchovávání informací.
    

4. Vyberte **Uložit**.


Při spuštění zálohovací úlohy vytvoří kontejneru (pro ukládání záložních dat) v okně úložiště účet, který jste zadali. Název kontejneru jmenuje *Služba BizTalk název datum a čas*. 

Pokud řídicí panel služeb BizTalk se zobrazí stav **se nezdařilo** :

![Poslední stav plánované zálohování][BackupStatus] 

Odkaz otevře protokoly operací služby správy můžou pomoct odstranit. V tématu [BizTalk služby: Poradce při potížích s protokoly operací](http://go.microsoft.com/fwlink/p/?LinkId=391211).

## <a name="restore"></a>Obnovení

Zálohování můžete obnovit z portálu Microsoft Azure klasické nebo z [Obnovení BizTalk služba REST API](http://go.microsoft.com/fwlink/p/?LinkID=325582). V této části jsou uvedené kroky k obnovení na klasické portálu.

#### <a name="before-restoring-a-backup"></a>Před obnovením zálohování

- Při obnovení služba BizTalk lze zadat nové sledování archivace a sledování úložiště.

- Stejná data upravit Runtime se obnoví. Zálohování upravit Runtime čísla ovládacího prvku. Jsou čísla obnovená ovládací prvek v pořadí od doby zálohování. Zpracování zpráv za poslední zálohování, obnovení záložní obsahu může způsobit duplicitní čísly.

#### <a name="restore-a-backup"></a>Obnovení záložní

1. Na portálu Azure klasické vyberte **Nový** > **Aplikace služby** > **Služba BizTalk** > **obnovení**:

    ![Obnovení záložní][Restore]

2. V **Záložní adresy URL**vyberte ikonu složky a rozbalte účet Azure úložiště, který ukládá zálohování konfigurace BizTalk služby. Rozbalte kontejner a v pravém podokně vyberte odpovídající záložní soubor txt. 
<br/><br/>
Vyberte **Otevřít**.

3. Na stránce **Služby BizTalk obnovení** zadejte **Název služby BizTalk** a ověřte **Adresu URL domény**, **Edition**a **oblast** pro obnovená služba BizTalk. **Vytvořit novou instanci databáze SQL** databáze sledování:

    ![][RestoreBizTalkService]

    Vyberte šipku Další.

4.  Ověřte název databáze SQL, zadejte fyzický server místo, kam databáze SQL vytvoří a uživatelského jména a hesla pro tento server.


    Pokud chcete nakonfigurovat edition databáze SQL, velikost a další vlastnosti vyberte **Konfigurace rozšířeného nastavení databáze**. 

    Vyberte šipku Další.

5. Vytvoření nového účtu úložiště nebo zadejte existujícího účtu úložiště služby BizTalk.

7. Zaškrtněte políčko Spustit na obnovit.

Po úspěšném dokončení obnovení novou službu BizTalk je uvedený v pozastaveném stavu na stránce BizTalk služby Azure klasické portálu.



### <a name="postrestore"></a>Po obnovení záložní

Služba BizTalk je vždy obnovit ve stavu **Suspended** . V tomto stavu můžete proveďte změny konfigurace před nové prostředí je funkční, včetně:

- Pokud jste vytvořili pomocí SDK služby Azure BizTalk aplikace služeb BizTalk, budete muset aktualizovat pověření řízení přístupu (ACS) v těchto aplikacích pro práci s obnovená prostředí.

- Obnovení služba BizTalk replikovat existující prostředí BizTalk služby. V takovém případě splnění smlouvami nakonfigurovaný na portálu původní BizTalk služby, které používají zdrojové FTP složky, budete muset aktualizovat smlouvy nově obnovená prostředí FTP Složka jiného zdroje. V ostatních případech může být dva různé smlouvami vyzkoušení na stejnou zprávu.

- Pokud jste obnovili mít víc služba BizTalk prostředí, zkontrolujte, jestli že cílové správné prostředí v aplikacích Visual Studiu, rutiny prostředí PowerShell, rozhraní REST API nebo obchodování rozhraní OM správy partnera.

- Je vhodné pro nastavení automatického zálohování nově obnovená služba BizTalk prostředí.

Spusťte službu BizTalk Azure klasické portálu, vyberte obnovená služba BizTalk a vyberte **životopisu** na hlavním panelu. 



## <a name="what-gets-backed-up"></a>Co se zálohovala

Po vytvoření zálohy se zálohovala následující položky:

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH>Zálohování položek</TH> 
</tr> 
<tr>
<td colspan="2">
 <strong>Portál služby Azure BizTalk</strong></td>
</tr> 
<tr>
<td>Konfigurace a modulu Runtime</td> 
<td>
<ul>
<li>Podrobnosti o partnera a profilu</li>
<li>Partner smlouvami</li>
<li>Sestavení vlastního nasazení</li>
<li>Mosty nasazení</li>
<li>Certifikáty</li>
<li>Transformace nasazení</li>
<li>Potrubí</li>
<li>Šablony vytvořili a uložili na portálu BizTalk služby</li>
<li>X12 ST01 a GS01 mapování</li>
<li>Ovládací prvek čísel (Upravit)</li>
<li>Hodnoty AS2 zprávy Mikrofon</li>
</ul>
</td>
</tr> 
 
<tr>
<td colspan="2">
 <strong>Služba Azure BizTalk</strong></td>
</tr> 
<tr>
<td>Certifikát SSL</td> 
<td>
<ul>
<li>Data certifikát SSL</li>
<li>Heslo certifikátu SSL</li>
</ul>
</td>
</tr> 
<tr>
<td>Nastavení služby BizTalk</td> 
<td>
<ul>
<li>Počet jednotku měřítko</li>
<li>Edition</li>
<li>Verze produktu</li>
<li>Oblast/datacentru</li>
<li>Obor názvů služby řízení (ACS) přístup a klíčem</li>
<li>Sledování připojovací řetězec databáze</li>
<li>Archivace úložiště účtu připojovacího řetězce</li>
<li>Sledování úložiště účtu připojovacího řetězce</li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2">
 <strong>Další položky</strong></td>
</tr> 
<tr>
<td>Sledování databáze</td> 
<td>Po vytvoření služba BizTalk jsou zadány podrobnosti o sledování databázi, včetně databáze SQL Azure serveru a název sledování databáze. Sledování databázi není automaticky zálohovat.
<br/><br/>
<strong>Důležité</strong><br/>
Pokud se odstraní databázi sledování a obnovit potřeb databázi, musí existovat předchozí zálohy. Pokud zálohy neexistuje, databázi sledování a její data nejsou obnovitelné. V takovém případě vytvořte novou databázi sledování se stejným názvem databáze. Replikace GEO doporučené.</td>
</tr> 
</table>

## <a name="next"></a>Další

Pokud chcete vytvořit Azure BizTalk služby na portálu Azure klasické, přejděte na [BizTalk služby: zřizování Azure pomocí klasické portál](http://go.microsoft.com/fwlink/p/?LinkID=302280). Zahájení vytváření aplikací, přejděte na [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).

## <a name="see-also"></a>Viz taky
- [Služba záložní BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=325584)
- [Obnovení služba BizTalk ze zálohy](http://go.microsoft.com/fwlink/p/?LinkID=325582)
- [BizTalk služby: Vývojář Basic, Standard a Premium edice grafu](http://go.microsoft.com/fwlink/p/?LinkID=302279)
- [BizTalk služby: Zřizování klasické portál pomocí Azure](http://go.microsoft.com/fwlink/p/?LinkID=302280)
- [BizTalk služby: Diagram stavu zřizování](http://go.microsoft.com/fwlink/p/?LinkID=329870)
- [BizTalk služby: Řídicí panel, sledování a měřítko karty](http://go.microsoft.com/fwlink/p/?LinkID=302281)
- [Služby BizTalk: omezení](http://go.microsoft.com/fwlink/p/?LinkID=302282)
- [BizTalk služby: Název vydavatele a Vystavitel klíč](http://go.microsoft.com/fwlink/p/?LinkID=303941)
- [Jak můžu začít používat služby SDK BizTalk Azure](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png
 

<properties
    pageTitle="Obnovení dat z jiného serveru DPM záložní trezoru | Microsoft Azure"
    description="Obnovení dat, které jste chráněné do trezoru Azure zálohování z jakéhokoli DPM serveru registrované do této trezoru."
    services="backup"
    documentationCenter=""
    authors="nkolli1"
    manager="shreeshd"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="giridham;jimpark;trinadhk;markgal"/>

# <a name="recovering-data-from-another-dpm-server-in-the-backup-vault"></a>Obnovení dat z jiného serveru DPM záložní trezoru
Nyní je možné obnovit data, která jste chráněné do trezoru Azure zálohování z jakéhokoli DPM serveru registrované do této trezoru. Proces provedete tak je plně integrovaný v konzole Správa DPM a je podobná jiných obnovení pracovních postupů.

Obnovení dat z jiného serveru DPM záložní trezoru musíte mít [nejnovější Azure záložní agent](http://aka.ms/azurebackup_agent)a [System Center správce ochranu dat UR7](https://support.microsoft.com/en-us/kb/3065246) .

## <a name="recover-data-from-another-dpm-server"></a>Obnovení dat z jiného DPM serveru
Obnovení dat z jiného DPM serveru:

1. Na kartě **zotavení** konzolu Správa DPM klikněte na **Přidat externí DPM** (v levém horním rohu obrazovky).

    ![Externí DPM AD](./media/backup-azure-alternate-dpm-server/add-external-dpm.png)

2. Stažení nové **přihlašovací údaje trezoru** z trezoru přidružených k **serveru DPM** kde obnovit data, zvolte DPM server ze seznamu DPM servery registrovaný u záložní trezoru a zadejte **heslo šifrování** přidružených k serveru DPM obnovit jejichž data.

    ![Externí DPM přihlašovací údaje](./media/backup-azure-alternate-dpm-server/external-dpm-credentials.png)

    >[AZURE.NOTE] Pouze DPM servery přidružené stejné trezoru registrace můžete obnovit data dalších uživatelů.

    Po úspěšném přidání externí DPM serveru můžete procházet data externího serveru DPM a místního serveru DPM z karty **obnovení** .

3. Projděte si seznam dostupných provozní servery chráněné pomocí externího serveru DPM a vyberte zdroj výsledků příslušná data.

    ![Procházet externí DPM serveru](./media/backup-azure-alternate-dpm-server/browse-external-dpm.png)

4. Vyberte **měsíc a rok** od **obnovení body** rozevírací seznam, vyberte požadované **obnovení datum** vytvoření obnovení čárky a vyberte **časového využití**.

    Seznam souborů a složek se zobrazí v dolním podokně, které se dají prohlížet a obnovit na libovolné místo.

    ![Externí DPM serveru obnovení body](./media/backup-azure-alternate-dpm-server/external-dpm-recoverypoint.png)

5. Klikněte pravým tlačítkem myši příslušnou položku a klikněte na **Obnovit**.

    ![Externí DPM obnovení](./media/backup-azure-alternate-dpm-server/recover.png)

6. Prohlédněte si **Obnovit výběr**. Ověřte data a času záložní kopie obnovována, stejně jako zdroje, ze kterého jste vytvořili záložní kopie. Pokud výběr je nesprávné, klepněte na tlačítko **Zrušit** přejděte zpátky na kartu zotavení vyberte odpovídající obnovení bod. Pokud je vybraný správný, klikněte na tlačítko **Další**.

    ![Obnovení externí DPM souhrn](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-summary.png)

7. Vyberte **Obnovit do jiného umístění**. **Přejděte** na správné místo pro obnovení.

    ![Externí DPM obnovení alternativní umístění](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-alternate-location.png)

8. Vyberte možnost související s **vytvořit kopii** **Přeskočit**a **Přepsat**.
    - **Vytvoření kopie** vytvoří kopii souboru v případě, že je konflikt názvů.
    - **Přeskočit** přeskočí obnovení soubor v případě, že je konflikt názvů.
    - **Přepsat** dojde k přepsání existující kopírování v oblasti zadané v případě konflikt názvů.

    Vyberte příslušnou možnost Obnovit **zabezpečení**. Můžete použít nastavení zabezpečení cíl počítače, na kterém obnovit data nebo nastavení zabezpečení, které jsou k dispozici produktu v době, kdy byla vytvořená bod obnovení.

    Zjistěte, jestli **oznámení** odešle po úspěšném dokončení obnovení.

    ![Upozornění na obnovení externí DPM](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-notifications.png)

9. Na obrazovce **Souhrn** jsou uvedeny možnosti zatím zvolili. Po kliknutí na **"obnovit**data obnovit až příslušný místního pracoviště.

    ![Externí DPM obnovení možnosti souhrnu](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-options-summary.png)

    >[AZURE.NOTE] Obnovení projektu můžete sledovat na kartě **monitorovací** DPM server.

    ![Sledování obnovení](./media/backup-azure-alternate-dpm-server/monitoring-recovery.png)

10. **Vymazat externí DPM** můžete kliknout na kartě **obnovení** serveru DPM odebrat zobrazení na externí server DPM.

    ![Zrušte zaškrtnutí políčka externí DPM](./media/backup-azure-alternate-dpm-server/clear-external-dpm.png)

## <a name="troubleshooting-error-messages"></a>Poradce při potížích s chybovými zprávami
|Ne. |  Chybová zpráva | Návody na řešení potíží |
| :-------------: |:-------------| :-----|
|1.|        Tento server není registrovaný do trezoru nastavil pověření trezoru.|  **Příčina:** Tato chyba se zobrazí, když vybraný soubor pověření trezoru nepatří do přiřazené DPM serveru, na kterém je pokus o obnovení záložní trezoru. <br> **Rozlišení:** Trezoru pověření moct soubor stáhněte z záložní trezoru, ke kterému je registrovaná DPM serveru.|
|2.|        Obnovitelné dat není k dispozici nebo ve vybraném serveru není DPM serveru.|   **Příčina:** Nejsou žádné další DPM servery s DPM 2012 R2 UR7 registrován záložní trezoru nebo DPM servery s DPM 2012 R2 UR7 nebyly dosud nahráli metadata nebo ve vybraném serveru není DPM serveru (označovaná taky jako Windows Server nebo Windows klienta). <br> **Rozlišení:** Pokud jsou jiné servery DPM registrované záložní trezoru, zkontrolujte SCDPM 2012 R2 UR7 a nejnovější Azure záložní agent jsou nainstalovány. <br>Pokud jsou jiné servery DPM registrované záložní trezoru s DPM 2012 R2 UR7, počkejte den po instalaci UR7 spustíte proces obnovení. Noční úlohy nahraje metadat pro všechny dříve chráněné zálohování do cloudu. Data budou k dispozici pro obnovení.|
|3.|        Žádný DPM server je registrovaná na trezoru.|   **Příčina:** Jsou žádné další DPM servery se DPM 2012 R2 UR7 nebo vyšší verzí, které jsou registrované do trezoru, ze kterého je Probíhá pokus o obnovení.<br>**Rozlišení:** Pokud jsou jiné servery DPM registrované záložní trezoru, zkontrolujte SCDPM 2012 R2 UR7 a nejnovější Azure záložní agent jsou nainstalovány.<br>Pokud jsou jiné servery DPM registrované záložní trezoru s DPM 2012 R2 UR7, počkejte den po instalaci UR7 spustíte proces obnovení. Noční úlohy nahraje metadat pro všechny dříve chráněné zálohování do cloudu. Data budou k dispozici pro obnovení.|
|4.|        Šifrování heslo k dispozici neshoduje s přístupové heslo přidružené následující server:**<server name>**|  **Příčina:** Šifrování heslo používá v procesu šifrování data z dat DPM serveru, který obnovuje neodpovídá šifrování heslo k dispozici. Agent se nemůže dešifrovat data. Proto vymáhání nezdaří.<br>**Rozlišení:** Zadejte přesně stejné heslo šifrování přidružených k serveru DPM obnovit jejichž data.|

## <a name="frequently-asked-questions"></a>Nejčastější dotazy:
1. **Proč se nedají přidat externí server DPM z jiného serveru DPM po instalaci UR7 a nejnovější agent zálohování Azure?**

    A) pro stávající servery DPM se zdroji dat, které jsou chráněny do cloudu (s použitím kumulativní aktualizace starší než 7 kumulativní aktualizace) projevit až po instalaci UR7 a nejnovější Azure Backup agent pro spuštění *Přidat externí DPM serveru*aspoň jeden den. Je potřeba nahrát metadat ochranu skupiny DPM Azure. K tomu dojde při prvním prostřednictvím noční úlohy.

2. **Co je minimální verze Azure Backup agent potřeba?**

    A) Azure zálohování agent minimální verze pro tuto funkci povolit je 2.0.8719.0.  Azure verze agent zálohování lze ověřit tak, že přejdete do ovládacích panelů **>** všech ovládacích panelů **>** programy a funkce **>** agentem služeb Microsoft Azure obnovení. Pokud verze je menší než 2.0.8719.0, stáhněte si [nejnovější agent Azure zálohování](https://go.microsoft.com/fwLink/?LinkID=288905) a nainstalujte.

    ![Zrušte zaškrtnutí políčka externí DPM](./media/backup-azure-alternate-dpm-server/external-dpm-azurebackupagentversion.png)

## <a name="next-steps"></a>Další kroky:
• [Azure záložní časté otázky](backup-azure-backup-faq.md)

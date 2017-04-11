<properties
   pageTitle="Zálohování a obnovení zašifrovaných VMs zálohování Azure"
   description="Tento článek pojednává o zálohování a obnovení prostředí pro VMs šifrovaná pomocí šifrování disku Azure."
   services="backup"
   documentationCenter=""
   authors="JPallavi"
   manager="vijayts"
   editor=""/>
<tags
   ms.service="backup"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="storage-backup-recovery"
   ms.date="10/25/2016"
   ms.author="markgal; jimpark; trinadhk"/>

# <a name="backup-and-restore-encrypted-vms-using-azure-backup"></a>Zálohování a obnovení zašifrovaných VMs zálohování Azure

Tento článek pojednává o postup zálohování a obnovení virtuálních počítačích pomocí Azure zálohování. Také poskytuje podrobnosti o podporovaných scénáře, předpoklady a řešení potíží pro názvy případů chyby.

## <a name="supported-scenarios"></a>Podporované scénáře

> [AZURE.NOTE]
1.  Zálohování a obnovení šifrované VMs je podporována pouze pro správce prostředků nasazené virtuálních počítačích. Není podporovaná pro klasické virtuálních počítačích. <br>
2.  Tuto možnost podporuje pouze pro virtuálních počítačích šifrovaná pomocí šifrovací klíč BitLocker a klíč šifrovacího klíče. Není podporovaná pro virtuálních počítačích šifrovaná pouze pomocí šifrovací klíč BitLocker. <br>

## <a name="pre-requisites"></a>Předpoklady

1.  Zašifrovaný virtuálního počítače pomocí [Šifrování disku Azure](../security/azure-security-disk-encryption.md). Je možné zašifrovat, pomocí šifrovací klíč BitLocker a klíč šifrovacího klíče.
2.  Obnovení služby trezoru byly vytvořeny a replikace úložiště nastavit pomocí kroků uvedených v článku [Příprava prostředí pro zálohování](backup-azure-arm-vms-prepare.md).

## <a name="backup-encrypted-vm"></a>Zálohování zašifrovaných OM
Pomocí následujících kroků nastavení zálohování cíle, definování zásad, konfigurace položek a aktivační událost zálohy.

### <a name="configure-backup"></a>Konfigurace zálohování

1. Pokud už máte služby Recovery trezoru otevřete, pokračujte dalším krokem. Pokud nemáte služby Recovery trezoru otevřít, ale jsou na portálu Azure v nabídce centrální, klikněte na **Procházet**.

  - V seznamu zdrojů zadejte **Obnovení služby**.
  - Jakmile začnete psát, filtry seznamu na základě vašich zadání. Pokud se zobrazí **služby Recovery trezorů**, klikněte na něj.
  
      ![Vytvoření obnovení služby trezoru kroku 1](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

    Zobrazí se seznam služby Recovery trezorů. Ze seznamu služby Recovery trezorů Výběr trezoru.

    Otevře vybranou trezoru řídicího panelu.

2. Ze seznamu položek, které se zobrazí v části trezoru klikněte na tlačítko **zálohování** otevřete zásuvné zálohování.

      ![Otevřete zásuvné zálohování](./media/backup-azure-vms-encryption/select-backup.png) 
    
3. Na zásuvné zálohování klepněte na **zálohování cíl** otevřete zásuvné cíl zálohování.

      ![Otevřete scénář zásuvné](./media/backup-azure-vms-encryption/select-backup-goal-one.png) 
    
4.   Zálohování cíl zásuvné nastavte na **kterém běží vaše pracovní zátěž** na Azure a **Co chcete zálohovat** do virtuálního počítače, klikněte na tlačítko **OK**.

    Zavře zásuvné zálohování cíl a otevře zásuvné zásad zálohování.

      ![Otevřete scénář zásuvné](./media/backup-azure-vms-encryption/select-backup-goal-two.png) 

5. Na zásuvné zásad zálohování vyberte záložní zásadu, který chcete použít trezoru a klepněte na **OK**.

      ![Vyberte zásady zálohování](./media/backup-azure-vms-encryption/setting-rs-backup-policy-new.png) 

    Podrobnosti o výchozí zásady jsou uvedené v části podrobností. Pokud chcete vytvořit zásady, vyberte **Vytvořit nový** z rozevírací nabídky. Po kliknutí na tlačítko **OK**zásady zálohování je přidružený trezoru.

    Klikněte na další VMs, ke kterému chcete přidružit trezoru.
    
6. Zvolte šifrované virtuálních počítačích přidružit zadané zásady a klikněte na **OK**.

      ![Vyberte šifrované VMs](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)
   
7. Tato stránka zobrazuje zprávu o klíčové trezoru přidružené k šifrované VMs vybrané. Zálohování služby vyžaduje jen pro čtení přístup k klíče a tajemství klíčové trezoru. Pomocí těchto oprávnění k zálohování spolu s tajná spolu s přidruženými VMs. 

      ![Zašifrovaná zpráva VMs](./media/backup-azure-vms-encryption/encrypted-vm-message.png)

      Teď, když jste definovali všechna nastavení pro trezoru v zásuvné zálohování, klikněte na tlačítko Povolit zálohování v dolní části stránky. Povolit zálohování nasazuje zásady trezoru a VMs.

8. Další fáze při přípravě instaluje agenta OM nebo jak zajistit Agent OM je nainstalovaný. Stejně postupujte i, pomocí kroků uvedených v článku [Příprava prostředí pro zálohování](backup-azure-arm-vms-prepare.md). 

### <a name="triggering-backup-job"></a>Spouštěcí úloh zálohování
Postupujte podle pokynů uvedených v článku [Záložní Azure VMs trezoru služby obnovení](backup-azure-arm-vms.md) záložní úlohy aktivační událost.

## <a name="restore-encrypted-vm"></a>Obnovení šifrované OM
Obnovení prostředí pro zašifrované a bez šifrování virtuálních počítačích je stejný. Postupujte podle pokynů uvedených v poli [Obnovit virtuálních počítačích Azure portálu](backup-azure-arm-restore-vms.md) obnovit zašifrované OM. V případě potřeby obnovit klíče a tajemství musíte se ujistit, že by již existují tohoto klíče trezoru jejich obnovení.

## <a name="troubleshooting-errors"></a>Poradce při potížích s chybami

| Operace | Podrobnosti o chybě | Rozlišení |
| -------- | -------- | -------|
| Zálohování | Ověření se nezdařilo jako zašifrované virtuálního počítače pomocí BEK samostatně. Zálohování může být užitečné jenom pro virtuálních počítačích zašifrovaných BEK a KEK. | Virtuální počítač zašifrovat pomocí BEK a KEK. Až to používat zálohování. |
| Obnovení | Tento šifrované OM nedokáže obnovit, protože klíčové trezoru přidružený k této OM neexistuje. | Vytvoření klíčových trezoru pomocí [Začít pracovat s Azure klíč trezoru](../key-vault/key-vault-get-started.md). Naleznete v článku [obnovení klíčové trezoru spolu s tajná zálohování Azure](backup-azure-restore-key-secret.md) obnovit klíč a tajná, pokud nejsou prezentovat. |
| Obnovení | Tento šifrované OM nedokáže obnovit, protože spolu s tajná přidružený k této OM není k dispozici. | Naleznete v článku [obnovení klíčové trezoru spolu s tajná zálohování Azure](backup-azure-restore-key-secret.md) obnovit klíč a tajná, pokud nejsou prezentovat. |

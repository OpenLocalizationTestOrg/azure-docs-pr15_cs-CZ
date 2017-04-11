<properties
    pageTitle="Konfigurace zabezpečeného LDAP (LDAPS) ve Azure AD Domain Services | Microsoft Azure"
    description="Konfigurovat zabezpečené LDAP (LDAPS) pro doménu spravovaný Azure AD Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Konfigurovat zabezpečené LDAP (LDAPS) pro doménu spravovaný Azure AD Domain Services
Tento článek popisuje, jak povolit zabezpečené Lightweight Directory Access Protocol (LDAPS) pro vaši doménu spravovaný Azure AD Domain Services. Zabezpečeného LDAP se nazývá také "Directory Access Protocol LDAP (Lightweight) přes Sockets Layer SSL (Secure) / zabezpečení TLS (Transport Layer)".

## <a name="before-you-begin"></a>Než začnete
Abyste mohli provést úkoly uvedené v tomto článku, musíte:

1. Platné **Azure předplatného**.

2. **Adresář Azure AD** - buď synchronizovat s místního adresáře nebo jen cloudu adresář.

3. **Azure AD Domain Services** musí být povolené na adresáři Azure AD. Pokud jste tak dosud neučinili, udělejte všechny úkoly uvedené v [příručce Začínáme](./active-directory-ds-getting-started.md).

4. **Certifikát, který chcete použít k povolení zabezpečeného LDAP**.
    - **Doporučené** - certifikát získat z vaší organizace certifikační Autority nebo veřejnou certifikační autoritou. Tato možnost konfigurace je bezpečnější.
    - Případně můžete také vytvořit [certifikát podepsaný svým držitelem](#task-1---obtain-a-certificate-for-secure-ldap) viz dále v tomto článku.

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a>Požadavky na zabezpečení certifikátu LDAP
Před povolením zabezpečeného LDAP získat platný certifikát podle následujících pokynů. Při pokusu o povolení zabezpečeného LDAP pro vaši doménu spravovaný neplatný/nesprávné certifikátem dojde k chybám.

1. **Důvěryhodné Vystavitel** - certifikát musí být vydán úřadu důvěryhodný počítačů, které potřebujete se připojit k domény pomocí zabezpečeného LDAP. Toto oprávnění může být organizace enterprise certifikační autority nebo veřejnou certifikační autorita důvěryhodný těchto počítačů.

2. **Životnost** - certifikát musí být platná pro další 3 až 6 měsíců. Zabezpečený přístup LDAP na vaši doménu spravovaný přerušení po vypršení platnosti certifikátu.

3. **Název subjektu** – název subjektu na certifikát musí být zástupný znak pro vaši doménu spravovaný. Například pokud "contoso100.com" název domény, název subjektu certifikát musí být "*. contoso100.com". Nastavte název DNS (alternativní název subjektu) na tento název zástupných znaků.

3. **Použití klíče** - certifikát musí být nakonfigurované pro následující používá - digitální podpisy a klíčových zašifrování.

4. **Účel certifikátu** - certifikát musí být platná pro ověřování serveru SSL.

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Úkol 1: získání certifikátu pro zabezpečeného LDAP
První úkol zahrnuje získání certifikát používaný k zabezpečeného přístupu LDAP spravovaných doménu. Máte dvě možnosti:

- Získejte certifikát od certifikační autority. Úřadu může být organizace podnikové certifikační Autority nebo veřejnou certifikační autoritou.

- Vytvoření certifikátu podepsaného svým držitelem.


### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>Možnost (doporučeno) – Získejte certifikát zabezpečené LDAP od certifikační autority
Pokud vaše organizace nasadí organizace veřejný klíčové infrastruktury, budete muset Získejte certifikát od certifikační autority enterprise (CA) pro vaši organizaci. Pokud vaše organizace získává své certifikáty z veřejné certifikační autority, musíte získat zabezpečené LDAP certifikát od tohoto veřejnou certifikační autoritou.

Při žádosti o certifikátu, zajistěte, aby používaly požadavkům v [požadavku na zabezpečené certifikátu LDAP](#requirements-for-the-secure-ldap-certificate).

> [AZURE.NOTE] Klientské počítače, které je potřeba se připojit k spravované domény pomocí zabezpečeného LDAP musí důvěřovat Vystavitel certifikátu LDAPS.


### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>Možnost B – vytvoření certifikátu podepsaného svým držitelem pro zabezpečeného LDAP
Můžete vytvořit certifikát podepsaný svým držitelem pro zabezpečeného LDAP, pokud:

- certifikáty ve vaší organizaci nejsou vydán certifikační autoritou organizace nebo
- neočekáváte použijte certifikát od veřejnou certifikační autority.

**Vytvoření certifikátu podepsaného svým držitelem pomocí prostředí PowerShell**

Na počítači s Windows otevřete nové okno prostředí PowerShell jako **Správce** a zadejte následující příkazy k vytvoření nové podepsaného svým držitelem.

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

V předchozím příkladu nahraďte "contoso100.com" s názvem domény DNS vaší domény spravovaných Azure AD Domain Services.

![Vyberte Azure AD adresář](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

Nově vytvořený certifikátu podepsaného svým držitelem se uloží do úložiště certifikátů místního počítače.


## <a name="task-2---export-the-secure-ldap-certificate-to-a-pfx-file"></a>Úkol 2 – Export zabezpečeného LDAP certifikát. Soubor PFX
Před zahájením tohoto úkolu, ujistěte se, že jste získali zabezpečené LDAP certifikát od certifikační autority vaší organizace nebo veřejnou certifikační autorita nebo vytvoření certifikátu podepsaného svým držitelem.

Proveďte následující kroky, chcete-li exportovat LDAPS certifikát, který chcete. Soubor PFX.

1. Stiskněte tlačítko **Start** a zadejte **R**. V dialogovém okně **Spustit** zadejte **příkaz mmc** a klikněte na **OK**.

    ![Spuštění konzoly MMC](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)

2. Na řádku **Řízení uživatelských účtů** klikněte na **Ano** spuštění MMC (Microsoft Management Console) jako správce.

3. V nabídce **soubor** klikněte na **Přidat/odebrat Snap-in..**.

    ![Přidání modulu snap-in konzoly MMC](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)

4. V dialogovém okně **Přidat nebo odebrat modulu Snap in** vyberte modulu snap-in **certifikáty** a klepněte **Přidat >** tlačítko.

    ![Přidání modulu snap-in Certifikáty konzoly MMC](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)

5. V průvodci **modulu snap-in Certifikáty** vyberte **účet** a klikněte na tlačítko **Další**.

    ![Přidání modulu snap-in Certifikáty pro účet počítače](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)

6. Na stránce **Vybrat počítač** vyberte **místním počítači: (počítač se systémem této konzoly)** a klikněte na tlačítko **Dokončit**.

    ![Přidání snap-in Certifikáty – vyberte počítače](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)

7. V dialogovém okně **Přidat nebo odebrat modulu Snap in** kliknutím na **OK** přidáte modulu snap-in Certifikáty do konzoly MMC.

    ![Přidání modulu snap-in Certifikáty do konzoly MMC - dokončení](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)

8. V okně MMC kliknutím rozbalte **Kořenový adresář konzoly**. Měli byste vidět modulu snap-in Certifikáty načíst. Klikněte na tlačítko **certifikáty (místní)** rozbalte. Kliknutím rozbalte uzel **osobní** , následovaný uzel **certifikáty** .

    ![Otevřít osobní úložiště](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)

9. Měli byste vidět certifikátu podepsaného svým držitelem, kterou jsme vytvořili. Můžete zkoumat vlastnosti certifikát, který chcete zajistit, aby neměl že miniaturu odpovídá vykázanému PowerShell windows při vytvoření certifikátu.

10. Vyberte certifikát podepsaný svým držitelem a **klikněte pravým tlačítkem myši**. Z nabídky po kliknutí pravým tlačítkem vyberte **Všechny úkoly** a zaškrtněte políčko **Exportovat …**.

    ![Exportujte certifikát](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)

11. V **Průvodci exportem certifikát**klikněte na **Další**.

    ![Certifikát Průvodce exportem](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)

12. Na stránce **Export privátním klíčem** vyberte **Ano, exportovat privátním klíčem**a klikněte na tlačítko **Další**.

    ![Exportujte certifikát privátním klíčem](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [AZURE.WARNING] Je třeba exportovat privátním klíčem spolu s certifikát. Pokud zadáte PFX, který neobsahuje privátním klíčem certifikátu, povolení zabezpečeného LDAP pro vaši doménu spravovaný nezdaří.

13. Na stránce **Formát souboru pro Export** vyberte **osobní informace Exchange - PKCS #12 (. PFX)** formátu souboru pro exportovaný certifikát.

    ![Formát souboru pro export certifikátu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [AZURE.NOTE] Pouze. Formát souboru PFX je podporován. Není exportujte certifikát, který chcete. Formát souboru CER.

14. Na kartě **zabezpečení** vyberte možnost **heslo** a zadejte v hesla můžete chránit. Soubor PFX. Protože bude potřeba v dalším úkolem, mějte na paměti Toto heslo. Klikněte na **Další** pokračujte.

    ![Heslo pro export certifikátu ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [AZURE.NOTE] Poznamenejte si toto heslo. Musíte při povolování zabezpečeného LDAP pro tuto doménu spravovaný v [úkolu 3 – povolení zabezpečeného LDAP spravované domény](#task-3---enable-secure-ldap-for-the-managed-domain)

15. Na stránce **Export souboru** zadejte název souboru a místo, kam chcete exportujte certifikát.

    ![Cesta k exportu certifikátu](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)

16. Na následující stránce klikněte na **Dokončit** pro export certifikátu do souboru PFX. Dialogové okno s potvrzením byste měli vidět při vyexportování certifikát.

    ![Exportujte certifikát Hotovo](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="task-3---enable-secure-ldap-for-the-managed-domain"></a>Úkol 3 – zabezpečeného LDAP povolit spravovanou domény
Povolit zabezpečeného LDAP, proveďte následující kroky konfigurace:

1. Přejděte na **[Azure klasický portálu](https://manage.windowsazure.com)**.

2. Vyberte uzel **Služby Active Directory** v levém podokně.

3. Vyberte adresář Azure AD (označovány jako "klienta"), u kterého jste povolili Azure AD Domain Services.

    ![Vyberte Azure AD adresář](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Klikněte na kartu **Konfigurovat** .

    ![Konfigurace na kartě adresáře](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Přejděte dolů k oddílu s názvem **domény služby**. Měli byste vidět možnost s názvem **Zabezpečené LDAP (LDAPS)** , jak je vidět na následující snímek obrazovky:

    ![Konfigurace služby domény](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)

6. Klikněte na tlačítko **Konfigurace certifikát …** zobrazte dialogové okno **Konfigurovat certifikát pro zabezpečené LDAP** .

    ![Konfigurace certifikátu pro zabezpečeného LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)

7. Klikněte na ikonu složky pod **PFX soubor pomocí certifikátu** za účelem určit PFX soubor obsahující certifikát, který chcete použít pro zabezpečený přístup LDAP spravovaných doménu. Také zadejte heslo, které jste zadali při exportu certifikátu do souboru PFX. Klikněte na tlačítko Hotovo dole.

    ![Zadejte zabezpečené LDAP PFX souboru nebo heslo](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)

8. Části **doméně služby** karty **Konfigurovat** by měla získat jako neaktivní a je v **na zjištění stavu úkolů …** stavu několik minut. Během tohoto období certifikát LDAPS ověření správnosti a zabezpečeného LDAP nakonfigurovaný pro vaši doménu spravovaný.

    ![Zabezpečený LDAP - nevyřízená](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

    > [AZURE.NOTE] Povolení zabezpečeného LDAP pro vaši doménu spravovaný trvá asi 10 až 15 minut. Pokud zadané certifikátu zabezpečení LDAP neodpovídá požadovaná kritéria, zabezpečeného LDAP není povolený adresáře a zobrazí se nepovede. Například název domény je nesprávné, certifikát vypršela nebo vyprší jejich platnost brzo atd.

9. Je-li zabezpečeného LDAP úspěšně pro vaši doménu spravovaný **na zjištění stavu úkolů …** zprávy by měl zmizet. Měli byste vidět Miniatura certifikátu zobrazeného.

    ![Zabezpečeného LDAP povolena](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>


## <a name="task-4---enable-secure-ldap-access-over-the-internet"></a>Úkol 4 - zabezpečené LDAP přístup přes internet
**Volitelné úkolu** – Pokud nebudete přístup k spravované domény pomocí LDAPS přes internet, přeskočte tento úkol konfigurace.

Před zahájením tohoto úkolu, zkontrolujte, zda že jste dokončili kroků uvedených v [úkolu 3](#task-3---enable-secure-ldap-for-the-managed-domain).

1. Měli byste vidět možnost **Povolit ZABEZPEČENÉ LDAP přístup přes INTERNET** v části **domény služby** na stránce **Konfigurovat** . Je tato možnost nastavená na hodnotu **Ne** ve výchozím nastavení od přístup k Internetu spravovaných doménu prostřednictvím zabezpečeného LDAP je ve výchozím nastavení zakázaná.

    ![Zabezpečeného LDAP povolena](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)

2. Přepnout **Povolení přístupu ke zabezpečeného LDAP přes INTERNET na** **Ano**. Klikněte na tlačítko **Uložit** na panelu dole.
    ![Zabezpečeného LDAP povolena](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)

3. **Domény** části služba karty **Konfigurovat** by měla získat jako neaktivní a je v **na zjištění stavu úkolů...** stavu několik minut. Po určité době je povolený přístup k Internetu na vaši doménu spravovaný prostřednictvím zabezpečeného LDAP.

    ![Zabezpečený LDAP - nevyřízená](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

    > [AZURE.NOTE] Chcete-li povolit přístup k Internetu prostřednictvím zabezpečeného LDAP pro vaši doménu spravovaný trvá asi 10 minut.

4. Při aktivované úspěšně zabezpečený přístup LDAP na vaši doménu spravovaný přes internet, **na zjištění stavu úkolů …** zprávy by měl zmizet. Měli byste vidět externí IP adresy, kterou lze použít pro přístup k adresáři přes LDAPS v poli **Externí IP adres pro LDAPS přístup**.

    ![Zabezpečeného LDAP povolena](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-to-access-the-managed-domain-from-the-internet"></a>Úkol 5: Konfigurace DNS pro přístup k spravované domény z Internetu
**Volitelné úkolu** – Pokud nebudete přístup k spravované domény pomocí LDAPS přes internet, přeskočte tento úkol konfigurace.

Před zahájením tohoto úkolu, zkontrolujte, zda že jste dokončili kroků uvedených v [úkolu 4](#task-4---enable-secure-ldap-access-over-the-internet).

Po povolení zabezpečený přístup LDAP přes internet pro vaši doménu spravovaný budete muset aktualizovat DNS tak, aby klientské počítače najdete této spravované domény. Na konci úkolu 4 externí IP adresu zobrazené na kartě **Konfigurovat** v **Externí IP adres pro LDAPS přístup**.

Konfigurace externí poskytovatele DNS tak, aby název DNS spravovaný domény (třeba "contoso100.com") odkazuje na tento externí IP adresu. V našem příkladu potřebujeme vytvořte následující položku DNS:

    contoso100.com  -> 52.165.38.113

To je vše: Nyní jste připravení se připojit k spravované domény pomocí zabezpečeného LDAP přes internet.

> [AZURE.WARNING] Myslete na to, že klientské počítače musí důvěřovat Vystavitel certifikátu LDAPS mohli připojit k spravované domény pomocí LDAPS. Pokud používáte enterprise certifikační autority nebo veřejně důvěryhodnou certifikační autoritou, nepotřebujete nic dělat, protože klientské počítače důvěřovat tyto vydavatelů certifikátu. Pokud používáte certifikátu podepsaného svým držitelem, musíte instalovat veřejnou část certifikátu podepsaného svým držitelem do úložiště důvěryhodnou certifikační v klientském počítači.

<br>

## <a name="related-content"></a>Související obsah

- [Spravovat spravovaná domain Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)

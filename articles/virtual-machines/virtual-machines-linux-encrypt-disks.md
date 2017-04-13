<properties
   pageTitle="Šifrování disků na Linux OM | Microsoft Azure"
   description="Postup při šifrování disků na Linux OM pomocí rozhraní příkazového řádku Azure a nasazení modelu správce prostředků"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/11/2016"
   ms.author="iainfou"/>

# <a name="encrypt-disks-on-a-linux-vm-using-the-azure-cli"></a>Šifrování disků na Linux OM pomocí rozhraní příkazového řádku Azure
Vylepšené virtuálního počítače (OM) zabezpečení a dodržování předpisů může být virtuálních disků v Azure zašifrován u ostatních. Disků jsou šifrovány pomocí klíče, které jsou správně zapojené trezoru klíč Azure. Ovládací prvek tyto klíče a můžete auditovat jejich použití. Tento článek popisuje postup při šifrování virtuálních disků na Linux OM pomocí rozhraní příkazového řádku Azure a nasazení modelu správce prostředků.


## <a name="quick-commands"></a>Rychlé příkazy
Pokud potřebujete rychle provést úkol tyto údaje oddíl základu příkazy k šifrování virtuálních disků na vaše OM. Podrobnější informace a kontext pro každý krok najdete zbytek dokumentu a [od tady](#overview-of-disk-encryption).

Je třeba [Nejnovější rozhraní příkazového řádku Azure](../xplat-cli-install.md) nainstalovaný a přihlášení k lyncu v režimu správce prostředků následujícím způsobem:

```
azure config mode arm
```

V následujících příkladech nahraďte názvy parametrů příklad vlastní hodnoty. Názvy parametrů příklad zahrnout `myResourceGroup`, `myKeyVault`, a `myVM`.

Nejprve povolit poskytovatele Azure klíč trezoru v rámci předplatného Azure a vytvořit skupinu zdrojů. Následující příklad vytvoří skupinu název zdroje `myResourceGroup` v `WestUS` umístění:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Vytvoření Azure klíčové trezoru. Následující příklad vytvoří klíč trezoru s názvem `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Vytvoření šifrovací klíč v trezoru klíče a povolení pro šifrování disku. Následující příklad vytvoří klíč s názvem `myKey`:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

Vytvoření koncového bodu pomocí služby Azure Active Directory pro zpracování ověřování a výměny klíče z trezoru klíče. `--home-page` a `--identifier-uris` nemusí být skutečná směrovatelné adresa. Pro nejvyšší úroveň zabezpečení tajemství klienta se má používat místo hesla. Rozhraní příkazového řádku Azure nelze generovat aktuálně tajemství klienta. Klient tajemství můžete generovat pouze na portálu Azure. Následující příklad vytvoří koncový bod služby Azure Active Directory s názvem `myAADApp` a používá heslo `myPassword`. Zadejte svoje vlastní heslo následujícím způsobem:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Poznámka `applicationId` ukazuje výstup z předchozího příkazu. Toto ID aplikace se používá v následujících kroků:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

Přidání disku dat do existující OM. Následující příklad přidává disk dat OM s názvem `myVM`:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Zkontrolujte podrobnosti pro trezoru klíče a klíčem, který jste vytvořili. Budete potřebovat ID trezoru klíče, URI a klíč URL v posledním kroku. Následující příklad kontroluje podrobnosti pro trezoru klíč s názvem `myKeyVault` a klíč s názvem `myKey`:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Šifrování disků takto, zadání vlastní názvy parametrů v celém:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Rozhraní příkazového řádku Azure nenabízí možnost podrobného chyby během procesu šifrování. Další informace o řešení problémů, zkontrolujte `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`. Příkaz předchozí má mnoha proměnných a dostanete nemusí mnohem označení proč nezdaří, všechny příkazy příklad by to bylo následujícím způsobem:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Nakonec zkontrolujte stav šifrování potvrďte, že jsou teď šifrovaná virtuální disků. Následující příklad zkontroluje stav OM s názvem `myVM` v `myResourceGroup` pole Skupina zdroje:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a>Základní informace o šifrování disku
V ostatních pomocí [dm crypt](https://wikipedia.org/wiki/Dm-crypt)jsou šifrovány virtuálních disků na Linux VMs. Je bezplatná pro šifrování virtuálních disků v Azure. Klíče jsou uložené v Azure klíč trezoru použijete ochranu software nebo můžete importovat nebo vygenerování klíčů v hardwarové zabezpečení moduly (moduly hardwarového zabezpečení) oprávnění FIPS standardy úrovně 2 140-2. Zachování kontroly tyto klíče a můžete auditovat jejich použití. Tyto klíče slouží k šifrování a dešifrování virtuálních disků připojené k vaší OM. Koncový bod služby Azure Active Directory poskytuje mechanismus zabezpečení pro vydávání tyto klíče jako VMs jsou zapnutá a vypnout.

Postup pro šifrování virtuálního počítače vypadá takto:

1. Vytvoření šifrovací klíč trezoru klíč Azure.
2. Konfigurace šifrovací klíč možné používat pro šifrování discích.
3. Číst šifrovací klíč z trezoru klíč Azure, vytvoření koncového bodu služby Azure Active Directory pomocí potřebná oprávnění.
4. Zadejte příkaz šifrování virtuální disků určující koncového bodu služby Azure Active Directory a odpovídající šifrovací klíč se nemusí používat.
5. Koncový bod služby Azure Active Directory žádosti požadované šifrovací klíč z Azure klíč trezoru.
6. Virtuální disků šifrování pomocí zadaného šifrovací klíč.


## <a name="supporting-services-and-encryption-process"></a>Podpora služby a šifrování obrázku
Šifrování disku závisí na následující součásti:

- **Azure klíč trezoru** – slouží k ochraně klíče a tajemství používá pro šifrování/dešifrování proces disku. 
  - Pokud existuje, můžete použít existující trezoru klíč Azure. Nemáte vyhrazeného trezoru klíč pro šifrování discích.
  - Oddělení pro správu omezení a klíče viditelnost, můžete vytvořit vyhrazené trezoru klíče.
- **Azure Active Directory** - zpracovává zabezpečené výměny požadované klíče a ověřování pro požadované akce. 
  - Můžete obvykle existující instanci služby Azure Active Directory pro uložení vaší aplikace. 
  - Aplikace se dozvíte víc koncového bodu služby klíč trezoru a virtuálního počítače a požádat o získat Vystavitel odpovídající klíče. Nejsou vývoje skutečné aplikace, která lze integrovat s Azure Active Directory.


## <a name="requirements-and-limitations"></a>Požadavky a omezení
Podporované scénáře a požadavky pro šifrování disku:

- Následující Linux server skladové jednotky - systémem Ubuntu CentOS, SUSE a SUSE Linux Enterprise Server (SLES) a červené klobouk Enterprise Linux.
- Všechny zdroje (například trezoru klíč účtu úložiště a OM) musí být ve stejné Azure oblasti a předplatným.
- Standardní A, D, Pošta, G a GS řady VMs.

Šifrování disku není aktuálně podporován v následujících situacích:

- Základní vrstva VMs.
- VMs vytvořené pomocí klasického nasazení modelu.
- Zakázání šifrování disku OS na Linux VMs.
- Aktualizace klíče na už šifrované OM Linux.


## <a name="create-the-azure-key-vault-and-keys"></a>Vytvoření Azure klíč trezoru a klíče
Dokončete zbývající část tohoto průvodce potřebujete [Nejnovější rozhraní příkazového řádku Azure](../xplat-cli-install.md) nainstalovaný a přihlášení k lyncu v režimu správce prostředků následujícím způsobem:

```
azure config mode arm
```

V celém příkazu příklady nahraďte všechny parametry příkladu s názvy, umístění a hodnoty klíče. Následující příklady konvence z `myResourceGroup`, `myKeyVault`, `myAADApp`atd.

Cílem prvního kroku je vytvořit trezoru Azure klíč pro uložení vaší klíče. Azure trezoru klíč mohou být uloženy klíčů, tajemství nebo hesla, které umožňují bezpečně jejich implementace ve vašich aplikací a služeb. Virtuální disk šifrování pomocí trezoru klíč pro ukládání kryptografický klíč, který se používá k šifrování a dešifrování virtuální disků. 

Povolení poskytovatele Azure klíč trezoru v Azure předplatné a pak vytvořit skupinu zdrojů. Následující příklad vytvoří skupinu zdroje s názvem `myResourceGroup` v `WestUS` umístění:

```bash
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Klíč trezoru Azure obsahující klíče a přidružená výpočetním zdroje, jako jsou úložiště a OM samotné musí být umístěny ve stejné oblasti. Následující příklad vytvoří trezoru klíč Azure s názvem `myKeyVault`:

```bash
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Klíče pomocí software nebo hardwaru zabezpečení modelu (HSM) ochranu mohou být uloženy. Použití HSM vyžaduje premium klíč trezoru. Je další náklady pro vytvoření premium klíč trezoru spíše než standardní trezoru klávesy, která ukládá software chráněn klíče. Vytvořit premium trezoru klíč, v předchozím kroku přidejte `--sku Premium` s příkazem. V následujícím příkladu software chráněn klíče protože jsme vytvořili standardní trezoru klíče. 

Pro oba modely ochrany Azure platformu musí mít udělený přístup požádat o cryptographic klíče při spuštění OM dešifrovat virtuálních disků. Vytvoření šifrovacího klíče v rámci trezoru klíče a pak ji povolit pro použití s šifrování virtuální disku. Následující příklad vytvoří klíč s názvem `myKey` a potom ji povolí pro šifrování disku:

```bash
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-the-azure-active-directory-application"></a>Vytvoření aplikace služby Azure Active Directory
Když virtuálních disků jsou zašifrované nebo dešifrovat, použijete koncový bod pro zpracování ověřování a výměny klíče z trezoru klíče. Tento koncový bod aplikaci služby Azure Active Directory, umožňuje Azure platformu požádat o odpovídající klíče jménem OM. Výchozí instanci služby Azure Active Directory je k dispozici ve vašem předplatném mnoho organizací máte vyhrazená adresářů Azure Active Directory.

Jak nevytváříte úplnou aplikaci služby Azure Active Directory `--home-page` a `--identifier-uris` parametrů v následujícím příkladu nemusí být skutečná směrovatelné adresa. Následující příklad také určuje tajná na základě heslo spíše než generování kláves sady v Azure portálu. Jako tentokrát vygenerování klíčů nelze provést z Azure rozhraní příkazového řádku. 

Vytvoření aplikace služby Azure Active Directory. Následující příklad vytvoří aplikace s názvem `myAADApp` a používá heslo `myPassword`. Zadejte svoje vlastní heslo následujícím způsobem:

```bash
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Poznamenejte si `applicationId` , je vrácena do výstupu z předchozího příkazu. Toto ID aplikace se používá v některé ze zbývajících kroků. Dále vytvořte hlavní název služby (hlavního názvu služby) tak, aby aplikace přístupný prostředí. Úspěšně šifrování nebo dešifrování virtuálních disků, oprávnění šifrovací klíč uložené v klíč trezoru nastavte povolit aplikaci služby Azure Active Directory číst klíče. 

Vytvořte hlavní název služby a nastavte příslušná oprávnění následujícím způsobem:

```bash
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a>Přidání virtuálního disku a zkontrolovat stav šifrování
Ve skutečnosti šifrování některé virtuálních disků umožňuje přidat do existující OM disk. Přidejte disk dat 5Gb na existující OM následujícím způsobem:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Virtuální disků nejsou aktuálně zašifrován. Zjištění aktuálního stavu šifrování z vaší OM následujícím způsobem:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a>Šifrování virtuálních disků
Teď šifrování virtuálních disků, posunete společně všechny předchozí součásti:

1. Zadejte aplikace služby Azure Active Directory a heslo.
2. Určení trezoru klíč pro ukládání metadata šifrované disku.
3. Určení klíče pro účely skutečné šifrování a dešifrování.
4. Určete, jestli chcete šifrovat disku OS disků dat nebo všechny.

Umožňuje zkontrolujte podrobnosti pro trezoru klíč Azure a klíčem, který jste vytvořili, jako je to potřeba ID trezoru klíče URI a potom základní adresu URL v posledním kroku:

```bash
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Šifrování virtuální disků pomocí výstup `azure keyvault show` a `azure keyvault key show` příkazy následujícím způsobem:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Podle předchozího příkazu má mnoha proměnných, v následujícím příkladu je všechny příkazy pro odkazy:

```bash
azure vm enable-disk-encryption -g myResourceGroup -n MyVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \ 
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Rozhraní příkazového řádku Azure nenabízí možnost podrobného chyby během procesu šifrování. Další informace o řešení problémů, zkontrolujte `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` na OM jsou šifrování.

Nakonec umožňuje zkontrolovat stav šifrování potvrďte, že jsou teď šifrovaná virtuální disků:

```bash
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a>Přidejte další data disků
Po zašifrujete disků dat můžete později přidat další virtuálních disků vaší OM a také šifrování. Když spustíte `azure vm enable-disk-encryption` příkazu, zvýšit pomocí verze pořadí `--sequence-version` parametr. Tento parametr posloupnost verze umožňuje provádět opakované operace na stejné OM.

Umožňuje například přidejte druhý virtuální disk k vaší OM takto:

```bash
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Spusťte příkaz šifrování virtuálních disků tento čas přidání `--sequence-version` parametry a přičítání z našich prvním spuštění následujícím způsobem:

```bash
azure vm enable-disk-encryption --resource-group myResourceGroup --vm-name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a>Další kroky

- Další informace o správě trezoru klíč Azure, včetně odstranění klíče a skříňky najdete v článku [Správa trezoru klíč pomocí rozhraní příkazového řádku](../key-vault/key-vault-manage-with-cli.md).
- Další informace o šifrování disku, například příprava šifrované vlastní OM k nahrání na Azure najdete v článku [Šifrování disku Azure](../security/azure-security-disk-encryption.md).
<properties
   pageTitle="Klíč aplikace Vault Developer's Guide | Microsoft Azure"
   description="Vývojáři mohou pomocí trezoru klíčů Azure ke správě šifrovacích klíčů v rámci prostředí Microsoft Azure. "
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/03/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-developers-guide"></a>Příručka pro vývojáře Azure trezor klíčů
Pomocí trezoru klíčů, bude možné získat zabezpečený přístup k citlivé informace z v rámci aplikace tak, aby:

- Klíče a tajné klíče budou chráněny bez nutnosti psát kód sami a budou moci snadno používat z aplikace.
- Budete moci mít vlastní zákazníků a spravovat vlastní klíče, takže se můžete soustředit na poskytování základních funkcí softwaru. Tímto způsobem nebude aplikace vlastní odpovědnost nebo odpovědnost potenciálních zákazníků klienta klíče a tajné klíče.
- Vaše aplikace může použít klíče pro podepisování a šifrování ještě zachová správy klíčů externí aplikace tak, aby roztok je vhodný pro aplikace, která je geograficky distribuovány.

- Trezor klíč po vydání září 2016, aplikace můžete nyní využít trezor klíčů certifikátů. Další informace viz **o tajemství, klíče a certifikáty** článku v [odkazu ostatní](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Další obecné informace o trezoru klíčů Azure naleznete v tématu [Co je trezor klíč](key-vault-whatis.md).

## <a name="videos"></a>Videa
Toto video ukazuje, jak vytvořit vlastní trezor klíčů a použití z ukázkové aplikace "Hello klíč trezor".

[AZURE.VIDEO azure-key-vault-developer-quick-start]

Odkazy na zdroje, které jsou uvedeny ve videu:
- [Azure PowerShell](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Ukázkový kód aplikace Azure Key Vault](http://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

Naučit více můžete sledovat [Blog trezor klíč](http://aka.ms/kvblog) a účast ve [Fóru trezor klíč](http://aka.ms/kvforum).

## <a name="creating-and-managing-key-vaults"></a>Vytváření a Správa klíčů trezory

Před prací s trezoru klíčů Azure ve vašem kódu, můžete vytvořit a spravovat trezorů odpočinku, šablony správce prostředků, PowerShell nebo rozhraní příkazového řádku, jak je popsáno v následujících článcích:

- [Vytvořit a spravovat klíčové trezory s ostatními](https://msdn.microsoft.com/library/azure/mt620024.aspx)
- [Vytvářet a spravovat klíče trezorů pomocí prostředí PowerShell](key-vault-get-started.md)
- [Vytvářet a spravovat klíče trezorů s CLI](key-vault-manage-with-cli.md)
- [Vytvořit klíče trezor a přidejte tajný klíč pomocí šablony správce prostředků Azure](../resource-manager-template-keyvault.md)

>[AZURE.NOTE] Operace proti klíčové trezory prostřednictvím zvukové poplašné zařízení ověřovány a autorizovány prostřednictvím klíč trezor na vlastní zásady přístupu, definované podle trezor.

## <a name="coding-with-key-vault"></a>Kódování pomocí trezoru klíčů

Systém správy trezor klíč pro programátory se skládá z několika rozhraní s zbytku jako nadace, [Klíč trezor REST API Reference](https://msdn.microsoft.com/library/azure/dn903609.aspx).

Je možné, za úspěšné ověření postupujte takto:

- Správu kryptografických klíčů pomocí [Vytvoření](https://msdn.microsoft.com/library/azure/dn903634.aspx), [Import](https://msdn.microsoft.com/library/azure/dn903626.aspx), [aktualizace](https://msdn.microsoft.com/library/azure/dn903616.aspx), [Odstranění](https://msdn.microsoft.com/library/azure/dn903611.aspx) a další operace

- Správa tajných klíčů pomocí [získat](https://msdn.microsoft.com/library/azure/dn903633.aspx), [Aktualizovat](https://msdn.microsoft.com/library/azure/dn986818.aspx), [Odstranit](https://msdn.microsoft.com/library/azure/dn903613.aspx) a další operace

- Kryptografické klíče jsou používány se [symbolem](https://msdn.microsoft.com/library/azure/dn878096.aspx)/[Ověřte](https://msdn.microsoft.com/library/azure/dn878082.aspx), [WrapKey](https://msdn.microsoft.com/library/azure/dn878066.aspx)/[UnwrapKey](https://msdn.microsoft.com/library/azure/dn878079.aspx) a [šifrovat](https://msdn.microsoft.com/library/azure/dn878060.aspx)/operace[dešifrování](https://msdn.microsoft.com/library/azure/dn878097.aspx)

Jsou k dispozici pro práci s trezor klíč následující sady SDK:

|[![ROZHRANÍ .NET](./media/key-vault-developers-guide/msft.netlogo_purple.png)](https://msdn.microsoft.com/library/mt765854.aspx)|[![Node.js](./media/key-vault-developers-guide/nodejs.png)](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)
|:--:|:--:|
|[Dokumentace sady SDK rozhraní .NET](https://msdn.microsoft.com/library/mt765854.aspx)|[Node.js SDK dokumentace](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest)|
|[Balíček sady SDK rozhraní .NET na Nuget](http://www.nuget.org/packages/Microsoft.Azure.KeyVault)|[Node.js SDK balíček](https://www.npmjs.com/package/azure-keyvault)|

Další informace o verze 2.x SDK rozhraní .NET naleznete v [poznámkách k verzi](key-vault-dotnet2api-release-notes.md).

## <a name="example-code"></a>Příklad kódu
Kompletní příklady pomocí trezoru klíčů s aplikací naleznete v tématu:

- Ukázková aplikace .NET *HelloKeyVault* a příklad Azure webové služby. [Ukázky kódu Azure trezor klíč](http://www.microsoft.com/download/details.aspx?id=45343)
- Návod k použití trezoru klíčů Azure z webové aplikace v Azure. [Použití Azure trezor klíčů z webové aplikace](key-vault-use-from-web-application.md)

## <a name="how-tos"></a>Postupy

Následující články a scénáře poskytují úkolu konkrétní návod pro práci s trezoru klíčů Azure:

- [Změnit ID klienta trezor klíčů po přesunutí předplatného](key-vault-subscription-move-fix.md) - při přesunout předplatné Azure z klienta A pro klienta B, vaše stávající klíčové trezory jsou přístupná pomocí objektů (uživatelů a aplikace) v klienta B. oprava pomocí této příručky.
- [Přístup k trezoru klíčů za branou firewall](key-vault-access-behind-firewall.md) - trezor klíče, trezor klíčů klientské aplikace musí být možné získat přístup k více ukazatelích pro různé funkce.

- [Jak generovat a Transfer HSM-Protected klíče pro trezor klíčů Azure](key-vault-hsm-protected-keys.md) - to vám pomůže plánování, generovat a potom přenést vlastní klíče hardwarového zabezpečení chráněné pomocí trezoru klíčů Azure.
- [Způsob předání zabezpečených hodnot (například hesla) během nasazení](../resource-manager-keyvault-parameter.md) - Pokud potřebujete předat hodnotu zabezpečení (například hesla) jako parametr během nasazení, lze uložit hodnotu jako tajný klíč do trezoru klíčů Azure a referenční hodnoty v jiných šablon správce prostředků.
- [Jak používat Trezor klíč pro extensible správy klíčů se systémem SQL Server](https://msdn.microsoft.com/library/dn198405.aspx) - The SQL Server Connector pro trezor klíčů Azure umožňuje serveru SQL Server a SQL v VM využít trezor klíčů Azure služby jako zprostředkovatel Extensible klíč Management (EKM) k ochraně šifrovací klíče pro propojení aplikací; Šifrování transparentní dat, zálohování šifrování a šifrování na úrovni sloupců.
- [Nasazení certifikátů pro VMs z trezoru na klíč](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) - cloud aplikace spuštěné v virtuálního počítače v Azure vyžaduje certifikát. Jak můžete získat tento certifikát do tohoto modulu VM dnes?
- [Postup instalace trezor klíč konce střídání klíčů a auditování](key-vault-key-rotation-log-monitoring.md) - tento nevystavíte slabé stránky zabezpečení prostřednictvím jak nastavit auditování pomocí trezoru klíčů Azure a střídání klíčů.

Další pokyny specifických úkolů integrace a trezory na klíče pomocí Azure naleznete [Příklady šablon správce prostředků Azure Ryan Jones pro trezor klíčů](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

## <a name="integrated-with-key-vault"></a>Integrována do trezoru klíčů

Tyto články pojednávají o další scénáře a služby, které vytvářejí naši z nebo integrovat trezor klíč.

- [Šifrování disku Azure](../security/azure-security-disk-encryption.md) využívá funkce [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux zajistit šifrování svazku operačního systému a datové disky a průmyslové standardní [nástroje BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funkce systému Windows. Řešení je integrováno trezoru klíčů Azure můžete řídit a spravovat disk šifrovací klíče a tajné klíče předplatné trezor klíčů při zajištění, že všechna data na discích virtuálních počítačů jsou zašifrovány v klidu v úložišti Azure.


## <a name="supporting-libraries"></a>Pomocné knihovny

- [Microsoft Azure Key trezor základní knihovna](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) poskytuje `IKey` a `IKeyResolver` rozhraní pro vyhledání klíče z identifikátorů a provádění operací s klíči.

- [Rozšíření trezor klíč Microsoft Azure](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) poskytuje rozšířené možnosti pro trezor klíčů Azure.

## <a name="other-key-vault-resources"></a>Další zdroje pro trezor klíčů
- [Trezor klíčů blogu](http://aka.ms/kvblog)
- [Fórum pro trezor klíčů](http://aka.ms/kvforum)

<properties
   pageTitle="Běžné příčiny cloudové služby role recyklace | Microsoft Azure"
   description="Role cloudové služby, která neočekávaně recykluje mohou způsobit významné prostoje. Tady jsou některé běžné problémy, které způsobují role je z koše, které lze zmenšit výpadek služeb."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="common-issues-that-cause-roles-to-recycle"></a>Běžné problémy, které způsobují role do koše

Tento článek popisuje některé běžné příčiny problémů s nasazením a obsahuje tipy k řešení těchto problémů. Označení k potížím s aplikací je role instance nejde spustit a cykly mezi stavy inicializace zaneprázdněn a ukončení.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-runtime-dependencies"></a>Chybějící modul runtime závislostí

Pokud roli v aplikaci závisí na kterékoli sestavení, která není součástí .NET Framework nebo Azure managed knihovny, je třeba explicitně zahrnout sestavení balíček aplikace. Mějte na paměti, že ostatní rámce Microsoft nejsou k dispozici na Azure ve výchozím nastavení. Pokud vaše role závisí na těchto framework, musíte přidat tyto sestavení na balíček aplikace.

Před budování a balíček aplikace, postupujte následovně:

- Pokud používáte Visual Studiu, zkontrolujte, jestli **Že místní kopie** vlastnost je nastavena na **hodnotu True** pro každé odkazované sestavení v projektu, který není součástí Azure SDK nebo .NET Framework.

- Zkontrolujte, že nastavení(Web.config)) neodkazuje nepoužitý sestavení v elementu kompilace.

- **Akce sestavit** každý soubor .cshtml nastavenou **obsahu**. Zajistíte, že soubory, které se zobrazí správně v okně balíček a umožňuje jiné odkazované soubory zobrazit v okně balíček.

## <a name="assembly-targets-wrong-platform"></a>Sestavení cílů nepovedlo platformy

Azure je prostředí 64bitová verze. Proto sestavení .NET kompilovaný pro 32bitovou verzi cílového nebude fungovat na Azure.

## <a name="role-throws-unhandled-exceptions-while-initializing-or-stopping"></a>Role vyvolá neošetřené výjimky při inicializace nebo ukončení

Požadované výjimky vyvolané metody třídy [RoleEntryPoint] , která zahrnuje [při spuštění], [OnStop]a [Spustit] metody, jsou neošetřené výjimky. V případě neošetřené výjimce v jednom z těchto postupů bude odpadkového roli. Pokud opakovaně recyklaci role může došlo k neošetřené výjimce pokaždé, když se pokusí o spuštění.

## <a name="role-returns-from-run-method"></a>Role vrátí z spustit metody

Způsob [spuštění] slouží ke spuštění donekonečna udržovat. Pokud váš kód přepíše metodu [Spustit] by měl spánek donekonečna udržovat. Vrátí metody [Run] recykluje roli.

## <a name="incorrect-diagnosticsconnectionstring-setting"></a>Nesprávné DiagnosticsConnectionString nastavení

Pokud aplikace používá diagnostiky Azure, musíte zadat konfigurační soubor služby `DiagnosticsConnectionString` nastavení. Toto nastavení zadají HTTPS připojení ke svému účtu úložiště v Azure.

Zajistit, aby vaše `DiagnosticsConnectionString` před nasazením balíčku aplikace Azure správnost nastavení, ověřte následující:  

- `DiagnosticsConnectionString` Nastavení bodů na účet platné úložné v Azure.  
  Ve výchozím nastavení bodů toto nastavení k tomuto účtu emulované úložiště, třeba explicitně toto nastavení změníte před nasazením balíčku aplikace. Pokud není toto nastavení změnit, je výjimka při pokusu o spuštění diagnostiky monitor instanci rolí. To může způsobit instanci rolí do koše donekonečna udržovat.

- Připojovací řetězec není zadán v tomto [formátu](../storage/storage-configure-connection-string.md). (Protokolu musí být zadán jako HTTPS.) Nahraďte název účtu úložiště a *MyAccountKey* s přístupová klávesa *MyAccountName* :    

        DefaultEndpointsProtocol=https;AccountName=MyAccountName;AccountKey=MyAccountKey

  Pokud vyvíjíte aplikace pomocí nástroje Azure pro Microsoft Visual Studio, můžete na [stránky vlastností](https://msdn.microsoft.com/library/ee405486) nastavte u této hodnoty.

## <a name="exported-certificate-does-not-include-private-key"></a>Exportovaný certifikát nezahrnuje privátním klíčem

Web role ve skupinovém rámečku SSL spustíte musíte se ujistit, že certifikát exportovaný správy obsahuje privátním klíčem. Pokud pomocí *Certifikátu správci* exportujte certifikát, ujistěte se, vyberte **Ano** pro možnost **Export privátním klíčem** . Certifikát musí exportovat ve formátu PFX, což je jenom formát v současnosti podporované.

## <a name="next-steps"></a>Další kroky

Zobrazte další [Poradce při potížích s články](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) při cloudovým službám.

Zobrazte další roli recyklace scénáře na [řada Jan Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

[RoleEntryPoint]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.aspx
[Při spuštění]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx
[OnStop]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx
[Spuštění]: https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx

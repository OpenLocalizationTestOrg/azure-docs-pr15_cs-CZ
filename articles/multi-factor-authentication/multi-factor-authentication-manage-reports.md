<properties
    pageTitle="Sestavy Azure Vícefaktorové ověřování"
    description="Tento scénář vystihuje jak používat funkci Azure Vícefaktorové ověřování - sestavy."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="reports-in-azure-multi-factor-authentication"></a>Sestavy v Azure Vícefaktorové ověřování

Azure Vícefaktorové ověřování obsahuje několik sestav, které lze použít tak, že se a vaše organizace. Tyto sestavy můžete k nim získat přístup prostřednictvím Multi-Factor Authentication portálu pro správu, která vyžaduje, jestli máte licenci Azure MFA poskytovatele nebo Azure MFA, Azure AD Premium nebo Enterprise mobilita sadu. Následuje seznam dostupných sestav.

Přístup k sestavám prostřednictvím portálu pro správu Azure.

Jméno| Popis
:------------- | :------------- |
Použití | Použití zprávy obsahují informace na celkové využití, uživatel souhrnných a podrobné informace o uživateli.
Stav serveru|Sestava stavu Multi-Factor Authentication servery přidruženého k vašemu účtu.
Historie blokovaného uživatele|Tyto sestavy zobrazit historii žádostí o zablokování nebo odblokování uživatelů.
Historie obejitím uživatele|Zobrazuje historii žádosti o obejít Vícefaktorové ověřování pro uživatele telefonní číslo.
Podvod upozornění|Zobrazuje historii podvod oznámení odeslané při rozsah kalendářních dat, které jste zadali.
Ve frontě|Sestavy seznamů ve frontě pro zpracování a jejich stav. Odkaz na stažení nebo zobrazení sestavy je k dispozici po dokončení sestavy.

## <a name="to-view-reports"></a>Chcete-li zobrazit sestavy

1.  Přihlaste se k http://azure.microsoft.com
2.  V levé části vyberte služby Active Directory.
3.  Vyberte jednu z následujících možností:
    - **Možnost 1**: klikněte na kartu Multi-Factor Auth poskytovatelů. Vyberte poskytovatele MFA a klikněte na tlačítko Spravovat dole.
    - **Možnost 2**: Vyberte adresáře a klikněte na kartu konfigurovat. V části vícefaktorové ověřování vyberte spravovat nastavení služby. V dolní části na stránce nastavení služby MFA klikněte odkaz přejít do portálu.
4.  V Azure Multi-Factor Authentication portálu pro správu zobrazí se zobrazení sekce sestavy v levém navigačním panelu. Tady můžete vybrat sestavy jsme je popsali výše.

<center>![Cloud](./media/multi-factor-authentication-manage-reports/report.png)</center>


**Další zdroje informací**

* [Pro uživatele](./end-user/multi-factor-authentication-end-user.md)
* [Azure Vícefaktorové ověřování na webu MSDN](https://msdn.microsoft.com/library/azure/dn249471.aspx)

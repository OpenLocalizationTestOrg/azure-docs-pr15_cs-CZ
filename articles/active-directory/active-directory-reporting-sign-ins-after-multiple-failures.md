<properties
    pageTitle="Sign in po k více chybám"
    description="Zprávu, která ukazuje uživatelům, kteří mají úspěšně přihlášení po více po sobě jdoucí selhání znak v pokusy."
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/04/2016"
    ms.author="saah;kenhoff"/>

# <a name="sign-ins-after-multiple-failures"></a>Přihlášení za k více chybám
Tato zpráva ukazuje uživatelům, kteří mají úspěšně přihlášení po více po sobě jdoucí selhání sign v pokusy. Možné příčiny patří:

- Uživatel měl zapomněli své heslo</li><li>Uživatel se obětí úspěšné hesla uhodnout hrubou vynutit útok

Výsledky tato zpráva se zobrazí počet po sobě jdoucí selhání přihlašovací pokusů před úspěšné přihlášení a časové razítko přidružený k první úspěšné přihlášení.

**Nastavení sestavy**: nakonfigurujete minimální počet po sobě jdoucí selhání znaménko při pokusech probíhajících musí před mohou být zobrazena v sestavě. Když uděláte změny toto nastavení je důležité mít na paměti, že tyto změny se projeví na všech existující selhalo sign in to aktuálně objeví v existující sestavy. Ale budou se použije pro všechny budoucí sign in. Tuhle sestavu můžete jenom měnit podle licencovaného správci.


![Sign in po k více chybám](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)

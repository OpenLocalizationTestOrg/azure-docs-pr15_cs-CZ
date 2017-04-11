<properties
    pageTitle="Sign in z různých zeměpisných"
    description="Zpráva oznamující, uživatelé kde dvě přihlásit ins zobrazovala pocházejí z různých oblastí a časový interval mezi značku, kterou ins znemožňuje uživateli mít ujeté mezi těmito oblastmi."
    services="active-directory"
    documentationCenter=""
    authors="SSalahAhmed"
    manager="gchander"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/04/2016"
    ms.author="saah;kenhoff"/>

# <a name="sign-ins-from-multiple-geographies"></a>Přihlášení z různých zeměpisných

Tato sestava obsahuje úspěšné přihlášení od uživatele, kde dva přihlášení se zobrazovala pochází z různých oblastí a časový interval mezi sign in znemožňuje uživateli mít cesty mezi těmito oblastmi. Možné příčiny patří:

- Uživatel je své heslo sdílení s ostatními uživateli

- Uživatel používá vzdálené plochy spustit webový prohlížeč pro přihlášení

- Hacker má přihlášení k účtu uživatele odlišného

- Uživatel používá virtuální privátní sítě nebo proxy serveru

- Je uživatel přihlášený z víc zařízení ve stejnou dobu, například plochy a na mobilní telefon, a IP adresu mobilního telefonu neobvyklého.

Výsledky v této sestavě řádku se zobrazí úspěšné přihlašovací události, spolu s dobu mezi sign in, oblastí, kde se k pocházejí z zobrazovala sign in a odhadovaná cestovní čas mezi těmito oblastmi. Cestovní čas zobrazený je pouze odhad a se může lišit od doby skutečné cestovní mezi umístění.


![Sign in z různých zeměpisných](./media/active-directory-reporting-sign-ins-from-multiple-geographies/signInsFromMultipleGeographies.PNG)

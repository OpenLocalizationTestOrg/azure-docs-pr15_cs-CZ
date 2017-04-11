<properties
   pageTitle="Vytvoření a registrace účtu aplikace publisher | Microsoft Azure"
   description="Pokyny pro vytvoření účtu Microsoft Developer tak po schválení můžete prodáváte různých nabízí typy na Azure Marketplace."
   services="Azure Marketplace"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="hascipio"/>

# <a name="create-a-microsoft-developer-account"></a>Vytvořit účet Microsoft Developer
Tento článek vás provede vytvoření potřebné účtu a proces zápisu schválené Microsoft Developer pro Azure Marketplace.

## <a name="1-create-a-microsoft-account"></a>1. vytvořit účet Microsoft
Spuštění procesu publikování, musíte vytvořit účet Microsoft. Tento účet se používá k registraci v **Microsoft Developer Center** a **Portál publikování Azure**. Měli byste mít jenom jeden účet Microsoft pro nabídka z Azure Marketplace. Nesmí být specifické pro služby nebo nabídky.

Adresa, který tvoří uživatelské jméno by měl být ve vaší doméně a řídit tak, že vaše oddělení IT. Publikování související aktivity by měl provést prostřednictvím tohoto účtu.

  >[AZURE.WARNING] Slova jako **"Azure"** a **"Microsoft"** nejsou podporované pro registrace účtu Microsoft. Nepoužívání tato slova dokončete vytvoření účtu a procesu registrace.

### <a name="instructions"></a>Pokyny k

1. Vytvoření distribučního seznamu (DL) nebo skupiny zabezpečení (PV) v rámci vaší společnosti domény. Disk DL připojíte více aby lidé dostávali e-mailová oznámení, které jsou důležité pro vykazování informací výběr. Také zajistí, že vlastnictví účet Microsoft lze převést a není stejným jedné osobě.
Postupujte podle pokynů níže.

    1. Přidejte DL týmu rychlého připojení.
    2. Zkontrolujte, zda DL/PV aktivní e-mailovou adresu a je moct přijímat e-maily, protože platby, daňových informací a vytváření sestav bude směrovaná přes tento účet.
    3. Doporučujeme používat nějak marketplace@partnercompany.com jako e-mailovou adresu pro DL/PV.

2. Otevřete nové okno chromu nebo Internet Exploreru se službou InPrivate relaci procházení zajistit, že nejste přihlášení k stávajícímu účtu.
3. Zaregistrujte DL vytvořili v kroku 1 jako účet Microsoft pomocí odkazu [https://signup.live.com/signup.aspx](https://signup.live.com/signup.aspx). Postupujte podle těchto pokynů.

    1. Při registraci účtu jako účet Microsoft, budete muset zadat platné telefonní číslo pro systém poslat kód ověření účtu jako textové zprávy nebo automatické hovoru.
    2. Při registraci účtu jako účet Microsoft, budete muset zadat id platnou e-mailu pro příjem automatické e-mailu pro účet ověření.

4. Ověření e-mailovou adresu příjemce DL.
5. Teď jste připraveni na webu Microsoft Developer Center pomocí nového účtu Microsoft.

## <a name="2-create-your-microsoft-developer-center-account"></a>2. vytvoření účtu Microsoft Developer Center
Webu Microsoft Developer Center slouží k registraci informací o společnosti jednou. Osob žádajících o registraci musí být platná zástupce společnosti a nutné zadat jejich osobních údajů jako způsob, jak ověřit identitu. Účet Microsoft, který je sdílený ve společnosti, musíte používat osoba registrace **a stejný účet je nutné použít portál publikování Azure.** Zkontrolujte, aby zkontrolovala, jestli že vaše společnost ještě nemá účet Microsoft Developer Center před pokusem o ho vytvořit. V průběhu Změníme shromáždit společnosti adresu, informací bankovní účet a daně informace. Toto jsou obvykle získat z finance nebo obchodních kontaktů.

> [AZURE.IMPORTANT] Abyste mohli projděte různé fáze vytváření nabídky a nasazení, musíte nejdřív udělat následující součásti profilu vývojář.


| Karta Vývojář v profilu | Spusťte koncept | Pracovní | Publikování volného a řešení šablon | Publikování komerční |
|----|----|----|----|----|
|Registrace společnosti | Musí mít | Musí mít | Musí mít | Musí mít |
|ID profilu daně | Volitelné | Volitelné | Volitelné | Musí mít |
|Bankovních účtů | Volitelné | Volitelné | Volitelné | Musí mít |


> [AZURE.NOTE] Přenést svůj vlastní licence (BYOL) je podporována pouze u virtuálních počítačích a se považuje za **bezplatnou** nabídku.


### <a name="register-your-company-account"></a>Registrace účtu vaší společnosti
1. Otevřete nový Internet Explorer InPrivate nebo okno Chrome relaci procházení zajistit, že nejste přihlášení k osobním účtem.

2. Přejděte na [http://dev.windows.com/registration?accountprogram=azure](http://dev.windows.com/registration?accountprogram=azure) na zaregistrovat jako prodejce v Centru pro vývojáře. Než budete pokračovat, přečtěte si následující důležité poznámky.

    >[AZURE.IMPORTANT] Zajistěte, aby byl na e-mailu id nebo distribuční seznam (distribučního seznamu se doporučuje odebrat závislost od osob) které budete používat pro registraci v Centru pro vývojáře na první registrovaná jako účet Microsoft. V opačném případě pak zaregistrujte pomocí tohoto [odkazu](https://signup.live.com/signup?uaid=e479342fe2824efeb0c3d92c8f961fd3&lic=1). Navíc **libovolné e-mailem id v části domény společnosti Microsoft tedy @microsoft.com nemohou být použity** pro registraci Centrum pro vývojáře.

    ![Kreslení][img-signin]

3. Dokončení průvodce "Pomoct při ochraně svého účtu", ve kterém bude ověření vaší identity prostřednictvím telefonní číslo nebo e-mailovou adresu.

    ![Kreslení][img-verify]

4. V části "Informace o registrace účtu" vyberte svůj **účet země/oblasti** v rozevírací nabídce.

    ![Kreslení](media/marketplace-publishing-accounts-creation-registration/imgRegisterCo_04.png)

    > [AZURE.WARNING] **"Zákazník od" země:** K prodeji vašich služeb na webu Azure Marketplace, musí být z jednoho schválené "zákazník od" zemí výše registrovaných entity. Toto omezení je pro výběr a daně důvodů. Aktivně chceme nevidět rozbalte seznam země, takže zůstávají sestavenou. Další informace najdete v tématu [zásady účast Marketplace](http://go.microsoft.com/fwlink/?LinkID=526833).

5. Vyberte typ účtu"" jako **společnosti** a potom klikněte na tlačítko **Další** .

    > [AZURE.IMPORTANT] Chcete-li lépe porozumět typům účtů a který je pro vás zvolit nejlepší, prosím zobrazení stránky [různých typů účtů, umístění a poplatky](https://msdn.microsoft.com/library/windows/apps/jj863494.aspx)

    ![Kreslení](media/marketplace-publishing-accounts-creation-registration/imgRegisterCo_05.png)

6. Zadejte **Publisheru zobrazované jméno**, obvykle název vaší společnosti.

    > [AZURE.TIP] Publisher zobrazované jméno v Centru pro vývojáře nezobrazuje v Azure Marketplace po vaši nabídku přejde uvedené. Ale to je nutné zadat dokončete proces registrace.

7. Zadejte **kontaktní informace** k ověření účtu.

    > [AZURE.IMPORTANT] Protože se použije v naší proces ověření pro vaši firmu vyžadovat schválení v Centru pro vývojáře je nutné zadat přesné kontaktní informace.

8. Zadejte kontaktní informace pro **Schvalovatel společnosti**. Schvalovatel společnosti je osoba, která lze ověřit, že máte oprávnění vytvořit účet v Centru pro vývojáře jménem vaší organizace. Klikněte na **Další** přejděte na **"Platby oddíl"** Jakmile jste hotovi.

    ![Kreslení](media/marketplace-publishing-accounts-creation-registration/imgRegisterCo_08.png)

9. Zadejte informace o vaší platební zaplatit pro váš účet. Pokud máte propagačního kódu, které bude pokrývat náklady registrace, zadáte, který tady. Jinak zadejte informace o platební kartě (nebo účet PayPal podporované trzích). Až budete hotovi, klikněte na **Další** přejděte **"Revize obrazovky"**.

    ![Kreslení](media/marketplace-publishing-accounts-creation-registration/imgRegisterCo_09.png)

10. Zkontrolujte informace o účtu a ověřte, jestli je všechno správně. Potom přečtěte si a přijměte podmínky a ujednání [Publisher smlouvou Microsoft Azure Marketplace](http://go.microsoft.com/fwlink/?LinkID=699560). Zaškrtněte políčko označíte přečetli a přijali těchto podmínek.

11. Klikněte na **Dokončit** registraci potvrdíte. Chcete e-mailové adresy se odeslat zprávu s potvrzením.

12. Pokud se chystáte publikovat pouze bezplatné nabídky, klikněte na **Přejít na portál publikování Azure Marketplace** a můžete můžete přejít ke části 3 v tomto dokumentu [Zaregistrujte svůj účet na portálu pro publikování](#3-register-your-account-in-the-publishing-portal).

Pokud se chystáte publikovat komerční nabízí (například virtuálního počítače nabízí pomocí hodinách fakturační modelu), kliknutím na **Aktualizovat informace o účtu** , kde je třeba zadat daň a bankovní informace ve vašem účtu středisko pro vývojáře.

Pokud chcete aktualizovat daň a banky informace o později a pak můžete přesunout na další části tedy část 3 tohoto dokumentu [Zaregistrujte svůj účet na portálu pro publikování](#3-register-your-account-in-the-publishing-portal)a pocházet později pomocí odkazů v portál publikování Azure.

> [AZURE.IMPORTANT] V případě obchodní nabídky nebudete moct použít vlastní nabídky pro výrobní bez dokončení informace o daňové a bankovních účtů.

Pokud chcete aktualizovat daň a banky informace o později, můžete naleznete v části 3 [Zaregistrujte svůj účet na portálu pro publikování](#3-register-your-account-in-the-publishing-portal)a pocházet později pomocí odkazů v portál publikování Azure.

### <a name="add-tax-and-banking-information"></a>Přidání daň a bankovní informace
 Pokud chcete publikovat obchodní nabídky k nákupu, budete potřebovat k přidání výběr a daně informace a jeho odeslání ověřovacích v Centru pro vývojáře. Pokud budete publikovat pouze bezplatné nabídky (nebo BYOL nabízí), ne muset přidat tyto informace. Můžete ho přidat později, ale některé dlouho k ověření informací daně. Pokud víte, že bude nabízet obchodní nabídky k nákupu, doporučujeme, aby ji přidáte co nejdříve.

**Bankovních informací**

1. Přihlaste se na [Webu Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) pomocí svého účtu Microsoft.

2. Klikněte na **Výběr účet** v levé nabídce získáte v části **zvolit způsob platby** **Bankovní účet** nebo **účet PayPal**.

    > [AZURE.IMPORTANT] Pokud máte komerční nabídek, které zákazníci zakoupit v Tržiště, je to účet místo, kam se obnoví výběr pro tyto nákup.

3. Zadejte informace o platbě a po dokončení klepněte na tlačítko **Uložit** .

    > [AZURE.IMPORTANT] Pokud potřebujete k aktualizaci nebo změně výběr účtu, postupujte stejně nad nahrazení aktuální informace o nové informace. Změna účtu výběr, může zdržet vaše platby do jednoho obrázku platby. Toto zpoždění příčinou potřebujeme ověření změnu účtu stejně jako jsme při nejdřív nastavit účet výběr. Budete pořád platby celou dobu po ověření účtu; veškeré platby termín aktuální platby se přidá obrázku na další stránku.

4. Klikněte na tlačítko **Další**.

**Informace o daně**

1. Přihlaste se k [Webu Microsoft Developer Center](http://dev.windows.com/registration?accountprogram=azure) pomocí svého účtu Microsoft (v případě potřeby).

2. Klikněte v nabídce nalevo na **daň profil** .

3. Na stránce **Nastavení formulář daň** vyberte zemi nebo oblasti, kde máte trvalé sídlo a vyberte zemi nebo oblasti, kde ho držíte primární občanství. Klikněte na tlačítko **Další**.

4. Zadejte podrobnosti o daňové a klikněte na tlačítko **Další**.

> [AZURE.WARNING] Nebude moct použít pro výrobní vaše obchodní nabízí bez dokončení daň a bankovních účtů informace ve vašem účtu Microsoft Developer Center.

Pokud máte problémy s středisko pro vývojáře registrace, přejděte prosím přihlaste se požadavek podpory můžete jako dole

1. Přejděte na odkaz Podpora [https://developer.microsoft.com/windows/support](https://developer.microsoft.com/windows/support)
2. V části **Kontakt** klikněte na tlačítko **Odeslat incident** (jak ukazuje následující obrázek)

    ![Kreslení](media/marketplace-publishing-accounts-creation-registration/imgAddTax_02.png)

3. Zvolte "Pomoct s Centrum pro vývojáře" jako **Typ problému** a "publikovat a Správa aplikací" jako **kategorie**. Po kliknutí na tlačítko "Start e-mailu".

    ![Kreslení](media/marketplace-publishing-accounts-creation-registration/imgAddTax_03.png)

4. Můžete se dozvíte na přihlašovací stránku. Používejte jakékoli přihlašovací účet Microsoft. Pokud nemáte účet Microsoft, vytvořte ho pomocí tohoto [odkazu](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
5. Vyplňte podrobnosti o problém a subit lístek kliknutím na tlačítko **Odeslat** .

    ![Kreslení](media/marketplace-publishing-accounts-creation-registration/imgAddTax_05.png)

## <a name="3-register-your-account-in-the-publishing-portal"></a>3. Zaregistrujte svůj účet v portál publikování
[Portál publikování](http://publish.windowsazure.com) slouží k publikování a spravovat tyto nabídky.

1. Otevřete nové okno chromu nebo Internet Exploreru se službou InPrivate relaci procházení zajistit, že nejste přihlášení k osobním účtem.

2. Přejděte na [http://publish.windowsazure.com](http://publish.windowsazure.com).

3. Pokud jste novým uživatelem a přihlášení k publikování portálu poprvé, pak se musíte přihlásit pomocí účtu stejné e-mailu id, ke kterému je registrovaná účtu Centrum pro vývojáře. Tímto způsobem bude vzájemně propojit účtu Centrum pro vývojáře a publikování portálu účtu. Ostatní členové společnost, kteří pracují na aplikaci později můžete přidat jako co správce publikování portálu podle následujících pokynů.

Pokud vás někdo přidá jako co správce publikování portálu, pak se přihlásit pomocí svého účtu správce co.

  > [AZURE.TIP] Zásady účast jsou popsány v [Azure webu](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).

## <a name="4-steps-to-add-a-co-admin-in-the-publishing-portal"></a>4. pokyny pro přidání dalších správců do publikování portálu
**Za předpokladu, že jste správce,** uvedeny níže najdete postup, jak přidat co správce.

>[AZURE.NOTE] **Pro nové uživatele** před přidáním dalších správců do publikování portál, ujistěte se, že jste vytvořili aspoň jeden aplikaci v publikování portálu. Tento postup je vyžadován jako na kartě **VYDAVATELÉ** se zobrazí pouze po vytvoření alespoň jeden aplikace v publikování portálu.

1. Zajistěte, aby byl id co správce e-mailu Microsoft account(MSA). V opačném případě zaregistrovat jako MSA pomocí tohoto [odkazu](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
2. Zajistěte, aby alespoň jednu žádost pod účtem správce před pokusem o přidání dalších – správce.
3. Po výše uvedené kroky hotovi, přihlaste se k publikování portálu s id co správce e-mailu a znovu přihlásit se.
4. Teď přihlášení k publikování portálu s id správce e-mailu.
5. Přejděte na vydavatelé -> Vybrat -> váš účet správce -> přidat správce co (snímek uvedeny níže)

  ![Kreslení](media/marketplace-publishing-accounts-creation-registration/imgAddAdmin_05.png)

## <a name="5-steps-to-delete-a-co-admin-in-the-publishing-portal"></a>5. postup odstranění co správce publikování portálu
**Za předpokladu, že jste správce,** uvedeny níže najdete postup, jak odstranit co – správce.

1. Přihlaste se k publikování portálu s id správce e-mailu.
2. Přejděte na **Vydavatelé** -> Vybrat účtu -> **Správci** -> **Dalších správců**.
3. Klikněte na tlačítko **X** vedle na co – správce chcete odstranit tot (snímek uvedeny níže).

    ![Kreslení](media/marketplace-publishing-accounts-creation-registration/imgDeleteAdmin_03.png)

## <a name="next-steps"></a>Další kroky
Teď, když váš účet je vytvořili a registrován, zajistit splnění nebo splňují všechny netechnických předpoklady publikování vaši nabídku kontrolou [netechnických předpoklady](marketplace-publishing-pre-requisites.md).

## <a name="see-also"></a>Viz taky
- [Začínáme: jak publikovat nabídka z Azure Marketplace](marketplace-publishing-getting-started.md)

[img-msalive]:media/marketplace-publishing-accounts-creation-registration/creating-msa-account-msa-live.jpg
[img-email]:media/marketplace-publishing-accounts-creation-registration/creating-msa-account-msa-verifyemail.jpg
[img-sd-url]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-incognito.jpg
[img-signin]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-login.jpg
[img-verify]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-verify.jpg
[img-sd-top]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-acc-details.jpg
[img-sd-info]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal.jpg
[img-sd-type]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-acc-type.jpg
[img-sd-mktg1]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-comp-det1.jpg
[img-sd-mktg2]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-comp-det2.jpg
[img-sd-addr]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-comp-add.jpg
[img-sd-legal]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-cmp.jpg
[img-sd-submit]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-approval.jpg

[link-msdndoc]: https://msdn.microsoft.com/library/jj552460.aspx
[link-sellerdashboard]: http://sellerdashboard.microsoft.com/
[link-pubportal]: https://publish.windowsazure.com
[link-single-vm]:marketplace-publishing-vm-image-creation.md
[link-single-vm-prereq]:marketplace-publishing-vm-image-creation-prerequisites.md
[link-multi-vm]:marketplace-publishing-solution-template-creation.md
[link-multi-vm-prereq]:marketplace-publishing-solution-template-creation-prerequisites.md
[link-datasvc]:marketplace-publishing-data-service-creation.md
[link-datasvc-prereq]:marketplace-publishing-data-service-creation-prerequisites.md
[link-devsvc]:marketplace-publishing-dev-service-creation.md
[link-devsvc-prereq]:marketplace-publishing-dev-service-creation-prerequisites.md
[link-pushstaging]:marketplace-publishing-push-to-staging.md

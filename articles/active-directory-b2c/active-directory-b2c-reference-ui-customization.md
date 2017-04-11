<properties
    pageTitle="Azure Active Directory B2C: Přizpůsobení uživatelského prostředí uživatel | Microsoft Azure"
    description="Téma funkce přizpůsobení uživatelského prostředí pro uživatele v Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a>Azure Active Directory B2C: Přizpůsobení uživatelského rozhraní Azure AD B2C (UI)

Uživatelské prostředí je prvořadá do spotř webových aplikací. Je rozdíl mezi dobré aplikace a některou ze velký a od opravdu obsazené z nich pouze aktivní uživatelé. Azure Active Directory (Azure AD) B2C umožňuje přizpůsobit spotř registrace, přihlašovací (*Viz poznámka níže*), úpravy, profilu a stránky s ovládacím prvkem bod přesný resetování hesla.

> [AZURE.NOTE]
V současné době místního účtu přihlašovací stránky jeho accompaning heslo resetovat stránky a lze jej přizpůsobit ověření e-mailů, pouze pomocí [funkce brandingu společnosti](../active-directory/active-directory-add-company-branding.md) a ne podle mechanismy popisované v tomto článku.

V tomto článku si přečíst o:

- Funkce vlastního nastavení stránky uživatelského rozhraní.
- Nástroj, který vám pomůže otestovat funkce vlastního nastavení uživatelského rozhraní stránky pomocí našich ukázkový obsah.
- Základní prvky uživatelského rozhraní v jednotlivých typů stránky.
- Osvědčené postupy při výkonu tuto funkci.

## <a name="the-page-ui-customization-feature"></a>Funkce vlastního nastavení stránky uživatelského rozhraní

Pomocí funkce přizpůsobení uživatelského rozhraní stránky můžete přizpůsobit vzhled a chování spotř registrace, přihlašovací, resetovat hesla a úpravy profilu stránky (pomocí konfigurace [zásad](active-directory-b2c-reference-policies.md)). Při přecházení mezi aplikací a stránky hodit Azure AD B2C, bude mít uživatele bezproblémové prostředí.

Na rozdíl od jiných služeb, kde možností uživatelského rozhraní jsou omezené nebo jenom k dispozici prostřednictvím rozhraní API používá Azure AD B2C moderní (a jednodušší) přístupu k přizpůsobení uživatelského rozhraní stránky.

Funguje takto: Azure AD B2C spustí kód v prohlížeči svého příjemce a k načtení obsah z adresy URL, které určíte v zásadu používá moderní přístup s názvem [Sdílení zdrojů mezi Origin (CORS)](http://www.w3.org/TR/cors/) . Můžete použít různé adresy URL pro různé stránky. Kód sloučí prvkům uživatelského rozhraní z Azure AD B2C s obsahem načte z svoji adresu URL a zobrazí na stránce svého příjemce. Je třeba udělat je:

1. Vytvoření obsahu pomocí ve správném formátu HTML5 `<div id="api"></div>` prvek (třeba prázdné prvek) součástí někde `<body>`. Tento element značky vloženy Azure AD B2C obsah.
2. Hostovat obsahu na koncový bod HTTPS (plus pár CORS povolené).
3. Styl prvkům uživatelského rozhraní, které Azure AD B2C vloží.

## <a name="test-out-the-ui-customization-feature"></a>Vyzkoušejte funkci přizpůsobení uživatelského rozhraní

Pokud chcete vyzkoušet uživatelského rozhraní funkce vlastního nastavení použitím naši stránku věnovanou ukázkové HTML a obsah šablony stylů CSS, jsme zadali je [jednoduchý pomocný nástroj](active-directory-b2c-reference-ui-customization-helper-tool.md) , který uloží na server a konfiguruje výběru obsahu v úložišti objektů Blob Azure.

> [AZURE.NOTE]
Můžete hostovat obsah uživatelského rozhraní kdekoli: na webových serverů, CDN, AWS S3, systémů sdílení souborů, atd. Dokud obsah hostuje na veřejně dostupný koncový bod HTTPS (CORS povolené), měli byste být můžete začít. Úložiště objektů Blob Azure používáme příkladů pouze pro účely.

### <a name="the-most-basic-html-content-for-testing"></a>Základní obsah HTML pro účely testování

Ukázáno v následujícím příkladu je základní HTML obsahu, které můžete otestovat tuto možnost. Nástroj stejné helper dříve zadanému nahrávat a konfigurace tento obsah v úložišti objektů Blob Azure. Pak můžete ověřit, že základní, není stylizované tlačítek a poli formuláře na každé stránce zobrazí se funkční.

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>!Add your title here!</title>
    </head>
    <body>
        <div id="api"></div>    <!-- IMP: This element is intentionally empty; don't enter anything here -->
    </body>
</html>

```

## <a name="the-core-ui-elements-in-each-type-of-page"></a>Základní prvky uživatelského rozhraní v jednotlivých typů stránky

V následujících částech najdete příklady neúplné HTML5, které Azure AD B2C sloučí `<div id="api"></div>` prvek nachází v obsahu. **Nevkládejte tyto neúplné v obsahu HTML 5.** Služby Azure AD B2C vloží je do za běhu. Tyto příklady použijte k návrhu svoje vlastní šablony stylů.

### <a name="azure-ad-b2c-content-inserted-into-the-identity-provider-selection-page"></a>Azure AD B2C obsah vložený do "Identity poskytovatele stránce Výběr"

Tato stránka obsahuje seznam Zprostředkovatelé identit jiní, které může uživatel vybrat z během registrace nebo přihlášení. Toto jsou poskytovatelé sociální identity třeba Facebook a Google + nebo místní účty (založené na e-mailovou adresu nebo jméno uživatele).

```HTML

<div id="api" data-name="IdpSelections">
    <div class="intro">
         <p>Sign up</p>
    </div>

    <div>
        <ul>
            <li>
                <button class="accountButton" id="FacebookExchange">Facebook</button>
            </li>
            <li>
                <button class="accountButton" id="GoogleExchange">Google+</button>
            </li>
            <li>
                <button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
            </li>
        </ul>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-local-account-sign-up-page"></a>Azure AD B2C obsah vložený do "místního účtu registrace stránky"

Tato stránka obsahuje registrace formulář, který má uživatel vyplnit při přihlašování ke místního účtu založeného na e-mailovou adresu nebo uživatelské jméno. Formuláře mohou obsahovat různé vstupní ovládací prvky například vstupním poli textové pole pro zadání hesla, přepínací tlačítko, jediného výběru rozevíracích seznamů a vícenásobného výběru zaškrtněte políčka.

```HTML

<div id="api" data-name="SelfAsserted">
    <div class="intro">
        <p>Create your account by providing the following details</p>
    </div>

    <div id="attributeVerification">
        <div class="errorText" id="passwordEntryMismatch" style="display: none;">The password entry fields do not match. Please enter the same password in both fields and try again.</div>
        <div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
        <div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
        <div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
        <div class="attr" id="attributeList">
            <ul>
                <li>
                    <div class="attrEntry validate">
                        <div>
                            <div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
                            <div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent to your inbox. Please copy it to the input box below.</div>
                            <div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
                            <div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
                            <div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need to send new code.</div>
                            <div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
                            <div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
                        </div>

                    <div class="helpText show">This information is required</div>
                        <label>Email</label>
                        <input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used to contact you.');" class="tiny">What is this?</a>

                    <div class="buttons verify" claim_id="email">
                        <div id="email_ver_wait" class="working" style="display: none;"></div>
                            <label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label>
                            <input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
                            <button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
                            <button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
                            <button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
                            <button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
                            <button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
                        <label>Enter password</label>
                        <input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"> This information is required</div>
                        <label>Reenter password</label>
                        <input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Name</label>
                        <input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Gender</label>
                        <input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
                        <label for="extension_Gender_F">Female</label>
                        <input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
                        <label for="extension_Gender_M">Male</label>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Loyalty number</label>
                        <input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>State</label>
                        <select class="dropdown_single" id="state">
                            <option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
                            <option value="WA">Washington</option>
                            <option value="NY">New York</option>
                            <option value="CA">California</option>
                        </select>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Zip code</label>
                        <input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('The postal code of your address.');" class="tiny">What is this?</a>
                    </div>
                </li>
            </ul>
        </div>
        <div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
    </div>
    <div class="verifying-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="verifying_blurb"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-social-account-sign-up-page"></a>Azure AD B2C obsah vložený do "" sociální registrace stránku s účtem"

Tato stránka obsahuje registrace formulář, který má příjemce k vyplnění při přihlašování pomocí existujícího účtu z zprostředkovatele sociální identit, jako je Facebook nebo Google +. Tato stránka je podobná místní registrace stránku s účtem (viz v předchozí části) s výjimkou pole zadání hesla.

### <a name="azure-ad-b2c-content-inserted-into-the-unified-sign-up-or-sign-in-page"></a>Azure AD B2C obsah vložený do "jednotné registrace nebo přihlašovací stránky"

Tato stránka zpracovává jak registrace & přihlásit uživatelé, kteří můžete použít zprostředkovatele identit sociální jako je Facebook nebo Google + nebo místní účty.

```HTML

<div id="api" data-name="Unified">
        <div class="social" role="form">
               <div class="intro">
                       <h2>Sign in with your social account</h2>
               </div>
               <div class="options">
                       <div><button class="accountButton firstButton" id="MicrosoftAccountExchange" tabindex="1">msa</button></div>
                       <div><button class="accountButton" id="FacebookExchange" tabindex="1">fb</button></div>
               </div>
        </div>
        <div class="divider">
               <h2>OR</h2>
        </div>
        <div class="localAccount" role="form">
               <div class="intro">
                       <h2>Sign in with your existing account</h2>
               </div>
               <div class="error pageLevel" aria-hidden="true" style="display: none;">
                       <p role="alert"></p>
               </div>
               <div class="entry">
                       <div class="entry-item">
                               <label for="logonIdentifier">Email Address</label> 
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="email" id="logonIdentifier" name="LogonIdentifier" pattern="^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" placeholder="LogonIdentifier" value="" tabindex="1">
                       </div>
                       <div class="entry-item">
                               <div class="password-label"> <label for="password">Password</label><a id="forgotPassword" tabindex="2">Forgot your password?</a></div>
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="password" id="password" name="Password" placeholder="Password" tabindex="1">
                       </div>
                       <div class="working"></div>
                       <div class="buttons"> <button id="next" tabindex="1">Sign in</button> </div>
               </div>
               <div class="divider">
                       <h2>OR</h2>
               </div>
               <div class="create">
                       <p>Don't have an account?<a id="createAccount" tabindex="1">Sign up now</a> </p>
               </div>
        </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-multi-factor-authentication-page"></a>Azure AD B2C obsah vložený do "stránka vícefaktorové ověřování"

Na této stránce uživatelé ověřit své telefonní čísla (s využitím textu nebo hlasové) během registrace nebo přihlášení.

```HTML

<div id="api" data-name="Phonefactor">
    <div id="phonefactor_initial">
        <div class="intro">
            <p>Enter a number below that we can send a code via SMS or phone to authenticate you.</p>
        </div>
        <div class="errorText" id="errorMessage" style="display:none"></div>
        <div class="phoneEntry" id="phoneEntry">
            <div class="phoneNumber">
                <select id="countryCode" style="display:inline-block">
                    <option value="+93">Afghanistan (+93)</option>
                    <!-- Not all country codes listed -->
                    <option value="+44">United Kingdom (+44)</option>
                    <option value="+1" selected="">United States (+1)</option>
                    <!-- Not all country codes listed -->
                </select>
            </div>
            <div class="phoneNumber">
                <input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
            </div>
        </div>
        <div id="codeVerification" style="display:none">
            <div>
                <p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
                <input type="text" id="verificationCode" placeholder="Verification code">
            </div>
        </div>
        <div class="buttons">
            <button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
            <button id="verifyPhone" style="display:inline-block">Call Me</button>
            <button id="cancel" style="display:inline-block">Cancel</button>
        </div>
    </div>
    <div class="dialing-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="dialing_blurb"></div><div id="dialing_number"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-error-page"></a>Azure AD B2C obsah vložený do "" Chyba stránky"

```HTML

<div id="api" class="error-page-content" data-name="GlobalException">
    <h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if the problem persists feel free to contact us. In the meantime, please try again.</div>
    <div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
    <div class="error-page-messagedetails">
        <div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
        <div>Timestamp:2015-09-14 23:22:35Z</div>
        <div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting the remote resource.</div>
    </div>
</div>

```

## <a name="things-to-remember-when-building-your-own-content"></a>Co je potřeba pamatovat při vytváření vlastní obsah

Pokud se chystáte používat funkci přizpůsobení uživatelského rozhraní stránky, přečtěte si následující doporučené postupy:

- Nechcete kopírovat Azure AD B2C výchozí obsah a pokusí se jej upravit. Je vhodné vytvořit HTML5 obsahu od začátku a používat výchozí obsah jako odkaz.
- Ve všech stránek (s výjimkou chybových stránek) hodit přihlášení, registrace a úpravy profilu zásady, bude potřeba stylů, které zadáte, abyste buď přijměte výchozí šablony stylů, které jsme přidali do těchto stránek v <head> neúplné. Na všech stránkách podávané množství tak, že registrace nebo přihlašovací a zásady resetovat hesla a chybových stránek na všech zásad, budete muset zadat všechny styl sami.
- Z bezpečnostních důvodů doporučujeme vám nedovolí zahrnout všechny JavaScript v obsahu. Většina co byste měli by měl být k dispozici mimo pole. V opačném případě použití [Uživatele hlasovou](http://feedback.azure.com/forums/169401-azure-active-directory) požádat o nové funkci.
- Podporovaných verzí prohlížečů:
    - Internet Explorer 11
    - Internet Explorer 10
    - Internet Explorer 9 (omezené)
    - Internet Explorer 8 (omezené)
    - Google Chrome 43.0
    - Google Chrome 42.0
    - Mozilla Firefox 38.0
    - Mozilla Firefox 37.0

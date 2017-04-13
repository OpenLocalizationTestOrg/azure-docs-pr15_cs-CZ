<properties 
    pageTitle="Výukové programy pro Avanced Media Encoder Premium pracovního postupu" 
    description="Tento dokument obsahuje návody, které ukazují, jak k provádění pokročilejších úkolů s pracovním postupem Media Encoder Premium a taky jak vytvářet složité pracovní postupy s Návrháři pracovního postupu." 
    services="media-services" 
    documentationCenter="" 
    authors="xstof" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"  
    ms.author="xstof;xpouyat;juliako"/>

#<a name="advanced-media-encoder-premium-workflow-tutorials"></a>Rozšířené výukové programy pro Media Encoder Premium pracovního postupu

##<a name="overview"></a>Základní informace 

Tento dokument obsahuje návody, které ukazují, jak přizpůsobit pracovní postupy s **Návrháři pracovního postupu**. Můžete vyhledat soubory vlastní pracovní postup [tady](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).  

##<a name="toc"></a>OBSAH

Následující témata:

- [Kódování MXF do jednoho bitrate MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
    - [Zahájení nového pracovního postupu](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new) 
    - [Používání souborů vstup média](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
    - [Podrobná datové proudy médií](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
    - [Přidání videa encoder pro. Generování souboru MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
    - [Kódování zvuku toku](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
    - [Multiplexní zvuk a Video proudy do kontejneru MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
    - [Psaní souboru MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
    - [Vytvoření materiálů služby médií od výstupní soubor](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
    - [Vyzkoušení místně dokončení pracovního postupu](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
- [Kódování MXF do multibitrate MP4s - dynamické balení povolena](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
    - [Přidání jednoho nebo více další výstupy MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
    - [Konfigurace názvů výstupní soubor](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
    - [Přidání samostatné sledování zvuku](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
    - [Přidání. Správci služeb sítě Internet SMIL soubor](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
- [Kódování MXF do multibitrate MP4 - lepší přehled](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
    - [Přehled pracovního postupu k vylepšení](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_overview)
    - [Konvence pojmenování souborů](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
    - [Publikování vlastnosti součásti do kořenové pracovního postupu](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
    - [Dosáhli výstupní soubor, který názvy spolehnout publikované nemovitostí s hodnotou](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
- [Přidání miniatury multibitrate výstup MP4](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
    - [Přehled pracovní postup přidáte miniatur](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to_multibitrate_MP4_overview)
    - [Přidání JPG kódování](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
    - [Práce s převodu barevného prostoru](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
    - [Psaní miniatur](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
    - [Zjišťování chyb v pracovním postupu](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
    - [Dokončení pracovního postupu](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
- [Oříznutí založená na čase výstupu multibitrate MP4](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
    - [Přehled pracovního postupu do ní začít přidávat oříznutí na](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
    - [Použití datového proudu oříznutí](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
    - [Dokončení pracovního postupu](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
- [Úvodní informace o komponentu skriptů](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
    - [Skriptování v rámci pracovního postupu: Vítáme](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
- [Oříznutí na základě snímků výstupu multibitrate MP4](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
    - [Přehled přehled do ní začít přidávat oříznutí na](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
    - [Použití seznamu galerie XML](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
    - [Úprava seznamu klipartu z komponentu skriptovány](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
    - [Přidání vlastnosti pohodlí ClippingEnabled](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

##<a id="MXF_to_MP4"></a>Kódování MXF do jednoho bitrate MP4
 
V tomto návodu vytvoříme jednoho bitrate. MP4 soubor s AAC-HE kódování zvuku z. Vstupní soubor MXF. 

###<a id="MXF_to_MP4_start_new"></a>Zahájení nového pracovního postupu 

Otevřete návrháře pracovního postupu a vyberte "Přehled souboru"-"nový pracovní prostor"-"převod" 

Nový pracovní postup se zobrazí 3 prvky: 

- Primární zdrojového souboru
- Seznam XML klipů
- Výstupní soubor/materiálů  

![Nový pracovní postup kódování](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

*Nový pracovní postup kódování*

###<a id="MXF_to_MP4_with_file_input"></a>Používání souborů vstup média

Abyste mohli přijmout našeho vstupním mediální soubor, jednu začíná přidáte komponentu vstupních média soubor. Přidání součásti pracovního postupu, najděte ho do pole Hledat úložiště a přetáhněte na požadovanou položku podokna návrháře. To udělat pro vstupních média soubor a připojte komponentu primární zdrojový soubor připnout zadávání názvu souboru z vstupních média soubor.

![Zadávání propojených mediálních souborů](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

*Zadávání propojených mediálních souborů*

Před jsme můžete provést, jsme nejdřív potřeba-li Návrháři pracovního postupu jaké ukázkový soubor rádi navrhnout naše pracovního postupu s. Klikněte na položku pozadí návrháře podokno a vyhledejte vlastnost primární zdrojový soubor v podokně vpravo vlastnost. Klikněte na ikonu složky a vyberte požadovaný soubor otestovat pracovního postupu s. Hned, jak to provést, komponentu vstup soubor média prozkoumat soubor a naplnění jeho výstupní spojky, aby odrážely soubor, který ho zkontrolovat.

![Vyplněné mediálních souborů zadání vstupních hodnot](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

*Vyplněné mediálních souborů zadání vstupních hodnot*

Během určuje s jaké vstupní rádi pracovat, nemá informace ještě kde zakódovaný výstup by měli přejít na. Konfigurovat podobným způsobem primární zdrojového souboru, teď konfigurace vlastnost výstup složky proměnné pod ním.

![Nakonfigurovanou vlastnosti vstupní a výstupní](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

*Nakonfigurovanou vlastnosti vstupní a výstupní*

###<a id="MXF_to_MP4_streams"></a>Podrobná datové proudy médií

Často má požadované vědět, jak proudu vypadá tak toků pomocí pracovního postupu. Kontrola datového proudu kdykoli v pracovním postupu, stačí klikněte výstup nebo zadávání kódu pin na kterékoli z součásti. V tomto případě zkuste kliknout na výstupní spojky nekomprimované Video z našich vstup soubor média. Dialogové okno se otevře, který umožňuje zkontrolovat odchozího videa.

![Podrobná výstupní spojky nekomprimované Video](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

*Podrobná výstupní spojky nekomprimované Video*

V našem případě ho víme, například, že jsme pracujete s 1920 × 1080 vstupní na 24 snímků za sekundu v 4: odběr 2:2 video prakticky 2 minuty.

###<a id="MXF_to_MP4_file_generation"></a>Přidání videa encoder pro. Generování souboru MP4

Všimněte si, že nyní, jsou k dispozici pro použití na naše vstup soubor média nekomprimované Video a více výstupní spojky nekomprimované zvuk. Abyste mohli kódování příchozí videa potřebujeme kódování součást - v tomto případě pro generování. MP4 soubory.

Kódování videa datového proudu H.264, přidejte součást videa Encoder AVC návrhové ploše. Tato součást zabírá uncompress datového proudu videa předávat na vstupu a poskytuje AVC mu tuhle zkomprimovanou stream videa na jeho výstup pin.

![Nepřipojené AVC Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

*Nepřipojené AVC Encoder*

Jeho vlastností určit, jak kódování přesně se stane. Podívejme se podívat na některé další důležitým nastavením:

- Výstup šířka a výška výstup: Tyto určují rozlišení zakódovaný video. V našem případě Pojďme s 640 × 360
- Snímek sazba: Pokud je nastavena na průchod jenom přijme rámce sazby zdroje, je možné přepsat přes. Všimněte si, že takové framerate převodu není pohybu nahrazená.
- Profil a úroveň: Tyto určují AVC profilu a úroveň. Snadno získat další informace o různých úrovních a profily, klikněte na ikonu otazník na komponentu AVC Video Encoder a na stránce nápovědy se zobrazí další podrobnosti o jednotlivých úrovní. Pro našimi ukázkovými Pojďme s hlavní profil na úrovni 3,2 (výchozí).
- Ohodnocení ovládat režim a Bitrate (kB / s): v našem případě jsme určovat, jestli se pro konstanty bitrate (CBR) výstup rychlost 1200 kB /
- : Video to je formát o VUI (Video použitelnosti informace), který získá napsali do datového proudu H.264 (boční informace, které nemusejí být používá společnost dekódovací k vylepšení zobrazení, ale není nutná správně dekódovat):
- NTSC (typické pro USA nebo Japonsko pomocí snímků 30 /)
- PAL (typické evropské pomocí snímků 25 /)
- Režim GOP velikost: jsme budete konfigurovat pevnou velikost GOP pro naše potřeby v intervalu klíč 2 sekund s zavřený GOPs. Díky poskytuje funkce pro kompatibilitu s dynamické balení Azure Media Services.

K naplnění naše AVC encoder propojte výstupní spojky nekomprimované Video komponentu vstupních média soubor zadávání kódu pin nekomprimované Video z AVC encoder.

![Připojení AVC Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

*Připojení AVC hlavní encoder*

###<a id="MXF_to_MP4_audio"></a>Kódování zvuku toku

V tomto okamžiku jsme mít zakódovaný video, ale původní nekomprimované zvuku toku ještě potřeba komprimovány. Pro tento budeme věnovat s kódováním AAC součástí Encoder AAC (zvuk Dolby). Přidáte k pracovnímu postupu.

![Nepřipojené AVC Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

*Nepřipojené AAC encoder*

Nově je nekompatibilita: během více než pravděpodobné vstupních média soubor bude mít dva různé nekomprimované zvuku toku dostupné je pouze jeden nekomprimované zvuku zadávání kódu pin z AAC Encoder: jedno pro levého zvuku kanálu a jedno pro vpravo. (Pokud pracujete s prostorový zvuk, který je 6 kanálů.) Takže není možné přímo propojte zvuk zdroji vstup soubor média do zvuku encoder AAC. Součásti AAC očekává takzvané "prokládaném" zvuku toku: jeden proudu, který obsahuje levé a pravé kanály interleaved vzájemně. Po víme z našich zdrojového souboru médií zvukové stopy jsou, na jaké pozici ve zdroji jsme generovat takové prokládaném zvuku toku s pozice správně přiřazený lektora pro vlevo a vpravo.

Nejdřív jednu chcete generované zvukové kanály požadované zdrojové prokládaném proudu. Nám to zpracuje komponentu Interleaver toku zvuk. Přidání pracovního postupu a připojení zvuku výstupy z vstup soubor média do ní.

![Připojení zvuku toku Interleaver](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

*Připojení zvuku toku Interleaver*

Teď máme prokládaném zvuku toku, jsme pořád neměli určení, kam chcete přiřadit umístění reproduktor doleva nebo doprava. Chcete-li určit to, jsme můžete využít přidělující uživatel pozici reproduktoru.

![Přidání přidělující uživatel pozici reproduktor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

*Přidání přidělující uživatel pozici mluvčí*

Konfigurace přidělující uživatel pozici lektora pro použití s stereofonní vstupní toku prostřednictvím filtr přednastavené Encoder "Vlastní" a kanálu přednastavené s názvem "2.0 (L, R)". (Tento krok přiřadí levé mluvčí pozice ke kanálu 1 a pozice správné mluvčí kanál 2.)

Výstup přidělující reproduktor pozici uživatel připojení k zadání AAC Encoder. Sdělte Encoder AAC pro práci s "2.0 (L, R)" přednastavené kanálu, aby věděli řešení stereo zvuku předávat na vstupu.

###<a id="MXF_to_MP4_audio_and_fideo"></a>Multiplexní zvuk a Video proudy do kontejneru MP4

Vzhledem k tomu naše AVC zakódovaný stream videa a naše AAC kódování zvuku toku, jsme zachytit do. MP4 kontejner. Míchání různé datové proudy do jeden proces se nazývá "multiplexování" (nebo "muxing"). V tomto případě jsme jste prokládání zvuk a datových proudů videa v jediném souvislý. MP4 balíček. Součásti, která souřadnic u. Kontejner MP4 se nazývá multiplexor ISO MPEG-4 při instalaci. Přidat návrhové ploše a připojte k jeho vstupů AVC Video Encoder a kódovací AAC.

![Připojení MPEG4 multiplexor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

*Připojení MPEG4 multiplexor*

###<a id="MXF_to_MP4_writing_mp4"></a>Psaní souboru MP4

Při psaní výstupní soubor, se používá komponentu výstupní soubor. Jsme můžete připojit to výstupu multiplexor ISO MPEG-4 při instalaci tak, aby jeho výstup získá zapsán disk. K tomuto účelu připojte k zápisu vstupní čepu výstupní soubor výstupní spojky kontejneru (MPEG-4).

![Připojení souboru výstup](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

*Připojení souboru výstup*

Název souboru, který bude sloužit je určen vlastností souboru. Vlastnost mohou být pevně kódovaná dané hodnotě, pravděpodobně jeden chtít nastavit pomocí výrazu.

Pracovní postup automaticky určit výstup soubor vlastnost název z výrazů, klikněte na neaktivním tlačítkem vedle názvu souboru (u ikony složky). Z rozevírací nabídky vyberte "Výraz". Tím zobrazíte editor výrazů. Vymazání editoru nejdřív.

![Prázdný Editor výrazů](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

*Prázdný Editor výrazů*

Editor výrazů umožňuje zadat žádnou literál hodnotu kombinovaný s jednu nebo více proměnných. Proměnné začínat znak dolaru. Jak stisknutím klávesy $ editor zobrazí rozevírací seznam obsahující s dotazem, jestli dostupných proměnných. V našem případě použijeme kombinace výstupní proměnná adresář a proměnná Název základní vstupní soubor:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![Vyplněné se Editor výrazů](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

*Vyplněné se Editor výrazu*

>[AZURE.NOTE]Abyste mohli vidět najdete v článku výstupní soubor kódování úlohy v Azure, je nutné zadat hodnoty v editoru výraz. 

Po potvrzení výraz zasažení ok okna vlastností bude náhled na co hodnotu, odstraňuje vlastnosti souboru v daném okamžiku.

![Soubor výraz odstraňuje výstup dir](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

*Soubor výraz odstraňuje výstup dir*

###<a id="MXF_to_MP4_asset_from_output"></a>Vytvoření materiálů služby médií od výstupní soubor

Během jsme vytvořilo výstupní soubor MP4, pořád potřebujeme označíte, že tento soubor patří do výstupu materiálů, které služba media vygeneruje důsledku spuštění tento pracovní postup. K tomuto účelu uzel výstupní soubor/materiálů na plátno pracovní postup slouží. Všechny příchozí soubory do tohoto uzlu bude součástí výsledné materiálů Azure Media Services.

Připojte komponentu výstupní soubor součásti výstupní soubor/materiálů k dokončení pracovního postupu.

![Dokončení pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

*Dokončení pracovního postupu*

###<a id="MXF_to_MP4_test"></a>Vyzkoušení místně dokončení pracovního postupu

Otestujte pracovní postup místně, klikněte na tlačítko Přehrát na panelu nahoře. Po dokončení spuštění pracovního postupu zkontrolujte výstup generovaný ve složce nakonfigurované výstupu. Zobrazí se dokončení MP4 výstupní soubor kódovaný ze souboru MXF vstupní zdroje.

##<a id="MXF_to_MP4_with_dyn_packaging"></a>Kódování MXF do MP4 - multibitrate dynamické balení povolena

V tomto návodu vytvoříme sadu souborů, více MP4 bitrate s AAC kódování zvuku z jednoho. Vstupní soubor MXF.

Při výstup majetku s víc bitrate požadované pro použití v kombinaci s funkcemi dynamické balení nabízená Azure Media Services najednou několik souborů zarovnaný GOP MP4 každé, která různých bitrate a rozlišení bude muset vytvořit. Postup návodu [Kódování MXF do jednoho bitrate MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) nabízí nám dobrý výchozí bod.

![Spuštění pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

*Spuštění pracovního postupu*

###<a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Přidání jednoho nebo více další výstupy MP4

Každý soubor MP4 v naší výsledné materiálů Azure Media Services podporují různé bitrate a řešení. Pojďme přidat jeden nebo víc souborů výstup MP4 k pracovnímu postupu.

Aby zkontrolovala, jestli že máme všechny naše videa kodéry vytvářené pomocí stejné nastavení, je nejvhodnější duplikovat již existujícího videa Encoder AVC a nakonfigurujte jiné kombinaci rozlišení a bitrate (přidáte jeden z 960 × 540 na 25 snímků za sekundu 2,5 MB/s). Pokud chcete duplikovat existující encoder, kopírovat ho zkopírovat na návrhové ploše.

Připojte PIN kód nekomprimované Video výstupní soubor vstupních média do naší novou součást AVC.

![Druhá AVC encoder připojení](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

*Druhá AVC encoder připojení*

Teď přizpůsobit konfiguraci našeho nového encoder AVC výstup 960 × 540 2,5 MB/s. (Použijte jeho vlastností "Šířka výstupu", "Výstup výšku" a "Bitrate (kB / s)" pro tuto.)

Dostali jsme chcete použít výsledné materiálů společně s dynamické balení Azure Media Services, třeba schopný vytvářet z těchto souborů MP4 neúplné HLS/Fragmented MP4/přerušované čáry, které jsou přesně zarovnané k dalším způsobem, získáte klienti, které jsou přepínání mezi různých bitrates jednoho vyhlazenými nepřetržitý videa a zvuku prostředí streamování koncového bodu. Chcete-li toho docílit, potřebujeme zajistit, aby ve vlastnostech obou AVC kodéry GOP ("okruhem obrázky") velikost pro oba soubory MP4 je 2 sekund, které lze provést:

- nastavení režimu velikost GOP pevných GOP velikost a
- Interval snímků klíč na dvě sekundy.
- nastavit taky ovládací prvek GOP IDR zavřený GOP zajistit všechny GOP stojící na svoje vlastní bez závislostí

Pokud chcete, aby naši pracovního postupu pohodlný pochopit, přejmenování první encoder AVC na "AVC Video Encoder 640 × 360 1200 kb/s" a druhý encoder AVC "AVC Video Encoder 960 × 540 2 500 kb/s".

Teď přidejte druhý multiplexor ISO MPEG-4 při instalaci a druhý výstupní soubor. Připojit multiplexor nové encoder AVC a ujistěte se, že je jeho výstup přesměrován do výstupní soubor. Připojte AAC encoder zvukový výstup do nového multiplexor na vstupní taky. Výstupní soubor zase pak jde připojit k uzlu výstupní soubor/materiálů přidáte majetku médií služby, který se vytvoří.

![Druhá multiplexor a výstupní soubor připojení](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

*Druhá multiplexor a výstupní soubor připojení*

Aby byl kompatibilní se dynamické balení Azure Media Services konfigurace multiplexor prvku režimu bloku GOP počet nebo dobu trvání a nastavte GOPs na jeden blok 1. (Toto má být výchozí.)

![Režimy multiplexor bloku](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

*Režimy multiplexor bloku*

Poznámka: je vhodné Tenhle postup opakovat pro všechny další bitrate a rozlišení kombinace, který chcete přidali do výstupu materiálů.

###<a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Konfigurace názvů výstupní soubor

Máme víc tvořená jedním souborem přidána do výstupu materiálů. Díky tomu třeba zkontrolujte, jestli že názvy souborů pro každou z výstupu soubory, které se liší od sebe a možná použít i konvence pojmenování souborů tak, aby bylo jasné, od názvu souboru jste práce s.

Pojmenování výstupní soubor lze ovládat pomocí výrazů v návrháři. Otevření podokna vlastností pro jednu ze složek výstupní soubor a otevřete editor výrazů vlastnosti souboru. Náš první výstupní soubor nakonfigurovanou prostřednictvím následující výraz (viz kurz pro přechod z [MXF do jednoho bitrate výstup MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

To znamená, že naše název souboru, je určený podle dvěma proměnnými: adresáři výstup k psaní a základní název zdrojového souboru. První se zobrazí jako vlastnost v kořenovém adresáři pracovního postupu a je určený podle příchozí soubor. Všimněte si, že v adresáři výstup použít pro místní testování; Tato vlastnost se přepíšou modul pracovního postupu při spuštění pracovního postupu procesor cloudové médií v Azure Media Services.
Přiřadit oba naše soubory výstup konzistentní výstup pojmenování, změňte první pojmenovávání výraz, který má:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

a druhý na:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

Provedení intermediate testu, aby zkontrolovala oba soubory výstup MP4 správně vygenerují.

###<a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Přidání samostatné sledování zvuku

Jak si ukážeme později při jsme generování souboru .ism zvolit naše soubory výstup MP4, jsme bude vyžadují navíc jenom hlasový přenos souboru MP4 jako zvuku sledování naše adaptivní datového proudu. Pokud chcete vytvořit tento soubor, pracovního postupu (ISO-MPEG-4 při instalaci multiplexor) přidat další multiplexor a připojte AAC encoder výstupní spojky s jeho vstupní PIN kód pro sledování 1.

![Přidání zvuku multiplexor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

*Přidání zvuku multiplexor*

Vytvoření třetí výstupní soubor součást výstupních odchozí tok z multiplexor a konfigurace pojmenování výraz jako souborů:
    
    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Zvukové multiplexor vytváření výstupní soubor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

*Zvukové multiplexor vytváření výstupní soubor*

###<a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Přidání. Správci služeb sítě Internet SMIL soubor

Pro dynamické balení pro práci v kombinaci s MP4 soubory (i jenom hlasový přenos MP4) v naší Media Services materiálů, potřebujeme taky seznamu souboru (taky se mu říká souboru "SMIL": synchronizovat multimédia Integration Language). Tento soubor označuje Azure Media Services, které MP4 soubory jsou k dispozici pro dynamické balení a které z nich vzít v úvahu pro zvukový přenos. Typické seznamu souboru pro sadu jeho MP4 proudem jednoho zvukového vypadat takto:
    
    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
        <meta name="formats" content="mp4" />
      </head>
      <body>
        <switch>
          <video src="H264_1900kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_1300kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_900kbps_AAC_und_ch2_96kbps.mp4" />
          <audio src="AAC_ch2_96kbps.mp4" title="AAC_und_ch2_96kbps" />
        </switch>
      </body>
    </smil>

Soubor .ism obsahuje příkaz switch, odkaz na každé jednotlivé MP4 videosoubory a kromě tyto odkazy zvukový soubor také jeden nebo více zástupců MP4, který obsahuje pouze zvuk.

Generování soubor pro naše sadu jeho MP4 lze provést pomocí komponenty s názvem "AMS Manifest pisatele". Můžete ho přetáhněte na plochu a připojte "Psaní dokončení" výstupní spojky ze tří součástí výstupní soubor na zapisovací Manifest AMS vstup. Zkontrolujte připojení výstup Redaktor Manifest AMS k výstupní soubor/materiálů.

Stejně jako u naše jiné soubor výstup součásti, konfiguraci názvu .ism souboru výstup pomocí výrazu

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

Náš dokončení pracovního postupu vypadá níže:

![Dokončení MXF multibitrate MP4 pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

*Dokončení MXF multibitrate MP4 pracovního postupu*

##<a id="MXF_to__multibitrate_MP4"></a>Kódování MXF do multibitrate MP4 - lepší přehled

V [předchozím postupu návodu](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) jsme jsme si, jak jeden vstupní materiálů MXF lze převést aktivum výstup s více bitrate MP4 soubory, jenom hlasový přenos souboru MP4 a souboru seznamu pro použití ve spojení s dynamické balení Azure Media Services.

Tento návod se zobrazí, jak některé z hlediska může být rozšířeného a pohodlnější.

###<a id="MXF_to_multibitrate_MP4_overview"></a>Přehled pracovního postupu k vylepšení

![Pracovní postup Multibitrate MP4 k vylepšení](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

*Pracovní postup Multibitrate MP4 k vylepšení*

###<a id="MXF_to__multibitrate_MP4_file_naming"></a>Konvence pojmenování souborů

V předchozím postupu jsme jednoduchý výraz zadaný jako základ pro vytváření názvů souborů výstup. Máme pár duplikace přes: všechny jednotlivé výstupní soubor součásti zadané tento výraz.

Například naše součást výstupní soubor pro první videosoubor nakonfigurována tento výraz:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

Pro druhý výstupu videa, máme výraz jako:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

By pak nebyl čištění nižší chyby chybám a pohodlnější, pokud může odebrat některé z této duplikování jsme konfigurovatelné věci více místo toho? Naštěstí můžeme: nám do designeru výrazu funkce v kombinaci s možnost vytvořit vlastní vlastnosti na naše kořenové pracovní postup umožní přidané vrstvy pohodlí.

Předpokládejme, že budete jsou odvozeny název_souboru konfigurace z bitrates jednotlivé soubory MP4. Tyto bitrates, kterou jsme budete cílem konfigurace na jednom místě (v kořenovém adresáři naše grafu), kde se k nim ke konfiguraci a jednotka generování názvu souboru. K tomuto účelu začneme publikováním vlastnost bitrate z obou AVC kodéry kořenového adresáře naší pracovního postupu, takže z něj stal přístupných osobám s postižením z obou kořenového adresáře a od kodéry AVC. (I když zobrazili ve dvou různých místech, není pouze jednu hodnotu základní.)

###<a id="MXF_to__multibitrate_MP4_publishing"></a>Publikování vlastnosti součásti do kořenové pracovního postupu

Otevřete první encoder AVC, přejděte na vlastnost Bitrate (kB / s) a z rozevíracího seznamu vyberte publikovat.

![Publikování vlastnost bitrate](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

*Publikování vlastnost bitrate*

Dialogové okno Publikovat publikovat do kořenového adresáře naší diagram pracovního postupu nakonfigurujte publikovaný název "video1bitrate" a čitelný zobrazované jméno "Video 1 Bitrate". Nastavit vlastní název skupiny s názvem "Streamování Bitrates" a stiskněte klávesu publikovat.

![Publikování vlastnost bitrate](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

*Dialogové okno Vlastnosti bitrate*

Opakujte stejný pro vlastnost bitrate druhý encoder AVC a nazvěte ji "video2bitrate" s zobrazované jméno "Video 2 Bitrate", ve stejné vlastní skupině "Streamování Bitrates".

Pokud nám teď zkontrolujte vlastnosti kořenové pracovního postupu, ukážeme naše vlastní skupiny se dvěma publikované vlastnostmi neprojevila. Obě odrážející hodnotu jejich odpovídajících encoder bitrate AVC.

![Publikované bitrate materiály ke kořenovému pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

Pokaždé, když nám chcete je zobrazit tyto vlastnosti z kód nebo výraz, jsme můžete to udělat takto:

- z vložený kód z komponentu přímo pod to kořenovém: node.getPropertyAsString('.. / video1bitrate ", null)
- ve výrazu: ${ROOT_video1bitrate}
 
Pojďme dokončit skupině "Streamování Bitrates" publikováním naše zvuku sledování bitrate v něm taky. Ve vlastnosti AAC Encoder hledat Bitrate nastavení a vyberte publikovat z rozevíracího seznamu vedle ní. Publikování na kořenovém graf s názvem "audio1bitrate" a zobrazovaný název "Bitrate zvuk 1" v rámci naší vlastní skupiny "Streamování Bitrates".

![Dialogové okno pro zvuk bitrate](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

*Dialogové okno pro zvuk bitrate*

![Výsledný videa a zvuku materiály ke kořenovému](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

*Výsledný videa a zvuku materiály ke kořenovému*

Změna kterékoliv z těchto tří hodnoty taky znovu nakonfiguruje a změní hodnoty u odpovídajících součástí jsou propojené s (přičemž publikované z).

###<a id="MXF_to__multibitrate_MP4_output_files"></a>Dosáhli výstupní soubor, který názvy spolehnout publikované nemovitostí s hodnotou

Místo hardcoding naše generované názvy, můžete teď Změníme naše název_souboru výraz jednotlivých součástí výstupní soubor spolehnout se na požadované vlastnosti bitrate jsme právě publikovaný v kořenovém adresáři grafu. Náš první výstupní soubor, najděte vlastnost soubor a upravit výraz takto:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

Různé parametrů v tento výraz jsou dostupné a můžou zadáváte zasažení znak dolaru na klávesnici v okně výraz. K dispozici parametrů je naše video1bitrate vlastnost, která jsme dříve publikované.

![Přístup k parametry ve výrazu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

*Přístup k parametry ve výrazu*

Podobně jako výstupního souboru pro naše druhý videa:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

a jako výstupního jenom zvukový soubor:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

Pokud dojde ke změně teď přenosová některého z videosouborů nebo zvukových souborů, bude překonfigurovat odpovídajících encoder a bude použito konvence jména na základě bitrate souborů všechny automatické.

##<a id="thumbnails_to__multibitrate_MP4"></a>Přidání miniatury multibitrate výstup MP4

Počínaje generovaný [multibitrate MP4 výstup z MXF při zadávání](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)pracovního postupu, jsme se teď najít do Přidání miniatury do výstupu.

###<a id="thumbnails_to__multibitrate_MP4_overview"></a>Přehled pracovní postup přidáte miniatur

![Multibitrate MP4 pracovní postup spustit z](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

*Multibitrate MP4 pracovní postup spustit z*

###<a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>Přidání JPG kódování

Srdce naše miniatur generování budou komponentu JPG Encoder moct výstupních formátů JPG souborů.

![JPG Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

*JPG Encoder*

Budeme se nemůže připojit ale přímo naše toku nekomprimované Video z vstup soubor média do JPG encoder. Místo toho očekává být písemný jednotlivé snímky. To, můžeme udělat prostřednictvím součásti bránu rámeček videa.

![Připojení k JPG encoder bránu snímku](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

*Připojení k JPG encoder bránu snímku*

Brána rámeček jednou každých tolik sekund nebo snímky umožňuje rámeček videa pro předávání. Interval a času odsazení s které tohoto chování je, která dokáže nahradit ve vlastnostech.

![Vlastnosti bránu rámeček videa](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

*Vlastnosti bránu rámeček videa*

Vytvoření miniaturu každou minutu tak, že nastavení režimu na čas (sekundy) a intervalu 60.

###<a id="thumbnails_to__multibitrate_MP4_color_space"></a>Práce s převodu barevného prostoru

I když by zdát logické, že obou nekomprimované Video spojky bránu snímek a vstup soubor média můžete být připojili, jsme byste dostali, upozornění jsme provést.

![Chyba místo zadávání barvy](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

*Chyba místo zadávání barvy*

Je to proto které barevně informace představuje v naší původní nezpracovanými nekomprimované videa toku, pocházejících z našich MXF se liší od co JPG Encoder očekává. Konkrétně takzvané "pro změnu barvy místo" "RGB" nebo "Ve stupních šedé" očekává plovoucí dlaždice v. To znamená, že videa rámeček bránu příchozí videa toku musí mít taky převod nejdříve použita týkající se místo jeho barvu.

Přetáhněte na pracovní postup převaděčem místo barev - Intel a připojte ji k naše rámeček bránu.

![Připojení převaděči místa barvy](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

*Připojení převaděči místo barev*

V okně Vlastnosti vyberte položku BGR 24 ze seznamu přednastavení.

###<a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Psaní miniatur

Liší od naše videa MP4, komponentu JPG Encoder bude výstupní více než jeden soubor. K čelit, mohou sloužit komponentu scény hledání JPG soubor zapisovací: bude trvat příchozí JPG miniatury a zapsat, každý NázevSouboru právě dat podle jiného čísla. (Číslo, obvykle udává počet sekund, než/jednotky v toku, které byly čerpány miniaturu.)


![Úvodní informace o Redaktor soubor ve formátu JPG hledání scény](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

*Úvodní informace o Redaktor soubor ve formátu JPG hledání scény*

Konfigurace vlastností výstup složka je zadán výraz: ${ROOT_outputWriteDirectory} 

a vlastnost předpony názvu souboru s: 

    ${ROOT_sourceFileBaseName}_thumb_

Tuto předponu bude určovat, jak jsou právě pojmenovanou miniatur souborů. Bude bude uveden s číslo označující pozici v proudu.


![Vlastnosti Redaktor soubor ve formátu JPG hledání scény](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

*Vlastnosti Redaktor soubor ve formátu JPG hledání scény*

Připojte soubor Redaktor scény hledání ve formátu JPG uzel výstupní soubor/materiálů.

###<a id="thumbnails_to__multibitrate_MP4_errors"></a>Zjišťování chyb v pracovním postupu

Připojte vstupní převaděče místa barvy do výstupu nezpracovanými nekomprimované videa. Teď testu místní spuštění pracovního postupu. Je velmi pravděpodobné se neočekávaně zastavit provádění a označení s červeným ohraničením komponenty došlo k chybě pracovního postupu:

![Barva místo chybě](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

*Barva místo chybě*

Klikněte na červené "E" ikonku v pravém rohu součást převaděče místa barvy a zjistěte, co je důvod, proč kódování pokus o failed horním.

![Dialogové okno barevné převaděče místo chyby](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

*Dialogové okno barevné převaděče místo chyby*

Zjistíte, jak můžete vidět, že standardní barvy místo převaděče pro příchozí barevný prostor musí být rec601 pro naše požadovaný převod YUV RGB. Očividně naše toku neznamená jeho rec601. (Záznamů 601 je standard kódování prokládaný analogový videa signály formou digitální video. Určuje aktivní oblast překrývajícím 720 jasu a 360 vzorků chrominance na řádek. Barva kódování systém jmenoval YCbCr 4:2:2.)

Pokud to pokud chcete opravit, budete vždycky označujeme metadat naše toku, který jsme pracujete s obsahem rec601. K tomu nevyzve použijeme Video datový typ Updater součásti, kterou jsme budete umístění mezi naše neformátovaná a součást převodu místa barvu. Tento datový typ updater umožňuje pro ruční aktualizaci určité videa údaje typ vlastnosti. Konfigurace označíte místo barev standardní z "Záznamů 601". To způsobí aktualizačního Video datový typ programu označení proudu s barevný prostor "Záznamů 601", pokud jste udělali bez barvy prostoru ještě. (Ho nepřepíšou existující metadata, pokud je zaškrtnuté políčko přepsat.)

![Aktualizace standardní barvy místa na aktualizačního programu typ dat](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

*Aktualizace standardní barvy místo na aktualizačního programu typ dat*

###<a id="thumbnails_to__multibitrate_MP4_finish"></a>Dokončení pracovního postupu

Teď, když naše dokončení naše pracovního postupu, proveďte další testovací spustit uvidíte ji předejte.

![Dokončení pracovního postupu pro více mp4 výstup s miniaturami](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

*Dokončení pracovního postupu pro více mp4 výstup s miniaturami*

##<a id="time_based_trim"></a>Oříznutí založená na čase výstupu multibitrate MP4

Počínaje generovaný [multibitrate MP4 výstup z MXF při zadávání](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)pracovního postupu, jsme se teď najít do oříznutí video zdroje založené na časových razítek.

###<a id="time_based_trim_start"></a>Přehled pracovního postupu do ní začít přidávat oříznutí na

![Spuštění pracovního postupu přidáte oříznutí na](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

*Spuštění pracovního postupu přidáte oříznutí na*

###<a id="time_based_trim_use_stream_trimmer"></a>Použití datového proudu oříznutí

Oříznutí toku umožňuje Pokud chcete oříznout na začátek a konec vstupní toku základní na časování informace (sekundy, minuty,...). Oříznutí nepodporuje založeného na snímcích oříznutí.

![Oříznutí toku](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

*Oříznutí toku*

Místo propojení AVC kodéry a přidělující reproduktor pozici uživatel vstup soubor média přímo, jsme budete umístění mezi můžou být oříznutí proudu. (Jedno pro video signálu a jeden pro prokládaném zvukového signálu.)

![Mezi umístit oříznutí toku](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

*Mezi umístit oříznutí toku*

Pojďme nakonfigurovat oříznutí jsme pouze zpracuje videa a zvuky 15 sekund až 60 sekund v tomto videu.

Přejděte do vlastnosti oříznutí toku Video a nastavte počáteční čas (15s) a koncový čas (60s) vlastnosti. Abyste měli jistotu, že jak naši zvuku a videa oříznutí jsou vždy nastavený na stejné začátek a konec hodnoty, jsme publikují můžou být kořenové pracovního postupu.

![Vlastnost čas zahájení z toku oříznutí publikování](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

*Vlastnost čas zahájení z toku oříznutí publikování*

![Publikování dialogové okno vlastností pro spuštění](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

*Publikování dialogové okno vlastností pro spuštění*

![Publikování dialogové okno vlastností pro čas ukončení](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

*Publikování dialogové okno vlastností pro čas ukončení*


Pokud jsme teď zkontrolovat kořenovém naše pracovního postupu, budou oba vlastnosti zobrazeného a přehledně, která dokáže nahradit odtud.

![Publikované vlastnosti jsou k dispozici v kořenovém](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

*Publikované vlastnosti jsou k dispozici v kořenovém*

Nyní otevřete dialogové okno Vlastnosti oříznutí z zvuku oříznutí a konfigurace počátečního a koncového času s výraz, který odkazuje na vlastnostech publikované na kořenovém naše pracovního postupu.

Čas začátku zvuku oříznutí:

    ${ROOT_TrimmingStartTime}

a koncový čas:

    ${ROOT_TrimmingEndTime}

###<a id="time_based_trim_finish"></a>Dokončení pracovního postupu

![Dokončení pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

*Dokončení pracovního postupu*


##<a id="scripting"></a>Úvodní informace o komponentu skriptů

Skriptů součásti můžete provést libovolný skripty fázích spuštění naše pracovního postupu. Existují čtyři různé skripty, které mohou být provedeny, oba objekty mají zvláštní znaky a jejich blokování cyklu pracovního postupu:

- **commandScript**
- **realizeScript**
- **processInputScript**
- **lifeCycleScript**

Dokumentace komponentu skriptovány jsou uvedeny v další podrobnosti pro každou z výše uvedených možností. V [následující části](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)skriptování součást **realizeScript** slouží k vytvoření cliplist xml rychlé úpravy při spuštění pracovního postupu. Tento skript nazývá během instalace součásti jenom jednou se stane v jeho poskytování.


###<a id="scripting_hello_world"></a>Skriptování v rámci pracovního postupu: Vítáme

Přetáhněte komponentu skriptovány na návrhové ploše a přejmenujte ho (například "SetClipListXML").

![Přidání skriptů komponenty](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Přidání skriptů komponenty*

Když zkontrolujte vlastnosti součásti skriptovány čtyři typy odlišný skript bude zachycujících každý, která dokáže nahradit různých skriptu.

![Skriptů vlastnosti komponent](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Skriptů vlastnosti komponent*

Vymazání processInputScript a otevřete editor realizeScript. Teď můžeme máte nastavené nahoru a začít skriptování.

Jazyk Groovy dynamicky zkompilované skriptovacího jazyka pro platformu Java, který zachovává kompatibility s Java. Většinu kódu jazyka Java ve skutečnosti je platný Groovy kód.

Pojďme vytvořit jednoduchý Ahoj groovy skript světa v rámci naší realizeScript. V editoru zadejte následující údaje:

    node.log("hello world");

Teď provést místní test spustit. Po tomto spuštění zkontrolujte (přes kartu systém na komponentu skriptovány) vlastnost protokoly.

![Ahoj světě protokolování výstupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

*Ahoj světě protokolování výstupu*

Uzel objekt, který jsme volat metodu protokolu, odkazuje na naše aktuální "uzel" nebo součást, kterou jsme jste skriptování v rámci. Každá součást jako takové má možnost výstup protokolování data k dispozici prostřednictvím karta systému. V tomto případě jsme výstupní literál řetězec "Vítáme". Důležité pochopit, tady je to může být velmi užitečné v případě ladění nástroj, poskytne vám přehled o jaké skript právě dělá.

Z naší skriptu prostředí jsme také mít přístup k vlastnosti na další součásti. Zkuste toto:


    //inspect current node: 
    def nodepath = node.getNodePath(); 
    node.log("this node path: " + nodepath);
    
    //walking up to other nodes: 
    def parentnode = node.getParentNode(); 
    def parentnodepath = parentnode.getNodePath(); 
    node.log("parent node path: " + parentnodepath);
    
    //read properties from a node: 
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null ); 
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null); 
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);

Náš protokolu v okně se zobrazí nám takto:

![Výstup protokolu pro přístup k cesty uzlů](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

*Výstup protokolu pro přístup k cesty uzlů*


##<a id="frame_based_trim"></a>Oříznutí na základě snímků výstupu multibitrate MP4

Počínaje generovaný [multibitrate MP4 výstup z MXF při zadávání](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)pracovního postupu, jsme se teď najít do oříznutí video zdroje založené na rámeček počty.

###<a id="frame_based_trim_start"></a>Přehled přehled do ní začít přidávat oříznutí na

![Pracovní postup do ní začít přidávat oříznutí na](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

*Pracovní postup do ní začít přidávat oříznutí na*

###<a id="frame_based_trim_clip_list"></a>Použití seznamu galerie XML

V všechny kurzy předchozí pracovního postupu jsme použili komponentu vstupních média soubor jako naše videa vstupní zdroje. V tomto scénáři konkrétní však budeme používat klipartu seznamu zdrojovou komponentu místo. Nezapomeňte, že to by neměly být upřednostňovaný způsob práce; použít pouze zdroje seznamu klipartu po skutečné důvod, proč k tomu nevyzve (jako v pod případu, kdy jsme vytváříte pomocí funkce oříznutí klipu seznam).

Pokud chcete přepnout snímání z naší médií soubor předávat na vstupu zdroje seznamu klipartu, přetáhněte komponentu zdroje seznamu galerie na návrhovou plochu a připojte PIN kód XML seznamu galerie na uzel klipartu seznam XML Návrháře pracovního postupu. Zdroje seznamu klipart s výstupní spojky, to měli naplnění podle našeho vstupní video. Teď může připojit nekomprimované videa a zvuky nekomprimované PIN z zdroje seznamu galerie odpovídajících AVC kodéry a Interleaver toku zvuk. Nyní odeberte vstup soubor média.

![Soubor vstup média nahrazeny zdroje seznamu klipartu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

*Soubor vstup média nahrazeny zdroje seznamu klipartu*

Galerie seznamu zdrojovou komponentu přenese jeho předávat na vstupu "Klipartu přehled jazyka XML". Výběr zdrojového souboru k testování místně, tento seznam xml klipartu při automatické vyplněné za vás.

![Automatické vyplněné vlastnost klipartu seznam XML](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

*Automatické vyplněné vlastnost klipartu seznam XML*

Hledání trochu blíž k souboru xml, jde o tom, jak to vypadá:

![Úprava seznamu dialogu klipartu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

*Úprava seznamu dialogu klipartu*

Tato funkce xml seznamu galerie však neodráží. Jednou z možností, které máme je přidejte prvek "Střih" v části obou videa a zvuku zdroji, třeba takto:

![Přidání prvku PROČISTIT do seznamu klipartu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

*Přidání prvku PROČISTIT do seznamu klipartu*

Pokud změníte xml seznamu klip takto nad a spustit místní test, uvidíte video správně byla odstřihnuté mezi 10 až 20 sekund v tomto videu.

Na rozdíl od co se stane, když provedete přes místní spustit tento stejné xml cliplist neměli stejný efekt při použití v pracovním postupu, které se spouští v Azure Media Services. Po spuštění Azure Premium Encoder cliplist xml generováno pokaždé, když znovu na základě vstupní souboru přidělenou kódování projektu. To znamená, že by bohužel přepsat všechny změny, které jsme udělejte na xml.

Aby se zabránilo cliplist xml je vzdálené vymazání zařízení při spuštění kódování úlohy, jsme znovu jej generovat rychlé úpravy jenom po zahájení naše pracovního postupu. Tato vlastní akce můžete odebírat přes takzvanou "Skriptovány komponentu". Další informace najdete v tématu [Představení komponentu skriptovány](media-services-media-encoder-premium-workflow-tutorials.md#scripting).


Přetáhněte komponentu skriptovány na návrhové ploše a přejmenujte ho na "SetClipListXML".

![Přidání skriptů komponenty](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Přidání skriptů komponenty*

Když zkontrolujte vlastnosti součásti skriptovány čtyři typy odlišný skript bude zachycujících každý, která dokáže nahradit různých skriptu.

![Skriptů vlastnosti komponent](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Skriptů vlastnosti komponent*


###<a id="frame_based_trim_modify_clip_list"></a>Úprava seznamu klipartu z komponentu skriptovány

Před nám napsat znovu cliplist xml, které vygenerovalo během spuštění pracovního postupu, jsme musíte mít přístup k vlastnost cliplist xml a obsah. Jsme můžete to udělat takto:

    // get cliplist xml: 
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);

![Příchozí klipartu seznamu jsou přihlášení k lyncu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

*Příchozí klipartu seznamu jsou přihlášení k lyncu*

Nejdřív potřebujeme způsob, jak zjistit v tomto okamžiku, do které čárky chceme střih videa. Chcete-li toto pohodlný uživateli méně technické pracovního postupu, publikujte dvě vlastnosti do kořenové složky v grafu. K tomuto účelu klikněte pravým tlačítkem myši návrhové ploše a zvolte "Přidat vlastnost":

- První vlastnost: "ClippingTimeStart" tohoto typu: "Časový kód"
- Druhá vlastnost: "ClippingTimeEnd" tohoto typu: "Časový kód"

![Vlastnost dialogové okno Přidat čas začátku oříznutí](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

*Vlastnost dialogové okno Přidat čas začátku oříznutí*

![Publikování výřez čas materiály ke kořenovému pracovního postupu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

*Publikování výřez čas materiály ke kořenovému pracovního postupu*

Konfigurace obou vlastností vhodné hodnotu:

![Konfigurace výřez zahájení a ukončení vlastnosti](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

*Konfigurace výřez zahájení a ukončení vlastnosti*

Teď z v rámci naší skriptu jsme dostanete obě vlastnosti takto:

    
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();
    
    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);

![Okno protokolu začátek a konec oříznutí](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

*Okno protokolu začátek a konec oříznutí*

Pojďme analyzovat řetězce časový kód do pohodlnější, formulář, pomocí jednoduchého regulárních výrazů:
    
    //parse the start timing: 
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart); 
    startregresult.matches(); 
    def starttimecode = startregresult.group(1); 
    node.log("timecode start is: " + starttimecode); 
    def startframerate = startregresult.group(2); 
    node.log("framerate start is: " + startframerate);
    
    //parse the end timing: 
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend); 
    endregresult.matches(); 
    def endtimecode = endregresult.group(1); 
    node.log("timecode end is: " + endtimecode); 
    def endframerate = endregresult.group(2); 
    node.log("framerate end is: " + endframerate);

![Okno Protokol s výkonem analyzované časový kód](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

*Okno Protokol s výkonem analyzované časový kód*

Pomocí těchto informací po ruce jsme nyní můžete upravit xml cliplist tak, aby odrážely počátečního a koncového času u výřez požadované přesné rámeček videa.

![Kód skriptu přidat další prvky PROČISTIT](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

*Kód skriptu přidat další prvky PROČISTIT*

Důvodem bylo prostřednictvím běžných řetězcové manipulaci s operace. Výsledný seznam xml Klipart je zpět došlo k zápisu vlastnost clipListXML v kořenovém adresáři pracovního postupu pomocí metody "NastavitVlastnost". Okno Protokol po spuštění jiný test by zobrazit takto:

![Protokolování v rozevíracím seznamu klipartu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

*Protokolování v rozevíracím seznamu klipartu*

Provedení test-zobrazíte, jak byly oříznuté proudů videa a zvuku. Jak můžete udělat víc test spustit s různými hodnotami body oříznutí, zjistíte, že můžou být nebude vzít v úvahu však! Důvodem je, že návrháři, na rozdíl od runtime modul Azure přepsat cliplist xml každé spustit. To znamená, že pouze při prvním nastavíte vstupní a výstupní body, bude mít za následek xml transformace, všechny ostatní časy naše stráž klauzule (pokud (clipListXML.indexOf ("<trim>") == -1)) zabrání přidávání jiný PROČISTIT prvek, pokud je už prezentovat pracovního postupu.

Pokud chcete, aby naši pracovního postupu vhodné otestovat místně, nejlepší přičteme některé chaloupku zachováním kód, který zjistí Pokud PROČISTIT prvek obsahoval už. Pokud ano, jsme ho odebrat před pokračováním změnou xml se nové hodnoty. Namísto použití manipulace s jednoduchým řetězci, je nejspíš bezpečnější provést pomocí objektový model skutečné xml analýza.

Před přidáním takový kód přes, jsme budete muset nejdřív přidat číslo příkazy pro import na začátku naše skript:
    
    import javax.xml.parsers.*; 
    import org.xml.sax.*; 
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*; 
    import javax.xml.transform.*; 
    import javax.xml.transform.stream.*; 
    import javax.xml.transform.dom.*;

Po uplynutí tohoto jsme přidejte požadované vymazáním kód:

    //for local testing: delete any pre-existing trim elements from the clip list xml by parsing the xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML)); 
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already: 
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim"; 
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection 
    Set<Element> elementsToDelete = new HashSet<Element>(); 
    for (int i = 0; i < trimelems.getLength(); i++) { 
        Element e = (Element)trimelems.item(i); 
        elementsToDelete.add(e); 
    }

    node.log("about to delete any existing trim nodes");
     //delete the trim nodes: 
    elementsToDelete.each{ 
        e -> e.getParentNode().removeChild(e);
    }; 
    node.log("deleted any existing trim nodes");
    
    //serialize the modified clip list xml dom into a string: 
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result); 
    clipListXML = result.getWriter().toString();
    
Tento kód přejde přímo nad čárky niž jsme přidali PROČISTIT prvky cliplist xml.

V tomto okamžiku nemůžeme spustit a upravte naše pracovního postupu jako velmi podobným způsobem jako chceme, aniž by provedené změny někdy čas.    

###<a id="frame_based_trim_clippingenabled_prop"></a>Přidání vlastnosti pohodlí ClippingEnabled

Libovolný nemusí vždy oříznutí stát, Pojďme dokončení úkolu mimo naši pracovního postupu přidáním pohodlný způsob příznak, který označuje, zda chcete nebo nechcete jsme povolit oříznutí / výřez.

Stejně jako před, publikování nové vlastnosti do kořenového adresáře naší pracovního postupu s názvem "ClippingEnabled" typ "logická hodnota".

![Publikování vlastnost umožňující oříznutí](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

*Publikování vlastnost umožňující oříznutí*

S pod jednoduché stráž klauzule můžeme zaškrtněte, pokud je potřeba oříznutí a rozhodnout, pokud našem seznamu galerie jako takové musí být změnili nebo ne.

    //check if clipping is required: 
    def clippingrequired = node.getProperty("../ClippingEnabled"); 
    node.log("clipping required: " + clippingrequired.toString()); 
    if(clippingrequired == null || clippingrequired == false) 
    {
        node.setProperty("../clipListXml",clipListXML); 
        node.log("no clipping required"); 
        return; 
    }


###<a id="code"></a>Kompletní kód

    import javax.xml.parsers.*; 
    import org.xml.sax.*; 
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*; 
    import javax.xml.transform.*; 
    import javax.xml.transform.stream.*; 
    import javax.xml.transform.dom.*;
    
    // get cliplist xml: 
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: \n" + clipListXML);
    // get start and end of clipping: 
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();
    
    //parse the start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart); 
    startregresult.matches(); 
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);
    
    //parse the end timing: 
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches(); 
    def endtimecode = endregresult.group(1); 
    node.log("timecode end is: " + endtimecode); 
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);
    
    //for local testing: delete any pre-existing trim elements 
    //from the clip list xml by parsing the xml into a DOM:
    
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder(); 
    InputSource is=new InputSource(new StringReader(clipListXML)); 
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath(); 
    String findAllTrimElements = "//trim"; 
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection 
    Set<Element> elementsToDelete = new HashSet<Element>(); 
    for (int i = 0; i < trimelems.getLength(); i++) { 
        Element e = (Element)trimelems.item(i); 
        elementsToDelete.add(e); 
    }
    
    node.log("about to delete any existing trim nodes");
    //delete the trim nodes:
    elementsToDelete.each{ e -> 
        e.getParentNode().removeChild(e); 
    };
    node.log("deleted any existing trim nodes");

    //serialize the modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString()); 
    if(clippingrequired == null || clippingrequired == false) 
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return; 
    }

    //add trim elements to cliplist xml 
    if ( clipListXML.indexOf("<trim>") == -1 ) 
    {
        //trim video 
        clipListXML = clipListXML.replace("<videoSource>","<videoSource>\n <trim>\n <inPoint fps=\""+ 
            startframerate +"\">" + starttimecode + 
            "</inPoint>\n" + "<outPoint fps=\"" + endframerate +"\"> " + endtimecode + 
            " </outPoint>\n </trim> \n"); 
        //trim audio 
        clipListXML = clipListXML.replace("<audioSource>","<audioSource>\n <trim>\n <inPoint fps=\""+ 
            startframerate +"\">" + starttimecode + 
            "</inPoint>\n" + "<outPoint fps=\""+ endframerate +"\">" + 
            endtimecode + "</outPoint>\n </trim>\n");
        node.log( "clip list going out: \n" +clipListXML ); 
        node.setProperty("../clipListXml",clipListXML); 
    }


##<a name="also-see"></a>Viz také 

[Úvodní informace o Premium kódování v Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[Jak používat Premium kódování v Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[Kódování obsahu na vyžádání se službou Azure Media](media-services-encode-asset.md#media_encoder_premium_workflow)

[Media Encoder Premium pracovního postupu formáty a kodeky](media-services-premium-workflow-encoder-formats.md)

[Ukázkové soubory pracovního postupu](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[Nástroj služby Azure Media Services Explorer](http://aka.ms/amse)

##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

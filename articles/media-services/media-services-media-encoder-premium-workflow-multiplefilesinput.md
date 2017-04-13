<properties
    pageTitle="Použití více vstupních souborů a vlastnosti součásti s Premium Encoder | Microsoft Azure"
    description="Toto téma vysvětluje, jak používat setRuntimeProperties k používání více vstupní souborů a vlastní data předat procesor médií Media Encoder Premium pracovního postupu."
    services="media-services"
    documentationCenter=""
    authors="xpouyat"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"  
    ms.author="xpouyat;anilmur;juliako"/>

# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a>Použití více vstupních souborů a vlastnosti součásti s Premium Encoder

## <a name="overview"></a>Základní informace

Existují scénáře, ve kterých může třeba přizpůsobit vlastnosti součásti zadáte klipartu seznam XML obsahu, nebo odeslat více vstupní souborů, když odešlete úkolu s procesorem médií **Media Encoder Premium pracovního postupu** . Příklady:

- Překrývajícího textu na videa a nastavení textové hodnoty (například aktuální datum) pro každé vstupní video za běhu.
- Úpravy seznamu XML klipart (Chcete-li určit jednoho nebo několika zdrojové soubory, pomocí hesla nebo bez oříznutí, atd.).
- Překrývajícího obrázek loga na vstupní video během zakódovaný video.

Jestli Pokud chcete, aby **Media Encoder Premium pracovního postupu** vědět, že změníte některé vlastnosti v pracovním postupu při vytváření úkol nebo odeslat více vstupní souborů, je nutné použít řetězec konfigurace obsahující **setRuntimeProperties** a/nebo **transcodeSource**. Toto téma vysvětluje, jak je používat.


## <a name="configuration-string-syntax"></a>Syntaxe řetězce konfigurace

Konfigurace řetězec, který má v poli kódování úkolu je definována dokumentu XML, která vypadá takto:

    <?xml version="1.0" encoding="utf-8"?>
    <transcodeRequest>
      <transcodeSource>
      </transcodeSource>
      <setRuntimeProperties>
        <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      </setRuntimeProperties>
    </transcodeRequest>


Následující obrázek je C# kódu, který bude číst konfigurace XML ze souboru a předá úkol v projektu:

    XDocument configurationXml = XDocument.Load(xmlFileName);
    IJob job = _context.Jobs.CreateWithSingleTask(
                                                  "Media Encoder Premium Workflow",
                                                  configurationXml.ToString(),
                                                  myAsset,
                                                  "Output asset",
                                                  AssetCreationOptions.None);


## <a name="customizing-component-properties"></a>Přizpůsobení vlastnosti komponent  

### <a name="property-with-a-simple-value"></a>Vlastnost jednoduché hodnotou
V některých případech je užitečný pro přizpůsobení vlastnost komponenty společně s souboru pracovního postupu, který bude prováděnou Media Encoder Premium pracovního postupu.

Předpokládejme navržený pracovního postupu textu překrytí videí a text (například aktuální datum) by měl nastavit za běhu. Můžete to uděláte tak, že text, který chcete nastavit jako nová hodnota pro vlastnost text součástí překryvném z kódování úkolu. Chcete-li změnit další vlastnosti součásti pracovního postupu (například umístění nebo barvu překrytí, přenosová AVC encoder atd.) můžete použít tento postup.

**setRuntimeProperties** slouží k vlastnost v části pracovního postupu přepsat.

Příklad:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Optional Overlay/Overlay/filename" value="MyLogo.png"/>
          <property propertyPath="Optional Text Overlay/Text To Image Converter/text" value="Today is Friday the 13th of May, 2016"/>
      </setRuntimeProperties>
    </transcodeRequest>


### <a name="property-with-an-xml-value"></a>Vlastnost s hodnotou XML

Chcete-li nastavit vlastnost, která předpokládá, že hodnoty XML, zapouzdřit pomocí `<![CDATA[ and ]]>`.

Příklad:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

>[AZURE.NOTE]Zkontrolujte, že není umístění znak konce řádku hned za zpáteční `<![CDATA[`.


### <a name="propertypath-value"></a>Hodnota propertyPath

V předchozích příkladech propertyPath byl "/ mediálních souborů vstupní/NázevSouboru" nebo "/ inactiveTimeout" nebo "clipListXml".
Toto je, obecně názvu součásti a pak na název vlastnosti. Cesta lze vytvořit více nebo méně úrovně, třeba "/ primarySourceFile" (protože vlastnost je na kořenové úrovni pracovního postupu) nebo "/ Video zpracování/grafické/průhlednost překrytí" (protože se překrytí skupiny).    

Zkontrolujte název cesty a vlastnosti, použijte tlačítko akce, které je hned vedle jednotlivých vlastností. Můžete kliknutím na toto tlačítko akce a vyberte **Upravit**. Tím zobrazíte skutečný název vlastnosti a bezprostředně nad ním obor názvů.

![Akce a úpravy](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Vlastnost](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a>Více vstupní souborů

Každý úkol, který odeslat do **Media Encoder Premium pracovní postup** vyžaduje dva prostředky:

- První z nich je *Pracovní postup materiálů* , která obsahuje soubor pracovního postupu. Návrh pracovního postupu souborů pomocí [Návrháře pracovního postupu](media-services-workflow-designer.md).
- Druhý je *Multimediální materiály* , který obsahuje soubory médií, kterou chcete kódování.

Když posíláte více mediální soubory encoder **Media Encoder Premium pracovního postupu** , platí následující omezení:

- Všechny soubory médií musí být ve stejném *Multimediální materiály*. Není podporované použití více multimediálních materiálů.
- Je třeba nastavit primární soubor v této multimediální materiály (v ideálním případě jde hlavní videosoubor, který jej výzva k obrázku).
- Je nezbytné k předání dat konfigurace, která obsahuje prvek **setRuntimeProperties** a/nebo **transcodeSource** procesoru.
  - **setRuntimeProperties** slouží k přepsání vlastnost název souboru nebo jiné vlastnosti v části pracovního postupu.
  - **transcodeSource** slouží k určení obsahu galerie seznam XML.

Připojení v pracovním postupu:

 - Pokud používáte jednu nebo několik součásti vstup soubor média a plánu používat **setRuntimeProperties** k zadání názvu souboru, pak Nepřipojovat pin součásti primární soubor na ně. Ujistěte se, že je žádné propojení mezi objektem primární soubor a Input(s) soubor média.
 - Pokud raději používáte klipartu seznam XML a jeden médií zdrojovou komponentu, pak se můžete připojit obojí dohromady.

![Spojení z primární zdrojového souboru se vstup soubor média](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Pokud používáte setRuntimeProperties nastavovaná vlastnost název souboru je bez připojení ze souboru primární součásti vstup soubor média.*

![Připojení z Galerie seznam XML do Galerie zdroje seznamu](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*Můžete propojení se zdrojem multimediální klip seznam XML a používat transcodeSource.*


### <a name="clip-list-xml-customization"></a>Galerie přizpůsobení seznam XML
Zadaný seznam XML klipartu v pracovním postupu za běhu pomocí **transcodeSource** v řetězci konfigurace XML. Při této akci musí pin klipartu seznam XML k připojení k médií zdrojovou komponentu v pracovním postupu.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <transcodeSource>
          <clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
          </clipList>
        </transcodeSource>
        <setRuntimeProperties>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Pokud chcete zadat /primarySourceFile používat tuto vlastnost pojmenování souborů výstup pomocí "Výrazy", pak doporučujeme předávání klipartu seznam XML jako vlastnost *Po* vlastnost /primarySourceFile, abyste nemuseli seznamu klipartu přepsat nastavením /primarySourceFile.

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

S další snímek přesné oříznutí:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
               <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


## <a name="example"></a>Příklad

Zvažte příklad, ve kterém chcete překryvném obrázek loga na vstupní video během zakódovaný video. V tomto příkladu vstupní video jmenuje "MyInputVideo.mp4" a loga jmenuje "MyLogo.png". Provedením následujících kroků:

- Vytvoření pracovního postupu majetku s soubor pracovního postupu (viz následující příklad).
- Vytvoření médií materiálů, která obsahuje dva soubory: MyInputVideo.mp4 jako primární souboru a požadovaného MyLogo.png.
- Odeslat úkolu procesor médií Media Encoder Premium pracovního postupu s nad vstupní prostředky a zadejte následující řetězec konfigurace.

Konfigurace:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="MyLogo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


Ve výše uvedeném příkladu název videosouboru se pošle komponentu vstup soubor média vlastnost primarySourceFile. Název souboru s logem odeslaný do jiného médií soubor zadání vstupních hodnot, který je spojený s komponentu obrázku překryvném.

>[AZURE.NOTE]Název videosouboru jsou odeslány vlastnost primarySourceFile Důvodem je použít tuto vlastnost v pracovním postupu pro vytváření názvu souboru správný výstup pomocí výrazů, například.


### <a name="step-by-step-workflow-creation-that-overlays-a-logo-on-top-of-the-video"></a>Podrobné pracovní postup vytvoření které překrývá a loga přes video     

Tady najdete postup, jak vytvořit pracovní postup, který má dvě soubory předávat na vstupu: video a obrázek. Bude překryvném obrázek nad video.

Otevřete **Návrháře pracovního postupu** a vyberte **soubor** > **Nový pracovní prostor** > **Přehled program**.

Nový pracovní postup ukazuje tři prvky:

- Primární zdrojového souboru
- Seznam XML klipů
- Výstupní soubor/materiálů  

![Nový pracovní postup kódování](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Nový pracovní postup kódování*


Pro příjem vstupního multimediálního souboru, začněte s přidáte komponentu vstupních média soubor. Přidání součásti pracovního postupu, najděte ho do pole Hledat úložiště a přetáhněte na požadovanou položku podokna návrháře.

Dále přidejte videosoubor pro navrhování pracovní postup. Klikněte na podokno pozadí v Návrháři pracovního postupu a vyhledejte vlastnost primární zdrojový soubor v podokně vpravo vlastnost. Klikněte na ikonu složky a vyberte příslušný soubor videa.

![Primární soubor zdroje](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Primární soubor zdroje*


Potom zadejte videosoubor součásti vstup soubor média.   

![Mediální soubor vstupní zdroj](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Mediální soubor vstupní zdroj*


Hned, jak to provést, komponentu vstup soubor média prozkoumat soubor a naplnění jeho výstupní spojky, aby odrážely soubor, který ho kontrolovat.

Dalším krokem je přidat "Video datový typ Updater" k určení místa barvy pro Rec.709. Přidání "Video formát převaděče", která je nastavená jako typ dat rozložení/rozložení = konfigurovatelné planární. Videa proudu bude převedena na formát, který můžete považuje za zdroj součásti překryvném.

![videa Updater typ dat a převaděč formátu](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Videa datový typ Updater a převaděč formátu*

![Typ rozložení = konfigurovatelné planární](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Typ rozložení je konfigurovatelné planární*

Potom přidat komponentu videa překryvném a připojení (nekomprimované) videa pin (nekomprimované) videa připnout soubor vstupních média.

Přidání jiného médií soubor předávat na vstupu (načtení souboru s logem), klepněte na tuto součást a přejmenujte ho na "Médií soubor vstupní Logo" a vyberte obrázek (PNG soubor například) ve vlastnosti souboru. Připojení k nekomprimované obrázek čepu překrytí pin nekomprimované obrázek.

![Překryvné zobrazení součást a obrázek souboru zdroje](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Překryvné zobrazení součást a obrázek souboru zdroje*


Pokud chcete změnit umístění loga na video (například můžete umístit na 10 % z levého horního rohu video), zrušte zaškrtnutí políčka "Ručního zadávání". To můžete udělat, protože používáte vstup soubor média poskytnout souboru s logem ke komponentě překryvném.

![Překryvné zobrazení polohy](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Překryvné zobrazení polohy*


Kódování videa datového proudu H.264, přidejte encoder součásti videa Encoder AVC a AAC na návrhové ploše. Připojte PIN kódy.
Nastavení AAC encoder a vyberte přednastavení převodu formátu zvuku: 2.0 (L, R).

![Kodéry zvuku a videa](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Kodéry zvuku a videa*


Teď přidejte součásti **ISO Mpeg-4 při instalaci multiplexor** a **Výstupní soubor** a připojte spojky, jak je vidět.

![MP4 multiplexor a výstupní soubor](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*MP4 multiplexor a výstupní soubor*


Budete muset nastavit název výstupní soubor. Klepněte na ni **Výstupní soubor** a úpravu výrazu souboru:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Název souboru výstup](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Název souboru výstup*

Spuštěním pracovního postupu místně zkontrolujte, jestli je spuštěný správně.

Po dokončení, můžete ho v Azure Media Services spustit.

Nejprve připravte se dvěma soubory v něm aktivum v Azure Media Services: videosoubor a logo. Lze provést pomocí .NET nebo rozhraní REST API. Můžete taky provést pomocí Azure portál nebo [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).

Tento kurz se dozvíte, jak spravovat aktiv pomocí AMSE. Přidání souborů do aktivum dvěma způsoby:

- Vytvoření do místní složky, zkopírujte dva soubory a přetáhněte složku na kartu **materiálů** .
- Nahrání videosouborů jako aktivum, zobrazit informace o majetku, přejděte na kartu soubory a nahrajte další soubor (logo).

>[AZURE.NOTE]Ujistěte se, jak nastavit primární soubor materiálů (hlavním videosoubor).

![Soubory materiálů v AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*Soubory materiálů v AMSE*


Vyberte majetek a zvolte kódování s Premium Encoder. Nahrání pracovního postupu a vyberte ho.

Klikněte na tlačítko data předat procesoru a přidejte následující XML k nastavení vlastností runtime:

![Premium Encoder v AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*Premium Encoder v AMSE*


Vložte následující dat XML. Budete muset zadat název videosouboru pro vstup soubor média a primarySourceFile. Zadejte název název souboru pro logo příliš.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

*setRuntimeProperties*


Pokud použijete .NET SDK k vytváření a spouštění úkol, má tato data XML předávat jako řetězec konfigurace.

    public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);

Po dokončení projektu MP4 souboru v materiálů výstup zobrazí překrytí!

![Překrytí videa](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*Překrytí videa*


Pracovní postup Ukázka si můžete stáhnout z [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/).


## <a name="see-also"></a>Viz taky

- [Úvodní informace o Premium kódování v Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

- [Použití Premium kódování v Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

- [Kódování obsahu na vyžádání se službou Azure Media Services](media-services-encode-asset.md#media_encoder_premium_workflow)

- [Formáty Media Encoder Premium pracovního postupu a kodeky](media-services-premium-workflow-encoder-formats.md)

- [Ukázkové soubory pracovního postupu](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)

- [Nástroj služby Azure Media Services Explorer](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

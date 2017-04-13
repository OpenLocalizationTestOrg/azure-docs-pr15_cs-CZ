<properties 
    pageTitle="Příručka pro řešení potíží pro živou streamování | Microsoft Azure" 
    description="Toto téma obsahuje návrhy na řešení potíží živou streamování." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/12/2016"  
    ms.author="juliako"/>

#<a name="troubleshooting-guide-for-live-streaming"></a>Průvodce pro řešení živou datového proudu

Toto téma obsahuje návrhy na řešení potíží některé živou streamování.

## <a name="issues-related-to-on-premises-encoders"></a>Problémy související s místním kodéry 

Tato část nabízí návrhy, jak řešit problémy související s místním kodéry nakonfigurované jednoho bitrate toku odešlete AMS kanály, kteří mají povolenou živou kódování.

###<a name="problem-would-like-to-see-logs"></a>Problém: Chtěli zobrazit protokoly 

- **Možný problém**: nemůžete najít encoder protokoly, který vám mohou pomoci ladění problémy.
    
    - **Telestream Wirecast**: budete obvykle moct najít protokolů ve skupinovém rámečku C:\Users\{username} \AppData\Roaming\Wirecast\ 
    - **Elementární Live**: můžete najít obsahuje odkazy na protokoly na portálu pro správu. Klikněte na **Statistika**a potom **protokoly**. Na stránce **Protokoly** zobrazí se seznam protokoly pro všechny položky LiveEvent; Vyberte položku odpovídající aktuální relaci. 
    - **Flash Media Live Encoder**: **Adresář protokolu...** najdete tak, že přejdete na kartu **Kódování protokolu** .
    
###<a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Problém: Žádným způsobem pro výstup postupné toku

- **Možný problém**: encoder použit nemá Zrušit prokládání automaticky. 

    **Návody na řešení potíží**: Vyhledejte odstraňování prokládání možnost encoder rozhraní aplikace. Po povolení zrušte prokládání znova zkontroloval nastavení postupné výstupu. 
 
###<a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Problém: Vyzkoušeli několik nastavení výstupní encoder a pořád nepodařilo připojit. 

- **Možný problém**: Azure kódování kanálu nebyl obnovit správně. 

    **Návody na řešení potíží**: Ujistěte se, že encoder je už předání do AMS, ukončit a resetování kanálu. Po opětovném spuštění připojte svůj encoder nové nastavení. Pokud tato pořád neodstraní problém, zkuste vytvořit úplně nový kanál, někdy kanály, může být poškozená po několika neúspěšné pokusy o.  

- **Možný problém**: velikost GOP nebo nastavení klíčových snímků jsou optimální. 

    **Návody na řešení potíží**: doporučuje GOP velikosti nebo klíčový interval je 2 sekund. Některé kodéry výpočet toto nastavení používají v počtu snímků, zatímco ostatní používají sekund. Příklad: při výstupu snímků 30 / velikost GOP měl by 60 snímky, které se rovná 2 sekund.  
     
- **Možný problém**: skrytých porty blokují proudu. 

    **Návody na řešení potíží**: Jestliže datových proudů prostřednictvím RTMP, zkontrolujte, zda nastavení brány firewall nebo proxy k potvrzení, jestli jsou porty odchozí 1935 a 1936 otevřené. Při použití datových proudů RTP, zkontrolujte, zda odchozí port 2010 otevřít. 


###<a name="problem-when-configuring-the-encoder-to-stream-with-the-rtp-protocol-there-is-no-place-to-enter-a-host-name"></a>Problém: Při konfiguraci encoder proudu s protokolem RTP, není žádný místo a zadejte název hostitele. 

- **Možný problém**: mnoho RTP kodéry neumožňují přidávání pro názvy hostitelů a IP adresu potřebovali získat.  

    **Návody na řešení potíží**: najít na IP adresu, otevřete okno příkazového řádku v jakémkoli počítači. Ve Windows, otevřete si Spouštěč spustit (Windows + R) a napište "cmd" otevřete.  

    Po otevření příkazovém řádku zadejte "Ping [název hostitele AMS]". 

    Název hostitele můžou pocházet vynecháním číslo portu z Azure jedí adresy URL, jako zvýrazněný v následujícím příkladu: 

    RTP://Test2-amstest009.RTP.Channel.mediaservices.Windows.NET:2010 / 

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

###<a name="problem-unable-to-playback-the-published-stream"></a>Problém: Nejde přehrávání publikované proudu.
 
- **Možný problém**: neexistuje žádné streamování Endpoint spuštění nebo je žádné streamování jednotky (jednotkách) přidělený. 

    **Návody na řešení potíží**: přejděte na kartu "Streamování koncový bod" v nástroji AMSE a potvrďte je Endpoint datových proudů s datové proudy jednu jednotku. 
    


>[AZURE.NOTE] Pokud po provedení kroků řešení potíží, které se pořád nemůžete můžete vysílat datovými proudy úspěšně, odešlete požadavek podpory můžete pomocí portálu Azure.

##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

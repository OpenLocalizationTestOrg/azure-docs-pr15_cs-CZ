## <a name="send-messages-to-event-hubs"></a>Odeslání zprávy rozbočovače události

V této části jsme napsali C app události odešlete rozbočovače události. Použijeme knihovnu kanálem AMQP z [Apache Qpid projektu](http://qpid.apache.org/). Toto je podobná pomocí služby Bus frontách a témata AMQP z C jako znázorněno [zde](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Další informace najdete v dokumentaci [Qpid kanálem](http://qpid.apache.org/proton/index.html).

1. Na [stránce Qpid AMQP Messenger](http://qpid.apache.org/components/messenger/index.html)klikněte na odkaz **Instalaci kanálem Qpid** a postupujte podle pokynů v závislosti na vašem prostředí. Jsme převezmou Linux prostředí; například [Azure Linux OM](../articles/virtual-machines/virtual-machines-linux-quick-create-cli.md) se systémem Ubuntu 14.04.

2. Kompilace knihovnu kanálem, nainstalujte následující balíčky:

    ```
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```

3. Stáhnout knihovnu [Qpid kanálem knihovny](http://qpid.apache.org/proton/index.html) a extrahovat, například:

    ```
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```

4. Vytvoření buildu adresář, kompilace a nainstalovat:

    ```
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```

5. V adresáři práce vytvoření nového souboru s názvem **sender.c** s následující obsah. Nezapomeňte nahradit hodnoty pro událost rozbočovače název a název názvů (je obvykle `{event hub name}-ns`). Zakódovaný URL verzi klávesu musíte taky nahradit **SendRule** vytvořili. Můžete provést tyto akce kódovat adresa URL je [tady](http://www.w3schools.com/tags/ref_urlencode.asp).

    ```
    #include "proton/message.h"
    #include "proton/messenger.h"

    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>

    #define check(messenger)                                                     \
      {                                                                          \
        if(pn_messenger_errno(messenger))                                        \
        {                                                                        \
          printf("check\n");                                                     \
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); \
        }                                                                        \
      }  

    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  

    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }

    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://SendRule:{Send Rule key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";

        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();

        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);

        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));

        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);

        pn_message_free(message);
    }

    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");

        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);

        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }

        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);

        return 0;
    }
    ```

6. Kompilace soubor za předpokladu, že **RSZ**:

    ```
    gcc sender.c -o sender -lqpid-proton
    ```

> [AZURE.NOTE] V tomto kódu používáme odchozí okna 1 Vynutit zprávy, co nejdříve. Obecně aplikace pokuste se dávku zprávy chcete zvýšit výkon. Další informace o tom, jak používat knihovnu Qpid kanálem v tomto a jiných prostředí a od platformy, které jsou poskytovány vazby najdete v článku [Qpid AMQP Messenger stránky](http://qpid.apache.org/components/messenger/index.html) (aktuálně Perl PHP, Python a skutečné).

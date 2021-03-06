---
title: 快速入門：自訂語音優先虛擬助理 (預覽)，Java (Windows, Linux) - 語音服務
titleSuffix: Azure Cognitive Services
description: 在本快速入門中，您將了解如何在 Java 主控台應用程式中使用認知服務語音軟體開發套件 (SDK)。 您將了解如何將用戶端應用程式連線到先前建立的 Bot Framework Bot，該 Bot 設定為使用 Direct Line Speech 頻道和啟用語音優先虛擬助理體驗。
services: cognitive-services
author: bidishac
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 05/02/2019
ms.author: bidishac
ms.openlocfilehash: 83149a8422db25106a97b1711c0ae9ce3c6603eb
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2019
ms.locfileid: "65465671"
---
# <a name="quickstart-create-a-voice-first-virtual-assistant-with-the-speech-sdk-java"></a>快速入門：使用語音 SDK、Java 建立語音優先虛擬助理

另備有[語音轉換文字](quickstart-java-jre.md)和[語音翻譯](quickstart-translate-speech-java-jre.md)的快速入門。

在本文中，您會使用[認知服務語音 SDK](speech-sdk.md)，建立 Java 主控台應用程式。 此應用程式會連線到先前撰寫的 Bot，該 Bot 設定為使用 Direct Line Speech 頻道、傳送語音要求，以及傳回語音回應活動 (若已設定的話)。 此應用程式是使用語音 SDK Maven 套件，以及Windows、Ubuntu Linux 或 macOS 上的 Eclipse Java IDE 來建置。 它會在 64 位元 Java 8 Runtime Environment (JRE) 上執行。

## <a name="prerequisites"></a>必要條件

本快速入門需要：

* 作業系統：Windows (64 位元)、Ubuntu Linux 16.04/18.04 (64 位元) 或 macOS 10.13 或更新版本
* [Eclipse Java IDE](https://www.eclipse.org/downloads/) \(英文\)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) \(英文\) 或 [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html) \(英文\)
* 適用於語音服務的 Azure 訂用帳戶金鑰。 [免費取得一個金鑰](get-started.md)。
* 預先設定的 Bot，其使用 Bot Framework 4.2 版或更新版本來建立。 Bot 必須訂閱新的 "Direct Line Speech" 頻道才能接收語音輸入。

    > [!NOTE]
    > 在預覽中，Direct Line 語音通道目前僅支援 **westus2** 區域。

    > [!NOTE]
    > [免費試用語音服務](get-started.md)中所述標準定價層的 30 天試用受限於 **westus** (不是 **westus2**)，因此與 Direct Line Speech 不相容。 免費與標準層 **westus2** 訂用帳戶相容。

如果您執行 Ubuntu 16.04/18.04，請先確定已安裝下列相依性，再啟動 Eclipse：

```console
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libasound2 wget
```

如果您執行 Windows (64 位元)，請確定您已為平台安裝 Microsoft Visual C++ 可轉散發套件：
* [下載適用於 Visual Studio 2017 的 Microsoft Visual C++ 可轉散發套件](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)

## <a name="optional-get-started-fast"></a>選用：快速開始使用

本快速入門會逐步說明如何製作簡單的用戶端應用程式以連線到您具備語音功能的 Bot。 如果您想要深入了解，可以在[語音 SDK 範例](https://aka.ms/csspeech/samples)的 `quickstart` 資料夾之下取得本快速入門中使用的完整、可立即編譯的原始程式碼。

## <a name="create-and-configure-project"></a>建立及設定專案

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

此外，若要啟用記錄功能，請更新 **pom.xml** 檔案以包含下列相依性。

   ```xml
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-simple</artifactId>
        <version>1.7.5</version>
    </dependency>
   ```

## <a name="add-sample-code"></a>新增範例程式碼

1. 若要將新的空白類別新增到您的 Java 專案，請選取 [檔案] > [新增] > [類別]。

1. 在 [新增 Java 類別] 視窗中，於 [套件] 欄位中輸入 **speechsdk.quickstart**，並在 [名稱] 欄位中輸入 **Main**。

   ![[新增 Java 類別] 視窗的螢幕擷取畫面](media/sdk/qs-java-jre-06-create-main-java.png)

1. 開啟新建立的 **Main** 類別，並以下列起始程式碼取代 `Main.java` 檔案的內容。

    ```java
    package speechsdk.quickstart;

    import com.microsoft.cognitiveservices.speech.audio.AudioConfig;
    import com.microsoft.cognitiveservices.speech.audio.PullAudioOutputStream;
    import com.microsoft.cognitiveservices.speech.dialog.BotConnectorConfig;
    import com.microsoft.cognitiveservices.speech.dialog.SpeechBotConnector;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;

    import javax.sound.sampled.AudioFormat;
    import javax.sound.sampled.AudioSystem;
    import javax.sound.sampled.DataLine;
    import javax.sound.sampled.SourceDataLine;
    import java.io.InputStream;

    public class Main {
        final Logger log = LoggerFactory.getLogger(Main.class);

        public static void main(String[] args) {
            // New code will go here
        }

        private void playAudioStream(PullAudioOutputStream audio) {
            ActivityAudioStream stream = new ActivityAudioStream(audio);
            final ActivityAudioStream.ActivityAudioFormat audioFormat = stream.getActivityAudioFormat();
            final AudioFormat format = new AudioFormat(
                    AudioFormat.Encoding.PCM_SIGNED,
                    audioFormat.getSamplesPerSecond(),
                    audioFormat.getBitsPerSample(),
                    audioFormat.getChannels(),
                    audioFormat.getFrameSize(),
                    audioFormat.getSamplesPerSecond(),
                    false);
            try {
                int bufferSize = format.getFrameSize();
                final byte[] data = new byte[bufferSize];

                SourceDataLine.Info info = new DataLine.Info(SourceDataLine.class, format);
                SourceDataLine line = (SourceDataLine) AudioSystem.getLine(info);
                line.open(format);

                if (line != null) {
                    line.start();
                    int nBytesRead = 0;
                    while (nBytesRead != -1) {
                        nBytesRead = stream.read(data);
                        if (nBytesRead != -1) {
                            line.write(data, 0, nBytesRead);
                        }
                    }
                    line.drain();
                    line.stop();
                    line.close();
                }
                stream.close();

            } catch (Exception e) {
                e.printStackTrace();
            }
        }

    }
    ```

1. 在 **main** 方法中，您會先設定 `BotConnectorConfig` 並使用它來建立 `SpeechBotConnector` 執行個體。 這會連線至 Direct Line Speech 頻道來與您的 Bot 互動。 `AudioConfig` 執行個體也用來指定音訊輸入來源。 在此範例中，預設麥克風會搭配 `AudioConfig.fromDefaultMicrophoneInput()` 使用。

    * 將字串 `YourSubscriptionKey` 取代為您的訂用帳戶金鑰，您可以從[這裡](get-started.md)取得該金鑰。
    * 將字串 `YourServiceRegion` 取代為與您的訂用帳戶相關聯的[區域](regions.md)。
    * 將字串 `YourChannelSecret` 取代為您的 Direct Line Speech 頻道祕密。

    > [!NOTE]
    > 在預覽中，Direct Line Speech 頻道目前僅支援 **westus2** 區域。

    > [!NOTE]
    > [免費試用語音服務](get-started.md)中所述標準定價層的 30 天試用受限於 **westus** (不是 **westus2**)，因此與 Direct Line Speech 不相容。 免費與標準層 **westus2** 訂用帳戶相容。

    ```java
    final String channelSecret = "YourChannelSecret"; // Your channel secret
    final String subscriptionKey = "YourSubscriptionKey"; // your subscription key
    final String region = "YourServiceRegion"; // Your service region. Currently assumed to be westus2
    final BotConnectorConfig botConnectorConfig = BotConnectorConfig.fromSecretKey(channelSecret, subscriptionKey, region);

    // Configure audio input from microphone.
    final AudioConfig audioConfig = AudioConfig.fromDefaultMicrophoneInput();

    // Create a SpeechjBotConnector instance
    final SpeechBotConnector botConnector = new SpeechBotConnector(botConnectorConfig, audioConfig);
    ```

1. `SpeechBotConnector` 依賴多個事件來傳達其 Bot 活動、語音辨識結果及其他資訊。 接下來新增這些事件接聽程式。

    ```java
    // Recognizing will provide the intermediate recognized text while an audio stream is being processed
    botConnector.recognizing.addEventListener((o, speechRecognitionResultEventArgs) -> {
        log.info("Recognizing speech event text: {}", speechRecognitionResultEventArgs.getResult().getText());
    });

    // Recognized will provide the final recognized text once audio capture is completed
    botConnector.recognized.addEventListener((o, speechRecognitionResultEventArgs) -> {
        log.info("Recognized speech event reason text: {}", speechRecognitionResultEventArgs.getResult().getText());
    });

    // SessionStarted will notify when audio begins flowing to the service for a turn
    botConnector.sessionStarted.addEventListener((o, sessionEventArgs) -> {
        log.info("Session Started event id: {} ", sessionEventArgs.getSessionId());
    });

    // SessionStopped will notify when a turn is complete and it's safe to begin listening again
    botConnector.sessionStopped.addEventListener((o, sessionEventArgs) -> {
        log.info("Session stopped event id: {}", sessionEventArgs.getSessionId());
    });

    // Canceled will be signaled when a turn is aborted or experiences an error condition
    botConnector.canceled.addEventListener((o, canceledEventArgs) -> {
        log.info("Canceled event details: {}", canceledEventArgs.getErrorDetails());
        botConnector.disconnectAsync();
    });

    // ActivityReceived is the main way your bot will communicate with the client and uses bot framework activities.
    botConnector.activityReceived.addEventListener((o, activityEventArgs) -> {
        final String act = activityEventArgs.getActivity().serialize();
            log.info("Received activity {} audio", activityEventArgs.hasAudio() ? "with" : "without");
            if (activityEventArgs.hasAudio()) {
                playAudioStream(activityEventArgs.getAudio());
            }
        });
    ```

1. 叫用 `connectAsync()` 方法，以將 `SpeechBotConnector` 連線到 Direct Line Speech。 若要測試您的 Bot，您可以叫用 `listenOnceAsync` 方法，以傳送來自麥克風的音訊輸入。 此外，您也可以使用 `sendActivityAsync` 方法，以序列化字串的形式傳送自訂活動。 這些自訂活動可以提供您的 Bot 會在交談中使用的其他資料。

    ```java
    botConnector.connectAsync();
    // Start listening.
    System.out.println("Say something ...");
    botConnector.listenOnceAsync();

    // botConnector.sendActivityAsync(...)
    ```

1. 將變更儲存到 `Main` 檔案。

1. 如需支援回應播放，請在 Java inputStream 中新增額外的類別以轉換從 getAudio() API 傳回的 PullAudioOutputStream 物件，以便輕鬆處理。 此 ActivityAudioStream 是特殊的類別，可處理來自 Direct Line Speech 通道的語音回應。 也會提供處理播放所需的存取子，以擷取音訊格式資訊：如需此功能，請選取 [檔案]  >  [新建]  >  [類別]。

1. 在 [新建 Java 類別] 視窗中，將 **speechsdk.quickstart** 輸入 [套件] 欄位；並將 **ActivityAudioStream** 輸入 [名稱]欄位。

1. 開啟新建立的 **ActivityAudioStream** 類別，並以下面提供的程式碼取代。

    ```java
    package com.speechsdk.quickstart;

    import com.microsoft.cognitiveservices.speech.audio.PullAudioOutputStream;

    import java.io.IOException;
    import java.io.InputStream;


    public final class ActivityAudioStream extends InputStream {
        /**
         * The number of samples played per second. (16 kHz)
         */
        public static final long SAMPLE_RATE = 16000;
        /**
         * The number of bits in each sample of a sound that has this format. (16 bits)
         */
        public static final int BITS_PER_SECOND = 16;
        /**
         * The number of audio channels in this format (1 for mono).
         */
        public static final int CHANNELS = 1;
        /**
         * The number of bytes in each frame of a sound that has this format (2).
         */
        public static final int FRAME_SIZE = 2;

        /**
         * Reads up to a specified maximum number of bytes of data from the audio
         * stream, putting them into the given byte array.
         *
         * @param b   the buffer into which the data is read
         * @param off the offset, from the beginning of array <code>b</code>, at which
         *            the data will be written
         * @param len the maximum number of bytes to read
         * @return the total number of bytes read into the buffer, or -1 if there
         * is no more data because the end of the stream has been reached
         */
        @Override
        public int read(byte[] b, int off, int len) {
            byte[] tempBuffer = new byte[len];
            int n = (int) this.pullStreamImpl.read(tempBuffer);
            for (int i = 0; i < n; i++) {
                if (off + i > b.length) {
                    throw new ArrayIndexOutOfBoundsException(b.length);
                }
                b[off + i] = tempBuffer[i];
            }
            if (n == 0) {
                return -1;
            }
            return n;
        }

        /**
         * Reads the next byte of data from the activity audio stream if available.
         *
         * @return the next byte of data, or -1 if the end of the stream is reached
         * @see #read(byte[], int, int)
         * @see #read(byte[])
         * @see #available
         * <p>
         */
        @Override
        public int read() {
            byte[] data = new byte[1];
            int temp = read(data);
            if (temp <= 0) {
                // we have a weird situation if read(byte[]) returns 0!
                return -1;
            }
            return data[0] & 0xFF;
        }

        /**
         * Reads up to a specified maximum number of bytes of data from the activity audio stream
         * putting them into the given byte array.
         *
         * @param b the buffer into which the data is read
         * @return the total number of bytes read into the buffer, or -1 if there
         * is no more data because the end of the stream has been reached
         */
        @Override
        public int read(byte[] b) {
            int n = (int) pullStreamImpl.read(b);
            if (n == 0) {
                return -1;
            }
            return n;
        }

        /**
         * Skips over and discards a specified number of bytes from this
         * audio input stream.
         *
         * @param n the requested number of bytes to be skipped
         * @return the actual number of bytes skipped
         * @throws IOException if an input or output error occurs
         * @see #read
         * @see #available
         */
        @Override
        public long skip(long n) {
            if (n <= 0) {
                return 0;
            }
            if (n <= Integer.MAX_VALUE) {
                byte[] tempBuffer = new byte[(int) n];
                return read(tempBuffer);
            }
            long count = 0;
            for (long i = n; i > 0; i -= Integer.MAX_VALUE) {
                int size = (int) Math.min(Integer.MAX_VALUE, i);
                byte[] tempBuffer = new byte[size];
                count += read(tempBuffer);
            }
            return count;
        }

        /**
         * Closes this audio input stream and releases any system resources associated
         * with the stream.
         */
        @Override
        public void close() {
            this.pullStreamImpl.close();
        }

        /**
         * Fetch the audio format for the ActivityAudioStream. The ActivityAudioFormat defines the sample rate, bits per sample and the # channels
         *
         * @return instance of the ActivityAudioFormat associated with the stream
         */
        public ActivityAudioStream.ActivityAudioFormat getActivityAudioFormat() {
            return activityAudioFormat;
        }

        /**
         * Returns the maximum number of bytes that can be read (or skipped over) from this
         * audio input stream without blocking.
         *
         * @return the number of bytes that can be read from this audio input stream without blocking.
         * As this implementation does not buffer this will be defaulted to 0
         */
        @Override
        public int available() {
            return 0;
        }

        public ActivityAudioStream(final PullAudioOutputStream stream) {
            pullStreamImpl = stream;
            this.activityAudioFormat = new ActivityAudioStream.ActivityAudioFormat(SAMPLE_RATE, BITS_PER_SECOND, CHANNELS, FRAME_SIZE, AudioEncoding.PCM_SIGNED);
        }

        private PullAudioOutputStream pullStreamImpl;

        private ActivityAudioFormat activityAudioFormat;

        /**
         * ActivityAudioFormat is an internal format which contains metadata regarding the type of arrangement of
         * audio bits in this activity audio stream.
         */
        static class ActivityAudioFormat {

            private long samplesPerSecond;
            private int bitsPerSample;
            private int channels;
            private int frameSize;
            private AudioEncoding encoding;

            public ActivityAudioFormat(long samplesPerSecond, int bitsPerSample, int channels, int frameSize, AudioEncoding encoding) {
                this.samplesPerSecond = samplesPerSecond;
                this.bitsPerSample = bitsPerSample;
                this.channels = channels;
                this.encoding = encoding;
                this.frameSize = frameSize;
            }

            /**
             * Fetch the number of samples played per second for the associated audio stream format.
             *
             * @return the number of samples played per second
             */
            public long getSamplesPerSecond() {
                return samplesPerSecond;
            }

            /**
             * Fetch the number of bits in each sample of a sound that has this audio stream format.
             *
             * @return the number of bits per sample
             */
            public int getBitsPerSample() {
                return bitsPerSample;
            }

            /**
             * Fetch the number of audio channels used by this audio stream format.
             *
             * @return the number of channels
             */
            public int getChannels() {
                return channels;
            }

            /**
             * Fetch the default number of bytes in a frame required by this audio stream format.
             *
             * @return the number of bytes
             */
            public int getFrameSize() {
                return frameSize;
            }

            /**
             * Fetch the audio encoding type associated with this audio stream format.
             *
             * @return the encoding associated
             */
            public AudioEncoding getEncoding() {
                return encoding;
            }
        }

        /**
         * Enum defining the types of audio encoding supported by this stream
         */
        public enum AudioEncoding {
            PCM_SIGNED("PCM_SIGNED");

            String value;

            AudioEncoding(String value) {
                this.value = value;
            }
        }
    }

    ```

1. 將變更儲存到 `ActivityAudioStream` 檔案。

## <a name="build-and-run-the-app"></a>建置並執行應用程式

按 F11 鍵，或選取 [執行] > [偵錯]。
主控台會顯示「說一些話」訊息。此時，您可以說您的 Bot 可理解的英文片語或句子。 您的語音將透過 Direct Line Speech 頻道傳輸到您的 Bot，您的 Bot 將會辨識、處理語音，並以活動形式傳回回應。 如果您的 Bot 傳回語音作為回應，則會使用 `AudioPlayer` 類別播放音訊。

![成功辨識後主控台輸出的螢幕擷取畫面](media/sdk/qs-java-jre-08-console-output.png)

## <a name="next-steps"></a>後續步驟

在 GitHub 上可取得其他範例，例如如何讀取音訊檔案中的語音。

> [!div class="nextstepaction"]
> [瀏覽 GitHub 上的 Java 範例](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>另請參閱

- [快速入門：翻譯語音，Java (Windows、Linux)](quickstart-translate-speech-java-jre.md)
- [自訂原音模型](how-to-customize-acoustic-models.md)
- [自訂語言模型](how-to-customize-language-model.md)

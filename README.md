# Audio Recognition Android SDK for File Recognition

## Overview
  [ACRCloud](https://www.acrcloud.com/) provides [Automatic Content Recognition](https://www.acrcloud.com/docs/introduction/automatic-content-recognition/) services for [Audio Fingerprinting](https://www.acrcloud.com/docs/introduction/audio-fingerprinting/) based applications such as **[Audio Recognition](https://www.acrcloud.com/music-recognition)** (supports music, video, ads for both online and offline), **[Broadcast Monitoring](https://www.acrcloud.com/broadcast-monitoring)**, **[Second Screen](https://www.acrcloud.com/second-screen-synchronization)**, **[Copyright Protection](https://www.acrcloud.com/copyright-protection-de-duplication)** and etc.<br>
  
  This **audio recognition Android SDK** support most of audio / video files. 

>>Audio: mp3, wav, m4a, flac, aac, amr, ape, ogg ...<br>
>>Video: mp4, mkv, wmv, flv, ts, avi ...

## Recognize audio while recording
If you want to recognize audio by recording sound through microphone.
Download [Android SDK for Recording](https://ap-console.acrcloud.com/downloads/sdk/android) instead and see [Android Demo](https://www.acrcloud.com/docs/demos/android-demo/).

## Requirements
Follow one of the tutorials to create a project and get your host, access_key and access_secret.

 * [How to identify songs by sound](https://www.acrcloud.com/docs/tutorials/identify-music-by-sound/)
 
 * [How to detect custom audio content by sound](https://www.acrcloud.com/docs/tutorials/identify-audio-custom-content/)
 
## Functions
Introduction all API.
### src/com/acrcloud/utils/ACRCloudRecognizer.java
```java
      public String recognizeByFile(String filePath, int startSeconds)
      /**
      *
      *  recognize by file path of (Audio/Video file)
      *          Audio: mp3, wav, m4a, flac, aac, amr, ape, ogg ...
      *
      *  @param filePath query file path
      *  @param startSeconds skip (startSeconds) seconds from from the beginning of (filePath)
      *
      *  @return result
      *
      **/
    
      public String recognizeByFileBuffer(byte[] fileBuffer, int fileBufferLen, int startSeconds)
      /**
      *
      *  recognize by buffer of (Audio/Video file)
      *          Audio: mp3, wav, m4a, flac, aac, amr, ape, ogg ...
      *
      *  @param fileBuffer query buffer
      *  @param fileBufferLen the length of fileBufferLen
      *  @param startSeconds skip (startSeconds) seconds from from the beginning of fileBuffer
      *
      *  @return result
      *
      **/
    
      public String recognize(byte[] wavAudioBuffer, int wavAudioBufferLen)
      /**
      *
      *  recognize by wav audio buffer(RIFF (little-endian) data, WAVE audio, Microsoft PCM, 16 bit, mono 8000 Hz)
      *
      *  @param wavAudioBuffer query audio buffer
      *  @param wavAudioBufferLen the length of wavAudioBuffer
      *
      *  @return result
      *
      **/
```

### src/com/acrcloud/utils/ACRCloudExtrTool.java 
```java
public static byte[] createFingerprintByFile(String fileName, int startTimeSeconds, int audioLenSeconds, boolean isDB)
      //fileName: Path of input file;
      //startTimeSeconds: Start time of input file, default is 0;
      //audioLenSeconds: Length of audio data you need. if you create recogize frigerprint, default is 12 seconds, if you create db frigerprint, it is not usefully;
      //isDB: If it is True, it will create db frigerprint;
      //@return "ACRCloud Fingerprint". If can not create frigerprint, return null.

public static byte[] createFingerprintByFileBuffer(byte[] dataBuffer, int dataBufferLen, int startTimeSeconds, int audioLenSeconds, boolean isDB)
      //dataBuffer: data buffer of input file;
      //dataBufferLen: length of dataBuffer
      //startTimeSeconds: Start time of input file, default is 0;
      //audioLenSeconds: Length of audio data you need. if you create recogize frigerprint, default is 12 seconds, if you create db frigerprint, it is not usefully;
      //isDB: If it is True, it will create db frigerprint;
      //@return "ACRCloud Fingerprint". If can not create frigerprint, return null.

public static byte[] createFingerprint(byte[] dataBuffer, int dataBufferLen, boolean isDB)
      //dataBuffer: audio data buffer(RIFF (little-endian) data, WAVE audio, Microsoft PCM, 16 bit, mono 8000 Hz);
      //isDB: If it is True, it will create db frigerprint;
      //@return "ACRCloud Fingerprint". If can not create frigerprint, return null.

public static byte[] decodeAudioByFile(String fileName, int startTimeSeconds, int audioLenSeconds) 
      //It will return the audio data(RIFF (little-endian) data, WAVE audio, Microsoft PCM, 16 bit, mono 8000 Hz);
      //fileName: Path of input file;
      //startTimeSeconds: Start time of input file, default is 0;
      //audioLenSeconds: Length of audio data you need, if it is 0, will decode all the audio;
      //@return audio data.

public static byte[] decodeAudioByFileBuffer(byte[] dataBuffer, int dataBufferLen, int startTimeSeconds, int audioLenSeconds)
      //It will return the audio data(RIFF (little-endian) data, WAVE audio, Microsoft PCM, 16 bit, mono 8000 Hz);
      //dataBuffer: data buffer of input file;
      //startTimeSeconds: Start time of input file, default is 0;
      //audioLenSeconds: Length of audio data you need, if it is 0, will decode all the audio;
      //@return audio data.

def version()
      //return the version of this module
```
## Example
Replace "xxxxxxxx" below with your project's host, access_key and access_secret.<br>
```java
import java.io.*;
import java.util.Map;
import java.util.HashMap;

import com.acrcloud.utils.ACRCloudRecognizer;

public class Test {

    public static void main(String[] args) {
        Map<String, Object> config = new HashMap<String, Object>();
        config.put("host", "XXXXXXXX");
        config.put("access_key", "XXXXXXXX");
        config.put("access_secret", "XXXXXXXX");
        config.put("debug", false);
        config.put("timeout", 10); // seconds

        ACRCloudRecognizer re = new ACRCloudRecognizer(config);

        // It will skip 80 seconds.
        String result = re.recognizeByFile(args[0], 80);
        System.out.println(result);
        
        /**
          *
          *  recognize by buffer of (Formatter: Audio/Video)
          *     Audio: mp3, wav, m4a, flac, aac, amr, ape, ogg ...
          *
          *
          **/
        File file = new File(args[0]);
        byte[] buffer = new byte[3 * 1024 * 1024];
        if (!file.exists()) {
            return;
        }
        FileInputStream fin = null;
        int bufferLen = 0;
        try {
            fin = new FileInputStream(file);
            bufferLen = fin.read(buffer, 0, buffer.length);
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                if (fin != null) {
                        fin.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        System.out.println("bufferLen=" + bufferLen);

        if (bufferLen <= 0)
            return;

        // It will skip 80 seconds from the begginning of (buffer).
        result = re.recognizeByFileBuffer(buffer, bufferLen, 80);
        System.out.println(result);
      }
}
```

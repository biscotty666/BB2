up::
tags:: #note/boat🚤 
X:: 

## Video Terminology

-   **Container format**: audio and video tracks are stored in a single file, in a container. The container format does not influence the audio or video quality, it is only a way of storing video and audio in a single file. In Avidemux, the container format is selected in the (Output) Format section at the left.  
    _Note_: Always select an appropriate container format for the file you are saving! See the [Output formats](https://www.avidemux.org/admWiki/doku.php?id=general:output_formats "general:output_formats") chapter for more details. You should also add a suitable extension to the file name when you save the video, like .avi for the AVI container format, or .mkv for the Matroska container format. Avidemux version 2.5 or older does not add the extensions automatically!
    
-   **Video format**: this is the way the video stream is encoded in the file, usually in compressed form. Modern compression fomats usually offer better quality/size ratio than old ones. Common video formats include H.264, MPEG-4 Part 2 or MPEG-2 Part 2.
    
-   **Audio format**: the way the audio stream is stored in the file. Common audio formats include AAC, MP3, MP2, Vorbis or PCM (uncompressed).  
    _Note_: Do not confuse audio and video formats with audio and video codecs. Codecs are tools used for encoding and decoding, while formats are methods of encoding the data. See the [Common myths](https://www.avidemux.org/admWiki/doku.php?id=general:common_myths#software_vs_format_format_vs_codec "general:common_myths") chapter for more details.
    
-   **Encoder**: this is a tool used for encoding the audio or video stream into the desired format. Some encoders are better than others – even if there are multiple encoders for the same format, one of them may offer higher quality at the same size. In Avidemux, you can select the software encoder in the Video and Audio sections. Of course, the selected encoder then implies the output format as well.
    
-   **Decoder**: a tool used for decoding the input video or audio stream. Avidemux uses internal built-in decoders. If it does not have an appropriate decoder for the video or audio format, there will be no video or audio.


---

### References
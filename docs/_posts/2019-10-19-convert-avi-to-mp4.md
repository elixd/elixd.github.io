---
title: "Convert AVI to MP4"
date: "2019-10-19"
---

```
ffmpeg -i input.avi output.mp4                       # Default H.264 codec
ffmpeg -i input.avi -c:v libx265 -c:a aac output.mp4 # Encode video with HEVC and audio with AAC:
```

FFmpeg options:

```
-c:a copy        # copy audio without re-encoding
-c:a aac         # convert audio to AAC
-c:v libaom-av1  # video in AV format
-qscale:v N      # Video Quality: N = 1..31 (1 is highest quality)
-qscale:v N      # Audio Quality: N = 1..31 (1 is highest quality)
-strict -2       # turn on experimental features 
```

#### Batch Convert

Convert entire folder to MP4

```
for i in *.avi; do ffmpeg -i "$i" "${i%.*}.mp4"; done
```

Convert AVI to MP4 - folder with sub-directories:

```
find . -name "*.avi" -exec sh -c 'f="{}"; ffmpeg -i "$f" "${f%.avi}.mp4"' \;
```

#### Choose Audio Track
"map" option is used to map video and aduio stream
```
-map 0:v -map 0:a:1  # Convert only second audio track
<no option>          # only first audio track will be encoded
```
### Links

[https://unix.stackexchange.com/questions/35746/encode-with-ffmpeg-using-avi-to-mp4](https://unix.stackexchange.com/questions/35746/encode-with-ffmpeg-using-avi-to-mp4)

[https://trac.ffmpeg.org/wiki/Encode/H.265](https://trac.ffmpeg.org/wiki/Encode/H.265)

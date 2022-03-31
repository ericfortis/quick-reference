### No Audio Flag
```shell script
-an
```

### Interpolate slow motion
First see the original fps `ffmpeg -i file.mp4`
```shell script
ffmpeg -i file.mp4 -an -filter "minterpolate='fps=120'" out.mp4

# Without scene detection
ffmpeg -i file.mp4 -an -filter "minterpolate='fps=90:scd=none'" out.mp4
```

### Speed up 200
First check the framerate, as it needs to be doubled
```shell script
ffmpeg -i myvideo.mp4
# 60fps
```

```shell script
ffmpeg -i myvideo.mp4 -r 120 -filter:v "setpts=0.5*PTS" video2x.mp4
```


### mkv to mp4
```shell script
for file in *.mkv; do
  ffmpeg -i "$file" -c copy "${file%.mkv}.mp4"
done
```


### mp3 from mp4
```shell script
for file in *.mp4; do
  ffmpeg -i "$file" -q:a 0 -map a "$(basename "${file/.mp4}").mp3";
done;
```

### Remove cover art from mp3s (ffmpeg inplace)
https://stackoverflow.com/a/20202233
https://stackoverflow.com/a/60506254

```shell script
find * -type f -iname \*.mp3 -exec ffmpeg -i {} -vn -codec:a copy -map_metadata -1 {}.tmp.mp3 \; -exec mv {}.tmp.mp3 {} \;
```

In addition, we could check if the mp3 has cover art by:
```shell script
ffmpeg -i myfile.mp3 2>&1  | grep "attached pic"
```


### Change Containers without altering the streams
```shell script
ffmpeg -i input.mp4 -acodec copy -vcodec copy -f mov output.mov
```


## Scene Cuts
### Finding Scene Cuts Timestamps
```shell script
INPUT=Testcut.mp4
THRESHOLD=0.1
OUTPUT=timestamps
ffmpeg -i $INPUT  -filter:v "select='gt(scene,$THRESHOLD)',showinfo"  -f null  - 2> tmpffout
grep showinfo tmpffout | grep 'pts_time:[0-9.]*' -o | grep '[0-9]*\.[0-9]*' -o > $OUTPUT
rm tmpffout
```


### Splitting One
```shell script
INPUT=Testcut.mp4
START=0
END=6.76 # this should be minus one frame
ffmpeg -ss $START -i $INPUT -to $END -c copy file_$START_$END.mp4
```


### Splitting From a list
```shell script
INPUT=Testcut.mp4
prevStamp=0
cat timestamps | while read stamp
do
  ffmpeg -ss $prevStamp -i $INPUT -to $stamp -c copy out_$stamp.mp4
  prevStamp=$stamp
done
```

### Video to frames
```shell
ffmpeg -i video.mp4 out_%05d.png
ffmpeg -i myfile.mp4 -vf fps=24 $filename%03d.jpg
```

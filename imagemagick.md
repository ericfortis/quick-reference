# ImageMagick

## Resizing Images (Conditionally)
```bash
target_width=1280
for f in *.jpg; do
  if [ `identify -format "%w" $f` != $target_width ]; then
    mogrify -resize ${target_width}x -quality 100 $f
  fi
done
```


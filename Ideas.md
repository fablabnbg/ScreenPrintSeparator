# Collection of Ideas   

First we want to collect some ideas what we can do with ImageMagick

### Convert an image to CMYK

```sh
$ convert myimage.tif -colorspace cmyk  -profile ISOcoated_v2_300_eci.icc myimage_cmyk.tif
```

### Modify CMYK channels

Sometimes we want to modify channels

```sh
$ convert myimage_cmyk.jpg -channel CMY -evaluate Add 5% -channel K -evaluate Subtract 10% myimage_cmyk_corr.jpg
```


### Separate CMYK channels

The -negate is necessary to get the correct extraction.

0 = cyan
1 = magenta
2 = yellow
3 = k ('black')

```sh
$ convert myimage_cmyk.jpg -colorspace cmyk -negate -separated myimage_cmyk_extract_%d.jpg
```


#### Dither a grayscale channel

```sh
$ convert myimage_cmyk_extract_0.jpg -ordered-dither o8x8 mystic-forest_cmyk_corr_dither.gif
```

You can find the The different dither patters [here]](https://www.imagemagick.org/Usage/quantize/#thresholds_xml).



#### All in one:

This might take some time (2 minutes on MacBookPro)

```sh
$ convert balloons.jpg  -set option:distort:viewport '%wx%h+0+0' -colorspace CMYK -separate null: \( -size 2x2 xc: \( +clone -negate \) +append \( +clone -negate \) -append \) -virtual-pixel tile -filter gaussian \( +clone -distort SRT 2,60 \) +swap  \( +clone -distort SRT 2,30 \) +swap \( +clone -distort SRT 2,45 \) +swap \( +clone -distort SRT 2,0  -blur 0x0.7 \) +swap +delete -compose Overlay -layers composite -set colorspace CMYK -combine -colorspace RGB offset_balloons.png
```


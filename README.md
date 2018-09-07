# Introduction
This project defines a [Docker](https://www.docker.com) image that contains [OpenImageIO](https://sites.google.com/site/openimageio/home) command line utilities.

# Installation
```
$ docker pull ayoburgess/oiio
```

# Usage
## Using iinfo to inspect exr file metadata
```
$ git clone https://github.com/openexr/openexr-images.git
$ docker run --rm -v ${PWD}:/mnt/${PWD} ayoburgess/oiio iinfo -v /mnt/${PWD}/openexr-images/Chromaticities/Rec709.exr

/mnt//home/ayo/workspace/github/openexr-images/Chromaticities/Rec709.exr :  610 x  406, 3 channel, half openexr
    channel list: R, G, B
    oiio:ColorSpace: "Linear"
    compression: "piz"
    Copyright: "Copyright 2006 Industrial Light & Magic"
    PixelAspectRatio: 1
    screenWindowCenter: 0 0
    screenWindowWidth: 1
```

## Using maketx to generate a tiled mip-mapped exr
```
$ git clone https://github.com/openexr/openexr-images.git
$ docker run --rm -v ${PWD}:/mnt/${PWD} ayoburgess/oiio maketx -v -u --oiio --checknan --filter lanczos3 /mnt/${PWD}/openexr-images/Chromaticities/Rec709.exr -o /mnt/${PWD}/openexr-images/Chromaticities/Rec709.tx

  prep                      0.01s   (10.5 MB)
Reading file: /mnt//home/ayo/workspace/github/openexr-images/Chromaticities/Rec709.exr
  read "/mnt//home/ayo/workspace/github/openexr-images/Chromaticities/Rec709.exr" 0.01s   (14.5 MB)
  misc2                     0.02s   (14.6 MB)
  misc3                     0.00s   (14.7 MB)
  resize & data convert     0.00s   (17.1 MB)
  SHA-1: 7541108BFFD2A7DE91BA4FCBEBD1ABAEDCEA4528
  SHA-1 hash                0.01s   (15.9 MB)
  AverageColor: 0.365261,0.277788,0.115344
  misc4                     0.00s   (15.9 MB)
  Writing file: /mnt//home/ayo/workspace/github/openexr-images/Chromaticities/Rec709.ef2ce43a.temp.tx
  Filter "lanczos3"
  Top level is 610x406
  Mipmapping...
  Downsampling filter "lanczos3" width = 6
    305x203         (16.8 MB)
  Downsampling filter "lanczos3" width = 6
    152x101         (14.0 MB)
  Downsampling filter "lanczos3" width = 6
    76x50           (14.2 MB)
  Downsampling filter "lanczos3" width = 6
    38x25           (14.2 MB)
  Downsampling filter "lanczos3" width = 6
    19x12           (14.2 MB)
  Downsampling filter "lanczos3" width = 6
    9x6             (14.2 MB)
  Downsampling filter "lanczos3" width = 6
    4x3             (14.2 MB)
  Downsampling filter "lanczos3" width = 6
    2x1             (14.2 MB)
  Downsampling filter "lanczos3" width = 6
    1x1             (14.2 MB)
  Wrote file: /mnt//home/ayo/workspace/github/openexr-images/Chromaticities/Rec709.ef2ce43a.temp.tx  (14.2 MB)
maketx run time (seconds):  0.54
  file read:        0.01
  file write:       0.43
  initial resize:   0.00
  hash:             0.01
  mip computation:  0.06
  color convert:    0.00
  unaccounted:      0.03  ( 0.01  0.02  0.00  0.00)
maketx peak memory used: 17.1 MB
```

## Use iinfo again to check the newly created tiled mip-mapped exr file metadata
```
docker run --rm -v ${PWD}:/mnt/${PWD} ayoburgess/oiio iinfo -v /mnt/${PWD}/openexr-images/Chromaticities/Rec709.tx

/mnt//home/ayo/workspace/github/openexr-images/Chromaticities/Rec709.tx :  610 x  406, 3 channel, float tiff
    MIP-map levels: 610x406 305x203 152x101 76x50 38x25 19x12 9x6 4x3 2x1 1x1
    channel list: R, G, B
    tile size: 64 x 64
    oiio:BitsPerSample: 32
    Orientation: 1 (normal)
    Software: "OpenImageIO 1.8.14 : maketx -v -u --oiio --checknan --filter lanczos3 /mnt//home/ayo/workspace/github/openexr-images/Chromaticities/Rec709.exr -o /mnt//home/ayo/workspace/github/openexr-images/Chromaticities/Rec709.tx"
    Copyright: "Copyright 2006 Industrial Light & Magic"
    DateTime: "2018:09:07 16:00:34"
    textureformat: "Plain Texture"
    wrapmodes: "black,black"
    fovcot: 1.50246
    tiff:PhotometricInterpretation: 2
    tiff:PlanarConfiguration: 1
    planarconfig: "contig"
    tiff:Compression: 8
    compression: "zip"
    IPTC:CopyrightNotice: "Copyright 2006 Industrial Light & Magic"
    oiio:AverageColor: "0.365261,0.277788,0.115344"
    oiio:SHA-1: "7541108BFFD2A7DE91BA4FCBEBD1ABAEDCEA4528"
```

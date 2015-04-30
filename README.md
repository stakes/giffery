# giffery
An experiment comparing the results of a bunch of gif creation tools: LICEcap, GIF Brewery, GifGrabber, Gifrocket and a gif-creation workflow using [ffmpeg](https://www.ffmpeg.org/) and [gifsicle](http://www.lcdf.org/gifsicle/).

I screen-grabbed a video of a UI protoype using Quicktime and used five different gif creation tools to generate a gif from the .mov input file, exported from Quicktime at 720p.

The original video file measures 364x720 and is 920 KB. I used each tool's default settings to create gifs from that file, except where noted.

_[OG video file: gif-experiment.mov (920 KB)](/assets/gif-experiment.mov?raw=true)_

#### Spoiler alert

In my opinion, the winners based on a sweet spot of file size and image quality for this particular challenge were:

1) [Alex Sexton's gif creation workflow](https://gist.github.com/SlexAxton/4989674), if you're comfortable in terminal
2) [GIF Brewery](http://gifbrewery.com), if you want a GUI and as long as you're using an adaptive color palette


## [LICEcap](http://www.cockos.com/licecap/)

![LICEcap @ 10FPS (3.1 MB)](/assets/licecap-10fps.gif?raw=true)

__LICEcap @ 10FPS (3.1 MB)__ 
Set "Max FPS" to 10.

![LICEcap @ 24FPS (3.1 MB)](/assets/licecap-24fps.gif?raw=true)

__LICEcap @ 24FPS (4.1 MB)__ 
Cranked "Max FPS" to 24.


## [GIF Brewery](http://gifbrewery.com/)

![GIF Brewery @ 10FPS (3.3 MB)](/assets/gifbrewery-10fps.gif?raw=true)

__GIF Brewery @ 10FPS (3.3 MB)__ I ticked the "Automatically Calculate Count & Delay" box under "GIF Properties" and left it at the default 10 FPS.

![GIF Brewery @ 24FPS/Adaptive Palette (914 KB)](/assets/gifbrewery-10fps.gif?raw=true)

__GIF Brewery @ 24FPS/Adaptive Palette (914 KB)__ I cranked the FPS to 24 FPS and also ticked "Reduce the number of colors in GIF" and selected an Adaptive palette with 256 colors.


## [GifGrabber](http://www.gifgrabber.com/)

![GifGrabber @ default settings (4.6 MB)](/assets/gifgrabber.gif?raw=true)

__GifGrabber @ default settings (4.6 MB)__ Nothing special: just capture the video out playing with Quicktime and made the gif.


### [Gifrocket](http://www.gifrocket.com/)

![Gifrocket @ default settings (2.7 MB)](/assets/gifrocket.gif?raw=true)

__Gifrocket @ default settings (2.7 MB)__ Adjusted settings for output width to match the original's 364x720 dimensions, nothing more.


### [Hardcore G(if) Shit](https://gist.github.com/SlexAxton/4989674)
[Alex Sexton's](https://gist.github.com/SlexAxton) gif creation workflow

![Gifify @ default settings (375 KB)](/assets/gifbrewery-10fps.gif?raw=true)

__Gifify @ default settings (375 KB)__ Simply running `gifify gif-experiment.mov`.

![Gifify with `--good` flag (693 KB)](/assets/gifbrewery-10fps.gif?raw=true)

__Gifify with `--good` flag (693 KB)__ Went in for better quality with `gifify gif-experiment.mov --good`.

I did make one significant, but small change to the original `gifify()` function from [this gist]: removed resizing so it wouldn't rezize the output and would simply output a gif at the same size as the original video file.

```
gifify() {
  if [[ -n "$1" ]]; then
    if [[ $2 == '--good' ]]; then
      ffmpeg -i $1 -r 10 -vcodec png out-static-%05d.png
      time convert -verbose +dither -layers Optimize out-static*.png  GIF:- | gifsicle --colors 128 --delay=5 --loop --optimize=3 --multifile - > $1.gif
      rm out-static*.png
    else
      ffmpeg -i $1 -pix_fmt rgb24 -r 10 -f gif - | gifsicle --optimize=3 --delay=3 > $1.gif
    fi
  else
    echo "proper usage: gifify <input_movie.mov>. You DO need to include extension."
  fi
}
```


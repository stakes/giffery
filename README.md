# giffery
An experiment comparing the results of a bunch of gif creation tools.

I screen-grabbed a video of a UI protoype using Quicktime and used five different gif creation tools to generate a gif from the .mov input file, exported from Quicktime at 720p.

The original video file measures 364x720 and is 920 KB. I used each tool's default settings to create gifs from that file, except where noted.

### [LICEcap](http://www.cockos.com/licecap/)



_First pass:_ Set "Max FPS" to 10.

_Second:_ Cranked "Max FPS" to 24.


### [GIF Brewery](http://gifbrewery.com/)

_First pass:_ I ticked the "Automatically Calculate Count & Delay" box under "GIF Properties" and left it at the default 10 FPS.

_Second go-round:_ I cranked the FPS to 24 FPS and also ticked "Reduce the number of colors in GIF" and selected an Adaptive palette with 256 colors.

### [GifGrabber](http://www.gifgrabber.com/)



Default settings all the way.

### [Gifrocket](http://www.gifrocket.com/)



Adjusted Gifrocket settings for output width, to match the original's 364x720 dimensions.

### [Hardcore G(if) Shit](https://gist.github.com/SlexAxton/4989674)
[SlexAxton's](https://gist.github.com/SlexAxton) gif creation workflow



_First pass:_ just running `gifify gif-experiment.mov`

_Second pass:_ going in for better quality with `gifify gif-experiment.mov --good`

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


# vid2webp

Easily clip videos into animated webp format using ffmpeg. Automatically crops black bars and optionally applies scaling to the result.

## Basic Usage

```bash
$ vid2webp <input_file> [start_time] [end_time] [out_name]
```

No flags, just positional parameters for brevity. To set `end_time`, `start_time` must be provided first. Don't add **.webp** to `out_name`, it will be added automatically. If not provided, `out_name` will be the original file name with .webp ext. If that file exists, it will add `-1`, `-2`, etc to filename incrementally.

So, basic usage looks like this:
```bash
$ vid2webp vidfile.mp4 5 15
$ vid2webp vidfile.mp4 30 45
$ vid2webp vidfile.mp4 54 66
```

Which generates:
```
vidfile.webp
vidfile-1.webp
vidfile-2.webp
```

## Advanced Usage

Several parameters can be set by ENV variable.

```bash
$ [SCALE=1] [FPS=20] [SAMPLE=<start_time>|1] [FRAMES=10] vid2webp <input_file> [start_time] [end_time] [out_name]
```

To scale and convert entire video :
```
$ SCALE=0.5 vid2webp vidfile.mp4
```

## Auto Cropping

This tool automatically crops black bars. This is also useful if clipping a compilation with a mix of portrait and landscape videos. It does this by sampling 10 frames from the start time provided (or 1 second mark if no start time provided). You can override the sample time by ENV variable. Supported formats are the same as for start time parameter. Sample frame count can also be set this way though 10 frames should be fine in basically every case. The first crop value detected will be used so extra frames don't really help, but too few frames may not provide enough data to actually detect the video dimensions.

```bash
$ SAMPLE=90 FRAMES=30 vid2webp vidfile.mp4
```

## Frame Rate

The default frame rate is set at **20fps** to keep the output filesize down but you can set this to any value that you want.

```bash
$ FPS=30 vid2webp vidfile.mp4
```

# Batch processing

Any value set by ENV var persists until changed to make batch processing easier.

```bash
$ FPS=30 SAMPLE=5 vid2webp video.mp4 0 10 # set framerate, override initial sample position
$ vid2webp video.mp4 10 20 # outputs to video-1.webp
$ vid2webp video.mp4 20 30 # outputs to video-2.webp
$ unset SAMPLE # revert to automatic crop detection
$ vid2webp video.mp4 30 40 # continue sequential output:
$ SCALE=1.5 vid2webp video.mp4 40 50 larger # outputs as larger.webp
$ SCALE=0.5 vid2webp video.mp4 40 50 smaller #  output as smaller.webp
$ SCALE=1 vid2webp video.mp4 40 50 # reset scale and continue sequential output
$ vid2webp video.mp4 65.2 75.6  # use millisecond prescion if desired
$ vid2webp video.mp4 "00:07:05" "00:07:25" # or full timestamp notation
```

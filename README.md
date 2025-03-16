# vid2webp

Easily clip videos into animated webp format using ffmpeg. Automatically crops black bars and optionally applies scaling to the result.

## Usage

```bash
$ [SCALE=1] [FPS=20] [SAMPLE=10] vid2webp <input_file> [start_time] [end_time] [out_name]
```

No flags, just positional parameters for brevity. To set `end_time`, `start_time` must be provided first. Set scale and fps by ENV vars. Don't add **.webp** to `out_name`, it will be added automatically. If not provided, `out_name` will be the original filename with .webp ext. If that file exists, it will add `-2`, `-3`, etc to filename incrementally.

So, basic usage looks like this:
```bash
$ vid2webp vidfile.mp4 5 15
$ vid2webp vidfile.mp4 30 45
$ vid2webp vidfile.mp4 54 66
```

Which generates:
```
vidfile.webp
vidfile-2.webp
vidfile-3.webp
```

To scale and convert entire video :
```
$ SCALE=0.5 vid2webp vidfile.mp4
$ SCALE=1.2 vid2webp vidfile.mp4
```

## Auto Cropping

This tool automatically crops black bars. It does this by sampling one frame from 10 seconds into the video. If your clip is less than 10s total or your video has a long intro with black background, you can set the sample time to something different.

```bash
$ SAMPLE=90 vid2webp vidfile.mp4
```

This is also useful if clipping a compilation with a mix of portrait and landscape videos.

```bash
$ SAMPLE=31 vid2webp vidfile.mp4 30 45
$ SAMPLE=47 vid2webp vidfile.mp4 46 52
$ SAMPLE=54 vid2webp vidfile.mp4 53 59
```

## Frame Rate

The default frame rate is set at 20fps to keep the output filesize down but you can set this to any value that you want.

```bash
$ FPS=30 vid2webp vidfile.mp4
```

# Batch processing

Any value set by env var persists until changed to make batch processing easier.

```bash
$ FPS=30 SAMPLE=1 vid2webp video.mp4 0 10 # set framerate for batch and initial sample position
$ vid2webp video.mp4 11 20 # outputs to <input_path>/video.webp
$ vid2webp video.mp4 21 30 # outputs to <input_path>/video-2.webp
$ SAMPLE=31 vid2webp video.mp4 31 40 # set new sample position and continue sequential output
$ vid2webp video.mp4 41 50 # continue with current sample position
$ SCALE=1.5 vid2webp video.mp4 51 60 larger # outputs as <input_path>/larger.webp
$ SCALE=0.5 vid2webp video.mp4 51 60 smaller #  output as <input_path>/smaller.webp
$ SCALE=1 vid2webp video.mp4 51 60 # reset scale and continue sequential output
$ vid2webp video.mp4 65.2 75.6  # use millisecond prescion if desired
$ vid2webp video.mp4 "00:00:05" "00:00:15" # or full timestamp notation
```
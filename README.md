# vid2webp

Easily cut clips from video files into animated webp format.

## Usage

```bash
$ [SCALE=1] [FPS=20] vid2webp <input_file> [start_time] [end_time] [out_name]
```

No flags, just positional parameters for brevity. To set `end_time`, `start_time` must be provided first. Set scale and fps by ENV vars. Don't add **.webp** to `out_name`, it will be added automatically. If not provided, `out_name` will be the original filename with .webp ext. If that file exists, it will add `-2`, `-3`, etc to filename incrementally.

So, basic usage looks like this:
```bash
$ vid2webp vidfile.mp4 "00:00:05" "00:00:15"
$ vid2webp vidfile.mp4 "00:00:30" "00:00:45"
$ vid2webp vidfile.mp4 "00:00:54" "00:01:17"
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
```

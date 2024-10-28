# Large media files used by www.prisonpc.com

These are currently only local copies of YouTube videos used by folks whose
employers block access to YouTube.

The files in `videos/` have been created like so:

1. Download the video with `yt-dlp`:

    ```bash
    yt-dlp --write-subs --write-auto-subs "https://www.youtube.com/watch?v=8VOYS1jJ8a8"
    ```

> [!NOTE]
> That the sub(titles) mentioned above downloads the subs and embeds them
> in the mkv, but these don't get embedded in the mp4 or webm files by
> `ffmpeg`. [Mozilla
> says](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API#examples)
> you can use the vtt file, but I've not tried this.

2. Remove the full video name, leaving only the YouTube ID:

    ```bash
    $ ls
    'PrisonPC Introduction [8VOYS1jJ8a8].en-GB.vtt'  'PrisonPC Introduction [8VOYS1jJ8a8].mkv'
    $ rename 's/.*\[([^\]]*)\]/$1/' *
    $ ls
    8VOYS1jJ8a8.en-GB.vtt  8VOYS1jJ8a8.mkv
    ```

3. Convert video to mp4 (and possible webm):

    ```bash
    for f in *.mkv
    do
        ffmpeg -i "$f" "${f/mkv/mp4}"
        #ffmpeg -i "$f" "${f/mkv/webm}"
    done
    ```

4. Use the [local video short
   code](https://github.com/cyberitsolutions/www.prisonpc.com/blob/main/layouts/shortcodes/local_video.html)
   in the website markdown to reference the video:

    ```markdown
    {{< local_video 8VOYS1jJ8a8 >}}
    ```

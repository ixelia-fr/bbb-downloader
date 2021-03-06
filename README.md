# bbb-downloader
A few scripts for downloading BBB videos


## Downloading BBB data

To download BBB videos and slides, simply pass its URL to the `download_bbb_data.py`:
```
$ ./download_bbb_data.py URL
```

The script downloads the following files:

- `Videos/deskshare.[webm|mp4]`: contains the video of the deskshare

- `Videos/webcam.[webm|mp4]`: contains the video of the webcam with the recorded sound track

- `Slides/`: contains the slides

- `Thumbnails/`: contains the thumbnails


### Adding the sound track to the deskshare video


The recorded sound track is stored in the webcam video, and the
deskshare does not contain any sound. To integrate the sound track to
the deskshare video, run the `integrate_soundtrack.sh` script:

```./integrate_soundtrack.sh Videos [output_file]```


The script creates two files:

- `output.opus`: contains the recorded sound track extracted from `webcam.webm`

- output_file (by default: `output.webm`): contains the video of the deskshare with the sound track.


### Selecting video format

Depending on the configuration of your BBB instance, the recorded
videos will be available on the server directly in `mp4` or `webm`
format. By default, the script will try to download in webm format,
and tries to fallback to mp4 if webm is not available. You can select
the desired format with the `-f webm` or `-f mp4`
option. Unfortunately, there doesn't seem to be a way to auto-detect
which format is available on a prticular server.

In case you downloaded the webm and want mp4, you can convert it using
`webm_to_mp4.sh`:

```$ ./webm_to_mp4.sh output.webm output.mp4```

## Capturing the full playback with the elgalu/selenium Docker image

This will play the recording in a browser running inside a Docker
container, and will capture the video and sound of that browser windows.

See this video for an explanation and demo: 
[<img src="https://i.vimeocdn.com/video/895688106.jpg" width="50%">](https://player.vimeo.com/video/420302036)

See https://github.com/elgalu/docker-selenium for more details on the
docker image that pilots a web browser.

Assembled in a the run.sh script

1. npm install
2. pip3 install progressbar
3. bash capture-full-replay.sh URL

Wait until the full playback is done, and get the resulting MP4 video
in the 'videos/' subdir.

### Cropping a captured video

By default, the script captures a firefox window that displays the BBB stream, you remove the firefox window  by cropping the video with `crop_video.sh`:

```
./crop_video.sh [OPTION] input.mp4 output.mp4
OPTIONS:
   -?                               Show this message
   -s startup_duration              Remove the first startup_duration seconds of the video
   -e stop_duration                 Cut the video after stop_duration (from the start of the input video)
   -m                               Only show the main screen (ie. remove the webcam)

```

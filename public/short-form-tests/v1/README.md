# Mux Shortform HLS tests v1

These streams are intended as part of a proof-of-concept for short-form via HLS

They were generated using ffmpeg, from an fmp4 file prepared for this purpose

## How to generate the streams 

### Create a Source fmp4 file

Making this file is kind of a pain, but you can find a guide [here](https://www.notion.so/mux/Short-MP4-JIT-8e4297d1a7cd4a9d95f5874323c98184#0fc389e52e074fe0975c443f507549c2) if you want to try.

If you want to skip this step, there's a properly-formatted version [on notion](https://www.notion.so/mux/muxed-av-with-sidx-mp4-1bb97a7f89d080e499e4fb2596477a9a?pvs=4)

### Create the HLS stream

```shell
# for example, if the filename is muxed-fragmented-with-sidx.mp4
ffmpeg -i muxed-fragmented-with-sidx.mp4 -hls_segment_type fmp4 -hls_segment_filename '%d.mp4' -hls_list_size 0 -hls_flags append_list media.m3u8
```

### Edit the init segment to include the overall duration

#### Get the overall duration

MP4Box will give you the duration of each track by adding up the movie fragments. The duration from the moov is 0, but the track has a real duration, eg `Fragmented track: 28127 samples - Media Duration 00:10:00.059`

```shell
$ MP4Box -info 2 ~/tmp/shortform-test/original/muxed-fragmented-with-sidx.mp4 

# Track 2 Info - ID 2 - TimeScale 48000
Media Duration 00:00:00.000 
Track flags: Enabled In Movie
Media Language: English (eng)
Media Samples: 0
Fragmented track: 28127 samples - Media Duration 00:10:00.059
Fragment sample defaults: duration 0 size 0 stsd 1 sync 1 padding 0 degradation_priority 0
Handler name: Core Media Audio
Alternate Group ID 1
Sample Description #1 - soun:mp4a
		MPEG-4 Audio AAC LC (AOT=2 implicit) - 2 Channel(s) - SampleRate 48000
	Decoding Buffer size 0 - Bitrate: avg 159 - max 159 kbps
	No stream dependencies for decoding
	StreamPriority 0
	RFC6381 Codec Parameters: mp4a.40.2
	All samples are sync
	Max sample duration: 0 / 48000

Computed info from media:
	Total size 0 bytes - Total samples duration 0 ms (00:00:00.000)

```

#### Edit the init segment to add the duration

If you don't have a favorite way of doing this, try HexFiend

You want to find the 'Duration' in the `mvhd` box. It's 4 bytes long, and starts 16 bytes after the type field (`mvhd`). The timebase is immediately before it, so make sure your duration is in those units. 

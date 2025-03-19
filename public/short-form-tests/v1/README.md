# Mux Shortform HLS tests v1

These streams are intended as part of a proof-of-concept for short-form via HLS

They were generated using ffmpeg, from an fmp4 file with a well-formated global `sidx`.



```shell
# for example
ffmpeg -i muxed-fragmented-with-sidx.mp4 -hls_segment_type fmp4 -hls_segment_filename '%d.mp4' -hls_list_size 0 -hls_flags append_list media.m3u8
```
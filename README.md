![alt text](https://zqwqz.org/assets/img/logo-black.png "Zikuvikuzi")
# Vader's HLS BEST TRANSCODING SETTING FOR FFMPEG
## Native Decode / Native Encode Single Audio (libx264 | AAC)
```bash
ffmpeg -hide_banner -i <input> \
                -map 0:a:0 -map 0:v:0 -map 0:v:0 -map 0:v:0 -map 0:v:0 \
                -c:v libx264 -c:a aac -ac 2 -ar 44100 \
                -filter:v:0 "scale=768:-2" -filter:v:1 "scale=960:-2" -filter:v:2 "scale=1280:-2" -filter:v:3 "scale=1920:-2" \
                -b:a:0 192k \
                -b:v:0 960k -bufsize:v:0 1920k -maxrate:v:0 1200k \
                -b:v:1 2040k -bufsize:v:1 4080k -maxrate:v:1 2400k \
                -b:v:2 3260k -bufsize:v:2 6520k -maxrate:v:2 3400k \
                -b:v:3 4600k -bufsize:v:3 9200k -maxrate:v:3 5200k \
                -coder:v cabac -bf:v 3 -sc_threshold 0 \
                -bsf:v "filter_units=remove_types=6" -bsf:a aac_adtstoasc -rc-lookahead:v 32 -tune film \
                -profile:v:0 main -profile:v:1 main -profile:v:2 high -profile:v:3 high \
                -var_stream_map "a:0,agroup:audio,default:yes,language:ENG,name:1 v:0,agroup:audio,name:432 v:1,agroup:audio,name:540 v:2,agroup:audio,name:720 v:3,agroup:audio,name:1080" \
                -threads 0 -preset fast -g <fps*2> -keyint_min <fps*2> \
                -x264opts "keyint=<fps*2>:min-keyint=<fps*2>:no-scenecut:nal-hrd=vbr" -pix_fmt yuv420p \
                -f hls -hls_playlist_type vod -hls_allow_cache 1 -hls_time 6 \
                -map_metadata -1 -map_chapters -1 -hls_list_size 0 \
                -master_pl_name manifest.m3u8 \
                -hls_segment_filename ts/%v_%05d.ts \
                ts/%v.m3u8 >cikti.log 2>&1
```
[Output Native Single Audio](https://videoseyred.in/embed/93fc4d24f6h4Tx282008UcQqfa9dc063c1)

## Native Decode / Native Encode Multi Audio (libx264 | AAC)
```bash
ffmpeg -hide_banner -i <input> \
                -map 0:a:m:language:eng -map 0:a:m:language:tur -map 0:v:0 -map 0:v:0 -map 0:v:0 -map 0:v:0 \
                -c:v libx264 -c:a aac -ac 2 -ar 44100 \
                -filter:v:0 "scale=768:-2" -filter:v:1 "scale=960:-2" -filter:v:2 "scale=1280:-2" -filter:v:3 "scale=1920:-2" \
                -b:a:0 192k \
                -b:a:1 192k \
                -b:v:0 960k -bufsize:v:0 1920k -maxrate:v:0 1200k \
                -b:v:1 2040k -bufsize:v:1 4080k -maxrate:v:1 2400k \
                -b:v:2 3260k -bufsize:v:2 6520k -maxrate:v:2 3400k \
                -b:v:3 4600k -bufsize:v:3 9200k -maxrate:v:3 5200k \
                -coder:v cabac -bf:v 3 -sc_threshold 0 \
                -bsf:v "filter_units=remove_types=6" -bsf:a aac_adtstoasc -rc-lookahead:v 32 -tune film \
                -profile:v:0 main -profile:v:1 main -profile:v:2 high -profile:v:3 high \
                -var_stream_map "a:0,agroup:audio,default:yes,language:ENG,name:1 a:1,agroup:audio,default:no,language:TUR,name:0 v:0,agroup:audio,name:432 v:1,agroup:audio,name:540 v:2,agroup:audio,name:720 v:3,agroup:audio,name:1080" \
                -threads 0 -preset fast -g <fps*2> -keyint_min <fps*2> \
                -x264opts "keyint=<fps*2>:min-keyint=<fps*2>:no-scenecut:nal-hrd=vbr" -pix_fmt yuv420p \
                -f hls -hls_playlist_type vod -hls_allow_cache 1 -hls_time 6 \
                -map_metadata -1 -map_chapters -1 -hls_list_size 0 \
                -master_pl_name manifest.m3u8 \
                -hls_segment_filename ts/%v_%05d.ts \
                ts/%v.m3u8 >cikti.log 2>&1
```
[Output Native Multi Audio](https://videoseyred.in/embed/9e782811aah4Tx266215UcQq9f86e8bc74)


## Nvidia Cuvid Decode / Nvidia Nvenc Encode / Libfdk_aac Multi Audio (H264_CUVID | H264_NVENC | LIBFDK_AAC)
```bash
ffmpeg -hide_banner -hwaccel cuvid -c:v h264_cuvid -i <input> \
                -map 0:a:m:language:eng -map 0:a:m:language:tur -map 0:v:0 -map 0:v:0 -map 0:v:0 -map 0:v:0 \
                -codec:v h264_nvenc -c:a libfdk_aac -ac 2 -ar 44100 \
                -filter:v:0 "scale_npp=768:-2:interp_algo=super" -filter:v:1 "scale=960:-2:interp_algo=super" -filter:v:2 "scale=1280:-2:interp_algo=super" -filter:v:3 "scale=1920:-2:interp_algo=super" \
                -b:a:0 192k \
                -b:a:1 192k \
                -b:v:0 960k -bufsize:v:0 1920k -maxrate:v:0 1200k \
                -b:v:1 2040k -bufsize:v:1 4080k -maxrate:v:1 2400k \
                -b:v:2 3260k -bufsize:v:2 6520k -maxrate:v:2 3400k \
                -b:v:3 4600k -bufsize:v:3 9200k -maxrate:v:3 5200k \
                -coder:v cabac -bf:v 3 -rc:v vbr_hq -sc_threshold 0 -refs:v 16 \
                -bsf:v "filter_units=remove_types=6" -bsf:a aac_adtstoasc -rc-lookahead:v 32 \
                -profile:v:0 main -profile:v:1 main -profile:v:2 high -profile:v:3 high \
                -var_stream_map "a:0,agroup:audio,default:yes,language:ENG,name:1 a:1,agroup:audio,default:no,language:TUR,name:0 v:0,agroup:audio,name:432 v:1,agroup:audio,name:540 v:2,agroup:audio,name:720 v:3,agroup:audio,name:1080" \
                -preset llhq -g <fps*2> -keyint_min <fps*2> \
                -pix_fmt cuda \
                -f hls -hls_playlist_type vod -hls_allow_cache 1 -hls_time 6 \
                -map_metadata -1 -map_chapters -1 -hls_list_size 0 \
                -master_pl_name manifest.m3u8 \
                -hls_segment_filename ts/%v_%05d.ts \
                ts/%v.m3u8 >cikti.log 2>&1
```
[Output Nvidia Multi Audio 4K Quality](https://videoseyred.in/embed/55dc214198h4Tx269583UcQq2f46d46616)


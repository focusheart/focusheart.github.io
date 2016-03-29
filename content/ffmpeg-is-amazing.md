Title: FFmpeg is amazing! A simple tip for HTML5 use case
Date: 2016-02-29 21:40
Category: development

Video/Audio processing is not easy as far as I know, and the FFmpeg is like the Swiss Knife in my thought. In this case, I've some FLV files stored in Hadoop. I want to make it playable on web page without Flash plugins. The solution is quite simple, using <video> tag in HTML5. The <video> is supported differently in various browser, and I want to support Chrome basicly. So, H.264 encoded mp4 format is the only choice. I've used some simple commands and got mp4 files which were playable when served by Apache using the following code:

    <!doctype html>
    <html>
    <body>
    <h1>Apache Demo</h1>
    <video controls=controls src="http://192.168.1.26/videos/test_yi02.mp4"/>
    </body>
    </html>
    
I've an Apache running on 192.168.1.26, and the above codes work well. However, when I placed the mp4 file on Hadoop and accessed by httpfs, it's not playable and halted when downloaded whole mp4 file. The code is only different in `src` prop:
    
    http://192.168.1.111:50070/webhdfs/v1/test/mp4/test_yi02.mp4?op=OPEN

This URL is accessable through browser. I think there must be something wrong in the mp4 file header or other config. After bing and bing some keywords, I found a solution: add a parameter:

    $ ffmpeg -i input.flv -ar 22050 -movflags faststart output.mp4
   
Yes! The parameter is `-movflags faststart`! After this, the video is playable from Hadoop!
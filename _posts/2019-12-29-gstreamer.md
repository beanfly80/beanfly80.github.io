---
layout: post
title:  "Gstreamer"
categories: dev
date : 2019-12-29 20:09:40 +09:00
---

- GStreamer Open-source Multimedia Framework
    - Multimedia processing library
    - Includes parsing & A/V sync support
    - Provides uniform framework across platforms
    - Modular with flexibility to add new functionality via plugins
    - Easy bindings to other frameworks
    - LGPL License

- Reference
    - gstreamer.freedesktop.org
    - [gstreamer 기본](http://www.aesop.or.kr/index.php?mid=Board_Documents_Android_Frameworks&document_srl=34997)
    - http://4youngpadawans.com/gstreamer-real-life-examples/
    - https://coaxion.net/blog/2014/05/http-adaptive-streaming-with-gstreamer/
    - Sample
        - python code : https://gist.github.com/joar/1381366
        - aom sample : https://aomedia.googlesource.com/aom/+/master/examples/simple_decoder.c
        - Audio Extracter : https://github.com/tuna74/TunaAudioExtracter
        - appsrc : https://gstreamer.freedesktop.org/documentation/application-development/advanced/pipeline-manipulation.html?gi-language=c#appsrc-example


## AVPlayer on web
1. A/V Download
1. Decode (to Raw format) using codec
1. Rendering on Apps - is influenced by Opengl texture performance on graphic chipset.

### codec
A codec is a HW device or SW program that encodes or decodes a data stream or signal.
- HW codec : (Because of using CPU or devices)fast, dependent on device, low compatibility
- SW codec : slower than HW codec
    - ffmpeg/avformat
    - libav/avcodec

## GStreamer Software Stack
- Elements: Sources, filters, sinks
- Pads: Element source / sink connection points
- Caps: Capabilities organized by stream type with a set of properties
- Plugin:Collectionofelements
- Bin: Container for collection of elements
- Pipeline: Top-level bin that allows scheduling and running of all of the elements
- Bus: Message interface that allows asynchronous interaction with an active pipeline

## GStreamer Pipeline Architecture                                            
```
[filesrc (src)] -- [(sink) demuxer

    (src1)] -- [(sink) queue (src)] -- [(sink) decodebin (src)]--[(sink) videosink] 
    (src2)] --  [(sink) queue (src)] -- [(sink) decodebin (src)]--[(sink) audiosink] 
```

- Elements are connected through src/sink pads.
- Data is queued until the maximum specified buffer limit is reached.Element queue will then create a new thread to decouple src/sink processing.
- Post-processing elements, such as color conversion to support various display panels, may be required.
- Parsers can be used to cut streams into buffers. They do not modify the data otherwise.

## GStreamer: Installed Programs
- gst-inspect-1.0 prints information about a GStreamer plugin or element.
- gst-launch-1.0 is a tool that builds and runs basic GStreamer pipelines.
- gst-feedback-1.0 generates debug info for GStreamer bug reports.
- gst-typefind-1.0 uses the GStreamer type finding system to determine the relevant GStreamer plugin to parse or decode a file.

### Debugging options
```
--gst-debug-help prints available debug categories and exit.

--gst-debug-level=LEVEL sets the default debug level, which can range from 0 (no output) to 9(everything).

--gst-debug=LIST takes a comma-separated list of category_name:level pairs to set specific levels for the individual categories. Example:GST_AUTOPLUG:5,avidemux:3. Alternatively, you can also set
the GST_DEBUG environment variable, which has the same effect.

--gst-debug-no-color disables color debugging. You can also set the GST_DEBUG_NO_COLOR environment variable to 1 if you want to disable colored debug output permanently.
NOTE: If you are disabling color purely to avoid messing up your pager output, try using less -R.

--gst-debug-disable disables debugging altogether.

--gst-plugin-spew enables printout of errors while loading GStreamer plugins.
```

## Pipeline Instruction
```
gst-launch-1.0 filesrc location=<source file> ! <demux> demux.video_0 ! queue ! <video decode> ! <posr-processing> ! video-sink demux.audio_0 ! queue ! <audio- decode> ! <audio-sink>
```

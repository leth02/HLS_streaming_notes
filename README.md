# HLS_streaming_notes

### This document contains the concepts of live streaming H.264 video data from an iOS client to a server which then transforms the data into MPEG-TS format suitable for HLS live streaming.

#### What is H.264
H.264 is a codec format that can be packaged into a container format ready to be decoded and displayed by a compatible media player.

#### Packaging of H.264 data
The H.264 spec defines a couple of ways to package H.264 data:
Elementary stream packaging: used in elementary streams, transport streams, etc.
MPEG-4 packaging: used in movie files and MP4 files

Apple’s Core Media and AVFoundation frameworks exclusively want the data packaged in MPEG-4 packaging.


#### H.264 stream
An H.264 stream consists of a sequence of blocks of data packaged in NAL (Network Abstraction Layer) units aka. NALUs.

A NAL unit may contain:
- Sample data: a single frame of video could be packaged in one NALUs, or be spread across several NALUs. They can be of three types: i-frame, p-frame, b-frame
- Parameter sets: which can be of two types 1) Sequence Parameter Sets (SPS) and 2) Picture Parameter Set (PPS). The decoder uses metadata from parameter sets and apply them to the sample data NALUs

#### NAL units in Elementary stream packaging vs MPEG-4 packaging
In Elementary Stream packaging, the parameter sets are included in NALUs right inside the stream. This is suitable for sequential playback.

In MPEG-4 packaging, parameter set NALUs are pulled out to a separate block of data. In Core Media, this block of data is stored in the `CMVideoFormatDescription`. This type of packaging is suitable for random access. It allows the decoder to jump anywhere and start decoding at an i-frame.

Another difference is in the NALU headers. Each NAL Unit in an Elementary Stream will have a three or four bytes `start code` as the header and in MPEG-4 packaging, we have a `length code`, which is the length of the NAL Unit.
Assume that we want to convert a NAL unit from an ES to MPEG-4, for each NAL Unit in the stream, we have to strip off that start code and replace it with the length code.





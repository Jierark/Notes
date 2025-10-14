#TODO finish this after Salsify slides gets posted or I get to the recording

About 70% of peak internet traffic is video, with about 15 billion hours being streamed on Netflix each month. As a result, there is much interest in ensuring that video feeds are delivered in a way that makes the user experience enjoyable. Some qualities of a good user experience are:
- Good quality video (you shouldn't be able to count the number of pixels on screen)
- Little buffering (curse that stupid wheel)
- Able to skip around places in the video
- Fast to start
- And more.
So, how does this work?

# Video Pipeline
The pipeline of the video takes the following path:
- Production - A video gets recorded and produced.
- Encoding - The video file is encoded according to a specific rate and then sent to a server. 
	- This compression is done on multiple levels. it needs to be able to support a wide range of devices, content, network conditions, and other different factors.
	- Additionally, raw video files tend to be massive, especially if they were recorded on high resolutions. Compression allows for some savings on having to send smaller files.
- Deployment - The video file is sent to edge servers for quick transmission to customers.
	- These edge servers are typically known as Content Delivery Networks, and there can be many of these spread across a geographic domain.
		- Netflix has a series of theses CDNs which caches content and sends them to ISPs at no cost. It's a benefit to both parties as both receive more traffic from customers.
- Adapt Quality - The video file is downloaded from the server to the client and played, using the most appropriate quality.

We're going to spend most of this lecture in the "Adapt Quality".

# Framework
- The video is split into chunks (x second pieces of the video), each of which are encoded into different streams.
- The video client selects a bitrate for each chunk. The question is how to choose a bitrate to get a "good" streaming experience?
	- The simplest definition would be good quality.

Suppose there's a buffer which stores parts of the downloaded video file. Every second, 1 second of video is removed and played to the user, and the buffer has some fixed capacity.
- If the buffer occupancy is decreasing, a lower bitrate should be selected so there is always some amount of video to play. Otherwise, rebuffering of the video would occur.
- If the buffer occupancy is increasing, then a higher bit rate should be chosen. The buffer shouldn't be filled completely, but more time can be spent on downloading than is played.
The BBA paper sets low and high thresholds for these behaviors.

Let's look at a sample problem.
![[Pasted image 20251014142616.png]]
The best way to approach this problem is to to discretize by each chunk and look at what happens in each one, perhaps by using a table to organize everything.
- The first chunk downloads at the minimum rate $R_{1}$, and nothing is played because playback starts after the download finishes. There is 5 seconds of video buffer at the end.
- The second chunk also downloads at the minimum rate: it downloads in 1 second and there is 1 second of playback done. Each chunk is 5 seconds of video (as stated), so there are $5 + 5 - 1 = 9$ seconds of video buffer.
- In the third chunk, the bitrate increases to 1 Mb/s, meaning this chunk takes longer to download. It takes $\frac{1 Mb/s * 5s}{2.5 Mb/s} = 2$ seconds to download the 5 second chunk, and an equivalent amount of time is played, exiting the buffer. The buffer now contains $9+5-2=12$ seconds of video to playback.
- Repeat this analysis for each subsequent chunk, choosing larger bitrates when the buffer contains more video to playback.
- Chunk 4: $12 + 5 - \frac{2 * 5}{2.5} = 13$
- Chunk 5: $13 + 5 - \frac{2 * 5}{2.5} = 14$
- Chunk 6: $13 + 5 - \frac{2 * 5}{2.5} = 15$
Formatted as a table:

| Chunk           | 1   | 2   | 3   | 4   | 5   | 6   |
| --------------- | --- | --- | --- | --- | --- | --- |
| Bit Rate (Mb/s) | 0.5 | 0.5 | 1.0 | 2.0 | 2.0 | 2.0 |
| Buffer (sec)    | 5   | 9   | 12  | 13  | 14  | 15  |

While useful, BBA does have a few lingering issues:
- The behavior on startup isn't clear, and short videos might have some efficiency issues
- Other Quality of Experience metrics might be more relevant. BBA doesn't consider them and would need modification to account for them.
- Can capacity estimation still be useful at times?
Video playback is still quite an open problem and new literature is continuously being worked on to this day.

# Real-Time Video
Unlike video streaming, real-time video is a harder problem. Instead of having to upload the video to a server, the video must be recorded, encoded, and sent to other video participants on the fly. This is much more latency-sensitive, so it should be done in a fast manner. This is what the Salsify paper was working on. Current real-time video systems cannot recover fast when network conditions suddenly shift. This is what the users sees as lag/stuttering of video.

# Salsify
There are two control loops that are maintained in Salsify:
- Transport Control - manages the sending of packets in a similar fashion to congestion control that we've seen many times
- Video Codec - Online compression of webcam or whatever medium is being used to record video
#TODO 

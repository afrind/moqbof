# Name: Media Over QUIC (MOQ)
# Description

There are video/media related use-cases that do not appear to be well met by existing protocols. This is a non-WG-forming BOF to discuss these use cases, and determine how to go forward. 

## Issue tracking for this BOF description and agenda

* This BOF description is being maintained in the GitHub repository at https://github.com/afrind/moqbof/.
* Issues to be discussed (and, if possible, resolved) on this BOF description and agenda can be found at https://github.com/afrind/moqbof/issues.
* New issues can be raised in this repository, or on the MOQ mailing list (details below). 
* The BOF co-chairs (details below) welcome discussion on these issues on the MOQ mailing list (details below). You don't need to wait for the BOF to start to express an opinion! 

# BOF Scope

## BoF Goals
The purpose of this BoF is to:

1. Identify media-related use cases with a significant interested community that are not well supported by existing protocols
2. Understand the gaps implementers encounter in using existing protocols to meet the needs of each use case
3. Understand whether these existing protocols could reasonably be extended to meet each use case
4. If not, is there agreement we should forge a new protocol to meet these use cases?

### BoF Non-goals

1. It is possible that after the BoF we will decide that a single protocol will meet all the needs of the targeted use cases, but we shouldn't work too hard on making that happen, during our discussions. 

### Post-BoF Goals

1. If we must extend an existing protocol(s) or forge a new protocol(s) to meet these requirements, determine whether an existing working group(s) or a new working group(s) will be best suited to carry out work on this protocol(s).
2. If this work does not fit cleanly into one IETF area (probably ART or TSV), determine which area will be best-suited for the work. 

## Media Ingest Use Case
Example: live stream broadcast, with later video-on-demand playback.

**Requirements:**
1. Have a mechanism to tune latency/quality tradeoffs. 
2. Have a mechanism to allow bitrate adaptation in the encoders. The adaptation can also be based on latency/quality tradeoffs. 
3. Able to support low latency in the range of 500ms - 1s. 
4. Able to support high quality such as 4K HDR 
5. Able to support new video/audio codecs in the future. 
6. Frame-based, not chunk-based, i.e. frames are sent out immediately as they are available. 
7. Able to support live captions and cuepoints for Ad insertions. 
8. Supports switching networks. (ex. WiFi => cellular) 
9. Support “server migration”: a mechanism that allows clients to continue ingest even if servers on the ingest path need to restart for maintenance/etc.

**Existing Protocols/Challenges:**
1. RTMP: The standard is a bit stagnant and doesn’t have an official way to add new codecs support. It’s TCP based which has the head-of-line blocking issue and can’t use newer congestion control algorithms such as BBR. 
2. RTC/WebRTC: Doesn’t have an easy way to tune latency/quality tradeoffs and the existing protocol sacrifices too much quality for latency which is not suitable for live streams with premium content. Requires SDP exchange between peers which is unnecessary for client server live streaming use cases. 
3. HTTP based protocols (HLS, DASH): Chunk-based protocols incur more latency. These protocols are defined for delivery, but have also been used for ingestion in some cases as well.

## Large-scale Media Playback Use Case
Example: 10k+ viewers watching a live broadcast with different quality networks 

**Requirements:**
1. Same as media ingest. 
2. Browser playback support. 
3. Able to automatically choose the best rendition for playback. (ABR) 
4. Able to drop media during severe congestion/starvation. 
5. Does not rely on encoder feedback. 
6. Minimal handshake RTTs and fast playback start. 
7. CDN support is possible. 
8. Support for timed metadata 
9. Support for media seeking

**Existing Protocols/Challenges:**
1. HTTP based protocols (HLS, DASH): Head-of-line blocking and chunk-based protocols incur more latency. Low latency client-side ABR algorithms are unable to determine if the stream is network bound or encoder bound, limiting their effectiveness. 
2. RTP/WebRTC: 
    1. Focused on real-time latency and doesn’t have a way to tune latency/quality tradeoffs. 
    2. Peer-to-peer support complicates everything: long handshake, blocked by corporate firewalls, no roaming support, port per connection. Frame dropping requires encoder feedback (Full Intra Request) or minimal reference frames (lower quality) otherwise it causes artifacting. 
    3. No b-frame support for higher quality/latency video. 
    4. Cannot easily leverage CDN infrastructure. 
    5. Tight coupling between media framing and transport. 
3. Others: No browser support.

# Required Details
* Status: not WG Forming
* Responsible AD: Murray Kucherawy <superuser@gmail.com>
* BOF proponents: Alan Frindell <afrind@fb.com> and Ying Yin <yingyin@google.com>
* BOF chairs: Magnus Westerlund <magnus.westerlund@ericsson.com> and Alan Frindell <afrind@fb.com>
* Number of people expected to attend: 50
* Length of session (1 or 2 hours): 2 hours
* Conflicts (whole Areas and/or WGs)
* Chair Conflicts: quic, webtransport, masque, ohai
* Technology Overlap: quic, mops, avtcore, mmusic
* Key Participant Conflict: webtransport, masque

# Agenda
1. Administrative Details (5m) 
    1. Note Well 
    2. Note taker, Jabber relay
2. Use cases (20m) 
    1. Present Media Ingest and Playback use cases not well covered by existing protocols 
    2. Discussion of use cases
3. BoF Questions and Discussion (30m) 
    1. Does an existing protocol cover these use cases? 
    2. Can an existing protocol be extended to cover these use cases? 
    3. Should we create a new protocol to cover these use cases? 
    4. Should an existing working group take on this work or should we form a new working group?
4. Wrap Up and Next Steps (5m)

# Links to the mailing list, draft charter if any, relevant Internet-Drafts, etc.
* Mailing List: https://www.ietf.org/mailman/listinfo/moq
* Draft charter: Not WG Forming
* Relevant drafts:
    * Use Cases:
        * https://fiestajetsam.github.io/2021/08/22/quic-video.html
    * Solutions
        * RUSH - https://datatracker.ietf.org/doc/draft-kpugin-rush/
        * WARP - https://www.ietf.org/archive/id/draft-lcurley-warp-00.html, https://www.youtube.com/watch?v=hG0nmy3Otg4

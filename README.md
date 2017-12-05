nrkrip - Rip video streams from nrk.no
======================================

*Note: this utility no longer works properly, as NRK changed their streaming methods a while back. I have no incentives to fix it yet, but leave it here for inspiration*

Command line utility to rip video streams from the Norwegian national
broadcasting service NRK.no.

Video streams are saved as MP4 files, optionally with accompanying subtitles in
SRT format.

Requirements
------------

*   Recent-ish version of ffmpeg binary for best results, available in PATH
*   Perl with modules `WWW::Curl` and `HTML::Parser` installed.

It has only been tested on UNIX-like operating systems (Linux, OSX). I have no
idea how it works on the Windows platform.

It is written in Perl more by coincidence than choice. Expect breakage whenever
NRK changes things around significantly on the video publishing site.

Examples
--------

Fetch a video in foreground, save to auto-generated file name:

    $ nrkrip https://tv.nrk.no/serie/lille-loerdag/FKUN19001096/27-11-1996


Fetch a video in foreground, save to auto-generated parent directory and file:

    $ nrkrip --dir https://tv.nrk.no/serie/lille-loerdag/FKUN19001096/27-11-1996


Fetch videos in parallel and wait for all rips to finish:


    $ nrkrip --dir --wait --async https://tv.nrk.no/serie/lille-loerdag/FKUN19000696/30-10-1996 \
                                  https://tv.nrk.no/serie/lille-loerdag/FKUN19001096/27-11-1996


This generates a common parent directory for the series and puts the episodes in that dir:

    Lille Lørdag/Lille Lørdag - 30.10.1996 S01E06.mp4
    Lille Lørdag/Lille Lørdag - 27.11.1996 S01E10.mp4


Usage
-----
    Use: nrkrip [opts] SOURCE [SOURCE ..]

    SOURCE is typically a playback URL for an episode or program on
    tv.nrk.no/tv.nrksuper.no. It can also be a file with list of such URLs.

    Example:
    $ nrkrip https://tv.nrk.no/serie/lille-loerdag/FKUN19001096/27-11-1996

    You may alternativly provide direct HLS stream URLs, which will be ripped. This
    may be used to rip from sources other than NRK.

    By default, output files are saved to current directory, possibly with a
    generated parent directory based on title.

    --meta, -m
      Only show extracted metadata from URL, do not actually rip video
      stream or write any files. Can be used to test metadata extraction.

    --async, -a
      Enable async mode - stream rips are executed in parallel as background jobs.

    --wait, -w
      When using async mode, wait for all background jobs before exiting,
      and report their status as they finish.

    --out FILE, -o FILE
      Optionally set output FILE. (Normally, a suitable file name is generated.)
      This option is not compatible with multiple URLs.

    --jobs, -j
      Max number of simultaneous background jobs (integer >= 1).
      Default is unlimited. Only applicable when using '--wait'.

    --dir, -d
      Always use a title-based directory as parent of output files, even
      though it does not already exist (will be created on demand).

    --subs, -s
      Experimental: try to fetch and convert subtitles. If successful, the subtitles
      will be saved in ".srt" format using the same basename as the output file.

    --yes, -y
      Always overwrite output files. Useful especially when running in async mode.

    --progress LOGFILE|-, -p LOGFILE|-
      Analyze ffmpeg progress based on contents of LOGFILE and print a numeric
      percentage between 0.0 and 100.0, then exit. This can be used to periodically
      check progress of async jobs. If LOGFILE is "-", then input is continually
      read from STDIN and progress continually printed to STDOUT.

    --version, -v
      Show versions of script, Perl and ffmpeg.

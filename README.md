# dlna-media-archive

Archive and access material from DLNA media servers

These tools are for use with any DLNA media server, accessible directly on
the same LAN or indirectly using `ssdp-fake`.

There are three tools: `dlna-mediaserver-walk`, `dlna-media-archive` and `dlna-playlist-play`.

## `dlna-mediaserver-walk`

This tool displays the names and content tree of all the DLNA media servers it
can see.

## `dlna-media-archive`

This tool downloads all the content from all the DLNA media servers it can see.
A command line argument can specify a server name to match (using wildcards).

The files are stored as a directory tree (default) or all in one directory (--flat).
If a tree is used, top level directory is the name the media server advertises.
Subdirectories correspond to containers in the content tree.

Files are not redownloaded if they already exist.

The `--delete` option changes the archiving to mirroring as content is removed if
it is no longer present on the server.

### Usage

```
  dlna-media-archive [options] [<server-name>]

  <server-name> - string with optional wildcards (file glob style, not regex) to
                  match against the name advertised by the server

Options:

  -D, --delete			Delete files in destination tree not found on server
  -d, --destination=<path>	Destination directory for creating archive, default is current directory
  -f, --flat			Store all files in one directory, not hierarchically
  -v, --verbose[=N]		Increment or set verbosity level (0 - quiet, 1 - info, 2 - progress, 3 - debug)

Example:

  dlna-media-archive "HDR-2000T*" --delete --destination /nas/archive/humax/
```

Note: `dlna-mediaserver-walk` can be used to display the names of all the
visible servers.

## `dlna-playlist-play`

This tool is a simple media player which plays a container (and its nested containers) in the order specified by the server.
It is most useful to play a playlist stored on the server as a container, although it can play a single item or album if desired.

In normal use, it plays the files by passing the URL to a local command (defaults to `mplayer`).
Alternatively, it can create a playlist of the URLs of the media files, which can later be passed to another
media player (for example, `vlc`).

The tool can keep track of where in the playlist it has got to so that playing can be resumed later.
This is only on a per-track basis and playing restarts at the start of the track which was being played
when it was stopped.

The tool is mainly intended for playing very long playlists (hundreds or thousands of tracks),
normally intended to be played over multiple days or even longer, and which are likely to be interrupted
for many reasons (network connection loss, system shutdown, etc).

### Usage
```
  dlna-playlist-play [options] <server-name> <object-id>|<list-name>|<list-path>

  dlna-playlist-play -?|--help|--man|--version

Commands:

  -?, --help			brief help message
  --man 			full documentation
  --version			script version

Options:

  -e, --execute <command>       Command to execute. The URL for the content will be provided as the only argument.
  -L, --log <log-file>          Log which tracks were played when.
  -l, --append-log <log-file>   Same as --log but append to log file.
  -n, --dry-run			Do not actually play files or create playlist, just print messages
  -o, --output <file>           Do not execute commands, write the commands (or playlist) to the named file
  -p, --playlist                Do not issue commands, just write the URLs as playlist.
  -R, --resume[=<file>]         Restart playing with the item interrupted last time.
  -S, --save[=<file>]           Save the id of the object being played to allow resume next time.
  -v, --verbose[=N]		Increment or set verbosity level (0 - quiet, 1 - info, 2 - progress, 3 - debug)

Examples:

  dlna-playlist-play Gerbera christmas-playlist.m3u --save --resume

  dlna-playlist-play "Nas*" /Videos --playlist --output all-my-videos.m3u

```

## `dlna-play`

This instructs a DLNA Media Renderer to play a single media file. It is intended
to be used as the `execute` command for `dlna-playlist-play` although it is standalone
and can be used as an ordinary media player command as well.

The command instructs the media renderer to play the file, then monitors the playing
and exits when the state goes to "STOPPED".

### Usage
```
NAME
    dlna-play - Play a single URL on a DLNA media renderer

SYNOPSIS
    dlna-play [options] [<renderer-name>] <url>

    dlna-play -?|--help|--man|--version

     Commands:

      -?, --help                    brief help message
      --man                         full documentation
      --version                     script version

     Options:

      -w, --wait=N                  Seconds to wait for renderer to be found (default = 3)
      -l, --location=<url>          URL providing description XML for a renderer to include in the search
      -S, --stop                    Stop playback but do not start another track
      -n, --dry-run                 Do not actually play file, just monitor
      -v, --verbose[=N]             Increment or set verbosity level (0 - quiet, 1 - info, 2 - progress, 3 - debug)

OPTIONS
    --wait  Number of seconds to wait searching for all renderers visible on
            LAN. Specifying 0 disables the LAN search meaning only renderers
            specified using --location will be considered.

    --location
            URL of the "description XML" for a renderer to include in the
            search. This is used to access renderers which are not visible
            on the LAN (typically accessed over a wide area network).

            The value to be provided is the same as the LOCATION: field in
            the DLNA announcement messages the renderer transmits on its own
            LAN.

            For example:

              --location http://192.168.111.222:49152/uuid-12345678-abcd-0987-ffff-1234567890abc/description.xml

            If local renderers should not also be considered, specify
            '--wait 0' as well. See also the description below about how
            renderer names interact with --location.

    --stop  Stop playback of the current track but do not start another.
            Note that any track URL is ignored: this is to make it easy to
            recall an earlier playback command but add "--stop" to just stop
            the playback.

    --dry-run
            Do not actually play file.

    --help  Print a brief help message and exits.

    --man   Prints the manual page and exits.

DESCRIPTION
    dlna-play plays a single content file on a DLNA UPNP AV media renderer
    and waits for it to complete. It is designed to be used much like
    "mplayer" is used.

    renderer-name specifies the name of the media renderer (the name is
    advertised by the renderer itself). File glob style wildcards can be
    used. If the renderer name is omitted, the first renderer found will be
    used.

    Note that renderer name matching is still checked even if --location is
    specified. In most cases, when --location is used the renderer name
    should be omitted and --wait 0 specified to avoid finding a local
    renderer

    url is the file to play.

EXAMPLES
    dlna-play "HDR-2000T*" <url>
            Play the url on a Humax HDR-2000T.

    dlna-play <url>
            Play the url on any renderer we can see on the network.

    dlna-play --wait 0 --location=http://192.168.111.222:4321/desc <url>
            Play the url on the renderer described at the specified location
            whatever name it may be advertising.

```

## TODO

`dlna-media-archive` must be more controllable:

1. Specify subtrees of the media server content to archive.

2. Insist on media server name and provide option to list names of all servers
visible.

## Requirements

All tools require the Perl modules: `Net::UPnP::ControlPoint` and
`Net::UPnP::AV::MediaServer`.

## Credits

Much of the code is taken from the documentation for Net::UPnP::AV::MediaServer, by Satoshi Konno

## Notices
Copyright (c) 2014-2018 Graham R. Cobb.
This software is distributed under the GPL (see the copyright notices and the LICENSE file).

`dlna-media-archive` is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or
(at your option) any later version.

`dlna-media-archive` is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

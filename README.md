# dlna-media-archive

Archive material from DLNA media servers

These tools are for use with any DLNA media server, accessible directly on
the same LAN or indirectly using `ssdp-fake`.

There are two tools: `dlna-mediaserver-walk` and `dlna-media-archive`.

## `dlna-mediaserver-walk`

This tool displays the names and content tree of all the DLNA media servers it
can see.

## `dlna-media-archive`

This tool downloads all the content from all the DLNA media servers it can see.
A command line argument can specify a server name to match (using wildcards).

The files are stored in a directory tree within the current directory.
The top level directory is the name the media server advertises. Subdirectories
correspond to containers in the content tree.

Files are not redownloaded if they already exist.

### Usage

```
  dlna-media-archive [options] [<server-name>]

  <server-name> - string with optional wildcards (file glob style, not regex) to
                  match against the name advertised by the server

Options:

  -D, --delete			Delete files in destination tree not found on server
  -d, --destination=<path>	Destination directory for creating archive tree
  -v, --verbose[=N]		Increment or set verbosity level (0 - quiet, 1 - info, 2 - progress, 3 - debug)

Example:

  dlna-media-archive "HDR-2000T*" --delete --destination /nas/archive/humax/
```

Note: `dlna-mediaserver-walk` can be used to display the names of all the
visible servers.

## TODO

`dlna-media-archive` must be more controllable:

1. Specify subtrees of the media server content to archive.

2. Insist on media server name and provide option to list names of all servers
visible.

## Requirements

Both tools require the Perl modules: `Net::UPnP::ControlPoint` and
`Net::UPnP::AV::MediaServer`.

## Credits

Much of the code is taken from the documentation for Net::UPnP::AV::MediaServer, by Satoshi Konno

## Notices
Copyright (c) 2014 Graham R. Cobb.
This software is distributed under the GPL (see the copyright notices and the LICENSE file).

`dlna-media-archive` is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or
(at your option) any later version.

`dlna-media-archive` is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

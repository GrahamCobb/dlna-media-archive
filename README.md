# dlna-media-archive

Archive material from DLNA media servers

These tools are for use with any DLNA media server, accessible directly on
the same LAN or indirectly using `ssdp-fake`.

There are two tools: `dlna-mediaserver-walk` and `dlna-media-archive`.

## `dlna-mediaserver-walk`

This tool displays the content tree of all the DLNA media servers it can see.

## `dlna-media-archive`

This tool downloads all the content from all the DLNA media servers it can see.

The files are stored in a directory tree within the current directory.
The top level directory is the name the media server advertises. Subdirectories
correspond to containers in the content tree.

Files are not redownloaded if they already exist.

## TODO

`dlna-media-archive` should be more controllable:

1. Specify the particular media server to archive.

2. Specify subtrees of the media server content to archive.

3. Specify the local directory to use as the top level.

## Requirements

Both tools require the Perl packages: `Net::UPnP::ControlPoint` and `Net::UPnP::AV::MediaServer`.

## Credits

Much of the code is taken from the documentation for Net::UPnP::AV::MediaServer, by Satoshi Konno

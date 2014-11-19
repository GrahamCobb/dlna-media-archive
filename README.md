# dlna-media-archive

Archive material from DLNA media servers

These tools are for use with any DLNA media server, accessible directly on
the same LAN or indirectly using `ssdp-fake`.

There are two tools: `dlna-mediaserver-walk` and `dlna-media-archive`.

## `dlna-mediaserver-walk`

This tool displays the content tree of all the DLNA media servers it can see.

## `dlna-media-archive`

This tool downloads all the content from all the DLNA media servers it can see.
A command line argument can specify a server name to match (using wildcards).

The files are stored in a directory tree within the current directory.
The top level directory is the name the media server advertises. Subdirectories
correspond to containers in the content tree.

Files are not redownloaded if they already exist.

### Usage

```
  dlna-media-archive [<server-name>]

  <server-name> - string with optional wildcards (file glob style, not regex) to
                  match against the name advertised by the server

Example:

  dlna-media-archive "HDR-2000T*"
```

## TODO

`dlna-media-archive` must be more controllable:

1. Specify the local directory to use as the top level.

2. Specify subtrees of the media server content to archive.

3. Optional deletion of files which have gone from the server.

## Requirements

Both tools require the Perl packages: `Net::UPnP::ControlPoint` and `Net::UPnP::AV::MediaServer`.

## Credits

Much of the code is taken from the documentation for Net::UPnP::AV::MediaServer, by Satoshi Konno

# dlna-media-archive

Archive material from DLNA media servers

These tools are for use with any DLNA media server, accessible directly on
the same LAN or indirectly using `ssdp-fake`.

There are two tools: `dlna-mediaserver-walk` and `dlna-media-archive`.

## `dlna-mediaserver-walk`

This tool displays the content tree of all the DLNA media servers it can see.

## Requirements

Both tools require the Perl packages: `Net::UPnP::ControlPoint` and `Net::UPnP::AV::MediaServer`.

## Credits

Much of the code is taken from the documentation for Net::UPnP::AV::MediaServer, by Satoshi Konno

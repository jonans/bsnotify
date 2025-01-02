# BSnotify

Use Bambu Studio or Orca Slicer with printers outside your LAN.

Bambu Lab printers have a LAN only mode to avoid relying on cloud services for general tasks.
One limitation is that Bambu Studio can only find your printer when both are on the same LAN.
Discovery happens using SSDP but due non-standard/broken implementation it does not work when
they are on different LANs. Since BS lacks the simple ability to add printer via IP, 
SSDP is the only way.

Thats where bsnotify comes in, it will notify BS of printers outside your LAN.

Tested on Linux and Windows with P1P. 
Tested on MacOS and Linux with X1C.

## Requirements

- Python 3
- Printer set to LAN only mode
- Info from your printer:
  - IP address
  - Serial Number
  - Access Code
- Run bsnotify on the same LAN as your slicer (the device running Bambu Studio).


```
python3 bsnotify <printer-ip> <printer-serial-number>
```

Optional:
Specify the source address(es) as comma separated values. This will allow for broadcasting on specific network(s) from a server or router (such as a Unifi Dream Machine) that has multiple interfaces.

```
python3 bsnotify <printer-ip> <printer-serial-number> <local address(es)>
```

## The Details

- Printer SSDP server does not seem to respond to SEARCH requests in any format.
- Printer periodically sends out the following NOTIFYs :
  - to 255.255.255.255:2021  - BS responds to this
  - to 239.255.255.250:1990  - BS ignores this
- BS sends out SEARCH requests but never gets a reply.
- This script spoofs the multicast response from the printer but on the network that you're using for Bambu Studio. Even if you add the device manually, Bambu Studio still expects this multicast packet.


# Packet Capture

## Packet Capture

### Using CLI

To run a packet capture through CLI, you can use:

```
diagnose sniffer packet INTERFACE 'BPF_FILTER' [1-6] MAX_PACKET_COUNT {a|l} [FRAME_SIZE]

# INTERFACE = interface name. Can be 'any'
# BPF_FILTER = fiter captured packets using a BPF filter similar to tcpdump 
# 1-6 = verbosity level. Default: 1
# MAX_PACKET_COUNT = capture stops after MAX_PACKET_COUNT packets
# a = absolute timestamp. l = local time-zone timestamp
# FRAME_SIZE = Size of the captured frame. It defaults to MTU of the interface or 1600 for 'any'
```

Verbosity level map:



<table><thead><tr><th width="85">Level</th><th>Interface Name</th><th data-hidden width="118">L3 Headers</th><th data-hidden>L3 Payload</th><th data-hidden>L2 Headers</th></tr></thead><tbody><tr><td>1</td><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2714">✔</span></td><td></td><td></td></tr><tr><td>2</td><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2714">✔</span></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2714">✔</span></td><td></td></tr><tr><td>3*</td><td></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2714">✔</span></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2714">✔</span></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2714">✔</span></td></tr><tr><td>4</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2714">✔</span></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2714">✔</span></td><td></td><td></td></tr><tr><td>5</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2714">✔</span></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2714">✔</span></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2714">✔</span></td><td></td></tr><tr><td>6*</td><td><span data-gb-custom-inline data-tag="emoji" data-code="2714">✔</span></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2714">✔</span></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2714">✔</span></td><td><span data-gb-custom-inline data-tag="emoji" data-code="2714">✔</span></td></tr></tbody></table>

\*3 and 6 can be converted to pcap format

Packet captures can be stopped using `Ctrl+C`

### Using GUI

The GUI packet capture is only available for models with internal storage.

## Packet Flow Trace

```
diagnose debug enable
diagnose debug flow filter addr <IP>
diagnose debug flow trace start 10
```

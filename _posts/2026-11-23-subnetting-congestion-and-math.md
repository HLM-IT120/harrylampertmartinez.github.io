## Subnetting for Congestion Control and IPv4 Math

** My Objective **
This week, my goal was to connect networking concepts and subnetting math through my Divide & Conquer questions. I learned how subnet masks help relieve network congestion on a segment and how the math works when a question shifts into calculation—especially on power-of-two block boundaries.

** The Hurdle **
At first, subnetting felt split between big-picture concepts and number crunching. On one side, you have why subnets exist—like separating smart home gadgets or guest devices from personal computers and phones, and keeping broadcast traffic from flooding every device at once. On the other side, when a problem asks for the Network IP, Broadcast IP, First Host IP, Last Host IP, Next Network IP, and usable host count on `10.1.1.48/27`, or asks why every group size lands on `128`, it becomes much more math-heavy.

** The Solution **
Breaking the work down into traffic flow, core IPv4 rules, and step-by-step block math made everything tolerable:

* ** Relieving Network Congestion: ** Splitting a larger network into smaller subnets with a subnet mask keeps broadcast traffic contained inside each local group instead of flooding every device across the entire network.
  ** Why did this work? ** When hosts share one wide broadcast domain, every device has to process broadcast chatter even when it is not meant for them. Isolating groups into smaller subnets limits broadcast reach and lets routers control traffic moving between segments.
* ** Core Address Math & Reserved IPs: ** An 8-bit octet holds 256 values (`0` to `255`), and we subtract 2 (`256 - 2 = 254` on a `/24`) because the first address identifies the network itself (so routers can route traffic and admins can set group rules) and the last address is the broadcast address.
  ** Why did this work? ** Reserving the network ID and broadcast address keeps subnet boundaries clear and prevents assigning a host IP to an address meant to represent the whole network or every host in it.
* ** Question #5 & Power-of-Two Block Math: ** For `10.1.1.48/27`, `/27` leaves `5` host bits (`32 - 27 = 5`), giving a group size of `32` (`2^5`) and subnet mask `255.255.255.224`. Counting by `32` from `0` gives `0, 32, 64`, placing `.48` in `.32`–`.63` with Network IP `10.1.1.32`, Broadcast IP `10.1.1.63`, First Host IP `10.1.1.33`, Last Host IP `10.1.1.62`, Next Network IP `10.1.1.64`, and `30` usable addresses (`32 - 2`).
  ** Why did this work? ** Every subnet group size is a power of two (`1, 2, 4, 8, 16, 32, 64, 128`), and every power of two up to `128` (`2^7`) divides evenly into `128`, so stepping up from `0` by your group size reliably hits `128` and maps directly to each block boundary:

```text
CIDR /27 -> 32 - 27 = 5 host bits -> Group size = 2^5 = 32 (Mask: 255.255.255.224)
Block sequence: 0, 32, 64 ... (128)
Target host .48 falls between .32 and .64:
- Network IP:        10.1.1.32
- First Host IP:     10.1.1.33
- Last Host IP:      10.1.1.62
- Broadcast IP:      10.1.1.63
- Next Network IP:   10.1.1.64
- Usable addresses:  32 - 2 = 30
```
* ** Resolving Post-Move BIOS, Display & Boot-Drop Issues: ** After moving with my GPU out, adjusting BIOS settings alone didn't fix the missing boot drive. Putting the GPU back in got into Windows 11, but opening tools like Task Manager or using certain keys didn't respond right and the PC would drop back to BIOS after a few minutes, claiming no boot drive was detected—until I moved the HDMI cable from the motherboard port to a GPU port, which stabilized it. I'm also tracking slow Wi-Fi speeds (mostly kbps and peaking around 144 Mbps on a 1 Gbps connection) to discuss with the owner after Thursday.
  ** Why did this work? ** Routing display output through the GPU port instead of the motherboard port aligned display/PCIe handling with the installed GPU so Windows 11 stayed stable rather than flaking out or dropping back to BIOS.

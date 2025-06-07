Port scanning tool. Gives insight into what ports are open, and what services may be running on each.

Requires sudo privileges for full functionality of scanner.

- -sU does scans for UDP ports
- -F only scans the most common 100 ports, by default nmap scans the most common 1000
- -O attempts to discern OS information, though it won't usually be specific, and-sV checks the versions of applications listening on the different ports
	- use -A to combine both of these
- -T0/1/2/3/4/5 changes how fast or aggressive the scanning speed runs, with T0 being the slowest and T5 being the fastest. Useful to slow down scans to avoid tripping IDS systems.
- -v/vv/vvvv increase verbosity of output. Press v during a scan to dynamically increase verbosity
- -d\[0-9\] adds debugging output, increasing in details.
- -oN/-oX/-oG/-oA writes results to various formats: normal (human-friendly), XML, grep-able, or all 3 if desired
- 
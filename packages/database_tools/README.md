# database_tools

Pure AX database connection-string helpers.

This package deliberately does not open sockets or call native database drivers.
It models the first safe prerequisite layer for future database packages:
classifying DSNs, checking whether required fields are present, and reporting
whether the URL shape belongs to a protocol AX can reasonably target later.

Runtime database IO should wait for TCP/TLS/byte-buffer and native runtime ABI
support.

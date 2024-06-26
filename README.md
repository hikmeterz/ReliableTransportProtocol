﻿# ReliableTransportProtocol
## Project Description
This repository contains Python programs implementing a reliable transport protocol (RTP) on top of UDP. The programs include a sender and a receiver that ensure in-order, reliable delivery of UDP datagrams despite packet loss, delay, corruption, duplication, and re-ordering. The project is divided into two parts: RTP-base and RTP-opt.

## Files
- `sender.py`: Python program implementing the RTP sender.
- `receiver.py`: Python program implementing the RTP receiver.
- `util.py`: Utility functions for calculating checksums and other helper functions.
- `input.txt`, `output.txt`: Sample input and output files for testing.
- `rapor.pdf`: Project report describing the implementation and testing of the RTP protocol.

## RTP Specification
RTP sends data in the format of a header followed by a chunk of data. The header includes the following fields:
- `int type`: Packet type (0: START, 1: END, 2: DATA, 3: ACK)
- `int seq_num`: Sequence number
- `int length`: Length of data (0 for ACK, START, and END packets)
- `int checksum`: 32-bit CRC checksum

### Packet Size
The maximum size of RTP packets is limited to 1472 bytes to accommodate the UDP and IP headers within the Ethernet frame size of 1500 bytes.

## Part 1: RTP-Base Implementation

### `sender.py`
This program reads an input message and transmits it to the receiver using UDP sockets following the RTP protocol. It splits the input message into appropriately sized chunks of data, appends a checksum to each packet, and uses a sliding window mechanism for reliable transport.

#### Key Features:
- Sends a START message with a random sequence number and waits for an ACK.
- Transmits data packets with increasing sequence numbers.
- Implements a sliding window mechanism with a specified window size.
- Retransmits packets if ACKs are not received within 500 milliseconds.
- Sends an END message to terminate the connection.

#### Example Usage:

python sender.py 127.0.0.1 1234 3 < input.txt

### `receiver.py`
This program receives data packets from the sender, verifies checksums, and sends cumulative ACKs. It ensures reliable data transfer by handling packet loss, re-ordering, and duplication.

Key Features:
Listens for incoming packets on a specified port.
Verifies checksums and drops corrupted packets.
Sends cumulative ACKs for received packets.
Buffers out-of-order packets and sends the expected sequence number.
Example Usage:

python receiver.py 1234 3 > output.txt

## Part 2: RTP-Opt Implementation
### 'sender.py'
This optimized version of the sender program reduces unnecessary retransmissions by maintaining individual timers for each packet and only retransmitting packets that have not been acknowledged.

Key Features:
Sends a START message with a random sequence number and waits for an ACK.
Transmits data packets with increasing sequence numbers.
Implements a sliding window mechanism with individual timers for each packet.
Retransmits only the packets that have not been acknowledged.
Sends an END message to terminate the connection.
Example Usage:

python sender.py 127.0.0.1 1234 3 < input.txt
### 'receiver.py'
This optimized version of the receiver program sends ACKs for each packet individually, rather than cumulatively. It ensures reliable data transfer by handling packet loss, re-ordering, and duplication.

Key Features:
Listens for incoming packets on a specified port.
Verifies checksums and drops corrupted packets.
Sends individual ACKs for each received packet.
Buffers out-of-order packets and sends the expected sequence number.
Example Usage:
python receiver.py 1234 3 > output.txt
### Utilities
### util.py
This file contains utility functions for calculating checksums and other helper functions needed for the RTP protocol.

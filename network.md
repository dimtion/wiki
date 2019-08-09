# Network

## SCTP

SCTP (Stream Control Transmission Protocol) is a transport layer protocol (L4)
like UDP and TCP. It is messaged based (like TCP) but ensure reliability and
congestion control (like TCP). It also has the multi-homing feature and
multi-path to increate reliability and resilience.

It is implemented in Linux since 2.4 and FreeBSD since version 7. There is no
official driver force Microsoft Windows and MacOS, only third parties
implementations.

Sources:

* https://en.wikipedia.org/wiki/Stream_Control_Transmission_Protocol#Motivations_and_adoption


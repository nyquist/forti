# DoS Policy

Types of DoS Attacks:

* TCP SYN flood
* ICMP sweep
* TCP Port scan
* Distributed DoS (DDos)

DoS policy defines a number of thresholds and associated action to take when they are reached.

Multiple DoS policies can be applied to an interface. DoS protection supports TCP, UDP, ICMP and SCTP. Multiple sensors are involved:

* **Flood sensor**: detects a high volume of traffic for the specified protocol or signal in the protocol
* **Sweep/scan**: detects probing attempts
* **Source signatures**: detect large volumes of traffic originating from a single IP
* **Destination signatures**: detect large volumes of traffic destined to a single IP

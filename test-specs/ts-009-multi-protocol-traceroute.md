# Specification version number

2013-10-07-000

# Specification name

Multi Protocol Traceroute test

# Test preconditions

  * An internet connection.
  * Ability to run ooni-probe as root or with raw sockets capabilities

For reporting to the backend to work that it is possible for the probe to
establish a connection to the Tor network.

# Expected impact

  * Ability to determine differences in network paths of
    different protocols and ports


# Expected inputs

  * No additional inputs are required as the destination host
    (backend) defaults to 8.8.8.8. The backend can optionally
    be specified with argument -b.

  * Additional parameters such as source port, maximum TTL,
    and timeout values can also be set


## Semantics

  * The backend host is specified as a dotted-quad IPv4
    address.

  * Maximum TTL defaults to 30.

# Test description

A series of packets are constructed and sent to the backend
host with incrementing TTL values (traceroute).

See also: https://tools.ietf.org/rfc/rfc792.txt

For protocols UDP and TCP, a traceroute is performed for each
of the following ports: 0, 22, 23, 53, 80, 123, 443, 8080,
65535.

For protocol ICMP, there is only a single series of packets
as there is no concept of ports.

The time-exceeded ICMP responses are added to the report. 

# Expected output

The collected ICMP time-exceeded responses from IP routers
that properly implement RFC792 in the forward path towards
the backend host. 

## Parent data format

df-003-scapyt

## Semantics

The report object will contain keys of the format "hops_" +
port, e.g. report['hops_22']. The corresponding value is an
ordered list of the parsed responses, encoded like so:

{'ttl': The TTL of the packet sent,
 'address', The address of the remote host at which the TTL expired,
 'rtt', the round-trip-time, measured as the difference between the received response and sent packet time.
 'sport', the source port of the sent packet
}

## Possible conclusions

By examining the differences in the responses between protocols and destination ports, it is possible to ascertain that routing decisions are made on a protocol layer above IP, and determine what path was taken for different protocols.

## Example output sample

```
###########################################
# OONI Probe Report for multi_protocol_traceroute_test (0.2)
# Thu Jan  2 16:54:10 2014
###########################################
---
input_hashes: []
options: [-b, 8.8.8.8]
probe_asn: AS0
probe_cc: null
probe_city: null
probe_ip: 127.0.0.1
software_name: ooniprobe
software_version: 1.0.0-rc5
start_time: 1388678050.83065
test_name: multi_protocol_traceroute_test
test_version: '0.2'
...
---
answer_flags: [ipsrc]
answered_packets:
- - raw_packet: !!binary |
      RQAAOIKHAAD/ASMzCgABAX8AAAELAHHxAAAAAEUAAChcPgAAAQZCeQoAAQoICAgIgw4AAAAAAAAA
      AAAAUAIgAPG6AAA=
    summary: IP / ICMP 10.0.1.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOEGbAAD9AX7ECgHnmn8AAAELAHHxAAAAAEUAACgm8wAAAQZ3xAoAAQoICAgIgw4AAAAAAAAA
      AAAAUAIgAPG6AAA=
    summary: IP / ICMP 10.1.231.154 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOF4UAAD8AagiCvuhyX8AAAELAHHxAAAAAEUAACgexAAAAQZ/8woAAQoICAgIgw4AAAAAAAAA
      AAAAUAIgAPG6AAA=
    summary: IP / ICMP 10.251.161.201 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOAMQAAD7AQnWCvucGn8AAAELAHHxAAAAAEUAACg0uwAAAQZp/AoAAQoICAgIgw4AAAAAAAAA
      AAAAUAIgAPG6AAA=
    summary: IP / ICMP 10.251.156.26 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAODxjAAD8Ac6bCvudAX8AAAELAHHxAAAAAEUAACgBwgAAAQac9QoAAQoICAgIgw4AAAAAAAAA
      AAAAUAIgAPG6AAA=
    summary: IP / ICMP 10.251.157.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOIKJAAD/ASMxCgABAX8AAAELAOD/AAAAAEUAAChUkAAAAQZKJwoAAQoICAgIE+kAFwAAAAAA
      AAAAUAIgAGDJAAA=
    summary: IP / ICMP 10.0.1.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOBVxAAB+ASIBCkjwAX8AAAELAOD/AAAAAEUAACgibwAAAQZ8SAoAAQoICAgIE+kAFwAAAAAA
      AAAAUAIgAGDJAAA=
    summary: IP / ICMP 10.72.240.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOHJKAAD9AU4VCgHnmn8AAAELAOD/AAAAAEUAACglXAAAAQZ5WwoAAQoICAgIE+kAFwAAAAAA
      AAAAUAIgAGDJAAA=
    summary: IP / ICMP 10.1.231.154 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOGivAAD8AZ2HCvuhyX8AAAELAOD/AAAAAEUAACh9egAAAQYhPQoAAQoICAgIE+kAFwAAAAAA
      AAAAUAIgAGDJAAA=
    summary: IP / ICMP 10.251.161.201 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOGGbAAD7AatKCvucGn8AAAELAOD/AAAAAEUAACgQJAAAAQaOkwoAAQoICAgIE+kAFwAAAAAA
      AAAAUAIgAGDJAAA=
    summary: IP / ICMP 10.251.156.26 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOBn4AAD8AfEGCvudAX8AAAELAOD/AAAAAEUAACgovgAAAQZ1+QoAAQoICAgIE+kAFwAAAAAA
      AAAAUAIgAGDJAAA=
    summary: IP / ICMP 10.251.157.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOGuHAAD7AcAnCvd9VX8AAAELAOD/AAAAAEUAACgoPQAAAQZ2egoAAQoICAgIE+kAFwAAAAAA
      AAAAUAIgAGDJAAA=
    summary: IP / ICMP 10.247.125.85 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOEMPAAD6AcXuCvuhwn8AAAELAOD/AAAAAEUAACgrYwAAAQZzVAoAAQoICAgIE+kAFwAAAAAA
      AAAAUAIgAGDJAAA=
    summary: IP / ICMP 10.251.161.194 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOBCDAAD5AR/MCmd8BX8AAAELAOD/AAAAAEUAAChfGQAAAQY/ngoAAQoICAgIE+kAFwAAAAAA
      AAAAUAIgAGDJAAA=
    summary: IP / ICMP 10.103.124.5 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOF1gAAD4AdAcCgGAPX8AAAELAOD/AAAAAEUAACgU/gAAAQaJuQoAAQoICAgIE+kAFwAAAAAA
      AAAAUAIgAGDJAAA=
    summary: IP / ICMP 10.1.128.61 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOAFrAAD3AaEFCv4LTX8AAAELAOD/AAAAAEUAACguhwAAAQZwMAoAAQoICAgIE+kAFwAAAAAA
      AAAAUAIgAGDJAAA=
    summary: IP / ICMP 10.254.11.77 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAODuqAAD3AVtFWWHIan8AAAELAOD/AAAAAEUAACgqZAAAAQZ0UwoAAQoICAgIE+kAFwAAAAAA
      AAAAUAIgAGDJAAA=
    summary: IP / ICMP 89.97.200.106 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAYFCFAAD2AUauWWHI/n8AAAELAFI7AAAAAEUAACh3dQAAAQYnQgoAAQoICAgIE+kAFwAAAAAA
      AAAAUAIgAB7CAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    summary: IP / ICMP 89.97.200.254 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror / Padding
- - raw_packet: !!binary |
      RQAAOAy/AAD2AYsdWWDIfn8AAAELAOD/AAAAAEUAACgNDwAAAQaRqAoAAQoICAgIE+kAFwAAAAAA
      AAAAUAIgAGDJAAA=
    summary: IP / ICMP 89.96.200.126 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOIKLAAD/ASMvCgABAX8AAAELADZXAAAAAEUAACh24gAAAQYn1QoAAQoICAgIvlgAUAAAAAAA
      AAAAUAIgALYgAAA=
    summary: IP / ICMP 10.0.1.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOE7uAAD9AXFxCgHnmn8AAAELADZXAAAAAEUAACgSBQAAAQaMsgoAAQoICAgIvlgAUAAAAAAA
      AAAAUAIgALYgAAA=
    summary: IP / ICMP 10.1.231.154 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOELGAAB+AfSrCkjwAX8AAAELADZXAAAAAEUAACgzAQAAAQZrtgoAAQoICAgIvlgAUAAAAAAA
      AAAAUAIgALYgAAA=
    summary: IP / ICMP 10.72.240.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOE9IAAD8AbbuCvuhyX8AAAELADZXAAAAAEUAACgfbwAAAQZ/SAoAAQoICAgIvlgAUAAAAAAA
      AAAAUAIgALYgAAA=
    summary: IP / ICMP 10.251.161.201 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAODJHAAD7AdqeCvucGn8AAAELADZXAAAAAEUAAChw0gAAAQYt5QoAAQoICAgIvlgAUAAAAAAA
      AAAAUAIgALYgAAA=
    summary: IP / ICMP 10.251.156.26 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOHr8AAD8AZACCvudAX8AAAELADZXAAAAAEUAACgxTwAAAQZtaAoAAQoICAgIvlgAUAAAAAAA
      AAAAUAIgALYgAAA=
    summary: IP / ICMP 10.251.157.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOCZ6AAD7AQU1Cvd9VX8AAAELADZXAAAAAEUAACh6uwAAAQYj/AoAAQoICAgIvlgAUAAAAAAA
      AAAAUAIgALYgAAA=
    summary: IP / ICMP 10.247.125.85 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOBpqAAD6Ae6TCvuhwn8AAAELADZXAAAAAEUAACg6sgAAAQZkBQoAAQoICAgIvlgAUAAAAAAA
      AAAAUAIgALYgAAA=
    summary: IP / ICMP 10.251.161.194 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAODEjAAD5Af8rCmd8BX8AAAELADZXAAAAAEUAACg8gwAAAQZiNAoAAQoICAgIvlgAUAAAAAAA
      AAAAUAIgALYgAAA=
    summary: IP / ICMP 10.103.124.5 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOFftAAD4AdWPCgGAPX8AAAELADZXAAAAAEUAACgq7wAAAQZzyAoAAQoICAgIvlgAUAAAAAAA
      AAAAUAIgALYgAAA=
    summary: IP / ICMP 10.1.128.61 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOED1AAD3AWF7Cv4LTX8AAAELADZXAAAAAEUAACgo9QAAAQZ1wgoAAQoICAgIvlgAUAAAAAAA
      AAAAUAIgALYgAAA=
    summary: IP / ICMP 10.254.11.77 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAODAMAAD3AWbjWWHIan8AAAELADZXAAAAAEUAAChHGgAAAQZXnQoAAQoICAgIvlgAUAAAAAAA
      AAAAUAIgALYgAAA=
    summary: IP / ICMP 89.97.200.106 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAYEdUAAD2AU/fWWHI/n8AAAELAMoiAAAAAEUAACgWnQAAAQaIGgoAAQoICAgIvlgAUAAAAAAA
      AAAAUAIgAPwxAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    summary: IP / ICMP 89.97.200.254 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror / Padding
- - raw_packet: !!binary |
      RQAAOBb+AAD2AYDeWWDIfn8AAAELADZXAAAAAEUAACgR7AAAAQaMywoAAQoICAgIvlgAUAAAAAAA
      AAAAUAIgALYgAAA=
    summary: IP / ICMP 89.96.200.126 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOIKNAAD/ASMtCgABAX8AAAELAPmyAAAAAEUAACgRygAAAQaM7QoAAQoICAgI+ZEBuwAAAAAA
      AAAAUAIgAHl8AAA=
    summary: IP / ICMP 10.0.1.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOHk7AAD9AUckCgHnmn8AAAELAPmyAAAAAEUAACgZHQAAAQaFmgoAAQoICAgI+ZEBuwAAAAAA
      AAAAUAIgAHl8AAA=
    summary: IP / ICMP 10.1.231.154 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOHkEAAD8AY0yCvuhyX8AAAELAPmyAAAAAEUAAChagAAAAQZENwoAAQoICAgI+ZEBuwAAAAAA
      AAAAUAIgAHl8AAA=
    summary: IP / ICMP 10.251.161.201 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOEb4AAD7AcXtCvucGn8AAAELAPmyAAAAAEUAAChHjAAAAQZXKwoAAQoICAgI+ZEBuwAAAAAA
      AAAAUAIgAHl8AAA=
    summary: IP / ICMP 10.251.156.26 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOEeRAAD8AcNtCvudAX8AAAELAPmyAAAAAEUAAChcWwAAAQZCXAoAAQoICAgI+ZEBuwAAAAAA
      AAAAUAIgAHl8AAA=
    summary: IP / ICMP 10.251.157.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOFxtAAD7Ac9BCvd9VX8AAAELAPmyAAAAAEUAAChMHAAAAQZSmwoAAQoICAgI+ZEBuwAAAAAA
      AAAAUAIgAHl8AAA=
    summary: IP / ICMP 10.247.125.85 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAODriAAD6Ac4bCvuhwn8AAAELAPmyAAAAAEUAACgmkQAAAQZ4JgoAAQoICAgI+ZEBuwAAAAAA
      AAAAUAIgAHl8AAA=
    summary: IP / ICMP 10.251.161.194 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOF6XAAD5AdG3Cmd8BX8AAAELAPmyAAAAAEUAACgk9gAAAQZ5wQoAAQoICAgI+ZEBuwAAAAAA
      AAAAUAIgAHl8AAA=
    summary: IP / ICMP 10.103.124.5 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOCwQAAD4AQFtCgGAPX8AAAELAPmyAAAAAEUAACh8tQAAAQYiAgoAAQoICAgI+ZEBuwAAAAAA
      AAAAUAIgAHl8AAA=
    summary: IP / ICMP 10.1.128.61 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOB0EAAD3AYVsCv4LTX8AAAELAPmyAAAAAEUAACh6oAAAAQYkFwoAAQoICAgI+ZEBuwAAAAAA
      AAAAUAIgAHl8AAA=
    summary: IP / ICMP 10.254.11.77 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOFVxAAD3AUF+WWHIan8AAAELAPmyAAAAAEUAACggTwAAAQZ+aAoAAQoICAgI+ZEBuwAAAAAA
      AAAAUAIgAHl8AAA=
    summary: IP / ICMP 89.97.200.106 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAYDJ8AAD2AWS3WWHI/n8AAAELAGzNAAAAAEUAAChJIAAAAQZVlwoAAQoICAgI+ZEBuwAAAAAA
      AAAAUAIgABzjAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    summary: IP / ICMP 89.97.200.254 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror / Padding
- - raw_packet: !!binary |
      RQAAODLbAAD2AWT5WWDIhn8AAAELAPmyAAAAAEUAACgucgAAAQZwRQoAAQoICAgI+ZEBuwAAAAAA
      AAAAUAIgAHl8AAA=
    summary: IP / ICMP 89.96.200.134 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOIKPAAD/ASMrCgABAX8AAAELAGNSAAAAAEUAACg0TAAAAQZqawoAAQoICAgIka3//wAAAAAA
      AAAAUAIgAOMbAAA=
    summary: IP / ICMP 10.0.1.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOEk/AAB+Ae4yCkjwAX8AAAELAGNSAAAAAEUAACgF/gAAAQaYuQoAAQoICAgIka3//wAAAAAA
      AAAAUAIgAOMbAAA=
    summary: IP / ICMP 10.72.240.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOCihAAD9AZe+CgHnmn8AAAELAGNSAAAAAEUAACh8wgAAAQYh9QoAAQoICAgIka3//wAAAAAA
      AAAAUAIgAOMbAAA=
    summary: IP / ICMP 10.1.231.154 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOH04AAD8AYj+CvuhyX8AAAELAGNSAAAAAEUAACg/7AAAAQZeywoAAQoICAgIka3//wAAAAAA
      AAAAUAIgAOMbAAA=
    summary: IP / ICMP 10.251.161.201 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOGM+AAD7AamnCvucGn8AAAELAGNSAAAAAEUAAChmCwAAAQY4rAoAAQoICAgIka3//wAAAAAA
      AAAAUAIgAOMbAAA=
    summary: IP / ICMP 10.251.156.26 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOApTAAD8AQCsCvudAX8AAAELAGNSAAAAAEUAAChHmQAAAQZXHgoAAQoICAgIka3//wAAAAAA
      AAAAUAIgAOMbAAA=
    summary: IP / ICMP 10.251.157.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOBtSAAD7ARBdCvd9VX8AAAELAGNSAAAAAEUAACg1mAAAAQZpHwoAAQoICAgIka3//wAAAAAA
      AAAAUAIgAOMbAAA=
    summary: IP / ICMP 10.247.125.85 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOHiXAAD6AZBmCvuhwn8AAAELAGNSAAAAAEUAAChv9gAAAQYuwQoAAQoICAgIka3//wAAAAAA
      AAAAUAIgAOMbAAA=
    summary: IP / ICMP 10.251.161.194 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOGffAAD5AchvCmd8BX8AAAELAGNSAAAAAEUAAChZJQAAAQZFkgoAAQoICAgIka3//wAAAAAA
      AAAAUAIgAOMbAAA=
    summary: IP / ICMP 10.103.124.5 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOCw9AAD4AQFACgGAPX8AAAELAGNSAAAAAEUAACh3CwAAAQYnrAoAAQoICAgIka3//wAAAAAA
      AAAAUAIgAOMbAAA=
    summary: IP / ICMP 10.1.128.61 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOG8GAAD3ATNqCv4LTX8AAAELAGNSAAAAAEUAACgZRwAAAQaFcAoAAQoICAgIka3//wAAAAAA
      AAAAUAIgAOMbAAA=
    summary: IP / ICMP 10.254.11.77 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOEBhAAD3AVaOWWHIan8AAAELAGNSAAAAAEUAACg5lAAAAQZlIwoAAQoICAgIka3//wAAAAAA
      AAAAUAIgAOMbAAA=
    summary: IP / ICMP 89.97.200.106 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAYDcVAAD2AWAeWWHI/n8AAAELAPvAAAAAAEUAACgjogAAAQZ7FQoAAQoICAgIka3//wAAAAAA
      AAAAUAIgAPeOAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    summary: IP / ICMP 89.97.200.254 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror / Padding
- - raw_packet: !!binary |
      RQAAOEneAAD2AU32WWDIhn8AAAELAGNSAAAAAEUAACgprwAAAQZ1CAoAAQoICAgIka3//wAAAAAA
      AAAAUAIgAOMbAAA=
    summary: IP / ICMP 89.96.200.134 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOIKIAAD/ASMyCgABAX8AAAELAHmgAAAAAEUAACiAgwAAAQYeNAoAAQoICAgIe0kAFgAAAAAA
      AAAAUAIgAPlpAAA=
    summary: IP / ICMP 10.0.1.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAODfVAAB+Af+cCkjwAX8AAAELAHmgAAAAAEUAAChPqAAAAQZPDwoAAQoICAgIe0kAFgAAAAAA
      AAAAUAIgAPlpAAA=
    summary: IP / ICMP 10.72.240.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOAmvAAD9AbawCgHnmn8AAAELAHmgAAAAAEUAACgtrAAAAQZxCwoAAQoICAgIe0kAFgAAAAAA
      AAAAUAIgAPlpAAA=
    summary: IP / ICMP 10.1.231.154 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOD0PAAD8AcknCvuhyX8AAAELAHmgAAAAAEUAAChHnAAAAQZXGwoAAQoICAgIe0kAFgAAAAAA
      AAAAUAIgAPlpAAA=
    summary: IP / ICMP 10.251.161.201 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOArmAAD7AQIACvucGn8AAAELAHmgAAAAAEUAACh0ogAAAQYqFQoAAQoICAgIe0kAFgAAAAAA
      AAAAUAIgAPlpAAA=
    summary: IP / ICMP 10.251.156.26 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAODC1AAD8AdpJCvudAX8AAAELAHmgAAAAAEUAAChQfAAAAQZOOwoAAQoICAgIe0kAFgAAAAAA
      AAAAUAIgAPlpAAA=
    summary: IP / ICMP 10.251.157.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOAfoAAD7ASPHCvd9VX8AAAELAHmgAAAAAEUAAChSwQAAAQZL9goAAQoICAgIe0kAFgAAAAAA
      AAAAUAIgAPlpAAA=
    summary: IP / ICMP 10.247.125.85 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOEPGAAD6AcU3Cvuhwn8AAAELAHmgAAAAAEUAACg4kQAAAQZmJgoAAQoICAgIe0kAFgAAAAAA
      AAAAUAIgAPlpAAA=
    summary: IP / ICMP 10.251.161.194 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOGK6AAD5Ac2UCmd8BX8AAAELAHmgAAAAAEUAACg8jwAAAQZiKAoAAQoICAgIe0kAFgAAAAAA
      AAAAUAIgAPlpAAA=
    summary: IP / ICMP 10.103.124.5 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOC8sAAD4Af5QCgGAPX8AAAELAHmgAAAAAEUAACh6rgAAAQYkCQoAAQoICAgIe0kAFgAAAAAA
      AAAAUAIgAPlpAAA=
    summary: IP / ICMP 10.1.128.61 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAODMkAAD3AW9MCv4LTX8AAAELAHmgAAAAAEUAACg9EgAAAQZhpQoAAQoICAgIe0kAFgAAAAAA
      AAAAUAIgAPlpAAA=
    summary: IP / ICMP 10.254.11.77 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOHHtAAD3ASUCWWHIan8AAAELAHmgAAAAAEUAAChrqQAAAQYzDgoAAQoICAgIe0kAFgAAAAAA
      AAAAUAIgAPlpAAA=
    summary: IP / ICMP 89.97.200.106 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAYCipAAD2AW6KWWHI/n8AAAELAPcOAAAAAEUAACh+yQAAAQYf7goAAQoICAgIe0kAFgAAAAAA
      AAAAUAIgABKPAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    summary: IP / ICMP 89.97.200.254 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror / Padding
- - raw_packet: !!binary |
      RQAAOD/VAAD2AVf/WWDIhn8AAAELAHmgAAAAAEUAACgS3QAAAQaL2goAAQoICAgIe0kAFgAAAAAA
      AAAAUAIgAPlpAAA=
    summary: IP / ICMP 89.96.200.134 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOIKMAAD/ASMuCgABAX8AAAELAMqbAAAAAEUAAChzvgAAAQYq+QoAAQoICAgIKekAewAAAAAA
      AAAAUAIgAEplAAA=
    summary: IP / ICMP 10.0.1.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOADVAAB+ATadCkjwAX8AAAELAMqbAAAAAEUAACg27QAAAQZnygoAAQoICAgIKekAewAAAAAA
      AAAAUAIgAEplAAA=
    summary: IP / ICMP 10.72.240.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOBqVAAD9AaXKCgHnmn8AAAELAMqbAAAAAEUAACgbXgAAAQaDWQoAAQoICAgIKekAewAAAAAA
      AAAAUAIgAEplAAA=
    summary: IP / ICMP 10.1.231.154 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOEMTAAD8AcMjCvuhyX8AAAELAMqbAAAAAEUAAChdAwAAAQZBtAoAAQoICAgIKekAewAAAAAA
      AAAAUAIgAEplAAA=
    summary: IP / ICMP 10.251.161.201 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOBsqAAD7AfG7CvucGn8AAAELAMqbAAAAAEUAACh8VgAAAQYiYQoAAQoICAgIKekAewAAAAAA
      AAAAUAIgAEplAAA=
    summary: IP / ICMP 10.251.156.26 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOH3pAAD8AY0VCvudAX8AAAELAMqbAAAAAEUAACgIXwAAAQaWWAoAAQoICAgIKekAewAAAAAA
      AAAAUAIgAEplAAA=
    summary: IP / ICMP 10.251.157.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOE5BAAD7Ad1tCvd9VX8AAAELAMqbAAAAAEUAACheCQAAAQZArgoAAQoICAgIKekAewAAAAAA
      AAAAUAIgAEplAAA=
    summary: IP / ICMP 10.247.125.85 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOBdKAAD6AfGzCvuhwn8AAAELAMqbAAAAAEUAACgQywAAAQaN7AoAAQoICAgIKekAewAAAAAA
      AAAAUAIgAEplAAA=
    summary: IP / ICMP 10.251.161.194 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOGsMAAD5AcVCCmd8BX8AAAELAMqbAAAAAEUAACgcCwAAAQaCrAoAAQoICAgIKekAewAAAAAA
      AAAAUAIgAEplAAA=
    summary: IP / ICMP 10.103.124.5 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOBhtAAD4ARUQCgGAPX8AAAELAMqbAAAAAEUAAChCKQAAAQZcjgoAAQoICAgIKekAewAAAAAA
      AAAAUAIgAEplAAA=
    summary: IP / ICMP 10.1.128.61 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAODzTAAD3AWWdCv4LTX8AAAELAMqbAAAAAEUAACgQXQAAAQaOWgoAAQoICAgIKekAewAAAAAA
      AAAAUAIgAEplAAA=
    summary: IP / ICMP 10.254.11.77 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOBI6AAD3AYS1WWHIan8AAAELAMqbAAAAAEUAACgkEAAAAQZ6pwoAAQoICAgIKekAewAAAAAA
      AAAAUAIgAEplAAA=
    summary: IP / ICMP 89.97.200.106 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOC3zAAD2AdEhPmV8QX8AAAELAMqbAAAAAEUAACgWyAAAAQaH7woAAQoICAgIKekAewAAAAAA
      AAAAUAIgAEplAAA=
    summary: IP / ICMP 62.101.124.65 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAYGwiAAD2ASsRWWHI/n8AAAELAGD1AAAAAEUAACg3JwAAAQZnkAoAAQoICAgIKekAewAAAAAA
      AAAAUAIgAPmjAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    summary: IP / ICMP 89.97.200.254 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror / Padding
- - raw_packet: !!binary |
      RQAAOIKKAAD/ASMwCgABAX8AAAELANu1AAAAAEUAACi67wAAAQbjxwoAAQoICAgIGRUANQAAAAAA
      AAAAUAIgAFt/AAA=
    summary: IP / ICMP 10.0.1.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAODcjAAB+AQBPCkjwAX8AAAELANu1AAAAAEUAAChmjAAAAQY4KwoAAQoICAgIGRUANQAAAAAA
      AAAAUAIgAFt/AAA=
    summary: IP / ICMP 10.72.240.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOH8HAAD9AUFYCgHnmn8AAAELANu1AAAAAEUAACh5vwAAAQYk+AoAAQoICAgIGRUANQAAAAAA
      AAAAUAIgAFt/AAA=
    summary: IP / ICMP 10.1.231.154 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAODnqAAD8AcxMCvuhyX8AAAELANu1AAAAAEUAACgwaAAAAQZuTwoAAQoICAgIGRUANQAAAAAA
      AAAAUAIgAFt/AAA=
    summary: IP / ICMP 10.251.161.201 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAODG7AAD7AdsqCvucGn8AAAELANu1AAAAAEUAACgh4wAAAQZ81AoAAQoICAgIGRUANQAAAAAA
      AAAAUAIgAFt/AAA=
    summary: IP / ICMP 10.251.156.26 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOE65AAD8AbxFCvudAX8AAAELANu1AAAAAEUAACg+wwAAAQZf9AoAAQoICAgIGRUANQAAAAAA
      AAAAUAIgAFt/AAA=
    summary: IP / ICMP 10.251.157.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOFdRAAD7AdRdCvd9VX8AAAELANu1AAAAAEUAAChRbgAAAQZNSQoAAQoICAgIGRUANQAAAAAA
      AAAAUAIgAFt/AAA=
    summary: IP / ICMP 10.247.125.85 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOD3hAAD6AcscCvuhwn8AAAELANu1AAAAAEUAAChddAAAAQZBQwoAAQoICAgIGRUANQAAAAAA
      AAAAUAIgAFt/AAA=
    summary: IP / ICMP 10.251.161.194 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOF/GAAD5AdCICmd8BX8AAAELANu1AAAAAEUAACgOyQAAAQaP7goAAQoICAgIGRUANQAAAAAA
      AAAAUAIgAFt/AAA=
    summary: IP / ICMP 10.103.124.5 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOE1DAAD4AeA5CgGAPX8AAAELANu1AAAAAEUAACgH0QAAAQaW5goAAQoICAgIGRUANQAAAAAA
      AAAAUAIgAFt/AAA=
    summary: IP / ICMP 10.1.128.61 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOB+5AAD3AYK3Cv4LTX8AAAELANu1AAAAAEUAAChzbQAAAQYrSgoAAQoICAgIGRUANQAAAAAA
      AAAAUAIgAFt/AAA=
    summary: IP / ICMP 10.254.11.77 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOG1UAAD3ASmbWWHIan8AAAELANu1AAAAAEUAAChLowAAAQZTFAoAAQoICAgIGRUANQAAAAAA
      AAAAUAIgAFt/AAA=
    summary: IP / ICMP 89.97.200.106 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAYBk2AAD2AX39WWHI/n8AAAELAE2eAAAAAEUAACgftQAAAQZ/AgoAAQoICAgIGRUANQAAAAAA
      AAAAUAIgAB4VAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    summary: IP / ICMP 89.97.200.254 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror / Padding
- - raw_packet: !!binary |
      RQAAOGuvAAD2ASx9WWDILn8AAAELANu1AAAAAEUAAChiigAAAQY8LQoAAQoICAgIGRUANQAAAAAA
      AAAAUAIgAFt/AAA=
    summary: IP / ICMP 89.96.200.46 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOCEtAAD0AX8mSA7TWX8AAAELANu1AAAAAEUAAChfEgAAAQY/pQoAAQoICAgIGRUANQAAAAAA
      AAAAUAIgAFt/AAA=
    summary: IP / ICMP 72.14.211.89 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOAUmAAD1AfLi0VXxXH8AAAELANu1AAAAAEWAACh3ggAAAQYmtQoAAQoICAgIGRUANQAAAAAA
      AAAAUAIgAFt/AAA=
    summary: IP / ICMP 209.85.241.92 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAqFe+AADwATcwSA7oTn8AAAELAE2eAAAAAEWAACh7bAAAAQYiywoAAQoICAgIGRUANQAAAAAA
      AAAAUAIgAB4VAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
      AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIAC68AAIAQF7BKkB
    summary: IP / ICMP 72.14.232.78 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror / Padding
- - raw_packet: !!binary |
      RQAAOEWeAAD0AaZU0VX+cn8AAAELANu1AAAAAEWAAChsnAAAAQYxmwoAAQoICAgIGRUANQAAAAAA
      AAAAUAIgAFt/AAA=
    summary: IP / ICMP 209.85.254.114 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAALHw7AAArBvh3CAgICH8AAAEANRkVUFkhMQAAAAFgEqeUSrEAAAIEBZY=
    summary: IP / TCP 8.8.8.8:domain > 127.0.0.1:nim_wan SA
- - raw_packet: !!binary |
      RQAALCVxAAArBk9CCAgICH8AAAEANRkVUFkhMQAAAAFgEqeUSrEAAAIEBZY=
    summary: IP / TCP 8.8.8.8:domain > 127.0.0.1:nim_wan SA
- - raw_packet: !!binary |
      RQAALGaKAAArBg4pCAgICH8AAAEANRkVUFkhMQAAAAFgEqeUSrEAAAIEBZY=
    summary: IP / TCP 8.8.8.8:domain > 127.0.0.1:nim_wan SA
- - raw_packet: !!binary |
      RQAALDqzAAArBjoACAgICH8AAAEANRkVUFkhMQAAAAFgEqeUSrEAAAIEBZY=
    summary: IP / TCP 8.8.8.8:domain > 127.0.0.1:nim_wan SA
- - raw_packet: !!binary |
      RQAALEoqAAArBiqJCAgICH8AAAEANRkVUFkhMQAAAAFgEqeUSrEAAAIEBZY=
    summary: IP / TCP 8.8.8.8:domain > 127.0.0.1:nim_wan SA
- - raw_packet: !!binary |
      RQAALAhaAAArBmxZCAgICH8AAAEANRkVUFkhMQAAAAFgEqeUSrEAAAIEBZY=
    summary: IP / TCP 8.8.8.8:domain > 127.0.0.1:nim_wan SA
- - raw_packet: !!binary |
      RQAALCwbAAArBkiYCAgICH8AAAEANRkVUFkhMQAAAAFgEqeUSrEAAAIEBZY=
    summary: IP / TCP 8.8.8.8:domain > 127.0.0.1:nim_wan SA
- - raw_packet: !!binary |
      RQAALBfiAAArBlzRCAgICH8AAAEANRkVUFkhMQAAAAFgEqeUSrEAAAIEBZY=
    summary: IP / TCP 8.8.8.8:domain > 127.0.0.1:nim_wan SA
- - raw_packet: !!binary |
      RQAALChYAAArBkxbCAgICH8AAAEANRkVUFkhMQAAAAFgEqeUSrEAAAIEBZY=
    summary: IP / TCP 8.8.8.8:domain > 127.0.0.1:nim_wan SA
- - raw_packet: !!binary |
      RQAALGkeAAArBguVCAgICH8AAAEANRkVUFkhMQAAAAFgEqeUSrEAAAIEBZY=
    summary: IP / TCP 8.8.8.8:domain > 127.0.0.1:nim_wan SA
- - raw_packet: !!binary |
      RQAALF+pAAArBhUKCAgICH8AAAEANRkVUFkhMQAAAAFgEqeUSrEAAAIEBZY=
    summary: IP / TCP 8.8.8.8:domain > 127.0.0.1:nim_wan SA
- - raw_packet: !!binary |
      RQAAOIKOAAD/ASMsCgABAX8AAAELAPNSAAAAAEUAACjlPQAAAQa5eQoAAQoICAgI4hwfkAAAAAAA
      AAAAUAIgAHMcAAA=
    summary: IP / ICMP 10.0.1.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOFrfAAD9AWWACgHnmn8AAAELAPNSAAAAAEUAACgEFAAAAQaaowoAAQoICAgI4hwfkAAAAAAA
      AAAAUAIgAHMcAAA=
    summary: IP / ICMP 10.1.231.154 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOEvyAAD8AbpECvuhyX8AAAELAPNSAAAAAEUAACh2JwAAAQYokAoAAQoICAgI4hwfkAAAAAAA
      AAAAUAIgAHMcAAA=
    summary: IP / ICMP 10.251.161.201 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOEoVAAD7AcLQCvucGn8AAAELAPNSAAAAAEUAAChWaQAAAQZITgoAAQoICAgI4hwfkAAAAAAA
      AAAAUAIgAHMcAAA=
    summary: IP / ICMP 10.251.156.26 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOGOZAAD8AadlCvudAX8AAAELAPNSAAAAAEUAAChLfgAAAQZTOQoAAQoICAgI4hwfkAAAAAAA
      AAAAUAIgAHMcAAA=
    summary: IP / ICMP 10.251.157.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RcAAOCkEAAD7AQKrCvd9VX8AAAELAPNSAAAAAEUAAChKTwAAAQZUaAoAAQoICAgI4hwfkAAAAAAA
      AAAAUAIgAHMcAAA=
    summary: IP / ICMP 10.247.125.85 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOHL8AAD6AZYBCvuhwn8AAAELAPNSAAAAAEUAAChGKwAAAQZYjAoAAQoICAgI4hwfkAAAAAAA
      AAAAUAIgAHMcAAA=
    summary: IP / ICMP 10.251.161.194 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOCBNAAD5ARACCmd8BX8AAAELAPNSAAAAAEUAACgaHwAAAQaEmAoAAQoICAgI4hwfkAAAAAAA
      AAAAUAIgAHMcAAA=
    summary: IP / ICMP 10.103.124.5 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOF3sAAD4Ac+QCgGAPX8AAAELAPNSAAAAAEUAACgV/QAAAQaIugoAAQoICAgI4hwfkAAAAAAA
      AAAAUAIgAHMcAAA=
    summary: IP / ICMP 10.1.128.61 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOCBhAAD3AYIPCv4LTX8AAAELAPNSAAAAAEUAAChuGQAAAQYwngoAAQoICAgI4hwfkAAAAAAA
      AAAAUAIgAHMcAAA=
    summary: IP / ICMP 10.254.11.77 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOFcAAAD3AT/vWWHIan8AAAELAPNSAAAAAEUAACgTdQAAAQaLQgoAAQoICAgI4hwfkAAAAAAA
      AAAAUAIgAHMcAAA=
    summary: IP / ICMP 89.97.200.106 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAYEpQAAD2AUzjWWHI/n8AAAELAJUsAAAAAEUAAChxMAAAAQYthwoAAQoICAgI4hwfkAAAAAAA
      AAAAUAIgAO4jAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    summary: IP / ICMP 89.97.200.254 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror / Padding
- - raw_packet: !!binary |
      RQAAOAxnAAD2AfKtPmV8QX8AAAELAPNSAAAAAEUAACgBDgAAAQadqQoAAQoICAgI4hwfkAAAAAAA
      AAAAUAIgAHMcAAA=
    summary: IP / ICMP 62.101.124.65 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / TCPerror
- - raw_packet: !!binary |
      RQAAOIKRAAD/ASMpCgABAX8AAAELABAzAAAAAEUAABwvFgAAARFvogoAAQoICAgIqwYAFwAIOac=
    summary: IP / ICMP 10.0.1.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOFNsAAB+AeQFCkjwAX8AAAELAANFAAAAAEUAABw5ngAAARFlGgoAAQoICAgIqwYAFwAIRpU=
    summary: IP / ICMP 10.72.240.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOHE6AAD9AU8lCgHnmn8AAAELAANFAAAAAEUAABxGtgAAARFYAgoAAQoICAgIqwYAFwAIRpU=
    summary: IP / ICMP 10.1.231.154 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAODDfAAD8AdVXCvuhyX8AAAELAANFAAAAAEUAABxWXQAAARFIWwoAAQoICAgIqwYAFwAIRpU=
    summary: IP / ICMP 10.251.161.201 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOHMbAAD7AZnKCvucGn8AAAELAANFAAAAAEUAABwFUAAAARGZaAoAAQoICAgIqwYAFwAIRpU=
    summary: IP / ICMP 10.251.156.26 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOBQ5AAD8AfbFCvudAX8AAAELAANFAAAAAEUAABx9aQAAAREhTwoAAQoICAgIqwYAFwAIRpU=
    summary: IP / ICMP 10.251.157.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOBMIAAD7ARinCvd9VX8AAAELANyRAAAAAEUAABwqfQAAARF0OwoAAQoICAgIqwYAFwAIbUg=
    summary: IP / ICMP 10.247.125.85 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOFPQAAD6AbUtCvuhwn8AAAELANyRAAAAAEUAABwGygAAARGX7goAAQoICAgIqwYAFwAIbUg=
    summary: IP / ICMP 10.251.161.194 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOCzvAAD5AQNgCmd8BX8AAAELANyRAAAAAEUAABwnsAAAARF3CAoAAQoICAgIqwYAFwAIbUg=
    summary: IP / ICMP 10.103.124.5 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOArqAAD4ASKTCgGAPX8AAAELANyRAAAAAEUAABwXygAAARGG7goAAQoICAgIqwYAFwAIbUg=
    summary: IP / ICMP 10.1.128.61 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOC5tAAD3AXQDCv4LTX8AAAELANyRAAAAAEUAABwUwgAAARGJ9goAAQoICAgIqwYAFwAIbUg=
    summary: IP / ICMP 10.254.11.77 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOCujAAD3AWtMWWHIan8AAAELANyRAAAAAEUAABxoVgAAARE2YgoAAQoICAgIqwYAFwAIbUg=
    summary: IP / ICMP 89.97.200.106 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAYAbjAAD2AZBQWWHI/n8AAAELANyRAAAAAEUAABx9VQAAAREhYwoAAQoICAgIqwYAFwAIbUgA
      AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    summary: IP / ICMP 89.97.200.254 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror / Padding
- - raw_packet: !!binary |
      RQAAOCEkAAD2Ad3wPmV8QX8AAAELANyRAAAAAEUAABw+mwAAARFgHQoAAQoICAgIqwYAFwAIbUg=
    summary: IP / ICMP 62.101.124.65 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOIKTAAD/ASMnCgABAX8AAAELABAzAAAAAEUAABwyEQAAARFspwoAAQoICAgIXY8AUAAIhuU=
    summary: IP / ICMP 10.0.1.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOE9OAAD9AXERCgHnmn8AAAELAANFAAAAAEUAABxMDAAAARFSrAoAAQoICAgIXY8AUAAIk9M=
    summary: IP / ICMP 10.1.231.154 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOHJUAAB+AcUdCkjwAX8AAAELAANFAAAAAEUAABwhEwAAARF9pQoAAQoICAgIXY8AUAAIk9M=
    summary: IP / ICMP 10.72.240.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOBp0AAD8AevCCvuhyX8AAAELAANFAAAAAEUAABwb5AAAARGC1AoAAQoICAgIXY8AUAAIk9M=
    summary: IP / ICMP 10.251.161.201 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOFjeAAD7AbQHCvucGn8AAAELAANFAAAAAEUAABwB+QAAARGcvwoAAQoICAgIXY8AUAAIk9M=
    summary: IP / ICMP 10.251.156.26 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOHRxAAD8AZaNCvudAX8AAAELAANFAAAAAEUAABwLEAAAARGTqAoAAQoICAgIXY8AUAAIk9M=
    summary: IP / ICMP 10.251.157.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOFq4AAD7AdD2Cvd9VX8AAAELANyRAAAAAEUAABw11wAAARFo4QoAAQoICAgIXY8AUAAIuoY=
    summary: IP / ICMP 10.247.125.85 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOFKyAAD6AbZLCvuhwn8AAAELANyRAAAAAEUAABxcnwAAARFCGQoAAQoICAgIXY8AUAAIuoY=
    summary: IP / ICMP 10.251.161.194 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOHqHAAD5AbXHCmd8BX8AAAELANyRAAAAAEUAABwHlAAAARGXJAoAAQoICAgIXY8AUAAIuoY=
    summary: IP / ICMP 10.103.124.5 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOFQMAAD4AdlwCgGAPX8AAAELANyRAAAAAEUAABw7uQAAARFi/woAAQoICAgIXY8AUAAIuoY=
    summary: IP / ICMP 10.1.128.61 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOALXAAD3AZ+ZCv4LTX8AAAELANyRAAAAAEUAABxbqQAAARFDDwoAAQoICAgIXY8AUAAIuoY=
    summary: IP / ICMP 10.254.11.77 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAODYEAAD3AWDrWWHIan8AAAELANyRAAAAAEUAABxiqwAAARE8DQoAAQoICAgIXY8AUAAIuoY=
    summary: IP / ICMP 89.97.200.106 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAYHabAAD2ASCYWWHI/n8AAAELANyRAAAAAEUAABxGQwAAARFYdQoAAQoICAgIXY8AUAAIuoYA
      AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    summary: IP / ICMP 89.97.200.254 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror / Padding
- - raw_packet: !!binary |
      RQAAOHBeAAD2ASfOWWDILn8AAAELANyRAAAAAEUAABxWUQAAARFIZwoAAQoICAgIXY8AUAAIuoY=
    summary: IP / ICMP 89.96.200.46 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOIKVAAD/ASMlCgABAX8AAAELABAzAAAAAEUAABwahgAAARGEMgoAAQoICAgIPaYBuwAIpWM=
    summary: IP / ICMP 10.0.1.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOE8ZAAB+AehYCkjwAX8AAAELAANFAAAAAEUAABwUagAAARGKTgoAAQoICAgIPaYBuwAIslE=
    summary: IP / ICMP 10.72.240.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOFOeAAD9AWzBCgHnmn8AAAELAANFAAAAAEUAABxKrQAAARFUCwoAAQoICAgIPaYBuwAIslE=
    summary: IP / ICMP 10.1.231.154 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAODmqAAD8AcyMCvuhyX8AAAELAANFAAAAAEUAABxPPQAAARFPewoAAQoICAgIPaYBuwAIslE=
    summary: IP / ICMP 10.251.161.201 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOFLIAAD7AbodCvucGn8AAAELAANFAAAAAEUAABxYFAAAARFGpAoAAQoICAgIPaYBuwAIslE=
    summary: IP / ICMP 10.251.156.26 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOCJoAAD8AeiWCvudAX8AAAELAANFAAAAAEUAABxVHgAAARFJmgoAAQoICAgIPaYBuwAIslE=
    summary: IP / ICMP 10.251.157.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOA5xAAD7AR0+Cvd9VX8AAAELANyRAAAAAEUAABw7RgAAARFjcgoAAQoICAgIPaYBuwAI2QQ=
    summary: IP / ICMP 10.247.125.85 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAODo7AAD6Ac7CCvuhwn8AAAELANyRAAAAAEUAABx72QAAAREi3woAAQoICAgIPaYBuwAI2QQ=
    summary: IP / ICMP 10.251.161.194 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOAeMAAD5ASjDCmd8BX8AAAELANyRAAAAAEUAABxiTwAAARE8aQoAAQoICAgIPaYBuwAI2QQ=
    summary: IP / ICMP 10.103.124.5 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOGPpAAD4AcmTCgGAPX8AAAELANyRAAAAAEUAABxQHQAAARFOmwoAAQoICAgIPaYBuwAI2QQ=
    summary: IP / ICMP 10.1.128.61 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOAndAAD3AZiTCv4LTX8AAAELANyRAAAAAEUAABxzpQAAARErEwoAAQoICAgIPaYBuwAI2QQ=
    summary: IP / ICMP 10.254.11.77 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOFgXAAD3AT7YWWHIan8AAAELANyRAAAAAEUAABxnRwAAARE3cQoAAQoICAgIPaYBuwAI2QQ=
    summary: IP / ICMP 89.97.200.106 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAYFGuAAD2AUWFWWHI/n8AAAELANyRAAAAAEUAABxq/QAAAREzuwoAAQoICAgIPaYBuwAI2QQA
      AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    summary: IP / ICMP 89.97.200.254 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror / Padding
- - raw_packet: !!binary |
      RQAAOFZTAAD2AajBPmV8QX8AAAELANyRAAAAAEUAABwEFQAAARGaowoAAQoICAgIPaYBuwAI2QQ=
    summary: IP / ICMP 62.101.124.65 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOIKXAAD/ASMjCgABAX8AAAELABAzAAAAAEUAABwTtAAAARGLBAoAAQoICAgIKu///wAIudU=
    summary: IP / ICMP 10.0.1.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOEQZAAB+AfNYCkjwAX8AAAELAANFAAAAAEUAABxJIQAAARFVlwoAAQoICAgIKu///wAIxsM=
    summary: IP / ICMP 10.72.240.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOFUpAAD9AWs2CgHnmn8AAAELAANFAAAAAEUAABwFwQAAARGY9woAAQoICAgIKu///wAIxsM=
    summary: IP / ICMP 10.1.231.154 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOGsKAAD8AZssCvuhyX8AAAELAANFAAAAAEUAABxCgQAAARFcNwoAAQoICAgIKu///wAIxsM=
    summary: IP / ICMP 10.251.161.201 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOAmpAAD7AQM9CvucGn8AAAELAANFAAAAAEUAABwYIAAAARGGmAoAAQoICAgIKu///wAIxsM=
    summary: IP / ICMP 10.251.156.26 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOGCXAAD8AapnCvudAX8AAAELAANFAAAAAEUAABwTKgAAARGLjgoAAQoICAgIKu///wAIxsM=
    summary: IP / ICMP 10.251.157.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOBOTAAD7ARgcCvd9VX8AAAELANyRAAAAAEUAABwVJwAAARGJkQoAAQoICAgIKu///wAI7XY=
    summary: IP / ICMP 10.247.125.85 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAODC3AAD6AdhGCvuhwn8AAAELANyRAAAAAEUAABxDtQAAARFbAwoAAQoICAgIKu///wAI7XY=
    summary: IP / ICMP 10.251.161.194 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAODedAAD5AfixCmd8BX8AAAELANyRAAAAAEUAABxN4QAAARFQ1woAAQoICAgIKu///wAI7XY=
    summary: IP / ICMP 10.103.124.5 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOFx1AAD4AdEHCgGAPX8AAAELANyRAAAAAEUAABw0gwAAARFqNQoAAQoICAgIKu///wAI7XY=
    summary: IP / ICMP 10.1.128.61 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOHArAAD3ATJFCv4LTX8AAAELANyRAAAAAEUAABwNQgAAARGRdgoAAQoICAgIKu///wAI7XY=
    summary: IP / ICMP 10.254.11.77 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOEe3AAD3AU84WWHIan8AAAELANyRAAAAAEUAABwZfgAAARGFOgoAAQoICAgIKu///wAI7XY=
    summary: IP / ICMP 89.97.200.106 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAYFc0AAD2AT//WWHI/n8AAAELANyRAAAAAEUAABwz2wAAARFq3QoAAQoICAgIKu///wAI7XYA
      AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    summary: IP / ICMP 89.97.200.254 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror / Padding
- - raw_packet: !!binary |
      RQAAODTWAAD2AWNWWWDILn8AAAELANyRAAAAAEUAABxoqgAAARE2DgoAAQoICAgIKu///wAI7XY=
    summary: IP / ICMP 89.96.200.46 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOIKQAAD/ASMqCgABAX8AAAELABAzAAAAAEUAABxylwAAAREsIQoAAQoICAgIO4EAFgAIqS0=
    summary: IP / ICMP 10.0.1.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOCCDAAD9AZ/cCgHnmn8AAAELAANFAAAAAEUAABxqEAAAARE0qAoAAQoICAgIO4EAFgAIths=
    summary: IP / ICMP 10.1.231.154 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOHMJAAB+AcRoCkjwAX8AAAELAANFAAAAAEUAABxhYgAAARE9VgoAAQoICAgIO4EAFgAIths=
    summary: IP / ICMP 10.72.240.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOHFtAAD8AZTJCvuhyX8AAAELAANFAAAAAEUAABxsUwAAAREyZQoAAQoICAgIO4EAFgAIths=
    summary: IP / ICMP 10.251.161.201 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOCG0AAD7AesxCvucGn8AAAELAANFAAAAAEUAABw13gAAARFo2goAAQoICAgIO4EAFgAIths=
    summary: IP / ICMP 10.251.156.26 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOGzBAAD8AZ49CvudAX8AAAELAANFAAAAAEUAABxULwAAARFKiQoAAQoICAgIO4EAFgAIths=
    summary: IP / ICMP 10.251.157.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOGVtAAD7AcZBCvd9VX8AAAELANyRAAAAAEUAABwlYwAAARF5VQoAAQoICAgIO4EAFgAI3M4=
    summary: IP / ICMP 10.247.125.85 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAODepAAD6AdFUCvuhwn8AAAELANyRAAAAAEUAABwplgAAARF1IgoAAQoICAgIO4EAFgAI3M4=
    summary: IP / ICMP 10.251.161.194 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOHKlAAD5Ab2pCmd8BX8AAAELANyRAAAAAEUAABwebwAAARGASQoAAQoICAgIO4EAFgAI3M4=
    summary: IP / ICMP 10.103.124.5 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOD0XAAD4AfBlCgGAPX8AAAELANyRAAAAAEUAABwUBgAAARGKsgoAAQoICAgIO4EAFgAI3M4=
    summary: IP / ICMP 10.1.128.61 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOD9VAAD3AWMbCv4LTX8AAAELANyRAAAAAEUAABxcmwAAARFCHQoAAQoICAgIO4EAFgAI3M4=
    summary: IP / ICMP 10.254.11.77 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOBGoAAD3AYVHWWHIan8AAAELANyRAAAAAEUAABw3TAAAARFnbAoAAQoICAgIO4EAFgAI3M4=
    summary: IP / ICMP 89.97.200.106 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAYENwAAD2AVPDWWHI/n8AAAELANyRAAAAAEUAABw0HwAAARFqmQoAAQoICAgIO4EAFgAI3M4A
      AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    summary: IP / ICMP 89.97.200.254 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror / Padding
- - raw_packet: !!binary |
      RQAAOEnSAAD2AU4KWWDIfn8AAAELANyRAAAAAEUAABwrHAAAARFznAoAAQoICAgIO4EAFgAI3M4=
    summary: IP / ICMP 89.96.200.126 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOIKUAAD/ASMmCgABAX8AAAELABAzAAAAAEUAAByqJgAAARH0kQoAAQoICAgIc54AewAIcKs=
    summary: IP / ICMP 10.0.1.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOE82AAB+Aeg7CkjwAX8AAAELAANFAAAAAEUAABxr8AAAAREyyAoAAQoICAgIc54AewAIfZk=
    summary: IP / ICMP 10.72.240.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOG3VAAD9AVKKCgHnmn8AAAELAANFAAAAAEUAABx19QAAAREowwoAAQoICAgIc54AewAIfZk=
    summary: IP / ICMP 10.1.231.154 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOCIYAAD8AeQeCvuhyX8AAAELAANFAAAAAEUAABwhzQAAARF86woAAQoICAgIc54AewAIfZk=
    summary: IP / ICMP 10.251.161.201 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOGbTAAD7AaYSCvucGn8AAAELAANFAAAAAEUAABwbvQAAARGC+woAAQoICAgIc54AewAIfZk=
    summary: IP / ICMP 10.251.156.26 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOD1WAAD8Ac2oCvudAX8AAAELAANFAAAAAEUAABwr9QAAARFywwoAAQoICAgIc54AewAIfZk=
    summary: IP / ICMP 10.251.157.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOH6cAAD7Aa0SCvd9VX8AAAELANyRAAAAAEUAABxJwwAAARFU9QoAAQoICAgIc54AewAIpEw=
    summary: IP / ICMP 10.247.125.85 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOCzzAAD6AdwKCvuhwn8AAAELANyRAAAAAEUAABwhMAAAARF9iAoAAQoICAgIc54AewAIpEw=
    summary: IP / ICMP 10.251.161.194 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOGwyAAD5AcQcCmd8BX8AAAELANyRAAAAAEUAABwlAQAAARF5twoAAQoICAgIc54AewAIpEw=
    summary: IP / ICMP 10.103.124.5 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOB9UAAD4AQ4pCgGAPX8AAAELANyRAAAAAEUAABwZwAAAARGE+AoAAQoICAgIc54AewAIpEw=
    summary: IP / ICMP 10.1.128.61 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOAEzAAD3AaE9Cv4LTX8AAAELANyRAAAAAEUAABxWVgAAARFIYgoAAQoICAgIc54AewAIpEw=
    summary: IP / ICMP 10.254.11.77 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOEZsAAD3AVCDWWHIan8AAAELANyRAAAAAEUAABwVYgAAARGJVgoAAQoICAgIc54AewAIpEw=
    summary: IP / ICMP 89.97.200.106 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAYBbkAAD2AYBPWWHI/n8AAAELANyRAAAAAEUAABx+9wAAAREfwQoAAQoICAgIc54AewAIpEwA
      AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    summary: IP / ICMP 89.97.200.254 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror / Padding
- - raw_packet: !!binary |
      RQAAOBWnAAD2AYI1WWDIfn8AAAELANyRAAAAAEUAABxpoQAAARE1FwoAAQoICAgIc54AewAIpEw=
    summary: IP / ICMP 89.96.200.126 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOIKYAAD/ASMiCgABAX8AAAELAPT/AAAAAEUAABzb1QAAAQHC8goAAQoICAgICAD3/wAAAAA=
    summary: IP / ICMP 10.0.1.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / ICMPerror
- - raw_packet: !!binary |
      RQAAOHlSAAB+Ab4fCkjwAX8AAAELAJBjAAAAAEUAABxRTgAAAQFNegoAAQoICAgICABcnAAAAAA=
    summary: IP / ICMP 10.72.240.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / ICMPerror
- - raw_packet: !!binary |
      RcAAOCJxAAD9AZ3uCgHnmn8AAAELAJBjAAAAAEUAABwuRwAAAQFwgQoAAQoICAgICABcnAAAAAA=
    summary: IP / ICMP 10.1.231.154 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / ICMPerror
- - raw_packet: !!binary |
      RcAAOAkgAAD8Af0WCvuhyX8AAAELAJBjAAAAAEUAABxwFwAAAQEusQoAAQoICAgICABcnAAAAAA=
    summary: IP / ICMP 10.251.161.201 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / ICMPerror
- - raw_packet: !!binary |
      RcAAODfFAAD7AdUgCvucGn8AAAELAJBjAAAAAEUAABxTpQAAAQFLIwoAAQoICAgICABcnAAAAAA=
    summary: IP / ICMP 10.251.156.26 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / ICMPerror
- - raw_packet: !!binary |
      RcAAOD5TAAD8AcyrCvudAX8AAAELAJBjAAAAAEUAABwyrQAAAQFsGwoAAQoICAgICABcnAAAAAA=
    summary: IP / ICMP 10.251.157.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / ICMPerror
- - raw_packet: !!binary |
      RcAAOBEIAAD7ARqnCvd9VX8AAAELAJBjAAAAAEUAABwYRwAAAQGGgQoAAQoICAgICABcnAAAAAA=
    summary: IP / ICMP 10.247.125.85 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / ICMPerror
- - raw_packet: !!binary |
      RQAAOC3cAAD6AdshCvuhwn8AAAELAJBjAAAAAEUAABwg9gAAAQF90goAAQoICAgICABcnAAAAAA=
    summary: IP / ICMP 10.251.161.194 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / ICMPerror
- - raw_packet: !!binary |
      RQAAOCthAAD5AQTuCmd8BX8AAAELAJBjAAAAAEUAABwaNQAAAQGEkwoAAQoICAgICABcnAAAAAA=
    summary: IP / ICMP 10.103.124.5 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / ICMPerror
- - raw_packet: !!binary |
      RQAAODD8AAD4AfyACgGAPX8AAAELAJBjAAAAAEUAABwhdgAAAQF9UgoAAQoICAgICABcnAAAAAA=
    summary: IP / ICMP 10.1.128.61 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / ICMPerror
- - raw_packet: !!binary |
      RQAAODi3AAD3AWm5Cv4LTX8AAAELAJBjAAAAAEUAABwoFAAAAQF2tAoAAQoICAgICABcnAAAAAA=
    summary: IP / ICMP 10.254.11.77 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / ICMPerror
- - raw_packet: !!binary |
      RQAAOB6fAAD3AXhQWWHIan8AAAELAJBjAAAAAEUAABwC/gAAAQGbygoAAQoICAgICABcnAAAAAA=
    summary: IP / ICMP 89.97.200.106 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / ICMPerror
- - raw_packet: !!binary |
      RQAAYHGYAAD2ASWbWWHI/n8AAAELAJBjAAAAAEUAABxc4wAAAQFB5QoAAQoICAgICABcnAAAAAAA
      AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    summary: IP / ICMP 89.97.200.254 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / ICMPerror / Padding
- - raw_packet: !!binary |
      RQAAOEnfAAD2AU39WWDIfn8AAAELAJBjAAAAAEUAABwDBwAAAQGbwQoAAQoICAgICABcnAAAAAA=
    summary: IP / ICMP 89.96.200.126 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / ICMPerror
- - raw_packet: !!binary |
      RQAAODaSAAD2AWfjSA7TN38AAAELAJBjAAAAAEUAABxRwAAAAQFNCAoAAQoICAgICABcnAAAAAA=
    summary: IP / ICMP 72.14.211.55 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / ICMPerror
- - raw_packet: !!binary |
      RQAAOEIPAAD1AbX50VXxXH8AAAELAJBjAAAAAEWAABxj+gAAAQE6TgoAAQoICAgICABcnAAAAAA=
    summary: IP / ICMP 209.85.241.92 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / ICMPerror
- - raw_packet: !!binary |
      RQAAqBEjAADwAX3LSA7oTn8AAAELAJBjAAAAAEWAABwY8AAAAQGFWAoAAQoICAgICABcnAAAAAAA
      AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
      AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIADZJgAIAQFczqkB
    summary: IP / ICMP 72.14.232.78 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / ICMPerror / Padding
- - raw_packet: !!binary |
      RQAAOEMpAADxAavH0VX+dH8AAAELAJBjAAAAAEWAABw+9gAAAQFfUgoAAQoICAgICABcnAAAAAA=
    summary: IP / ICMP 209.85.254.116 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / ICMPerror
- - raw_packet: !!binary |
      RQAAHAV3AAArAW9RCAgICH8AAAEAAP//AAAAAA==
    summary: IP / ICMP 8.8.8.8 > 127.0.0.1 echo-reply 0
- - raw_packet: !!binary |
      RQAAHAJwAAArAXJYCAgICH8AAAEAAP//AAAAAA==
    summary: IP / ICMP 8.8.8.8 > 127.0.0.1 echo-reply 0
- - raw_packet: !!binary |
      RQAAOIKSAAD/ASMoCgABAX8AAAELABAzAAAAAEUAABz9GQAAARGhngoAAQoICAgIJAYANQAIwIk=
    summary: IP / ICMP 10.0.1.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAODjdAAD9AYeCCgHnmn8AAAELAANFAAAAAEUAABxkigAAARE6LgoAAQoICAgIJAYANQAIzXc=
    summary: IP / ICMP 10.1.231.154 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAODaTAAD8Ac+jCvuhyX8AAAELAANFAAAAAEUAABwL2wAAARGS3QoAAQoICAgIJAYANQAIzXc=
    summary: IP / ICMP 10.251.161.201 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOBigAAD7AfRFCvucGn8AAAELAANFAAAAAEUAABxTSQAAARFLbwoAAQoICAgIJAYANQAIzXc=
    summary: IP / ICMP 10.251.156.26 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOEegAAD8AcNeCvudAX8AAAELAANFAAAAAEUAABwXpgAAARGHEgoAAQoICAgIJAYANQAIzXc=
    summary: IP / ICMP 10.251.157.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOF63AAD7Acz3Cvd9VX8AAAELANyRAAAAAEUAABw6EgAAARFkpgoAAQoICAgIJAYANQAI9Co=
    summary: IP / ICMP 10.247.125.85 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAODCFAAB+AQbtCkjwAX8AAAELAANFAAAAAEUAABwq2QAAARFz3woAAQoICAgIJAYANQAIzXc=
    summary: IP / ICMP 10.72.240.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOFQoAAD6AbTVCvuhwn8AAAELANyRAAAAAEUAABwOIgAAARGQlgoAAQoICAgIJAYANQAI9Co=
    summary: IP / ICMP 10.251.161.194 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOAyAAAD5ASPPCmd8BX8AAAELANyRAAAAAEUAABxhaAAAARE9UAoAAQoICAgIJAYANQAI9Co=
    summary: IP / ICMP 10.103.124.5 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOAFtAAD4ASwQCgGAPX8AAAELANyRAAAAAEUAABxNUwAAARFRZQoAAQoICAgIJAYANQAI9Co=
    summary: IP / ICMP 10.1.128.61 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOFSiAAD3AU3OCv4LTX8AAAELANyRAAAAAEUAABw9YgAAARFhVgoAAQoICAgIJAYANQAI9Co=
    summary: IP / ICMP 10.254.11.77 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOEjBAAD3AU4uWWHIan8AAAELANyRAAAAAEUAABxokQAAARE2JwoAAQoICAgIJAYANQAI9Co=
    summary: IP / ICMP 89.97.200.106 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAYG2bAAD2ASmYWWHI/n8AAAELANyRAAAAAEUAABx07gAAAREpygoAAQoICAgIJAYANQAI9CoA
      AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    summary: IP / ICMP 89.97.200.254 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror / Padding
- - raw_packet: !!binary |
      RQAAOGhGAAD0ATgNSA7TWX8AAAELANyRAAAAAEUAABxZhwAAARFFMQoAAQoICAgIJAYANQAI9Co=
    summary: IP / ICMP 72.14.211.89 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOCFhAADyAdml0VXxXn8AAAELANyRAAAAAEWAABwSvgAAARGLegoAAQoICAgIJAYANQAI9Co=
    summary: IP / ICMP 209.85.241.94 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOEivAAD2AU8tWWDIfn8AAAELANyRAAAAAEUAABxjEwAAARE7pQoAAQoICAgIJAYANQAI9Co=
    summary: IP / ICMP 89.96.200.126 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAqAQMAADzAYfkSA7oTH8AAAELANyRAAAAAEWAABw3RQAAARFm8woAAQoICAgIJAYANQAI9CoA
      AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
      AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIAAsVwAIAQGJnikB
    summary: IP / ICMP 72.14.232.76 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror / Padding
- - raw_packet: !!binary |
      RQAAOB9EAAD0Acyw0VX+cH8AAAELANyRAAAAAEWAABwwgAAAARFtuAoAAQoICAgIJAYANQAI9Co=
    summary: IP / ICMP 209.85.254.112 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOIKWAAD/ASMkCgABAX8AAAELABAzAAAAAEUAABw/owAAARFfFQoAAQoICAgISZgfkAAIe5w=
    summary: IP / ICMP 10.0.1.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOFSYAAB+AeLZCkjwAX8AAAELAANFAAAAAEUAABw45QAAARFl0woAAQoICAgISZgfkAAIiIo=
    summary: IP / ICMP 10.72.240.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOGbxAAD9AVluCgHnmn8AAAELAANFAAAAAEUAABwTpAAAARGLFAoAAQoICAgISZgfkAAIiIo=
    summary: IP / ICMP 10.1.231.154 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOE4qAAD8AbgMCvuhyX8AAAELAANFAAAAAEUAABwCWgAAARGcXgoAAQoICAgISZgfkAAIiIo=
    summary: IP / ICMP 10.251.161.201 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOAoSAAD7AQLUCvucGn8AAAELAANFAAAAAEUAABwlRgAAARF5cgoAAQoICAgISZgfkAAIiIo=
    summary: IP / ICMP 10.251.156.26 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOArZAAD8AQAmCvudAX8AAAELAANFAAAAAEUAABxmogAAARE4FgoAAQoICAgISZgfkAAIiIo=
    summary: IP / ICMP 10.251.157.1 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RcAAOFSsAAD7AdcCCvd9VX8AAAELANyRAAAAAEUAABxb5gAAARFC0goAAQoICAgISZgfkAAIrz0=
    summary: IP / ICMP 10.247.125.85 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOEXfAAD6AcMeCvuhwn8AAAELANyRAAAAAEUAABwyUQAAARFsZwoAAQoICAgISZgfkAAIrz0=
    summary: IP / ICMP 10.251.161.194 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOBG3AAD5AR6YCmd8BX8AAAELANyRAAAAAEUAABwhUQAAARF9ZwoAAQoICAgISZgfkAAIrz0=
    summary: IP / ICMP 10.103.124.5 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOGqaAAD4AcLiCgGAPX8AAAELANyRAAAAAEUAABxRkQAAARFNJwoAAQoICAgISZgfkAAIrz0=
    summary: IP / ICMP 10.1.128.61 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAODJzAAD3AW/9Cv4LTX8AAAELANyRAAAAAEUAABxrGQAAAREznwoAAQoICAgISZgfkAAIrz0=
    summary: IP / ICMP 10.254.11.77 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAOA3LAAD3AYkkWWHIan8AAAELANyRAAAAAEUAABwaIAAAARGEmAoAAQoICAgISZgfkAAIrz0=
    summary: IP / ICMP 89.97.200.106 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
- - raw_packet: !!binary |
      RQAAYGx9AAD2ASq2WWHI/n8AAAELANyRAAAAAEUAABwmjgAAARF4KgoAAQoICAgISZgfkAAIrz0A
      AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
    summary: IP / ICMP 89.97.200.254 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror / Padding
- - raw_packet: !!binary |
      RQAAOB9FAAD2AXiPWWDIhn8AAAELANyRAAAAAEUAABwPIwAAARGPlQoAAQoICAgISZgfkAAIrz0=
    summary: IP / ICMP 89.96.200.134 > 127.0.0.1 time-exceeded ttl-zero-during-transit
      / IPerror / UDPerror
input: null
max_ttl: 30
sent_packets:
- - raw_packet: !!binary |
      RQAAKFw+AAABBs6BfwAAAQgICAiDDgAAAAAAAAAAAABQAiAAfcMAAA==
    summary: IP / TCP 127.0.0.1:33550 > 8.8.8.8:0 S
- - raw_packet: !!binary |
      RQAAKJOnAAACBpYYfwAAAQgICAiDDgAAAAAAAAAAAABQAiAAfcMAAA==
    summary: IP / TCP 127.0.0.1:33550 > 8.8.8.8:0 S
- - raw_packet: !!binary |
      RQAAKJHqAAADBpbVfwAAAQgICAiDDgAAAAAAAAAAAABQAiAAfcMAAA==
    summary: IP / TCP 127.0.0.1:33550 > 8.8.8.8:0 S
- - raw_packet: !!binary |
      RQAAKBF+AAAEBhZCfwAAAQgICAiDDgAAAAAAAAAAAABQAiAAfcMAAA==
    summary: IP / TCP 127.0.0.1:33550 > 8.8.8.8:0 S
- - raw_packet: !!binary |
      RQAAKOzMAAAFBjnzfwAAAQgICAiDDgAAAAAAAAAAAABQAiAAfcMAAA==
    summary: IP / TCP 127.0.0.1:33550 > 8.8.8.8:0 S
- - raw_packet: !!binary |
      RQAAKFSQAAABBtYvfwAAAQgICAgT6QAXAAAAAAAAAABQAiAA7NEAAA==
    summary: IP / TCP 127.0.0.1:5097 > 8.8.8.8:telnet S
- - raw_packet: !!binary |
      RQAAKIDRAAACBqjufwAAAQgICAgT6QAXAAAAAAAAAABQAiAA7NEAAA==
    summary: IP / TCP 127.0.0.1:5097 > 8.8.8.8:telnet S
- - raw_packet: !!binary |
      RQAAKLfnAAADBnDYfwAAAQgICAgT6QAXAAAAAAAAAABQAiAA7NEAAA==
    summary: IP / TCP 127.0.0.1:5097 > 8.8.8.8:telnet S
- - raw_packet: !!binary |
      RQAAKDn1AAAEBu3KfwAAAQgICAgT6QAXAAAAAAAAAABQAiAA7NEAAA==
    summary: IP / TCP 127.0.0.1:5097 > 8.8.8.8:telnet S
- - raw_packet: !!binary |
      RQAAKLfdAAAFBm7ifwAAAQgICAgT6QAXAAAAAAAAAABQAiAA7NEAAA==
    summary: IP / TCP 127.0.0.1:5097 > 8.8.8.8:telnet S
- - raw_packet: !!binary |
      RQAAKOvCAAAGBjn9fwAAAQgICAgT6QAXAAAAAAAAAABQAiAA7NEAAA==
    summary: IP / TCP 127.0.0.1:5097 > 8.8.8.8:telnet S
- - raw_packet: !!binary |
      RQAAKOK/AAAHBkIAfwAAAQgICAgT6QAXAAAAAAAAAABQAiAA7NEAAA==
    summary: IP / TCP 127.0.0.1:5097 > 8.8.8.8:telnet S
- - raw_packet: !!binary |
      RQAAKLztAAAIBmbSfwAAAQgICAgT6QAXAAAAAAAAAABQAiAA7NEAAA==
    summary: IP / TCP 127.0.0.1:5097 > 8.8.8.8:telnet S
- - raw_packet: !!binary |
      RQAAKAzkAAAJBhXcfwAAAQgICAgT6QAXAAAAAAAAAABQAiAA7NEAAA==
    summary: IP / TCP 127.0.0.1:5097 > 8.8.8.8:telnet S
- - raw_packet: !!binary |
      RQAAKDFvAAAKBvBQfwAAAQgICAgT6QAXAAAAAAAAAABQAiAA7NEAAA==
    summary: IP / TCP 127.0.0.1:5097 > 8.8.8.8:telnet S
- - raw_packet: !!binary |
      RQAAKOEmAAALBj+ZfwAAAQgICAgT6QAXAAAAAAAAAABQAiAA7NEAAA==
    summary: IP / TCP 127.0.0.1:5097 > 8.8.8.8:telnet S
- - raw_packet: !!binary |
      RQAAKFMvAAAMBsyQfwAAAQgICAgT6QAXAAAAAAAAAABQAiAA7NEAAA==
    summary: IP / TCP 127.0.0.1:5097 > 8.8.8.8:telnet S
- - raw_packet: !!binary |
      RQAAKMN6AAANBltFfwAAAQgICAgT6QAXAAAAAAAAAABQAiAA7NEAAA==
    summary: IP / TCP 127.0.0.1:5097 > 8.8.8.8:telnet S
- - raw_packet: !!binary |
      RQAAKG0nAAAOBrCYfwAAAQgICAgT6QAXAAAAAAAAAABQAiAA7NEAAA==
    summary: IP / TCP 127.0.0.1:5097 > 8.8.8.8:telnet S
- - raw_packet: !!binary |
      RQAAKHbiAAABBrPdfwAAAQgICAi+WABQAAAAAAAAAABQAiAAQikAAA==
    summary: IP / TCP 127.0.0.1:48728 > 8.8.8.8:http S
- - raw_packet: !!binary |
      RQAAKGakAAACBsMbfwAAAQgICAi+WABQAAAAAAAAAABQAiAAQikAAA==
    summary: IP / TCP 127.0.0.1:48728 > 8.8.8.8:http S
- - raw_packet: !!binary |
      RQAAKEY6AAADBuKFfwAAAQgICAi+WABQAAAAAAAAAABQAiAAQikAAA==
    summary: IP / TCP 127.0.0.1:48728 > 8.8.8.8:http S
- - raw_packet: !!binary |
      RQAAKB2mAAAEBgoafwAAAQgICAi+WABQAAAAAAAAAABQAiAAQikAAA==
    summary: IP / TCP 127.0.0.1:48728 > 8.8.8.8:http S
- - raw_packet: !!binary |
      RQAAKMM1AAAFBmOKfwAAAQgICAi+WABQAAAAAAAAAABQAiAAQikAAA==
    summary: IP / TCP 127.0.0.1:48728 > 8.8.8.8:http S
- - raw_packet: !!binary |
      RQAAKCInAAAGBgOZfwAAAQgICAi+WABQAAAAAAAAAABQAiAAQikAAA==
    summary: IP / TCP 127.0.0.1:48728 > 8.8.8.8:http S
- - raw_packet: !!binary |
      RQAAKDT8AAAHBu/DfwAAAQgICAi+WABQAAAAAAAAAABQAiAAQikAAA==
    summary: IP / TCP 127.0.0.1:48728 > 8.8.8.8:http S
- - raw_packet: !!binary |
      RQAAKALhAAAIBiDffwAAAQgICAi+WABQAAAAAAAAAABQAiAAQikAAA==
    summary: IP / TCP 127.0.0.1:48728 > 8.8.8.8:http S
- - raw_packet: !!binary |
      RQAAKGNpAAAJBr9WfwAAAQgICAi+WABQAAAAAAAAAABQAiAAQikAAA==
    summary: IP / TCP 127.0.0.1:48728 > 8.8.8.8:http S
- - raw_packet: !!binary |
      RQAAKOoKAAAKBje1fwAAAQgICAi+WABQAAAAAAAAAABQAiAAQikAAA==
    summary: IP / TCP 127.0.0.1:48728 > 8.8.8.8:http S
- - raw_packet: !!binary |
      RQAAKNFAAAALBk9/fwAAAQgICAi+WABQAAAAAAAAAABQAiAAQikAAA==
    summary: IP / TCP 127.0.0.1:48728 > 8.8.8.8:http S
- - raw_packet: !!binary |
      RQAAKNCoAAAMBk8XfwAAAQgICAi+WABQAAAAAAAAAABQAiAAQikAAA==
    summary: IP / TCP 127.0.0.1:48728 > 8.8.8.8:http S
- - raw_packet: !!binary |
      RQAAKPa+AAANBigBfwAAAQgICAi+WABQAAAAAAAAAABQAiAAQikAAA==
    summary: IP / TCP 127.0.0.1:48728 > 8.8.8.8:http S
- - raw_packet: !!binary |
      RQAAKI+PAAAOBo4wfwAAAQgICAi+WABQAAAAAAAAAABQAiAAQikAAA==
    summary: IP / TCP 127.0.0.1:48728 > 8.8.8.8:http S
- - raw_packet: !!binary |
      RQAAKBHKAAABBhj2fwAAAQgICAj5kQG7AAAAAAAAAABQAiAABYUAAA==
    summary: IP / TCP 127.0.0.1:63889 > 8.8.8.8:https S
- - raw_packet: !!binary |
      RQAAKHwXAAACBq2ofwAAAQgICAj5kQG7AAAAAAAAAABQAiAABYUAAA==
    summary: IP / TCP 127.0.0.1:63889 > 8.8.8.8:https S
- - raw_packet: !!binary |
      RQAAKOu2AAADBj0JfwAAAQgICAj5kQG7AAAAAAAAAABQAiAABYUAAA==
    summary: IP / TCP 127.0.0.1:63889 > 8.8.8.8:https S
- - raw_packet: !!binary |
      RQAAKCrXAAAEBvzofwAAAQgICAj5kQG7AAAAAAAAAABQAiAABYUAAA==
    summary: IP / TCP 127.0.0.1:63889 > 8.8.8.8:https S
- - raw_packet: !!binary |
      RQAAKAjxAAAFBh3PfwAAAQgICAj5kQG7AAAAAAAAAABQAiAABYUAAA==
    summary: IP / TCP 127.0.0.1:63889 > 8.8.8.8:https S
- - raw_packet: !!binary |
      RQAAKABrAAAGBiVVfwAAAQgICAj5kQG7AAAAAAAAAABQAiAABYUAAA==
    summary: IP / TCP 127.0.0.1:63889 > 8.8.8.8:https S
- - raw_packet: !!binary |
      RQAAKPZ9AAAHBi5CfwAAAQgICAj5kQG7AAAAAAAAAABQAiAABYUAAA==
    summary: IP / TCP 127.0.0.1:63889 > 8.8.8.8:https S
- - raw_packet: !!binary |
      RQAAKHDaAAAIBrLlfwAAAQgICAj5kQG7AAAAAAAAAABQAiAABYUAAA==
    summary: IP / TCP 127.0.0.1:63889 > 8.8.8.8:https S
- - raw_packet: !!binary |
      RQAAKEstAAAJBteSfwAAAQgICAj5kQG7AAAAAAAAAABQAiAABYUAAA==
    summary: IP / TCP 127.0.0.1:63889 > 8.8.8.8:https S
- - raw_packet: !!binary |
      RQAAKM77AAAKBlLEfwAAAQgICAj5kQG7AAAAAAAAAABQAiAABYUAAA==
    summary: IP / TCP 127.0.0.1:63889 > 8.8.8.8:https S
- - raw_packet: !!binary |
      RQAAKD9mAAALBuFZfwAAAQgICAj5kQG7AAAAAAAAAABQAiAABYUAAA==
    summary: IP / TCP 127.0.0.1:63889 > 8.8.8.8:https S
- - raw_packet: !!binary |
      RQAAKHghAAAMBqeefwAAAQgICAj5kQG7AAAAAAAAAABQAiAABYUAAA==
    summary: IP / TCP 127.0.0.1:63889 > 8.8.8.8:https S
- - raw_packet: !!binary |
      RQAAKHPmAAANBqrZfwAAAQgICAj5kQG7AAAAAAAAAABQAiAABYUAAA==
    summary: IP / TCP 127.0.0.1:63889 > 8.8.8.8:https S
- - raw_packet: !!binary |
      RQAAKDRMAAABBvZzfwAAAQgICAiRrf//AAAAAAAAAABQAiAAbyQAAA==
    summary: IP / TCP 127.0.0.1:37293 > 8.8.8.8:65535 S
- - raw_packet: !!binary |
      RQAAKE9xAAACBtpOfwAAAQgICAiRrf//AAAAAAAAAABQAiAAbyQAAA==
    summary: IP / TCP 127.0.0.1:37293 > 8.8.8.8:65535 S
- - raw_packet: !!binary |
      RQAAKPf1AAADBjDKfwAAAQgICAiRrf//AAAAAAAAAABQAiAAbyQAAA==
    summary: IP / TCP 127.0.0.1:37293 > 8.8.8.8:65535 S
- - raw_packet: !!binary |
      RQAAKD89AAAEBuiCfwAAAQgICAiRrf//AAAAAAAAAABQAiAAbyQAAA==
    summary: IP / TCP 127.0.0.1:37293 > 8.8.8.8:65535 S
- - raw_packet: !!binary |
      RQAAKLQcAAAFBnKjfwAAAQgICAiRrf//AAAAAAAAAABQAiAAbyQAAA==
    summary: IP / TCP 127.0.0.1:37293 > 8.8.8.8:65535 S
- - raw_packet: !!binary |
      RQAAKIQOAAAGBqGxfwAAAQgICAiRrf//AAAAAAAAAABQAiAAbyQAAA==
    summary: IP / TCP 127.0.0.1:37293 > 8.8.8.8:65535 S
- - raw_packet: !!binary |
      RQAAKCaJAAAHBv42fwAAAQgICAiRrf//AAAAAAAAAABQAiAAbyQAAA==
    summary: IP / TCP 127.0.0.1:37293 > 8.8.8.8:65535 S
- - raw_packet: !!binary |
      RQAAKOHXAAAIBkHofwAAAQgICAiRrf//AAAAAAAAAABQAiAAbyQAAA==
    summary: IP / TCP 127.0.0.1:37293 > 8.8.8.8:65535 S
- - raw_packet: !!binary |
      RQAAKOJcAAAJBkBjfwAAAQgICAiRrf//AAAAAAAAAABQAiAAbyQAAA==
    summary: IP / TCP 127.0.0.1:37293 > 8.8.8.8:65535 S
- - raw_packet: !!binary |
      RQAAKB1MAAAKBgR0fwAAAQgICAiRrf//AAAAAAAAAABQAiAAbyQAAA==
    summary: IP / TCP 127.0.0.1:37293 > 8.8.8.8:65535 S
- - raw_packet: !!binary |
      RQAAKNSbAAALBkwkfwAAAQgICAiRrf//AAAAAAAAAABQAiAAbyQAAA==
    summary: IP / TCP 127.0.0.1:37293 > 8.8.8.8:65535 S
- - raw_packet: !!binary |
      RQAAKKgHAAAMBne4fwAAAQgICAiRrf//AAAAAAAAAABQAiAAbyQAAA==
    summary: IP / TCP 127.0.0.1:37293 > 8.8.8.8:65535 S
- - raw_packet: !!binary |
      RQAAKJsTAAANBoOsfwAAAQgICAiRrf//AAAAAAAAAABQAiAAbyQAAA==
    summary: IP / TCP 127.0.0.1:37293 > 8.8.8.8:65535 S
- - raw_packet: !!binary |
      RQAAKN2lAAAOBkAafwAAAQgICAiRrf//AAAAAAAAAABQAiAAbyQAAA==
    summary: IP / TCP 127.0.0.1:37293 > 8.8.8.8:65535 S
- - raw_packet: !!binary |
      RQAAKICDAAABBqo8fwAAAQgICAh7SQAWAAAAAAAAAABQAiAAhXIAAA==
    summary: IP / TCP 127.0.0.1:31561 > 8.8.8.8:ssh S
- - raw_packet: !!binary |
      RQAAKAdOAAACBiJyfwAAAQgICAh7SQAWAAAAAAAAAABQAiAAhXIAAA==
    summary: IP / TCP 127.0.0.1:31561 > 8.8.8.8:ssh S
- - raw_packet: !!binary |
      RQAAKK/mAAADBnjZfwAAAQgICAh7SQAWAAAAAAAAAABQAiAAhXIAAA==
    summary: IP / TCP 127.0.0.1:31561 > 8.8.8.8:ssh S
- - raw_packet: !!binary |
      RQAAKDrnAAAEBuzYfwAAAQgICAh7SQAWAAAAAAAAAABQAiAAhXIAAA==
    summary: IP / TCP 127.0.0.1:31561 > 8.8.8.8:ssh S
- - raw_packet: !!binary |
      RQAAKED8AAAFBuXDfwAAAQgICAh7SQAWAAAAAAAAAABQAiAAhXIAAA==
    summary: IP / TCP 127.0.0.1:31561 > 8.8.8.8:ssh S
- - raw_packet: !!binary |
      RQAAKC/2AAAGBvXJfwAAAQgICAh7SQAWAAAAAAAAAABQAiAAhXIAAA==
    summary: IP / TCP 127.0.0.1:31561 > 8.8.8.8:ssh S
- - raw_packet: !!binary |
      RQAAKNPSAAAHBlDtfwAAAQgICAh7SQAWAAAAAAAAAABQAiAAhXIAAA==
    summary: IP / TCP 127.0.0.1:31561 > 8.8.8.8:ssh S
- - raw_packet: !!binary |
      RQAAKK8cAAAIBnSjfwAAAQgICAh7SQAWAAAAAAAAAABQAiAAhXIAAA==
    summary: IP / TCP 127.0.0.1:31561 > 8.8.8.8:ssh S
- - raw_packet: !!binary |
      RQAAKCjAAAAJBvn/fwAAAQgICAh7SQAWAAAAAAAAAABQAiAAhXIAAA==
    summary: IP / TCP 127.0.0.1:31561 > 8.8.8.8:ssh S
- - raw_packet: !!binary |
      RQAAKBsiAAAKBgaefwAAAQgICAh7SQAWAAAAAAAAAABQAiAAhXIAAA==
    summary: IP / TCP 127.0.0.1:31561 > 8.8.8.8:ssh S
- - raw_packet: !!binary |
      RQAAKBzHAAALBgP5fwAAAQgICAh7SQAWAAAAAAAAAABQAiAAhXIAAA==
    summary: IP / TCP 127.0.0.1:31561 > 8.8.8.8:ssh S
- - raw_packet: !!binary |
      RQAAKNJzAAAMBk1MfwAAAQgICAh7SQAWAAAAAAAAAABQAiAAhXIAAA==
    summary: IP / TCP 127.0.0.1:31561 > 8.8.8.8:ssh S
- - raw_packet: !!binary |
      RQAAKFORAAANBssufwAAAQgICAh7SQAWAAAAAAAAAABQAiAAhXIAAA==
    summary: IP / TCP 127.0.0.1:31561 > 8.8.8.8:ssh S
- - raw_packet: !!binary |
      RQAAKAQSAAAOBhmufwAAAQgICAh7SQAWAAAAAAAAAABQAiAAhXIAAA==
    summary: IP / TCP 127.0.0.1:31561 > 8.8.8.8:ssh S
- - raw_packet: !!binary |
      RQAAKHO+AAABBrcBfwAAAQgICAgp6QB7AAAAAAAAAABQAiAA1m0AAA==
    summary: IP / TCP 127.0.0.1:10729 > 8.8.8.8:ntp S
- - raw_packet: !!binary |
      RQAAKOH2AAACBkfJfwAAAQgICAgp6QB7AAAAAAAAAABQAiAA1m0AAA==
    summary: IP / TCP 127.0.0.1:10729 > 8.8.8.8:ntp S
- - raw_packet: !!binary |
      RQAAKBSMAAADBhQ0fwAAAQgICAgp6QB7AAAAAAAAAABQAiAA1m0AAA==
    summary: IP / TCP 127.0.0.1:10729 > 8.8.8.8:ntp S
- - raw_packet: !!binary |
      RQAAKMRzAAAEBmNMfwAAAQgICAgp6QB7AAAAAAAAAABQAiAA1m0AAA==
    summary: IP / TCP 127.0.0.1:10729 > 8.8.8.8:ntp S
- - raw_packet: !!binary |
      RQAAKBnkAAAFBgzcfwAAAQgICAgp6QB7AAAAAAAAAABQAiAA1m0AAA==
    summary: IP / TCP 127.0.0.1:10729 > 8.8.8.8:ntp S
- - raw_packet: !!binary |
      RQAAKKVKAAAGBoB1fwAAAQgICAgp6QB7AAAAAAAAAABQAiAA1m0AAA==
    summary: IP / TCP 127.0.0.1:10729 > 8.8.8.8:ntp S
- - raw_packet: !!binary |
      RQAAKOSdAAAHBkAifwAAAQgICAgp6QB7AAAAAAAAAABQAiAA1m0AAA==
    summary: IP / TCP 127.0.0.1:10729 > 8.8.8.8:ntp S
- - raw_packet: !!binary |
      RQAAKFpKAAAIBsl1fwAAAQgICAgp6QB7AAAAAAAAAABQAiAA1m0AAA==
    summary: IP / TCP 127.0.0.1:10729 > 8.8.8.8:ntp S
- - raw_packet: !!binary |
      RQAAKO//AAAJBjLAfwAAAQgICAgp6QB7AAAAAAAAAABQAiAA1m0AAA==
    summary: IP / TCP 127.0.0.1:10729 > 8.8.8.8:ntp S
- - raw_packet: !!binary |
      RQAAKNahAAAKBksefwAAAQgICAgp6QB7AAAAAAAAAABQAiAA1m0AAA==
    summary: IP / TCP 127.0.0.1:10729 > 8.8.8.8:ntp S
- - raw_packet: !!binary |
      RQAAKFp7AAALBsZEfwAAAQgICAgp6QB7AAAAAAAAAABQAiAA1m0AAA==
    summary: IP / TCP 127.0.0.1:10729 > 8.8.8.8:ntp S
- - raw_packet: !!binary |
      RQAAKAA8AAAMBh+EfwAAAQgICAgp6QB7AAAAAAAAAABQAiAA1m0AAA==
    summary: IP / TCP 127.0.0.1:10729 > 8.8.8.8:ntp S
- - raw_packet: !!binary |
      RQAAKJGTAAANBo0sfwAAAQgICAgp6QB7AAAAAAAAAABQAiAA1m0AAA==
    summary: IP / TCP 127.0.0.1:10729 > 8.8.8.8:ntp S
- - raw_packet: !!binary |
      RQAAKH97AAAOBp5EfwAAAQgICAgp6QB7AAAAAAAAAABQAiAA1m0AAA==
    summary: IP / TCP 127.0.0.1:10729 > 8.8.8.8:ntp S
- - raw_packet: !!binary |
      RQAAKLrvAAABBm/QfwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKGt6AAACBr5FfwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKMpXAAADBl5ofwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKJWZAAAEBpImfwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKNMhAAAFBlOefwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKNVIAAAGBlB3fwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKL1bAAAHBmdkfwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKIiDAAAIBps8fwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKINsAAAJBp9TfwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKOAbAAAKBkGkfwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKNldAAALBkdifwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKNSEAAAMBks7fwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKPsJAAANBiO2fwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKNXHAAAOBkf4fwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKP+lAAAPBh0afwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKFKwAAAQBskPfwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKIngAAARBpDffwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKGkqAAASBrCVfwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKPzhAAATBhvefwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKC7oAAAUBujXfwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKJGvAAAVBoUQfwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKL4+AAAWBleBfwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKBY+AAAXBv6BfwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKOsMAAAYBiizfwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKPBTAAAZBiJsfwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKFnUAAAaBrfrfwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKCPlAAAbBuzafwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKAW7AAAcBgoFfwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKAbRAAAdBgfvfwAAAQgICAgZFQA1AAAAAAAAAABQAiAA54cAAA==
    summary: IP / TCP 127.0.0.1:nim_wan > 8.8.8.8:domain S
- - raw_packet: !!binary |
      RQAAKOU9AAABBkWCfwAAAQgICAjiHB+QAAAAAAAAAABQAiAA/yQAAA==
    summary: IP / TCP 127.0.0.1:57884 > 8.8.8.8:http_alt S
- - raw_packet: !!binary |
      RQAAKNFOAAACBlhxfwAAAQgICAjiHB+QAAAAAAAAAABQAiAA/yQAAA==
    summary: IP / TCP 127.0.0.1:57884 > 8.8.8.8:http_alt S
- - raw_packet: !!binary |
      RQAAKES6AAADBuQFfwAAAQgICAjiHB+QAAAAAAAAAABQAiAA/yQAAA==
    summary: IP / TCP 127.0.0.1:57884 > 8.8.8.8:http_alt S
- - raw_packet: !!binary |
      RQAAKFqUAAAEBs0rfwAAAQgICAjiHB+QAAAAAAAAAABQAiAA/yQAAA==
    summary: IP / TCP 127.0.0.1:57884 > 8.8.8.8:http_alt S
- - raw_packet: !!binary |
      RQAAKKHtAAAFBoTSfwAAAQgICAjiHB+QAAAAAAAAAABQAiAA/yQAAA==
    summary: IP / TCP 127.0.0.1:57884 > 8.8.8.8:http_alt S
- - raw_packet: !!binary |
      RQAAKMIXAAAGBmOofwAAAQgICAjiHB+QAAAAAAAAAABQAiAA/yQAAA==
    summary: IP / TCP 127.0.0.1:57884 > 8.8.8.8:http_alt S
- - raw_packet: !!binary |
      RQAAKBphAAAHBgpffwAAAQgICAjiHB+QAAAAAAAAAABQAiAA/yQAAA==
    summary: IP / TCP 127.0.0.1:57884 > 8.8.8.8:http_alt S
- - raw_packet: !!binary |
      RQAAKMb7AAAIBlzEfwAAAQgICAjiHB+QAAAAAAAAAABQAiAA/yQAAA==
    summary: IP / TCP 127.0.0.1:57884 > 8.8.8.8:http_alt S
- - raw_packet: !!binary |
      RQAAKMoDAAAJBli8fwAAAQgICAjiHB+QAAAAAAAAAABQAiAA/yQAAA==
    summary: IP / TCP 127.0.0.1:57884 > 8.8.8.8:http_alt S
- - raw_packet: !!binary |
      RQAAKMjKAAAKBlj1fwAAAQgICAjiHB+QAAAAAAAAAABQAiAA/yQAAA==
    summary: IP / TCP 127.0.0.1:57884 > 8.8.8.8:http_alt S
- - raw_packet: !!binary |
      RQAAKBj7AAALBgfFfwAAAQgICAjiHB+QAAAAAAAAAABQAiAA/yQAAA==
    summary: IP / TCP 127.0.0.1:57884 > 8.8.8.8:http_alt S
- - raw_packet: !!binary |
      RQAAKAyCAAAMBhM+fwAAAQgICAjiHB+QAAAAAAAAAABQAiAA/yQAAA==
    summary: IP / TCP 127.0.0.1:57884 > 8.8.8.8:http_alt S
- - raw_packet: !!binary |
      RQAAKOF2AAANBj1JfwAAAQgICAjiHB+QAAAAAAAAAABQAiAA/yQAAA==
    summary: IP / TCP 127.0.0.1:57884 > 8.8.8.8:http_alt S
- - raw_packet: !!binary |
      RQAAHC8WAAABEfuqfwAAAQgICAirBgAXAAjFrw==
    summary: IP / UDP 127.0.0.1:43782 > 8.8.8.8:telnet
- - raw_packet: !!binary |
      RQAAHAJCAAACESd/fwAAAQgICAirBgAXAAjFrw==
    summary: IP / UDP 127.0.0.1:43782 > 8.8.8.8:telnet
- - raw_packet: !!binary |
      RQAAHIQuAAADEaSSfwAAAQgICAirBgAXAAjFrw==
    summary: IP / UDP 127.0.0.1:43782 > 8.8.8.8:telnet
- - raw_packet: !!binary |
      RQAAHIXkAAAEEaHcfwAAAQgICAirBgAXAAjFrw==
    summary: IP / UDP 127.0.0.1:43782 > 8.8.8.8:telnet
- - raw_packet: !!binary |
      RQAAHPTkAAAFETHcfwAAAQgICAirBgAXAAjFrw==
    summary: IP / UDP 127.0.0.1:43782 > 8.8.8.8:telnet
- - raw_packet: !!binary |
      RQAAHH1mAAAGEahafwAAAQgICAirBgAXAAjFrw==
    summary: IP / UDP 127.0.0.1:43782 > 8.8.8.8:telnet
- - raw_packet: !!binary |
      RQAAHBr3AAAHEQnKfwAAAQgICAirBgAXAAjFrw==
    summary: IP / UDP 127.0.0.1:43782 > 8.8.8.8:telnet
- - raw_packet: !!binary |
      RQAAHB4VAAAIEQWsfwAAAQgICAirBgAXAAjFrw==
    summary: IP / UDP 127.0.0.1:43782 > 8.8.8.8:telnet
- - raw_packet: !!binary |
      RQAAHEOxAAAJEd8PfwAAAQgICAirBgAXAAjFrw==
    summary: IP / UDP 127.0.0.1:43782 > 8.8.8.8:telnet
- - raw_packet: !!binary |
      RQAAHLfFAAAKEWn7fwAAAQgICAirBgAXAAjFrw==
    summary: IP / UDP 127.0.0.1:43782 > 8.8.8.8:telnet
- - raw_packet: !!binary |
      RQAAHEJ8AAALEd5EfwAAAQgICAirBgAXAAjFrw==
    summary: IP / UDP 127.0.0.1:43782 > 8.8.8.8:telnet
- - raw_packet: !!binary |
      RQAAHEmwAAAMEdYQfwAAAQgICAirBgAXAAjFrw==
    summary: IP / UDP 127.0.0.1:43782 > 8.8.8.8:telnet
- - raw_packet: !!binary |
      RQAAHPOaAAANESsmfwAAAQgICAirBgAXAAjFrw==
    summary: IP / UDP 127.0.0.1:43782 > 8.8.8.8:telnet
- - raw_packet: !!binary |
      RQAAHCQqAAAOEfmWfwAAAQgICAirBgAXAAjFrw==
    summary: IP / UDP 127.0.0.1:43782 > 8.8.8.8:telnet
- - raw_packet: !!binary |
      RQAAHDIRAAABEfivfwAAAQgICAhdjwBQAAgS7g==
    summary: IP / UDP 127.0.0.1:23951 > 8.8.8.8:http
- - raw_packet: !!binary |
      RQAAHKViAAACEYRefwAAAQgICAhdjwBQAAgS7g==
    summary: IP / UDP 127.0.0.1:23951 > 8.8.8.8:http
- - raw_packet: !!binary |
      RQAAHBDAAAADERgBfwAAAQgICAhdjwBQAAgS7g==
    summary: IP / UDP 127.0.0.1:23951 > 8.8.8.8:http
- - raw_packet: !!binary |
      RQAAHIOvAAAEEaQRfwAAAQgICAhdjwBQAAgS7g==
    summary: IP / UDP 127.0.0.1:23951 > 8.8.8.8:http
- - raw_packet: !!binary |
      RQAAHNa1AAAFEVALfwAAAQgICAhdjwBQAAgS7g==
    summary: IP / UDP 127.0.0.1:23951 > 8.8.8.8:http
- - raw_packet: !!binary |
      RQAAHEkaAAAGEdymfwAAAQgICAhdjwBQAAgS7g==
    summary: IP / UDP 127.0.0.1:23951 > 8.8.8.8:http
- - raw_packet: !!binary |
      RQAAHHEFAAAHEbO7fwAAAQgICAhdjwBQAAgS7g==
    summary: IP / UDP 127.0.0.1:23951 > 8.8.8.8:http
- - raw_packet: !!binary |
      RQAAHG+qAAAIEbQWfwAAAQgICAhdjwBQAAgS7g==
    summary: IP / UDP 127.0.0.1:23951 > 8.8.8.8:http
- - raw_packet: !!binary |
      RQAAHLRcAAAJEW5kfwAAAQgICAhdjwBQAAgS7g==
    summary: IP / UDP 127.0.0.1:23951 > 8.8.8.8:http
- - raw_packet: !!binary |
      RQAAHO71AAAKETLLfwAAAQgICAhdjwBQAAgS7g==
    summary: IP / UDP 127.0.0.1:23951 > 8.8.8.8:http
- - raw_packet: !!binary |
      RQAAHD0DAAALEeO9fwAAAQgICAhdjwBQAAgS7g==
    summary: IP / UDP 127.0.0.1:23951 > 8.8.8.8:http
- - raw_packet: !!binary |
      RQAAHJAkAAAMEY+cfwAAAQgICAhdjwBQAAgS7g==
    summary: IP / UDP 127.0.0.1:23951 > 8.8.8.8:http
- - raw_packet: !!binary |
      RQAAHNlvAAANEUVRfwAAAQgICAhdjwBQAAgS7g==
    summary: IP / UDP 127.0.0.1:23951 > 8.8.8.8:http
- - raw_packet: !!binary |
      RQAAHECQAAAOEd0wfwAAAQgICAhdjwBQAAgS7g==
    summary: IP / UDP 127.0.0.1:23951 > 8.8.8.8:http
- - raw_packet: !!binary |
      RQAAHBqGAAABERA7fwAAAQgICAg9pgG7AAgxbA==
    summary: IP / UDP 127.0.0.1:15782 > 8.8.8.8:https
- - raw_packet: !!binary |
      RQAAHLTJAAACEXT3fwAAAQgICAg9pgG7AAgxbA==
    summary: IP / UDP 127.0.0.1:15782 > 8.8.8.8:https
- - raw_packet: !!binary |
      RQAAHNdaAAADEVFmfwAAAQgICAg9pgG7AAgxbA==
    summary: IP / UDP 127.0.0.1:15782 > 8.8.8.8:https
- - raw_packet: !!binary |
      RQAAHGUXAAAEEcKpfwAAAQgICAg9pgG7AAgxbA==
    summary: IP / UDP 127.0.0.1:15782 > 8.8.8.8:https
- - raw_packet: !!binary |
      RQAAHNbNAAAFEU/zfwAAAQgICAg9pgG7AAgxbA==
    summary: IP / UDP 127.0.0.1:15782 > 8.8.8.8:https
- - raw_packet: !!binary |
      RQAAHCrmAAAGEfrafwAAAQgICAg9pgG7AAgxbA==
    summary: IP / UDP 127.0.0.1:15782 > 8.8.8.8:https
- - raw_packet: !!binary |
      RQAAHBmqAAAHEQsXfwAAAQgICAg9pgG7AAgxbA==
    summary: IP / UDP 127.0.0.1:15782 > 8.8.8.8:https
- - raw_packet: !!binary |
      RQAAHO0hAAAIETaffwAAAQgICAg9pgG7AAgxbA==
    summary: IP / UDP 127.0.0.1:15782 > 8.8.8.8:https
- - raw_packet: !!binary |
      RQAAHF5DAAAJEcR9fwAAAQgICAg9pgG7AAgxbA==
    summary: IP / UDP 127.0.0.1:15782 > 8.8.8.8:https
- - raw_packet: !!binary |
      RQAAHMFJAAAKEWB3fwAAAQgICAg9pgG7AAgxbA==
    summary: IP / UDP 127.0.0.1:15782 > 8.8.8.8:https
- - raw_packet: !!binary |
      RQAAHAqRAAALERYwfwAAAQgICAg9pgG7AAgxbA==
    summary: IP / UDP 127.0.0.1:15782 > 8.8.8.8:https
- - raw_packet: !!binary |
      RQAAHDaAAAAMEelAfwAAAQgICAg9pgG7AAgxbA==
    summary: IP / UDP 127.0.0.1:15782 > 8.8.8.8:https
- - raw_packet: !!binary |
      RQAAHOYzAAANETiNfwAAAQgICAg9pgG7AAgxbA==
    summary: IP / UDP 127.0.0.1:15782 > 8.8.8.8:https
- - raw_packet: !!binary |
      RQAAHH0cAAAOEaCkfwAAAQgICAg9pgG7AAgxbA==
    summary: IP / UDP 127.0.0.1:15782 > 8.8.8.8:https
- - raw_packet: !!binary |
      RQAAHBO0AAABERcNfwAAAQgICAgq7///AAhF3g==
    summary: IP / UDP 127.0.0.1:10991 > 8.8.8.8:65535
- - raw_packet: !!binary |
      RQAAHELQAAACEebwfwAAAQgICAgq7///AAhF3g==
    summary: IP / UDP 127.0.0.1:10991 > 8.8.8.8:65535
- - raw_packet: !!binary |
      RQAAHCQNAAADEQS0fwAAAQgICAgq7///AAhF3g==
    summary: IP / UDP 127.0.0.1:10991 > 8.8.8.8:65535
- - raw_packet: !!binary |
      RQAAHEy+AAAEEdsCfwAAAQgICAgq7///AAhF3g==
    summary: IP / UDP 127.0.0.1:10991 > 8.8.8.8:65535
- - raw_packet: !!binary |
      RQAAHFnEAAAFEcz8fwAAAQgICAgq7///AAhF3g==
    summary: IP / UDP 127.0.0.1:10991 > 8.8.8.8:65535
- - raw_packet: !!binary |
      RQAAHDD4AAAGEfTIfwAAAQgICAgq7///AAhF3g==
    summary: IP / UDP 127.0.0.1:10991 > 8.8.8.8:65535
- - raw_packet: !!binary |
      RQAAHFfLAAAHEcz1fwAAAQgICAgq7///AAhF3g==
    summary: IP / UDP 127.0.0.1:10991 > 8.8.8.8:65535
- - raw_packet: !!binary |
      RQAAHFtXAAAIEchpfwAAAQgICAgq7///AAhF3g==
    summary: IP / UDP 127.0.0.1:10991 > 8.8.8.8:65535
- - raw_packet: !!binary |
      RQAAHD2gAAAJEeUgfwAAAQgICAgq7///AAhF3g==
    summary: IP / UDP 127.0.0.1:10991 > 8.8.8.8:65535
- - raw_packet: !!binary |
      RQAAHM/GAAAKEVH6fwAAAQgICAgq7///AAhF3g==
    summary: IP / UDP 127.0.0.1:10991 > 8.8.8.8:65535
- - raw_packet: !!binary |
      RQAAHNDXAAALEU/pfwAAAQgICAgq7///AAhF3g==
    summary: IP / UDP 127.0.0.1:10991 > 8.8.8.8:65535
- - raw_packet: !!binary |
      RQAAHIwIAAAMEZO4fwAAAQgICAgq7///AAhF3g==
    summary: IP / UDP 127.0.0.1:10991 > 8.8.8.8:65535
- - raw_packet: !!binary |
      RQAAHEOjAAANEdsdfwAAAQgICAgq7///AAhF3g==
    summary: IP / UDP 127.0.0.1:10991 > 8.8.8.8:65535
- - raw_packet: !!binary |
      RQAAHHHdAAAOEavjfwAAAQgICAgq7///AAhF3g==
    summary: IP / UDP 127.0.0.1:10991 > 8.8.8.8:65535
- - raw_packet: !!binary |
      RQAAHHKXAAABEbgpfwAAAQgICAg7gQAWAAg1Ng==
    summary: IP / UDP 127.0.0.1:15233 > 8.8.8.8:ssh
- - raw_packet: !!binary |
      RQAAHBVFAAACERR8fwAAAQgICAg7gQAWAAg1Ng==
    summary: IP / UDP 127.0.0.1:15233 > 8.8.8.8:ssh
- - raw_packet: !!binary |
      RQAAHB2zAAADEQsOfwAAAQgICAg7gQAWAAg1Ng==
    summary: IP / UDP 127.0.0.1:15233 > 8.8.8.8:ssh
- - raw_packet: !!binary |
      RQAAHCsLAAAEEfy1fwAAAQgICAg7gQAWAAg1Ng==
    summary: IP / UDP 127.0.0.1:15233 > 8.8.8.8:ssh
- - raw_packet: !!binary |
      RQAAHM8mAAAFEVeafwAAAQgICAg7gQAWAAg1Ng==
    summary: IP / UDP 127.0.0.1:15233 > 8.8.8.8:ssh
- - raw_packet: !!binary |
      RQAAHH1/AAAGEahBfwAAAQgICAg7gQAWAAg1Ng==
    summary: IP / UDP 127.0.0.1:15233 > 8.8.8.8:ssh
- - raw_packet: !!binary |
      RQAAHLcgAAAHEW2gfwAAAQgICAg7gQAWAAg1Ng==
    summary: IP / UDP 127.0.0.1:15233 > 8.8.8.8:ssh
- - raw_packet: !!binary |
      RQAAHA8LAAAIERS2fwAAAQgICAg7gQAWAAg1Ng==
    summary: IP / UDP 127.0.0.1:15233 > 8.8.8.8:ssh
- - raw_packet: !!binary |
      RQAAHFuuAAAJEccSfwAAAQgICAg7gQAWAAg1Ng==
    summary: IP / UDP 127.0.0.1:15233 > 8.8.8.8:ssh
- - raw_packet: !!binary |
      RQAAHBjXAAAKEQjqfwAAAQgICAg7gQAWAAg1Ng==
    summary: IP / UDP 127.0.0.1:15233 > 8.8.8.8:ssh
- - raw_packet: !!binary |
      RQAAHOKqAAALET4WfwAAAQgICAg7gQAWAAg1Ng==
    summary: IP / UDP 127.0.0.1:15233 > 8.8.8.8:ssh
- - raw_packet: !!binary |
      RQAAHCtvAAAMEfRRfwAAAQgICAg7gQAWAAg1Ng==
    summary: IP / UDP 127.0.0.1:15233 > 8.8.8.8:ssh
- - raw_packet: !!binary |
      RQAAHOb2AAANETfKfwAAAQgICAg7gQAWAAg1Ng==
    summary: IP / UDP 127.0.0.1:15233 > 8.8.8.8:ssh
- - raw_packet: !!binary |
      RQAAHNZ4AAAOEUdIfwAAAQgICAg7gQAWAAg1Ng==
    summary: IP / UDP 127.0.0.1:15233 > 8.8.8.8:ssh
- - raw_packet: !!binary |
      RQAAHKomAAABEYCafwAAAQgICAhzngB7AAj8sw==
    summary: IP / UDP 127.0.0.1:29598 > 8.8.8.8:ntp
- - raw_packet: !!binary |
      RQAAHJzvAAACEYzRfwAAAQgICAhzngB7AAj8sw==
    summary: IP / UDP 127.0.0.1:29598 > 8.8.8.8:ntp
- - raw_packet: !!binary |
      RQAAHHyAAAADEaxAfwAAAQgICAhzngB7AAj8sw==
    summary: IP / UDP 127.0.0.1:29598 > 8.8.8.8:ntp
- - raw_packet: !!binary |
      RQAAHBx6AAAEEQtHfwAAAQgICAhzngB7AAj8sw==
    summary: IP / UDP 127.0.0.1:29598 > 8.8.8.8:ntp
- - raw_packet: !!binary |
      RQAAHCdcAAAFEf9kfwAAAQgICAhzngB7AAj8sw==
    summary: IP / UDP 127.0.0.1:29598 > 8.8.8.8:ntp
- - raw_packet: !!binary |
      RQAAHKblAAAGEX7bfwAAAQgICAhzngB7AAj8sw==
    summary: IP / UDP 127.0.0.1:29598 > 8.8.8.8:ntp
- - raw_packet: !!binary |
      RQAAHJ9dAAAHEYVjfwAAAQgICAhzngB7AAj8sw==
    summary: IP / UDP 127.0.0.1:29598 > 8.8.8.8:ntp
- - raw_packet: !!binary |
      RQAAHBPTAAAIEQ/ufwAAAQgICAhzngB7AAj8sw==
    summary: IP / UDP 127.0.0.1:29598 > 8.8.8.8:ntp
- - raw_packet: !!binary |
      RQAAHM2WAAAJEVUqfwAAAQgICAhzngB7AAj8sw==
    summary: IP / UDP 127.0.0.1:29598 > 8.8.8.8:ntp
- - raw_packet: !!binary |
      RQAAHH05AAAKEaSHfwAAAQgICAhzngB7AAj8sw==
    summary: IP / UDP 127.0.0.1:29598 > 8.8.8.8:ntp
- - raw_packet: !!binary |
      RQAAHHATAAALEbCtfwAAAQgICAhzngB7AAj8sw==
    summary: IP / UDP 127.0.0.1:29598 > 8.8.8.8:ntp
- - raw_packet: !!binary |
      RQAAHMwRAAAMEVOvfwAAAQgICAhzngB7AAj8sw==
    summary: IP / UDP 127.0.0.1:29598 > 8.8.8.8:ntp
- - raw_packet: !!binary |
      RQAAHFCmAAANEc4afwAAAQgICAhzngB7AAj8sw==
    summary: IP / UDP 127.0.0.1:29598 > 8.8.8.8:ntp
- - raw_packet: !!binary |
      RQAAHEq/AAAOEdMBfwAAAQgICAhzngB7AAj8sw==
    summary: IP / UDP 127.0.0.1:29598 > 8.8.8.8:ntp
- - raw_packet: !!binary |
      RQAAHNvVAAABAU77fwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHJnCAAACAZAOfwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHEhLAAADAeCFfwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHPG/AAAEATYRfwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHEvQAAAFAdsAfwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHNROAAAGAVGCfwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHG+aAAAHAbU2fwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHGw6AAAIAbeWfwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHE23AAAJAdUZfwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHETwAAAKAdzgfwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHMl5AAALAVdXfwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHEEkAAAMAd6sfwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHOLVAAANATv7fwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHPtzAAAOASJdfwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHKv2AAAPAXDafwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHJ4aAAAQAX22fwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHJmQAAARAYFAfwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHM9gAAASAUpwfwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHF87AAATAbmVfwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHJOVAAAUAYQ7fwAAAQgICAgIAPf/AAAAAA==
    summary: IP / ICMP 127.0.0.1 > 8.8.8.8 echo-request 0
- - raw_packet: !!binary |
      RQAAHP0ZAAABES2nfwAAAQgICAgkBgA1AAhMkg==
    summary: IP / UDP 127.0.0.1:teamcoherence > 8.8.8.8:domain
- - raw_packet: !!binary |
      RQAAHCrQAAACEf7wfwAAAQgICAgkBgA1AAhMkg==
    summary: IP / UDP 127.0.0.1:teamcoherence > 8.8.8.8:domain
- - raw_packet: !!binary |
      RQAAHHnUAAADEa7sfwAAAQgICAgkBgA1AAhMkg==
    summary: IP / UDP 127.0.0.1:teamcoherence > 8.8.8.8:domain
- - raw_packet: !!binary |
      RQAAHH4+AAAEEamCfwAAAQgICAgkBgA1AAhMkg==
    summary: IP / UDP 127.0.0.1:teamcoherence > 8.8.8.8:domain
- - raw_packet: !!binary |
      RQAAHPc4AAAFES+IfwAAAQgICAgkBgA1AAhMkg==
    summary: IP / UDP 127.0.0.1:teamcoherence > 8.8.8.8:domain
- - raw_packet: !!binary |
      RQAAHN53AAAGEUdJfwAAAQgICAgkBgA1AAhMkg==
    summary: IP / UDP 127.0.0.1:teamcoherence > 8.8.8.8:domain
- - raw_packet: !!binary |
      RQAAHFi/AAAHEcwBfwAAAQgICAgkBgA1AAhMkg==
    summary: IP / UDP 127.0.0.1:teamcoherence > 8.8.8.8:domain
- - raw_packet: !!binary |
      RQAAHM4nAAAIEVWZfwAAAQgICAgkBgA1AAhMkg==
    summary: IP / UDP 127.0.0.1:teamcoherence > 8.8.8.8:domain
- - raw_packet: !!binary |
      RQAAHP0vAAAJESWRfwAAAQgICAgkBgA1AAhMkg==
    summary: IP / UDP 127.0.0.1:teamcoherence > 8.8.8.8:domain
- - raw_packet: !!binary |
      RQAAHJmRAAAKEYgvfwAAAQgICAgkBgA1AAhMkg==
    summary: IP / UDP 127.0.0.1:teamcoherence > 8.8.8.8:domain
- - raw_packet: !!binary |
      RQAAHIqdAAALEZYjfwAAAQgICAgkBgA1AAhMkg==
    summary: IP / UDP 127.0.0.1:teamcoherence > 8.8.8.8:domain
- - raw_packet: !!binary |
      RQAAHPJhAAAMES1ffwAAAQgICAgkBgA1AAhMkg==
    summary: IP / UDP 127.0.0.1:teamcoherence > 8.8.8.8:domain
- - raw_packet: !!binary |
      RQAAHHdGAAANEad6fwAAAQgICAgkBgA1AAhMkg==
    summary: IP / UDP 127.0.0.1:teamcoherence > 8.8.8.8:domain
- - raw_packet: !!binary |
      RQAAHPTYAAAOESjofwAAAQgICAgkBgA1AAhMkg==
    summary: IP / UDP 127.0.0.1:teamcoherence > 8.8.8.8:domain
- - raw_packet: !!binary |
      RQAAHFvwAAAPEcDQfwAAAQgICAgkBgA1AAhMkg==
    summary: IP / UDP 127.0.0.1:teamcoherence > 8.8.8.8:domain
- - raw_packet: !!binary |
      RQAAHP3zAAAQER3NfwAAAQgICAgkBgA1AAhMkg==
    summary: IP / UDP 127.0.0.1:teamcoherence > 8.8.8.8:domain
- - raw_packet: !!binary |
      RQAAHGCzAAAREboNfwAAAQgICAgkBgA1AAhMkg==
    summary: IP / UDP 127.0.0.1:teamcoherence > 8.8.8.8:domain
- - raw_packet: !!binary |
      RQAAHIHZAAASEZfnfwAAAQgICAgkBgA1AAhMkg==
    summary: IP / UDP 127.0.0.1:teamcoherence > 8.8.8.8:domain
- - raw_packet: !!binary |
      RQAAHD+jAAABEesdfwAAAQgICAhJmB+QAAgHpQ==
    summary: IP / UDP 127.0.0.1:18840 > 8.8.8.8:http_alt
- - raw_packet: !!binary |
      RQAAHEMVAAACEearfwAAAQgICAhJmB+QAAgHpQ==
    summary: IP / UDP 127.0.0.1:18840 > 8.8.8.8:http_alt
- - raw_packet: !!binary |
      RQAAHPV4AAADETNIfwAAAQgICAhJmB+QAAgHpQ==
    summary: IP / UDP 127.0.0.1:18840 > 8.8.8.8:http_alt
- - raw_packet: !!binary |
      RQAAHPWLAAAEETI1fwAAAQgICAhJmB+QAAgHpQ==
    summary: IP / UDP 127.0.0.1:18840 > 8.8.8.8:http_alt
- - raw_packet: !!binary |
      RQAAHLphAAAFEWxffwAAAQgICAhJmB+QAAgHpQ==
    summary: IP / UDP 127.0.0.1:18840 > 8.8.8.8:http_alt
- - raw_packet: !!binary |
      RQAAHGAvAAAGEcWRfwAAAQgICAhJmB+QAAgHpQ==
    summary: IP / UDP 127.0.0.1:18840 > 8.8.8.8:http_alt
- - raw_packet: !!binary |
      RQAAHMi1AAAHEVwLfwAAAQgICAhJmB+QAAgHpQ==
    summary: IP / UDP 127.0.0.1:18840 > 8.8.8.8:http_alt
- - raw_packet: !!binary |
      RQAAHN7VAAAIEUTrfwAAAQgICAhJmB+QAAgHpQ==
    summary: IP / UDP 127.0.0.1:18840 > 8.8.8.8:http_alt
- - raw_packet: !!binary |
      RQAAHJmkAAAJEYkcfwAAAQgICAhJmB+QAAgHpQ==
    summary: IP / UDP 127.0.0.1:18840 > 8.8.8.8:http_alt
- - raw_packet: !!binary |
      RQAAHBSfAAAKEQ0ifwAAAQgICAhJmB+QAAgHpQ==
    summary: IP / UDP 127.0.0.1:18840 > 8.8.8.8:http_alt
- - raw_packet: !!binary |
      RQAAHOLfAAALET3hfwAAAQgICAhJmB+QAAgHpQ==
    summary: IP / UDP 127.0.0.1:18840 > 8.8.8.8:http_alt
- - raw_packet: !!binary |
      RQAAHL25AAAMEWIHfwAAAQgICAhJmB+QAAgHpQ==
    summary: IP / UDP 127.0.0.1:18840 > 8.8.8.8:http_alt
- - raw_packet: !!binary |
      RQAAHG7hAAANEa/ffwAAAQgICAhJmB+QAAgHpQ==
    summary: IP / UDP 127.0.0.1:18840 > 8.8.8.8:http_alt
- - raw_packet: !!binary |
      RQAAHAFfAAAOERxifwAAAQgICAhJmB+QAAgHpQ==
    summary: IP / UDP 127.0.0.1:18840 > 8.8.8.8:http_alt
test_icmp_traceroute:
  hops:
  - {address: 10.0.1.1, rtt: 3.9715609550476074, ttl: 1}
  - {address: 10.72.240.1, rtt: 4.018263101577759, ttl: 2}
  - {address: 10.1.231.154, rtt: 4.0202929973602295, ttl: 3}
  - {address: 10.251.161.201, rtt: 4.024235963821411, ttl: 4}
  - {address: 10.251.156.26, rtt: 4.028274059295654, ttl: 5}
  - {address: 10.251.157.1, rtt: 4.032228946685791, ttl: 6}
  - {address: 10.247.125.85, rtt: 4.035927057266235, ttl: 7}
  - {address: 10.251.161.194, rtt: 4.039880037307739, ttl: 8}
  - {address: 10.103.124.5, rtt: 4.043823003768921, ttl: 9}
  - {address: 10.1.128.61, rtt: 4.04759407043457, ttl: 10}
  - {address: 10.254.11.77, rtt: 4.052925109863281, ttl: 11}
  - {address: 89.97.200.106, rtt: 4.056672096252441, ttl: 12}
  - {address: 89.97.200.254, rtt: 4.067225933074951, ttl: 13}
  - {address: 89.96.200.126, rtt: 4.069293975830078, ttl: 14}
  - {address: 72.14.211.55, rtt: 4.071274995803833, ttl: 15}
  - {address: 209.85.241.92, rtt: 4.073256015777588, ttl: 16}
  - {address: 72.14.232.78, rtt: 4.075243949890137, ttl: 17}
  - {address: 209.85.254.116, rtt: 4.077281951904297, ttl: 18}
  - {address: 8.8.8.8, rtt: 4.079555988311768, ttl: 19}
  - {address: 8.8.8.8, rtt: 4.5740931034088135, ttl: 20}
test_tcp_traceroute:
  hops_0:
  - {address: 10.0.1.1, rtt: 0.8929989337921143, sport: 33550, ttl: 1}
  - {address: 10.1.231.154, rtt: 0.9173901081085205, sport: 33550, ttl: 2}
  - {address: 10.251.161.201, rtt: 0.9208340644836426, sport: 33550, ttl: 3}
  - {address: 10.251.156.26, rtt: 0.9243190288543701, sport: 33550, ttl: 4}
  - {address: 10.251.157.1, rtt: 0.9279429912567139, sport: 33550, ttl: 5}
  hops_123:
  - {address: 10.0.1.1, rtt: 1.057663917541504, sport: 10729, ttl: 1}
  - {address: 10.72.240.1, rtt: 1.0832798480987549, sport: 10729, ttl: 2}
  - {address: 10.1.231.154, rtt: 1.0852348804473877, sport: 10729, ttl: 3}
  - {address: 10.251.161.201, rtt: 1.0872218608856201, sport: 10729, ttl: 4}
  - {address: 10.251.156.26, rtt: 1.0913589000701904, sport: 10729, ttl: 5}
  - {address: 10.251.157.1, rtt: 1.0950088500976562, sport: 10729, ttl: 6}
  - {address: 10.247.125.85, rtt: 1.0984508991241455, sport: 10729, ttl: 7}
  - {address: 10.251.161.194, rtt: 1.106515884399414, sport: 10729, ttl: 8}
  - {address: 10.103.124.5, rtt: 1.1134669780731201, sport: 10729, ttl: 9}
  - {address: 10.1.128.61, rtt: 1.1153309345245361, sport: 10729, ttl: 10}
  - {address: 10.254.11.77, rtt: 1.1171259880065918, sport: 10729, ttl: 11}
  - {address: 89.97.200.106, rtt: 1.118973970413208, sport: 10729, ttl: 12}
  - {address: 62.101.124.65, rtt: 1.1317999362945557, sport: 10729, ttl: 13}
  - {address: 89.97.200.254, rtt: 1.1352579593658447, sport: 10729, ttl: 14}
  hops_22:
  - {address: 10.0.1.1, rtt: 0.887671947479248, sport: 31561, ttl: 1}
  - {address: 10.72.240.1, rtt: 0.9095978736877441, sport: 31561, ttl: 2}
  - {address: 10.1.231.154, rtt: 0.9134628772735596, sport: 31561, ttl: 3}
  - {address: 10.251.161.201, rtt: 0.9172408580780029, sport: 31561, ttl: 4}
  - {address: 10.251.156.26, rtt: 0.9208929538726807, sport: 31561, ttl: 5}
  - {address: 10.251.157.1, rtt: 0.9243419170379639, sport: 31561, ttl: 6}
  - {address: 10.247.125.85, rtt: 0.9276900291442871, sport: 31561, ttl: 7}
  - {address: 10.251.161.194, rtt: 0.9310119152069092, sport: 31561, ttl: 8}
  - {address: 10.103.124.5, rtt: 0.937445878982544, sport: 31561, ttl: 9}
  - {address: 10.1.128.61, rtt: 0.939384937286377, sport: 31561, ttl: 10}
  - {address: 10.254.11.77, rtt: 0.9427988529205322, sport: 31561, ttl: 11}
  - {address: 89.97.200.106, rtt: 0.9527590274810791, sport: 31561, ttl: 12}
  - {address: 89.97.200.254, rtt: 0.95943284034729, sport: 31561, ttl: 13}
  - {address: 89.96.200.134, rtt: 0.9697248935699463, sport: 31561, ttl: 14}
  hops_23:
  - {address: 10.0.1.1, rtt: 0.9211528301239014, sport: 5097, ttl: 1}
  - {address: 10.72.240.1, rtt: 0.9416170120239258, sport: 5097, ttl: 2}
  - {address: 10.1.231.154, rtt: 0.9434559345245361, sport: 5097, ttl: 3}
  - {address: 10.251.161.201, rtt: 0.9484758377075195, sport: 5097, ttl: 4}
  - {address: 10.251.156.26, rtt: 0.9505150318145752, sport: 5097, ttl: 5}
  - {address: 10.251.157.1, rtt: 0.9561848640441895, sport: 5097, ttl: 6}
  - {address: 10.247.125.85, rtt: 0.960468053817749, sport: 5097, ttl: 7}
  - {address: 10.251.161.194, rtt: 0.9639179706573486, sport: 5097, ttl: 8}
  - {address: 10.103.124.5, rtt: 0.9702839851379395, sport: 5097, ttl: 9}
  - {address: 10.1.128.61, rtt: 0.9737949371337891, sport: 5097, ttl: 10}
  - {address: 10.254.11.77, rtt: 0.9802989959716797, sport: 5097, ttl: 11}
  - {address: 89.97.200.106, rtt: 0.9836268424987793, sport: 5097, ttl: 12}
  - {address: 89.97.200.254, rtt: 0.99639892578125, sport: 5097, ttl: 13}
  - {address: 89.96.200.126, rtt: 1.0050489902496338, sport: 5097, ttl: 14}
  hops_443:
  - {address: 10.0.1.1, rtt: 1.0840370655059814, sport: 63889, ttl: 1}
  - {address: 10.1.231.154, rtt: 1.109666109085083, sport: 63889, ttl: 2}
  - {address: 10.251.161.201, rtt: 1.113111972808838, sport: 63889, ttl: 3}
  - {address: 10.251.156.26, rtt: 1.1165308952331543, sport: 63889, ttl: 4}
  - {address: 10.251.157.1, rtt: 1.1198999881744385, sport: 63889, ttl: 5}
  - {address: 10.247.125.85, rtt: 1.1232690811157227, sport: 63889, ttl: 6}
  - {address: 10.251.161.194, rtt: 1.1285760402679443, sport: 63889, ttl: 7}
  - {address: 10.103.124.5, rtt: 1.1326820850372314, sport: 63889, ttl: 8}
  - {address: 10.1.128.61, rtt: 1.139185905456543, sport: 63889, ttl: 9}
  - {address: 10.254.11.77, rtt: 1.1459689140319824, sport: 63889, ttl: 10}
  - {address: 89.97.200.106, rtt: 1.1481139659881592, sport: 63889, ttl: 11}
  - {address: 89.97.200.254, rtt: 1.1560420989990234, sport: 63889, ttl: 12}
  - {address: 89.96.200.134, rtt: 1.1640660762786865, sport: 63889, ttl: 13}
  hops_53:
  - {address: 10.0.1.1, rtt: 0.9541549682617188, sport: 6421, ttl: 1}
  - {address: 10.72.240.1, rtt: 0.9729020595550537, sport: 6421, ttl: 2}
  - {address: 10.1.231.154, rtt: 0.9748890399932861, sport: 6421, ttl: 3}
  - {address: 10.251.161.201, rtt: 0.9788179397583008, sport: 6421, ttl: 4}
  - {address: 10.251.156.26, rtt: 0.9826369285583496, sport: 6421, ttl: 5}
  - {address: 10.251.157.1, rtt: 0.9876649379730225, sport: 6421, ttl: 6}
  - {address: 10.247.125.85, rtt: 0.9913709163665771, sport: 6421, ttl: 7}
  - {address: 10.251.161.194, rtt: 0.997809886932373, sport: 6421, ttl: 8}
  - {address: 10.103.124.5, rtt: 0.9996218681335449, sport: 6421, ttl: 9}
  - {address: 10.1.128.61, rtt: 1.0029208660125732, sport: 6421, ttl: 10}
  - {address: 10.254.11.77, rtt: 1.0111238956451416, sport: 6421, ttl: 11}
  - {address: 89.97.200.106, rtt: 1.0159449577331543, sport: 6421, ttl: 12}
  - {address: 89.97.200.254, rtt: 1.0279550552368164, sport: 6421, ttl: 13}
  - {address: 89.96.200.46, rtt: 1.0326428413391113, sport: 6421, ttl: 14}
  - {address: 72.14.211.89, rtt: 1.039456844329834, sport: 6421, ttl: 15}
  - {address: 209.85.241.92, rtt: 1.0447049140930176, sport: 6421, ttl: 16}
  - {address: 72.14.232.78, rtt: 1.0516810417175293, sport: 6421, ttl: 17}
  - {address: 209.85.254.114, rtt: 1.058819055557251, sport: 6421, ttl: 18}
  - {address: 8.8.8.8, rtt: 1.064857006072998, sport: 6421, ttl: 19}
  - {address: 8.8.8.8, rtt: 1.074767827987671, sport: 6421, ttl: 20}
  - {address: 8.8.8.8, rtt: 1.0773730278015137, sport: 6421, ttl: 21}
  - {address: 8.8.8.8, rtt: 1.0835769176483154, sport: 6421, ttl: 22}
  - {address: 8.8.8.8, rtt: 1.0862410068511963, sport: 6421, ttl: 23}
  - {address: 8.8.8.8, rtt: 1.0980219841003418, sport: 6421, ttl: 24}
  - {address: 8.8.8.8, rtt: 1.1042819023132324, sport: 6421, ttl: 25}
  - {address: 8.8.8.8, rtt: 1.1147940158843994, sport: 6421, ttl: 26}
  - {address: 8.8.8.8, rtt: 1.1190860271453857, sport: 6421, ttl: 27}
  - {address: 8.8.8.8, rtt: 1.1216108798980713, sport: 6421, ttl: 28}
  - {address: 8.8.8.8, rtt: 1.127511978149414, sport: 6421, ttl: 29}
  hops_65535:
  - {address: 10.0.1.1, rtt: 1.1430189609527588, sport: 37293, ttl: 1}
  - {address: 10.72.240.1, rtt: 1.168022871017456, sport: 37293, ttl: 2}
  - {address: 10.1.231.154, rtt: 1.1717679500579834, sport: 37293, ttl: 3}
  - {address: 10.251.161.201, rtt: 1.1754209995269775, sport: 37293, ttl: 4}
  - {address: 10.251.156.26, rtt: 1.177292823791504, sport: 37293, ttl: 5}
  - {address: 10.251.157.1, rtt: 1.1840429306030273, sport: 37293, ttl: 6}
  - {address: 10.247.125.85, rtt: 1.185985803604126, sport: 37293, ttl: 7}
  - {address: 10.251.161.194, rtt: 1.1920018196105957, sport: 37293, ttl: 8}
  - {address: 10.103.124.5, rtt: 1.1955947875976562, sport: 37293, ttl: 9}
  - {address: 10.1.128.61, rtt: 1.1989409923553467, sport: 37293, ttl: 10}
  - {address: 10.254.11.77, rtt: 1.2022309303283691, sport: 37293, ttl: 11}
  - {address: 89.97.200.106, rtt: 1.2070260047912598, sport: 37293, ttl: 12}
  - {address: 89.97.200.254, rtt: 1.236454963684082, sport: 37293, ttl: 13}
  - {address: 89.96.200.134, rtt: 1.2652239799499512, sport: 37293, ttl: 14}
  hops_80:
  - {address: 10.0.1.1, rtt: 0.9794230461120605, sport: 48728, ttl: 1}
  - {address: 10.1.231.154, rtt: 1.0122148990631104, sport: 48728, ttl: 2}
  - {address: 10.72.240.1, rtt: 1.0160748958587646, sport: 48728, ttl: 3}
  - {address: 10.251.161.201, rtt: 1.022981882095337, sport: 48728, ttl: 4}
  - {address: 10.251.156.26, rtt: 1.0264310836791992, sport: 48728, ttl: 5}
  - {address: 10.251.157.1, rtt: 1.036902904510498, sport: 48728, ttl: 6}
  - {address: 10.247.125.85, rtt: 1.0441060066223145, sport: 48728, ttl: 7}
  - {address: 10.251.161.194, rtt: 1.0476000308990479, sport: 48728, ttl: 8}
  - {address: 10.103.124.5, rtt: 1.056222915649414, sport: 48728, ttl: 9}
  - {address: 10.1.128.61, rtt: 1.0620770454406738, sport: 48728, ttl: 10}
  - {address: 10.254.11.77, rtt: 1.0713310241699219, sport: 48728, ttl: 11}
  - {address: 89.97.200.106, rtt: 1.0809619426727295, sport: 48728, ttl: 12}
  - {address: 89.97.200.254, rtt: 1.0914230346679688, sport: 48728, ttl: 13}
  - {address: 89.96.200.126, rtt: 1.0984768867492676, sport: 48728, ttl: 14}
  hops_8080:
  - {address: 10.0.1.1, rtt: 1.1146080493927002, sport: 57884, ttl: 1}
  - {address: 10.1.231.154, rtt: 1.1394469738006592, sport: 57884, ttl: 2}
  - {address: 10.251.161.201, rtt: 1.1429920196533203, sport: 57884, ttl: 3}
  - {address: 10.251.156.26, rtt: 1.1465580463409424, sport: 57884, ttl: 4}
  - {address: 10.251.157.1, rtt: 1.150658130645752, sport: 57884, ttl: 5}
  - {address: 10.247.125.85, rtt: 1.1548030376434326, sport: 57884, ttl: 6}
  - {address: 10.251.161.194, rtt: 1.1582081317901611, sport: 57884, ttl: 7}
  - {address: 10.103.124.5, rtt: 1.1615681648254395, sport: 57884, ttl: 8}
  - {address: 10.1.128.61, rtt: 1.1648681163787842, sport: 57884, ttl: 9}
  - {address: 10.254.11.77, rtt: 1.1742961406707764, sport: 57884, ttl: 10}
  - {address: 89.97.200.106, rtt: 1.177865982055664, sport: 57884, ttl: 11}
  - {address: 89.97.200.254, rtt: 1.1897270679473877, sport: 57884, ttl: 12}
  - {address: 62.101.124.65, rtt: 1.1932711601257324, sport: 57884, ttl: 13}
test_udp_traceroute:
  hops_0: []
  hops_123:
  - {address: 10.0.1.1, rtt: 2.8643651008605957, sport: 29598, ttl: 1}
  - {address: 10.72.240.1, rtt: 2.9663000106811523, sport: 29598, ttl: 2}
  - {address: 10.1.231.154, rtt: 2.985211133956909, sport: 29598, ttl: 3}
  - {address: 10.251.161.201, rtt: 3.0045831203460693, sport: 29598, ttl: 4}
  - {address: 10.251.156.26, rtt: 3.018602132797241, sport: 29598, ttl: 5}
  - {address: 10.251.157.1, rtt: 3.0379550457000732, sport: 29598, ttl: 6}
  - {address: 10.247.125.85, rtt: 3.0577399730682373, sport: 29598, ttl: 7}
  - {address: 10.251.161.194, rtt: 3.071830987930298, sport: 29598, ttl: 8}
  - {address: 10.103.124.5, rtt: 3.096630096435547, sport: 29598, ttl: 9}
  - {address: 10.1.128.61, rtt: 3.110487937927246, sport: 29598, ttl: 10}
  - {address: 10.254.11.77, rtt: 3.159827947616577, sport: 29598, ttl: 11}
  - {address: 89.97.200.106, rtt: 3.1841320991516113, sport: 29598, ttl: 12}
  - {address: 89.97.200.254, rtt: 3.221647024154663, sport: 29598, ttl: 13}
  - {address: 89.96.200.126, rtt: 3.2409000396728516, sport: 29598, ttl: 14}
  hops_22:
  - {address: 10.0.1.1, rtt: 1.3334949016571045, sport: 15233, ttl: 1}
  - {address: 10.1.231.154, rtt: 1.4409840106964111, sport: 15233, ttl: 2}
  - {address: 10.72.240.1, rtt: 1.4585249423980713, sport: 15233, ttl: 3}
  - {address: 10.251.161.201, rtt: 1.482278823852539, sport: 15233, ttl: 4}
  - {address: 10.251.156.26, rtt: 1.5005879402160645, sport: 15233, ttl: 5}
  - {address: 10.251.157.1, rtt: 1.531883955001831, sport: 15233, ttl: 6}
  - {address: 10.247.125.85, rtt: 1.5560660362243652, sport: 15233, ttl: 7}
  - {address: 10.251.161.194, rtt: 1.5865838527679443, sport: 15233, ttl: 8}
  - {address: 10.103.124.5, rtt: 1.6111359596252441, sport: 15233, ttl: 9}
  - {address: 10.1.128.61, rtt: 1.6353850364685059, sport: 15233, ttl: 10}
  - {address: 10.254.11.77, rtt: 1.6537768840789795, sport: 15233, ttl: 11}
  - {address: 89.97.200.106, rtt: 1.6786339282989502, sport: 15233, ttl: 12}
  - {address: 89.97.200.254, rtt: 1.7631969451904297, sport: 15233, ttl: 13}
  - {address: 89.96.200.126, rtt: 1.7871499061584473, sport: 15233, ttl: 14}
  hops_23:
  - {address: 10.0.1.1, rtt: 1.6981499195098877, sport: 43782, ttl: 1}
  - {address: 10.72.240.1, rtt: 1.8316938877105713, sport: 43782, ttl: 2}
  - {address: 10.1.231.154, rtt: 1.8493030071258545, sport: 43782, ttl: 3}
  - {address: 10.251.161.201, rtt: 1.878938913345337, sport: 43782, ttl: 4}
  - {address: 10.251.156.26, rtt: 1.9026379585266113, sport: 43782, ttl: 5}
  - {address: 10.251.157.1, rtt: 1.9251139163970947, sport: 43782, ttl: 6}
  - {address: 10.247.125.85, rtt: 1.9485108852386475, sport: 43782, ttl: 7}
  - {address: 10.251.161.194, rtt: 1.9832098484039307, sport: 43782, ttl: 8}
  - {address: 10.103.124.5, rtt: 2.0001959800720215, sport: 43782, ttl: 9}
  - {address: 10.1.128.61, rtt: 2.0243330001831055, sport: 43782, ttl: 10}
  - {address: 10.254.11.77, rtt: 2.047346830368042, sport: 43782, ttl: 11}
  - {address: 89.97.200.106, rtt: 2.070168972015381, sport: 43782, ttl: 12}
  - {address: 89.97.200.254, rtt: 2.1522560119628906, sport: 43782, ttl: 13}
  - {address: 62.101.124.65, rtt: 2.9604408740997314, sport: 43782, ttl: 14}
  hops_443:
  - {address: 10.0.1.1, rtt: 3.1613810062408447, sport: 15782, ttl: 1}
  - {address: 10.72.240.1, rtt: 3.2684850692749023, sport: 15782, ttl: 2}
  - {address: 10.1.231.154, rtt: 3.2818000316619873, sport: 15782, ttl: 3}
  - {address: 10.251.161.201, rtt: 3.300002098083496, sport: 15782, ttl: 4}
  - {address: 10.251.156.26, rtt: 3.323151111602783, sport: 15782, ttl: 5}
  - {address: 10.251.157.1, rtt: 3.3413009643554688, sport: 15782, ttl: 6}
  - {address: 10.247.125.85, rtt: 3.359462022781372, sport: 15782, ttl: 7}
  - {address: 10.251.161.194, rtt: 3.377542018890381, sport: 15782, ttl: 8}
  - {address: 10.103.124.5, rtt: 3.396583080291748, sport: 15782, ttl: 9}
  - {address: 10.1.128.61, rtt: 3.4149301052093506, sport: 15782, ttl: 10}
  - {address: 10.254.11.77, rtt: 3.4378039836883545, sport: 15782, ttl: 11}
  - {address: 89.97.200.106, rtt: 3.4557440280914307, sport: 15782, ttl: 12}
  - {address: 89.97.200.254, rtt: 3.5199360847473145, sport: 15782, ttl: 13}
  - {address: 62.101.124.65, rtt: 3.5376710891723633, sport: 15782, ttl: 14}
  hops_53:
  - {address: 10.0.1.1, rtt: 2.0826339721679688, sport: 9222, ttl: 1}
  - {address: 10.1.231.154, rtt: 2.1999151706695557, sport: 9222, ttl: 2}
  - {address: 10.251.161.201, rtt: 2.2224841117858887, sport: 9222, ttl: 3}
  - {address: 10.251.156.26, rtt: 2.2450520992279053, sport: 9222, ttl: 4}
  - {address: 10.251.157.1, rtt: 2.267009973526001, sport: 9222, ttl: 5}
  - {address: 10.247.125.85, rtt: 2.2895491123199463, sport: 9222, ttl: 6}
  - {address: 10.72.240.1, rtt: 2.4096531867980957, sport: 9222, ttl: 7}
  - {address: 10.251.161.194, rtt: 2.425421953201294, sport: 9222, ttl: 8}
  - {address: 10.103.124.5, rtt: 2.441133975982666, sport: 9222, ttl: 9}
  - {address: 10.1.128.61, rtt: 2.457381010055542, sport: 9222, ttl: 10}
  - {address: 10.254.11.77, rtt: 2.4967970848083496, sport: 9222, ttl: 11}
  - {address: 89.97.200.106, rtt: 2.5189261436462402, sport: 9222, ttl: 12}
  - {address: 89.97.200.254, rtt: 2.550632953643799, sport: 9222, ttl: 13}
  - {address: 72.14.211.89, rtt: 2.5663211345672607, sport: 9222, ttl: 14}
  - {address: 209.85.241.94, rtt: 2.582092046737671, sport: 9222, ttl: 15}
  - {address: 89.96.200.126, rtt: 2.5982320308685303, sport: 9222, ttl: 16}
  - {address: 72.14.232.76, rtt: 2.661022186279297, sport: 9222, ttl: 17}
  - {address: 209.85.254.112, rtt: 2.7243690490722656, sport: 9222, ttl: 18}
  hops_65535:
  - {address: 10.0.1.1, rtt: 3.7303781509399414, sport: 10991, ttl: 1}
  - {address: 10.72.240.1, rtt: 3.7946829795837402, sport: 10991, ttl: 2}
  - {address: 10.1.231.154, rtt: 3.8064370155334473, sport: 10991, ttl: 3}
  - {address: 10.251.161.201, rtt: 3.8219411373138428, sport: 10991, ttl: 4}
  - {address: 10.251.156.26, rtt: 3.8330190181732178, sport: 10991, ttl: 5}
  - {address: 10.251.157.1, rtt: 3.8488430976867676, sport: 10991, ttl: 6}
  - {address: 10.247.125.85, rtt: 3.864877939224243, sport: 10991, ttl: 7}
  - {address: 10.251.161.194, rtt: 3.9216160774230957, sport: 10991, ttl: 8}
  - {address: 10.103.124.5, rtt: 3.9326281547546387, sport: 10991, ttl: 9}
  - {address: 10.1.128.61, rtt: 3.9524900913238525, sport: 10991, ttl: 10}
  - {address: 10.254.11.77, rtt: 3.9684531688690186, sport: 10991, ttl: 11}
  - {address: 89.97.200.106, rtt: 3.9926950931549072, sport: 10991, ttl: 12}
  - {address: 89.97.200.254, rtt: 4.027745008468628, sport: 10991, ttl: 13}
  - {address: 89.96.200.46, rtt: 4.04297399520874, sport: 10991, ttl: 14}
  hops_80:
  - {address: 10.0.1.1, rtt: 2.4942288398742676, sport: 23951, ttl: 1}
  - {address: 10.1.231.154, rtt: 2.584718942642212, sport: 23951, ttl: 2}
  - {address: 10.72.240.1, rtt: 2.5997419357299805, sport: 23951, ttl: 3}
  - {address: 10.251.161.201, rtt: 2.6360549926757812, sport: 23951, ttl: 4}
  - {address: 10.251.156.26, rtt: 2.6616249084472656, sport: 23951, ttl: 5}
  - {address: 10.251.157.1, rtt: 2.6992008686065674, sport: 23951, ttl: 6}
  - {address: 10.247.125.85, rtt: 2.7195730209350586, sport: 23951, ttl: 7}
  - {address: 10.251.161.194, rtt: 2.745745897293091, sport: 23951, ttl: 8}
  - {address: 10.103.124.5, rtt: 2.7869420051574707, sport: 23951, ttl: 9}
  - {address: 10.1.128.61, rtt: 2.8128459453582764, sport: 23951, ttl: 10}
  - {address: 10.254.11.77, rtt: 2.838701009750366, sport: 23951, ttl: 11}
  - {address: 89.97.200.106, rtt: 2.853529930114746, sport: 23951, ttl: 12}
  - {address: 89.97.200.254, rtt: 2.945657968521118, sport: 23951, ttl: 13}
  - {address: 89.96.200.46, rtt: 2.9657039642333984, sport: 23951, ttl: 14}
  hops_8080:
  - {address: 10.0.1.1, rtt: 3.457942008972168, sport: 18840, ttl: 1}
  - {address: 10.72.240.1, rtt: 3.5681920051574707, sport: 18840, ttl: 2}
  - {address: 10.1.231.154, rtt: 3.5803709030151367, sport: 18840, ttl: 3}
  - {address: 10.251.161.201, rtt: 3.5968568325042725, sport: 18840, ttl: 4}
  - {address: 10.251.156.26, rtt: 3.6134979724884033, sport: 18840, ttl: 5}
  - {address: 10.251.157.1, rtt: 3.62577486038208, sport: 18840, ttl: 6}
  - {address: 10.247.125.85, rtt: 3.6425728797912598, sport: 18840, ttl: 7}
  - {address: 10.251.161.194, rtt: 3.659623861312866, sport: 18840, ttl: 8}
  - {address: 10.103.124.5, rtt: 3.6761529445648193, sport: 18840, ttl: 9}
  - {address: 10.1.128.61, rtt: 3.6931769847869873, sport: 18840, ttl: 10}
  - {address: 10.254.11.77, rtt: 3.7094268798828125, sport: 18840, ttl: 11}
  - {address: 89.97.200.106, rtt: 3.731276035308838, sport: 18840, ttl: 12}
  - {address: 89.97.200.254, rtt: 3.7874269485473633, sport: 18840, ttl: 13}
  - {address: 89.96.200.134, rtt: 3.823248863220215, sport: 18840, ttl: 14}
timeout: 5
...
```

# Privacy considerations

The scapyt report format includes the binary packets as sent or received. In the ICMP response payload a portion (64 bits) or all of the original packet is returned. The client source address will be contained in the report.

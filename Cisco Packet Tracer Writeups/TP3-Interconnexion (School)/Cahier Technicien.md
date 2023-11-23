# Erreurs et solutions

## Erreur 1

Erreur:
```Ciosco IOS
%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/24 (777), with Switch2A FastEthernet0/21 (1).
%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/24 (777), with Switch2A FastEthernet0/22 (1).
%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/24 (777), with Switch2A FastEthernet0/23 (1).
%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/24 (777), with Switch2A FastEthernet0/24 (1).
```

Solution: https://community.cisco.com/t5/switching/native-vlan-mismatch-detected-by/td-p/3316606

## Erreur 2

Erreur
```Cisco IOS
C:\>ipconfig

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Link-local IPv6 Address.........: FE80::201:42FF:FE29:8347
   IPv6 Address....................: ::
   IPv4 Address....................: 10.10.40.11
   Subnet Mask.....................: 255.255.255.0
   Default Gateway.................: ::
                                     10.10.40.1

Bluetooth Connection:

   Connection-specific DNS Suffix..: 
   Link-local IPv6 Address.........: ::
   IPv6 Address....................: ::
   IPv4 Address....................: 0.0.0.0
   Subnet Mask.....................: 0.0.0.0
   Default Gateway.................: ::
                                     0.0.0.0

C:\>ping 10.10.88.1

Pinging 10.10.88.1 with 32 bytes of data:

Reply from 10.10.88.1: bytes=32 time=14ms TTL=255
Reply from 10.10.88.1: bytes=32 time<1ms TTL=255
Reply from 10.10.88.1: bytes=32 time<1ms TTL=255
Reply from 10.10.88.1: bytes=32 time<1ms TTL=255

Ping statistics for 10.10.88.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 14ms, Average = 3ms

C:\>tracert 10.10.88.1

Tracing route to 10.10.88.1 over a maximum of 30 hops: 

  1   0 ms      0 ms      0 ms      10.10.88.1

Trace complete.
```

Solution: https://community.cisco.com/t5/routing/ping-works-trace-doesn-t/td-p/1636692

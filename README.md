
Project Lab: Switchport Security.

 Topology
- 1 Switch 2960
- 3 Laptops
- Connected with Copper Straight-Through cables

 Baseline Check (Before Configuration)
I ran `show port-security`
Result: No output / Empty.
This means no port security was configured on the Switch 2960 yet.

 Lab Setup

 Fa0/1 - Dynamic
- The switch learns the MAC address by itself
- It learns when traffic is sent
- If the switch reboots, it forgets the address, so the first device mac is learnt
- Violation mode: Shutdown

 Fa0/2 - Static
- I typed the MAC address manually: `000A.418A.C544`
- If the switch reboots, it stays the same
- Violation mode: Protect

 Fa0/3 - Sticky
- I turned on sticky and added MAC: `0002.4AEB.CD11`
- The switch learns it and saves it in the running-configuration
- Violation mode: Restrict

 Error Encountered
Fa0/3 was configured for sticky with violation Restrict. The MAC `0002.4AEB.CD11` was configured, but the port remained Secure-down. This is a known Packet Tracer limitation.

Breach test was performed on Fa0/2 only, as it was sufficient to demonstrate violation handling.

 Breach Test on Fa0/2

Fa0/2 Protect Test
After connecting an unauthorized PC `000A.412B.5CD4`, ping failed with Request timed out. The port remained Secure-up and violation count remained 0.
This shows Protect quietly drops unauthorized traffic without shutting down the port. The Last Source Address changed to the intruder MAC, proving the attempt was detected.
What I Learned
Port Security with Protect mode silently drops traffic from unknown MACs without shutting down the port. This keeps the network available for authorized users while blocking attackers.

Commands Used for Fa0/2 (protect)
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security violation protect


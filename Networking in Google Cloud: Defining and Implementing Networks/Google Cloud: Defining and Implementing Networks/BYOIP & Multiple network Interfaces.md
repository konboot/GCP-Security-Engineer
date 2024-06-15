### BYOIP (Bring Your Own IP) in Google Cloud

BYOIP (Bring Your Own IP) is a feature in Google Cloud that allows organizations to bring their own public IP addresses to Google Cloud. 
- This enables businesses to maintain continuity and trust by using their existing IP addresses, which might already be whitelisted by customers or partners, or are well-known in the industry.
- Route traffic directly from thr internet to their VMs.

**Guidelines**: The object that the IP address is assigned to
- Can have a regional scope or a global scope.
- Must support an external address type.
- Cannot be the classic VPN gateway, GKE(Google Kubernetes Engine) node, GKE pod, autoscaling MIG (managed instance group).

**BYOIP Warnings**:
 - BYOIP prefixes cannot overlap with subnet or alias ranges in the VPC.
 - IP address must be IPv4.
 - Overlapping BGP route announcements can be problematic.

  =====================================================================

### Multiple Network Interfaces
**VPC networks are isolated by default**:
- VPC networks use:
  - Internal IP to communicate within the networks.
  - External IP to communicate across the networks.
- To communicate internally with multiple networks, add multiple network interface controller(NICs).
  - Each NIC is attached to a separate VPC network. Uses an internal IP to communicate across networks.

**Multiple Network Interface Warnings**:
- Network instance can only be configured when you create an instance.
- Each interface must be in a different network.
- The network IP ranges cannot overlap.
- The networks must exists before you create the VM.
- Cannot delete interface without deleting the VM.
- Internal DNS is only associated to nic0.
- You can have up to 8 NICs, depending on the VM.

  =====================================================================


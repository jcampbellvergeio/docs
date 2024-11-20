---
status: new
---

# Using Fibre Channel Storage with vSAN

VergeOS vSAN can utilize Fibre Channel (FC) LUNs as storage devices within its tiered architecture. This feature allows you to leverage existing FC storage infrastructure while maintaining vSAN's built-in redundancy and performance benefits.

## Overview 

The vSAN can incorporate FC LUNs as storage devices within any tier. When configured, these LUNs appear as block devices to the vSAN, complete with multipathing support for high availability.

!!! note
    Multipathing is enabled by default in VergeOS 4.13 and later versions.

## Path Management

### Multipath Configuration

The system automatically manages multiple paths to FC LUNs in an active/passive configuration:

- **Primary Path**: vSAN uses one path as the primary I/O channel
- **Secondary Paths**: Additional paths remain available for failover
- **Automatic Failover**: 7-second timeout before switching to alternate path
- **Path Persistence**: System maintains new path after failover until manually changed

### Path States

* **Active**: Currently handling I/O operations
* **Passive**: Available for failover
* **Failed**: Detected as non-functional

## Implementation Guidelines

### Hardware Requirements

- FC Host Bus Adapters (HBAs) in each node
- Compatible FC switches
- FC storage array with available LUNs

### Configuration Steps

1. **Zone FC Storage**:
    - Configure FC switch zoning using WWPN-based zoning
    - Ensure consistent LUN presentation across nodes

2. **Present LUNs**:
    - Present FC LUNs to all vSAN nodes
    - Verify multipath visibility on each node

3. **Add to vSAN**:
    - Navigate to **Storage Tiers** configuration
    - Add FC LUNs to desired tier
    - Apply changes to implement in vSAN

## Best Practices

Follow these guidelines for optimal FC storage implementation:

- **Path Redundancy**: Configure multiple physical paths from each node
- **Consistent Presentation**: Ensure identical LUN configuration across nodes
- **Regular Maintenance**: 
    - Monitor path status
    - Test failover functionality
    - Keep HBA firmware current
- **Performance Optimization**: 
    - Balance LUN distribution across available paths
    - Monitor I/O patterns for optimal path utilization

!!! warning "Storage Changes"
    Always use [**Maintenance Mode**](/product-guide/maintenancemode) when making changes to storage configuration.

## Monitoring

Monitor FC storage health through:

- vSAN dashboard metrics
- Path status indicators
- I/O performance statistics
- Error logs

!!! tip
    Configure [**Subscriptions**](/product-guide/subscriptions-overview) to receive alerts about path failures or performance issues.

## Related Documentation

- [Storage Tiers](/product-guide/vsan/storage-tiers)
- [vSAN Architecture](/product-guide/vsan/architecture)
- [Maintenance Mode](/product-guide/maintenancemode)

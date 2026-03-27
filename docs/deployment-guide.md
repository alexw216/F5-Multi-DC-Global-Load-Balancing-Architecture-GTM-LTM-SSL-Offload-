# Deployment guide

## 1. Plan

- Decide active-active vs active-standby.
- Define DC1/DC2 IPs, VIPs, and FQDNs.
- Obtain certificates for `www.example.com`.

## 2. Prepare BIG-IP LTM in each DC

1. Import certificates and keys.
2. Create HTTP monitors.
3. Create pools:
   - DC1: `pool_www_dc1` with backend servers.
   - DC2: `pool_www_dc2` with backend servers.
4. Create client SSL, HTTP, and OneConnect profiles.
5. Create HTTPS virtual servers:
   - `vs_www_dc1_https` on DC1 VIP.
   - `vs_www_dc2_https` on DC2 VIP.
6. Verify:
   - HTTPS to each VIP works.
   - Backends see HTTP traffic.
   - Monitors show pool members up.

## 3. Configure BIG-IP DNS (GTM)

1. Define datacenters `DC1` and `DC2`.
2. Define GTM servers `LTM_DC1` and `LTM_DC2` pointing to LTM self IPs.
3. Associate LTM virtual servers with GTM servers.
4. Create GSLB pool `www_pool_https` with both VIPs.
5. Create Wide IP `www.example.com` pointing to `www_pool_https`.

## 4. Choose global LB method

- For active-active: `lb-mode round-robin` or `ratio`.
- For geo-based: configure `gtm topology` and set `lb-mode topology`.
- For active-standby: set ratios (e.g., DC1=100, DC2=0).

## 5. DNS integration

- Delegate `www.example.com` (or its zone) to BIG-IP DNS.
- Set appropriate TTL (20–60 seconds recommended).

## 6. Validation

- Query DNS repeatedly and confirm responses alternate (active-active) or follow topology.
- Simulate DC1 failure:
  - Disable `vs_www_dc1_https` or its pool.
  - Confirm GTM stops returning DC1 VIP.
- Validate application behavior and session persistence.

## 7. Operations

- Document failover and maintenance procedures.
- Monitor health, logs, and certificate expiry.
- Regularly test DR and update configs in `configs/` as source of truth.

# Theory

This document explains the separation of concerns between:

- BIG-IP DNS (GTM) as the global traffic director
- BIG-IP LTM as the local traffic manager

Key ideas:

- GSLB via Wide IPs, GSLB pools, and GTM servers
- Local LB via LTM virtual servers, pools, and monitors
- SSL offload at each DC to enable L7 features

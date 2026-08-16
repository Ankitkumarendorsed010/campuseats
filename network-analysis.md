# Network Analysis Report

**Author:** Ankit
**Website analyzed:** [W3Schools](https://www.w3schools.com)
**Method:** Chrome DevTools → Network tab → "Disable cache" enabled → page reloaded → waterfall of requests inspected.

---

## Overview

| Metric | Value |
|---|---|
| **Total request count** | 62 |
| **Total page size** | 4.5 MB |
| **Slowest resource** | `crossfire.js` — **1.11 s** |

---

## Slowest Resource

The single slowest resource in the waterfall was **`crossfire.js`**, which took **1.11 seconds** to load. Scripts like this — typically third-party ad/analytics bundles — are common bottlenecks because they block on external network round-trips rather than serving from the same origin as the page.

---

## 3xx / 4xx Responses Observed

One redirect (3xx) response was captured during the reload:

| Status | Type | Request URL |
|---|---|---|
| **302** | Found (temporary redirect) | `https://csync.smilewanted.com/getuid?source=prebid-server&gdpr=0&gdpr_consent=&us_privacy=1---&redirect=https%3A%2F%2Fpbsj.bricks-co.com%2Fsetuid%3Fpslabel%3Dnull%26bidder%3Dsmilewanted%26gdpr%3D0%26gdpr_consent%3D%26us_privacy%3D1---%26gpp%3D%26gpp_sid%3D%26f%3Di%26uid%3D%24UID` |

**Note:** `302 Found` tells the browser the requested resource is temporarily available at a different URL (here, a cookie/ID-sync redirect used by the SmileWanted ad-tech/prebid-server pipeline, forwarding the user's sync ID to `pbsj.bricks-co.com`). No `4xx` (client error) responses were observed in this waterfall.

---

## Summary

The W3Schools homepage generated **62 requests** totaling **4.5 MB** on a cache-disabled reload. The load was dominated by a single slow third-party script (`crossfire.js`, 1.11 s), and the only non-2xx response was a `302` redirect tied to an ad-sync/tracking call chain (SmileWanted → Prebid Server → Bricks-Co). This pattern is typical of ad-supported sites, where third-party ad-tech and analytics scripts are the main contributors to both request count and page weight.

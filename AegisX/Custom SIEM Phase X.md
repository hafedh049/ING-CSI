- ✅ Add MITRE ATT&CK mapping
    
- ✅ Add OpenAPI docs for all internal APIs
    
- ✅ Add QRadar/ArcSight exporter for interop
    
- ✅ Connect to cloud threat feeds (Azure Sentinel, GCP Chronicle)
    
- ✅ Add support for encrypted logs (AES or PGP)

```js
[VMs/Logs/Honeypots]
        ↓
[Log Collectors (Vector/FluentBit)]
        ↓
[Flask Parsing Services]
        ↓
[Normalization + Enrichment]
        ↓
[Detection Layer (Regex + ML + Threat Intel)]
        ↓
[Alerts]
   → [MongoDB/Elastic]
   → [SOAR Engine (Python)]
   → [SOC Notification]
        ↓
[Next.js Dashboard (Charts + Events)]
```


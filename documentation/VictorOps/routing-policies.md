| routing key | escalation policy | secondary policy |
| ----------- | ----------------- | ---------------- |
|Cloudtamer | CloudTamer : Notify All |
|EnterpriseToolsTeam|Agile Enterprise Tools : ENT Tools Esc Policy|
|PlatformOps|Platform-Ops : standard|
|SecurityOnCallRoster|SecOps : Email | SecOps : SOC|
|SonarqubeIncident|DevOps : Standard|
|Tier2HelpDesk|Tier 2 + Patching : primary|
|xoc|Tier 2 + Patching : primary|
|Splunk|Logging : Standard|
|FileAnalysisService|SecOps : Email | SecOps : CaaS|
|TrendMicroEmail|SecOps : Email | SecOps : SOC|
|AWSSecurity|SecOps : Email | SecOps : SSaaS|
|NetworkArchitecture|Network-ops : Standard|
|NetworkOperations|Network-ops : Standard|
|Artifactory|DevOps : Standard|
|TrendSecurityCenter|SecOps : Email | SecOps : SSaaS|
|CloudBeesCore|DevOps : Standard|
|smtp|Platform-Ops : standard|
|eicar|SecOps : SOC|
|CRE|CRE - Customer Reliability Engineering : Standard|
|CMDB|Platform-Ops : standard|
|ARCS|SecOps : Email|SecOps : SSaaS|
|CPM|Platform-Ops : standard|
|ActiveDirectory|Cloud AD : Standard|
|GenesisAPI|Platform-Ops : standard|
|CCGBackend|ccg cloud.cms.gov : Standard|
|MAG-Ops|Platform-Ops : standard|MAG-Ops : Standard|
|SRE|Site Reliability Engineering : Standard|
|TWS|TWS : Standard|
|mag-ms-ticket|MS-support-requests : Standard|
|SSaaS|SecOps : SSaaS|
|SCS|SecOps : CaaS|
|EAC|EAC - Estimate At Completion : Standard|
|NCC|Global-Fallback (Eng Leadership) : Emergency|
|Selenium|DevOps : Standard|
|CET|CET - Cost Estimation Tool : Standard|
|Dragnet|Security Engineering (ITOPS) : Security Rotation|
|PlatformOpsTickets|Platform-Ops : Tickets|
|CRC|CRC - Cust Resource Config : CRC Standard|
|sreoncall|Site Reliability Engineering : Standard|
|ITSI|ITSI : ITSI|
|NoAction|Agile Enterprise Tools : ENT Tools Esc Policy|
|CAMP|CAMP : Standard|
|CloudDNS|CloudDNS : Standard|
|batCAVE|batCAVE : Sev 1 & Sev 2|batCAVE : Infra only|
|batCAVE|batCAVE : Pipeline only|batCAVE : TA only|
|batCAVE|batCAVE : Security only|

**Default Routing Policy**

- Incidents that do not match any routing key will be routed to the following Escalation Policy.

    -- Tier 2 + Patching : primary

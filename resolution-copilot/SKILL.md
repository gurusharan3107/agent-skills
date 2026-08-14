---
name: resolution-copilot
description: >
  Resume or extend the Resolution Copilot / agentic incident-management POC
  (ServiceNow intake, evidence-gated command center, verified KB, staging repro).
  Use when the user mentions Resolution Copilot, INC-3424, incident-command-center,
  xmattermost, live ServiceNow developer instance testing, or continuing the
  agentic incident POC after a prior Cloud Agent session.
---

# Resolution Copilot — continuation skill

## Load first

1. Clone/checkout **`gurusharan3107/incident-command-center`** branch **`cursor/resolution-copilot-foundation-a168`**.  
2. Read **`AGENTS.md`** in that repo (source of truth for status + next steps).  
3. Also checkout **`gurusharan3107/xmattermost-kb`** same branch name if touching KB.  

## Required env (at agent start)

```text
SERVICENOW_INSTANCE_URL   # https://devXXXXXX.service-now.com
SERVICENOW_USER
SERVICENOW_PASSWORD
```

If unset → stop and request secrets (Personal scope). Do not invent instance URLs.

## Immediate verification

```bash
cd incident-command-center
npm test
node scripts/snow-smoke.js
```

Success = ping OK + list of incidents (or fetch by number). If secrets were skipped, use mock ITSM `servicenow-incident-demo` on `:4400` and continue Phase 0 wiring.

## Architecture (short)

```text
ServiceNow (live) ──► Incident agent ──► Memory (xmattermost verified KB)
                           │
                     Command center :4600
                     contract / evidence / gates / kill
                           │
                     Coding + Verify (staging / Aegis :4300)
                           │
                     Gate1 approve fix → Gate2 write-back → SN + KB promote
```

## Do / don’t

| Do | Don’t |
|---|---|
| PR-only fixes, staging repro | Agent merge/deploy/prod write |
| Verified KB only (draft→promote) | Auto-publish KB |
| Evidence pack before gates | Rubber-stamp empty gates |
| chrome-control on **Windows** host | Expect chrome-control inside Linux Cloud VM |

## Related repos

- `aegis-onboarding-portal` — primary bug target (pricing string concat)  
- `servicenow-incident-demo` — mock ITSM (fallback if live SN down)  
- `poc-incident-webapp` — earlier Nimbus twin  
- `chrome-control` — Windows Chrome bridge skill  
- `docs/PRODUCT_PLAN.md` — full plan + SAP appendix  

## If secrets sync 400’d in Cursor UI

Use **Personal** secrets + **new** Cloud Agent. Mid-session secret save will not appear in `printenv`.

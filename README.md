# Zoho CRM Widget SDK v1.5 — Interactive Tester

A comprehensive single-page widget for testing **all** Zoho CRM JS SDK v1.5 methods, including the new **ZRC (Zoho Remote Call)** client.

## Project Structure

```
zrcwidget/
├── app/
│   ├── widget.html          # Main widget UI (all SDK methods)
│   └── translations/
│       └── en.json           # i18n translations (empty)
├── server/
│   └── index.js              # HTTPS Express dev server (port 5000)
├── plugin-manifest.json      # Zoho CRM widget manifest
├── package.json              # Node.js dependencies
└── README.md
```

## Setup (for new clones / forks)

```bash
# 1. Clone the repo
git clone <repo-url>
cd zrcwidget

# 2. Install dependencies
npm install

# 3. Generate self-signed SSL certs (required by the dev server)
openssl req -x509 -newkey rsa:2048 -nodes \
  -keyout key.pem -out cert.pem -days 365 \
  -subj "/CN=localhost"

# 4. Start the dev server
npm start
# Serves on https://localhost:5000
```

### Prerequisites

- **Node.js** v14+ and **npm**
- **openssl** (for generating self-signed certs — pre-installed on macOS/Linux)

### What's not in the repo

The `.gitignore` excludes:
- `node_modules/` — reinstall via `npm install`
- `key.pem` / `cert.pem` — regenerate with the openssl command above
- `.DS_Store`, `*.log`

## SDK Reference

**CDN:** `https://live.zwidgets.com/js-sdk/1.5/ZohoEmbededAppSDK.min.js`  
**Docs:** https://www.zohocrm.dev/explore/widgets/v1.5/jssdk

## Implemented Sections

### 1. Dashboard
- Quick-access cards to all sections
- Auto-captured `PageLoad` data display

### 2. Events & Init (`ZOHO.embeddedApp`)
- SDK initialization via `ZOHO.embeddedApp.init()` (called on `DOMContentLoaded`)
- Real-time event log capturing:
  - `PageLoad` — fires on entity detail page load
  - `Dial` — fires when CRM call icon is clicked
  - `DialerActive` — fires when softphone window toggles
  - `Notify` — client script flyout notification
  - `NotifyAndWait` — synchronous flyout notification
  - `ContextUpdate` — fires on wizard form changes

### 3. ZRC — Zoho Remote Call (`zrc.*`)
The global `zrc` object is provided by the SDK after init. No manual assignment needed.

| Method | Description |
|--------|-------------|
| `zrc.get(path, config)` | GET request |
| `zrc.post(path, body, config)` | POST request |
| `zrc.put(path, body, config)` | PUT request |
| `zrc.patch(path, body, config)` | PATCH request |
| `zrc.delete(path, config)` | DELETE request |
| `zrc.head(path, config)` | HEAD request (headers only) |
| `zrc.options(path, config)` | OPTIONS request |
| `zrc.request(config)` | Fully custom request |
| `zrc.createInstance(config)` | Reusable instance with baseUrl/connection/headers |

**Path formats:**
- CRM APIs: relative path like `/crm/v8/Leads`
- Connection-based: absolute URL + `{ connection: 'link_name' }` in config
- External APIs: absolute URL directly

### 4. Records CRUD (`ZOHO.CRM.API`)
| Function | SDK Method |
|----------|------------|
| `getAllRecords()` | `ZOHO.CRM.API.getAllRecords({ Entity, sort_order, page, per_page })` |
| `getRecord()` | `ZOHO.CRM.API.getRecord({ Entity, RecordID })` |
| `insertRecord()` | `ZOHO.CRM.API.insertRecord({ Entity, APIData, Trigger })` |
| `updateRecord()` | `ZOHO.CRM.API.updateRecord({ Entity, APIData, Trigger })` |
| `upsertRecord()` | `ZOHO.CRM.API.upsertRecord({ Entity, APIData, duplicate_check_fields })` |
| `deleteRecord()` | `ZOHO.CRM.API.deleteRecord({ Entity, RecordID })` |

### 5. Search & COQL (`ZOHO.CRM.API`)
| Function | SDK Method |
|----------|------------|
| `searchRecord()` | `ZOHO.CRM.API.searchRecord({ Entity, Type, Query })` — Types: `word`, `email`, `phone`, `criteria` |
| `coqlQuery()` | `ZOHO.CRM.API.coql({ select_query })` |

### 6. Related Records (`ZOHO.CRM.API`)
| Function | SDK Method |
|----------|------------|
| `getRelatedRecords()` | `ZOHO.CRM.API.getRelatedRecords({ Entity, RecordID, RelatedList })` |
| `updateRelatedRecords()` | `ZOHO.CRM.API.updateRelatedRecords({ Entity, RecordID, RelatedList, RelatedRecordID, APIData })` |
| `delinkRelatedRecord()` | `ZOHO.CRM.API.delinkRelatedRecord({ Entity, RecordID, RelatedList, RelatedRecordID })` |

### 7. Notes & Files (`ZOHO.CRM.API`)
| Function | SDK Method |
|----------|------------|
| `addNotes()` | `ZOHO.CRM.API.addNotes({ Entity, RecordID, Title, Content })` |
| `attachFile()` | `ZOHO.CRM.API.attachFile({ Entity, RecordID, File })` |
| `uploadFile()` | `ZOHO.CRM.API.uploadFile({ CONTENT_TYPE, PARTS, FILE })` |
| `getFile()` | `ZOHO.CRM.API.getFile({ id })` |

### 8. Users & Profiles (`ZOHO.CRM.API`)
| Function | SDK Method |
|----------|------------|
| `getAllUsers()` | `ZOHO.CRM.API.getAllUsers({ Type })` — Types: `AllUsers`, `ActiveUsers`, `AdminUsers`, etc. |
| `getUser()` | `ZOHO.CRM.API.getUser({ ID })` |
| `getAllProfiles()` | `ZOHO.CRM.API.getAllProfiles()` |
| `getProfile()` | `ZOHO.CRM.API.getProfile({ ID })` |
| `updateProfile()` | `ZOHO.CRM.API.updateProfile({ ID, APIData })` |

### 9. Approvals & Blueprint (`ZOHO.CRM.API`)
| Function | SDK Method |
|----------|------------|
| `getApprovalRecords()` | `ZOHO.CRM.API.getApprovalRecords({ type })` |
| `getApprovalsHistory()` | `ZOHO.CRM.API.getApprovalsHistory()` |
| `getApprovalById()` | `ZOHO.CRM.API.getApprovalById({ id })` |
| `approveRecord()` | `ZOHO.CRM.API.approveRecord({ Entity, RecordID, actionType, comments, user })` |
| `getAllActions()` | `ZOHO.CRM.API.getAllActions({ Entity, RecordID })` |
| `getBluePrint()` | `ZOHO.CRM.API.getBluePrint({ Entity, RecordID })` |
| `updateBluePrint()` | `ZOHO.CRM.API.updateBluePrint({ Entity, RecordID, BlueprintData })` |

### 10. Org Variables (`ZOHO.CRM.API`)
| Function | SDK Method |
|----------|------------|
| `getOrgVariable()` | `ZOHO.CRM.API.getOrgVariable(namespace)` or `ZOHO.CRM.API.getOrgVariable({ apiKeys })` |

### 11. Metadata (`ZOHO.CRM.META`)
| Function | SDK Method |
|----------|------------|
| `getModules()` | `ZOHO.CRM.META.getModules()` |
| `getFields()` | `ZOHO.CRM.META.getFields({ Entity })` |
| `getLayouts()` | `ZOHO.CRM.META.getLayouts({ Entity, Id })` |
| `getCustomViews()` | `ZOHO.CRM.META.getCustomViews({ Entity, Id })` |
| `getAssignmentRules()` | `ZOHO.CRM.META.getAssignmentRules({ Entity })` |
| `getRelatedList()` | `ZOHO.CRM.META.getRelatedList({ Entity })` |

### 12. Config (`ZOHO.CRM.CONFIG`)
| Function | SDK Method |
|----------|------------|
| `getCurrentUser()` | `ZOHO.CRM.CONFIG.getCurrentUser()` |
| `getOrgInfo()` | `ZOHO.CRM.CONFIG.getOrgInfo()` |
| `getUserPreference()` | `ZOHO.CRM.CONFIG.getUserPreference()` |

### 13. UI Actions (`ZOHO.CRM.UI`)
| Function | SDK Method |
|----------|------------|
| `uiResize()` | `ZOHO.CRM.UI.Resize({ height, width })` |
| `uiRecordOpen()` | `ZOHO.CRM.UI.Record.open({ Entity, RecordID })` |
| `uiRecordCreate()` | `ZOHO.CRM.UI.Record.create({ Entity })` |
| `uiRecordEdit()` | `ZOHO.CRM.UI.Record.edit({ Entity, RecordID })` |
| `uiRecordPopulate()` | `ZOHO.CRM.UI.Record.populate(data)` |
| `uiPopupClose()` | `ZOHO.CRM.UI.Popup.close()` |
| `uiPopupCloseReload()` | `ZOHO.CRM.UI.Popup.closeReload()` |
| `uiDialerMax()` | `ZOHO.CRM.UI.Dialer.maximize()` |
| `uiDialerMin()` | `ZOHO.CRM.UI.Dialer.minimize()` |
| `uiDialerNotify()` | `ZOHO.CRM.UI.Dialer.notify()` |
| `uiWidgetOpen()` | `ZOHO.CRM.UI.Widget.open({ Entity, Message })` |

### 14. Connections (`ZOHO.CRM.CONNECTION`)
| Function | SDK Method |
|----------|------------|
| `invokeConnection()` | `ZOHO.CRM.CONNECTION.invoke(conn_name, { url, method, parameters, headers, param_type })` |

### 15. Connector (`ZOHO.CRM.CONNECTOR`)
| Function | SDK Method |
|----------|------------|
| `connectorAuthorize()` | `ZOHO.CRM.CONNECTOR.authorize(nameSpace)` |
| `connectorInvokeAPI()` | `ZOHO.CRM.CONNECTOR.invokeAPI(nameSpace, data)` |
| `connectorInvokeFileUpload()` | `ZOHO.CRM.CONNECTOR.invokeAPI(nameSpace, { VARIABLES, CONTENT_TYPE, PARTS, FILE })` |

### 16. HTTP Requests (`ZOHO.CRM.HTTP`)
| Function | SDK Method |
|----------|------------|
| `httpRequest()` | `ZOHO.CRM.HTTP[method]({ url, headers, params, body })` — methods: `get`, `post`, `put`, `patch`, `delete` |

### 17. Functions (`ZOHO.CRM.FUNCTIONS`)
| Function | SDK Method |
|----------|------------|
| `executeFunction()` | `ZOHO.CRM.FUNCTIONS.execute(func_name, { arguments })` |

## Key Implementation Notes

1. **SDK Init** — `ZOHO.embeddedApp.init()` is called inside a `DOMContentLoaded` listener to ensure the DOM and SDK script are fully loaded.

2. **ZRC global** — The `zrc` object is a global provided automatically by the SDK after initialization. Do **not** declare a local `var zrc` as it will shadow the global.

3. **Event registration** — All event listeners (`PageLoad`, `Dial`, etc.) must be registered **before** calling `ZOHO.embeddedApp.init()`.

4. **ZRC paths** — Use relative paths (`/crm/v8/...`) for CRM API calls (domain auto-resolved by DC). Use absolute URLs for connection-based or external requests.

5. **JSON output** — Every SDK method's response is rendered as formatted JSON in a dark-themed output box with copy-to-clipboard support.

6. **Error handling** — All SDK calls use `.then()/.catch()` (or `try/catch` for async ZRC methods) with errors displayed in the same output area prefixed with `ERROR:`.

## External Docs

- [JS SDK Reference](https://www.zohocrm.dev/explore/widgets/v1.5/jssdk)
- [ZRC Overview](https://www.zohocrm.dev/explore/widgets/v1.5/zrc_overview)
- [ZRC Methods](https://www.zohocrm.dev/explore/widgets/v1.5/zrc_methods)
- [ZRC Code Samples](https://www.zohocrm.dev/explore/widgets/v1.5/zrc_code_samples)
- [CRM API v8 Docs](https://www.zoho.com/crm/developer/docs/api/v8/)

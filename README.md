<div align="center">
  <img src="./banner.svg" alt="Cloud-Storage-Artifacts" width="800">
</div>

# Cloud Storage Artifacts

### 🔍 Cloud Storage Artifact Comparison for DFIR (Endpoint-Based)

| Platform     | Artifact Type   | Path / File                                                                  | Forensic Value                                                |
| ------------ | --------------- | ---------------------------------------------------------------------------- | ------------------------------------------------------------- |
| **OneDrive** | Local Folder    | `C:\Users\<User>\OneDrive\`                                                  | Access to synced files and timestamps                         |
|              | Logs            | `%LocalAppData%\Microsoft\OneDrive\logs\`                                    | Tracks sync actions, errors, and file operations              |
|              | Sync DB         | `%LocalAppData%\Microsoft\OneDrive\settings\Business1\SyncEngineDatabase.db` | SQLite DB with file metadata: hash, timestamps, status        |
|              | Account Info    | Registry: `HKCU\Software\Microsoft\OneDrive\Accounts`                        | Username, CID, linked tenant info                             |
|              | Tenant Info     | Registry: `HKCU\Software\Microsoft\OneDrive\Tenants`                         | Shows shared folders from other users                         |
|              | Sync Info       | Registry: `HKCU\Software\SyncEngines\Providers\OneDrive`                     | Tracks all folders, even shared from others                   |
|              | Config / Tokens | `%LocalAppData%\Microsoft\OneDrive\settings\`                                | JSON & DAT files with sync metadata and account configuration |

***

| Platform                             | Artifact Type       | Path / File                                                   | Forensic Value                                             |
| ------------------------------------ | ------------------- | ------------------------------------------------------------- | ---------------------------------------------------------- |
| **Google Drive** (Drive for Desktop) | Local Files         | `C:\Users\<User>\Google Drive\My Drive\`                      | Synced files (local copies or placeholders)                |
|                                      | DriveFS Logs        | `C:\Users\<User>\AppData\Local\Google\DriveFS\logs\`          | Detailed logs of sync events, errors, file activity        |
|                                      | DriveFS DB          | `C:\Users\<User>\AppData\Local\Google\DriveFS\sync_config.db` | SQLite DB with file metadata, sync status                  |
|                                      | Cache / Mount Point | `C:\Users\<User>\AppData\Local\Google\DriveFS\content_cache`  | May contain cached files (including deleted/unsynced)      |
|                                      | Shortcut Files      | `.gshortcut` files in Google Drive folder                     | Links to cloud-only files; contains file ID and cloud path |
|                                      | Auth Info           | `user_default/account_info.json`                              | Google account email, DriveFS ID                           |

***

| Platform    | Artifact Type    | Path / File                                     | Forensic Value                                                 |
| ----------- | ---------------- | ----------------------------------------------- | -------------------------------------------------------------- |
| **Dropbox** | Local Folder     | `C:\Users\<User>\Dropbox\`                      | Full path to synced files and shared folders                   |
|             | Logs             | `C:\Users\<User>\AppData\Roaming\Dropbox\logs\` | Sync logs, error logs, timestamps                              |
|             | Config DBs       | `config.dbx`, `host.dbx` in `%AppData%\Dropbox` | Contains device info, user account, Dropbox paths              |
|             | File Metadata DB | `filecache.dbx`                                 | Metadata for all files Dropbox manages (SQLite or proprietary) |
|             | JSON State Files | `state.json`                                    | Sync status and client state                                   |
|             | Registry         | Minimal; some traces in `HKCU\Software\Dropbox` | May contain install/config paths                               |

***

| Platform                   | Artifact Type       | Path / File                                                                               | Forensic Value                                      |
| -------------------------- | ------------------- | ----------------------------------------------------------------------------------------- | --------------------------------------------------- |
| **iCloud Drive (Windows)** | Local Files         | `C:\Users\<User>\iCloudDrive\`                                                            | Files saved or available offline                    |
|                            | Sync Logs           | `C:\Users\<User>\AppData\Roaming\Apple Computer\Logs\CloudKit\`                           | Logs of sync events between local device and iCloud |
|                            | CloudKit DB         | `C:\Users\<User>\AppData\Roaming\Apple Computer\Preferences\CloudKit\CloudKitMetadata.db` | SQLite DB with file status, sync metadata           |
|                            | Account Info        | Found within plist or Apple config files                                                  | Apple ID, sync state                                |
|                            | iCloud Photos Cache | `%ProgramData%\Apple Computer\iCloud Photos\`                                             | Cached images if Photos sync is enabled             |

***

| Platform      | Artifact Type     | Path / File                                           | Forensic Value                                        |
| ------------- | ----------------- | ----------------------------------------------------- | ----------------------------------------------------- |
| **Box Drive** | Local Drive Mount | `C:\Users\<User>\Box\` or virtual drive (e.g., `X:\`) | Local mirror or placeholders of synced content        |
|               | Logs              | `C:\Users\<User>\AppData\Local\Box\Box\logs\`         | File sync activity, errors, timestamps                |
|               | Sync DB           | `C:\Users\<User>\AppData\Local\Box\Box\data\Box.db`   | SQLite DB tracking files, versions, sync status       |
|               | Config            | `AppData\Local\Box\Box\data\client.json`              | Contains user ID, linked Box account, and preferences |
|               | File Cache        | `AppData\Local\Box\Box\cache`                         | Temporary cached content from cloud                   |

***

### 🧠 Forensic Insights (Quick Tips)

* ✅ **Sync DBs** are often **SQLite**, allowing full parsing of file names, timestamps, sync status, and sometimes hashes.
* ✅ **Logs** reveal exact **timestamps of uploads, deletions, errors**, and **user actions**.
* ✅ **Registry** (for OneDrive) and **JSON/PLIST** configs (for others) can show **user accounts, tokens, and linked folders**.
* ⚠️ Many systems only store **cloud placeholder files**, meaning files may not exist locally unless they’ve been opened or marked "always available".
*

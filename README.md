# @kud/pcloud-sdk

Typed pCloud API client — shared SDK for CLI, MCP, and extensions.

## Install

```sh
npm install @kud/pcloud-sdk
```

Requires Node.js 20 or later.

## Quick start

### Username / password

```typescript
import { PCloudAPI } from "@kud/pcloud-sdk"

const api = new PCloudAPI()
await api.authenticate("user@example.com", "password")

const info = await api.userInfo()
```

### OAuth access token

```typescript
import { PCloudAPI } from "@kud/pcloud-sdk"

const api = new PCloudAPI()
api.setAccessToken("your-access-token")

const folder = await api.listFolder("/")
```

The default API server is `https://eapi.pcloud.com` (EU endpoint). Pass a second argument to `setAccessToken` to override it.

## API reference

All methods return a typed promise. `result: 0` indicates success; a non-zero `result` with an `error` string indicates failure.

### Authentication

| Method                              | Description                                     |
| ----------------------------------- | ----------------------------------------------- |
| `authenticate(username, password)`  | Password auth — stores session token internally |
| `setAccessToken(token, apiServer?)` | OAuth auth — optionally override the API server |

### Account

| Method       | Description                           |
| ------------ | ------------------------------------- |
| `userInfo()` | Returns account info, quota, and plan |

### Files & folders

| Method                       | Description                                    |
| ---------------------------- | ---------------------------------------------- |
| `listFolder(path?)`          | List folder contents (default `"/"`)           |
| `stat(path)`                 | File or folder metadata                        |
| `createFolder(path)`         | Create folder if it does not already exist     |
| `deleteFolder(folderid)`     | Delete folder and all its contents recursively |
| `copyFile(fileid, topath)`   | Copy file to a destination path                |
| `moveFile(fileid, topath)`   | Move file to a destination path                |
| `renameFile(fileid, toname)` | Rename file in place                           |
| `deleteFile(fileid)`         | Permanently delete a file                      |
| `getFileLink(fileid)`        | Get a temporary download link                  |
| `checksumFile(fileid)`       | Returns SHA-256, SHA-1, and MD5 checksums      |

### Revisions

| Method                               | Description                          |
| ------------------------------------ | ------------------------------------ |
| `listRevisions(fileid)`              | List available revisions for a file  |
| `revertRevision(fileid, revisionid)` | Revert a file to a previous revision |

### Sharing

| Method                                     | Description                       |
| ------------------------------------------ | --------------------------------- |
| `listShares()`                             | List active shares                |
| `shareFolder(folderid, mail, permissions)` | Share a folder with another user  |
| `acceptShare(sharerequestid)`              | Accept an incoming share request  |
| `declineShare(sharerequestid)`             | Decline an incoming share request |
| `removeShare(sharerequestid)`              | Remove an existing share          |

### Public links

| Method                                           | Description                       |
| ------------------------------------------------ | --------------------------------- |
| `getFilePublink(fileid, expire?, maxdownloads?)` | Create a public link for a file   |
| `getFolderPublink(folderid, expire?)`            | Create a public link for a folder |
| `listPublinks()`                                 | List all active public links      |
| `deletePublink(code)`                            | Delete a public link by code      |

### Zip

| Method                                         | Description                           |
| ---------------------------------------------- | ------------------------------------- |
| `getZipLink(fileids[], folderids?, filename?)` | Get a download link for a zip archive |

### Trash

| Method                     | Description                   |
| -------------------------- | ----------------------------- |
| `listTrash()`              | List items in the trash       |
| `restoreFromTrash(fileid)` | Restore a file from the trash |

### Rewind

| Method                              | Description                                |
| ----------------------------------- | ------------------------------------------ |
| `listRewindFiles(path)`             | List rewind events for a path              |
| `restoreFromRewind(fileid, topath)` | Restore a file from rewind to a given path |

### Low-level

| Method                       | Description                                              |
| ---------------------------- | -------------------------------------------------------- |
| `request<T>(method, params)` | Generic request — maps directly to a pCloud API endpoint |

## Types

All types are exported from the package root.

| Type                     | Description                                                 |
| ------------------------ | ----------------------------------------------------------- |
| `PCloudFile`             | File metadata: `fileid`, `name`, `path`, `size`, `modified` |
| `PCloudFolderItem`       | Item within a folder listing — file or subfolder            |
| `PCloudFolderMetadata`   | Folder metadata including its `contents` array              |
| `PCloudFolderResponse`   | Response wrapper for folder operations                      |
| `PCloudUserInfo`         | Account info: email, quota, usedquota, plan                 |
| `PCloudFileLinkResponse` | Download link: `hosts[]` and `path`                         |
| `PCloudChecksumResponse` | Checksums: `sha256`, `sha1`, `md5`                          |
| `PCloudRevision`         | Single revision entry                                       |
| `PCloudShareItem`        | Share record with permissions and status                    |
| `PCloudPublink`          | Public link with code, expiry, and download count           |
| `PCloudTrashItem`        | Trash entry extending `PCloudFile` with `deletetime`        |
| `PCloudRewindItem`       | Rewind entry with `fileid`, `path`, and `time`              |
| `PCloudResponse<T>`      | Generic response wrapper                                    |
| `PCloudAuthResponse`     | Raw authentication response                                 |

## Licence

MIT

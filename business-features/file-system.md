# FILE STORAGES

Thinger.io provides a flexible cloud storage system that allows uploading files to the IoT server in order to provide support to other platform features, such as the HTML Widget or the OTA System. The information will be stored in a non-volatile memory of the server host.

{% hint style="success" %}
Note that this feature is only available for [**private instances**](../server/deployment/), as it requires a cloud storage system.&#x20;
{% endhint %}

## File Storages

File Storages provide a flexible file management system for storing and serving files within the platform. You can use storages for hosting static websites, storing firmware files, managing custom assets, exporting data, and more.

To manage your storages, navigate to **Storages** in the console.

### Creating a Storage

Click **Add Storage** to create a new storage. The configuration includes:

#### Basic Information

* **Storage ID**: A unique identifier for the storage (alphanumeric and underscores, max 25 characters). This ID is used in URLs and API calls.
* **Storage Name**: A display name for the storage.
* **Storage Description**: Optional description for administrative purposes (max 255 characters).

#### Access Configuration

* **Public Read**: Enable to allow public access to files without authentication. When disabled, all file access requires authentication or a valid access token.
* **Index File**: The default file served when accessing the storage root URL (e.g., `index.html`). Useful for hosting static websites.

#### Terminal Configuration

* **Terminal Image**: A Docker image to use for the integrated terminal environment (e.g., `alpine:latest`, `python:3.12-alpine`). This allows running shell commands directly within the storage context.

#### HTTP Access

Once created, your storage is accessible via HTTP at:

```
https://your-platform.com/v2/users/{username}/storages/{storage_id}/files/
```

For public storages, files can be accessed directly without authentication. For private storages, include an authorization header or access token.

***

## File Explorer

The File Explorer provides a modern interface for managing files within your storage, similar to VS Code's file browser.

### Navigation

* **Tree View**: Browse your file structure in a collapsible tree on the left panel
* **Breadcrumbs**: Navigate using the path breadcrumbs at the top
* **Lazy Loading**: Large directories load content on demand for better performance

### File Operations

#### Uploading Files

1. Click the **Upload** button in the toolbar
2. Select one or multiple files from your computer
3. Monitor the upload progress
4. Files appear in the tree once uploaded

You can also drag and drop files directly into the file explorer.

#### Creating Files and Folders

* **New File**: Click the new file icon or use the context menu to create an empty file
* **New Folder**: Click the new folder icon or use the context menu to create a directory

#### Managing Files

* **Rename**: Right-click a file or folder and select Rename, or use the context menu
* **Delete**: Right-click and select Delete, or select files and use the delete button
* **Download**: Click the download button or right-click and select Download
* **Move**: Drag and drop files between folders

### Code Editor

The integrated Monaco editor (the same editor used in VS Code) provides a rich editing experience for text files:

* **Syntax Highlighting**: Automatic language detection based on file extension
* **Multiple Files**: Open multiple files in tabs
* **Unsaved Changes**: Visual indicator for files with unsaved changes
* **Find & Replace**: Full text search within files

Supported file types include JavaScript, TypeScript, Python, HTML, CSS, JSON, YAML, Markdown, and many more.

### File Preview

The explorer includes built-in preview support for various file types:

* **Images**: PNG, JPG, GIF, SVG rendered natively
* **PDFs**: Integrated PDF viewer
* **Videos**: Built-in video player
* **Text/Code**: Displayed in the Monaco editor
* **Other**: File metadata and information panel

***

## VS Code Integration

For a full IDE experience, access the VS Code integration:

1. Navigate to your storage
2. Click **VS Code** in the storage menu
3. A full VS Code environment opens with your storage mounted

The storage is mounted at `/home/coder/storages/{storage_id}` within the VS Code environment.

***

## Terminal Access

The integrated terminal allows running shell commands directly within your storage context:

1. Ensure a **Terminal Image** is configured in storage settings
2. Open the terminal panel in the File Explorer
3. The terminal connects via WebSocket to a container running your specified image
4. Execute commands with direct access to your storage files

Common use cases:

* Running build scripts
* Installing dependencies
* Processing files with command-line tools
* Git operations

***

## Common Use Cases

### Hosting Static Websites

1. Create a storage with **Public Read** enabled
2. Set **Index File** to `index.html`
3. Upload your HTML, CSS, JavaScript, and asset files
4. Access your site at the storage URL

### Firmware Storage

Store device firmware files for OTA updates:

1. Create a storage for firmware files
2. Organize by device type or version (e.g., `/firmware/v1.0.0/device.bin`)
3. Devices can download firmware via the storage URL
4. Use access tokens for secure, authenticated downloads

### Data Exports

Export data from buckets to file storage:

1. Configure bucket export to target a storage
2. Exported files (CSV, JSON) are saved to the storage
3. Download or process exported data as needed

### Custom Dashboard Widgets

Host custom HTML widgets for dashboards:

1. Create HTML/JavaScript widget files
2. Upload to a storage with **Public Read** enabled
3. Reference the file URL in dashboard HTML widgets

### Email Templates

Store custom email templates:

1. Create HTML email templates
2. Upload to a storage
3. Reference in notification configurations

***

## Integration Examples

File Storages can be accessed programmatically via the REST API, enabling integration with external systems, scripts, and devices. All examples use the v2 API endpoints.

### Authentication

All requests to private storages require a Bearer token:

```bash
Authorization: Bearer YOUR_ACCESS_TOKEN
```

Public storages (with **Public Read** enabled) allow GET requests without authentication.

### Listing Files

List contents of a directory:

```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
  "https://iot.thinger.io/v2/users/USERNAME/storages/STORAGE_ID/files/"
```

List including hidden files:

```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
  "https://iot.thinger.io/v2/users/USERNAME/storages/STORAGE_ID/files/?hidden=true"
```

List recursively (all subdirectories):

```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
  "https://iot.thinger.io/v2/users/USERNAME/storages/STORAGE_ID/files/?recursive=true"
```

### Downloading Files

Download a file:

```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
  -o firmware.bin \
  "https://iot.thinger.io/v2/users/USERNAME/storages/STORAGE_ID/files/firmware/v1.0.0/firmware.bin"
```

For public storages, no authorization is needed:

```bash
curl -o firmware.bin \
  "https://iot.thinger.io/v2/users/USERNAME/storages/STORAGE_ID/files/firmware/v1.0.0/firmware.bin"
```

### Uploading Files

Upload a binary file (e.g., firmware):

```bash
curl -X PUT \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/octet-stream" \
  --data-binary @firmware.bin \
  "https://iot.thinger.io/v2/users/USERNAME/storages/STORAGE_ID/files/firmware/v2.0.0/firmware.bin"
```

Upload a text file:

```bash
curl -X PUT \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: text/plain" \
  -d "Hello, World!" \
  "https://iot.thinger.io/v2/users/USERNAME/storages/STORAGE_ID/files/hello.txt"
```

Upload without overwriting existing files (returns 409 if file exists):

```bash
curl -X PUT \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/octet-stream" \
  --data-binary @config.json \
  "https://iot.thinger.io/v2/users/USERNAME/storages/STORAGE_ID/files/config.json?overwrite=false"
```

Upload with progress indicator (useful for large files):

```bash
curl -X PUT \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/octet-stream" \
  --data-binary @large_file.zip \
  --progress-bar \
  "https://iot.thinger.io/v2/users/USERNAME/storages/STORAGE_ID/files/backups/large_file.zip"
```

### Creating Directories

Create a directory (note the trailing slash):

```bash
curl -X PUT \
  -H "Authorization: Bearer YOUR_TOKEN" \
  "https://iot.thinger.io/v2/users/USERNAME/storages/STORAGE_ID/files/new_folder/"
```

Create nested directories:

```bash
curl -X PUT \
  -H "Authorization: Bearer YOUR_TOKEN" \
  "https://iot.thinger.io/v2/users/USERNAME/storages/STORAGE_ID/files/path/to/nested/folder/"
```

### Deleting Files

Delete a file:

```bash
curl -X DELETE \
  -H "Authorization: Bearer YOUR_TOKEN" \
  "https://iot.thinger.io/v2/users/USERNAME/storages/STORAGE_ID/files/old_file.txt"
```

Delete a directory and its contents:

```bash
curl -X DELETE \
  -H "Authorization: Bearer YOUR_TOKEN" \
  "https://iot.thinger.io/v2/users/USERNAME/storages/STORAGE_ID/files/old_folder?recursive=true"
```

### Renaming and Moving Files

Rename a file:

```bash
curl -X PATCH \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"path": "/new_name.txt"}' \
  "https://iot.thinger.io/v2/users/USERNAME/storages/STORAGE_ID/files/old_name.txt"
```

Move a file to a different directory:

```bash
curl -X PATCH \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"path": "/archive/2024/report.pdf"}' \
  "https://iot.thinger.io/v2/users/USERNAME/storages/STORAGE_ID/files/report.pdf"
```

### Getting File Information

Get metadata about a file without downloading it:

```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
  "https://iot.thinger.io/v2/users/USERNAME/storages/STORAGE_ID/files/firmware.bin?info=true"
```

Response:

```json
{
  "name": "firmware.bin",
  "path": "/firmware.bin",
  "type": "file",
  "size": 524288,
  "modified": 1707123456
}
```

### Device Integration Example

A typical IoT device firmware update flow:

```bash
# 1. Check for new firmware version
LATEST=$(curl -s "https://iot.thinger.io/v2/users/USERNAME/storages/firmware/files/latest.txt")

# 2. Download firmware if newer
curl -o /tmp/firmware.bin \
  "https://iot.thinger.io/v2/users/USERNAME/storages/firmware/files/releases/${LATEST}/firmware.bin"

# 3. Verify and apply update
# ... device-specific update logic
```

***

## Access Control

### Permissions

File Storages use fine-grained permissions:

| Permission              | Description                         |
| ----------------------- | ----------------------------------- |
| `ReadStorageFiles`      | List, read, and download files      |
| `UpdateStorageFiles`    | Upload files and create directories |
| `DeleteStorageFiles`    | Delete files and directories        |
| `RenameStorageFiles`    | Rename and move files               |
| `AccessStorageTerminal` | Access the integrated terminal      |

### Access Tokens

Create access tokens with limited permissions for external access:

1. Navigate to the storage settings
2. Create an access token with specific permissions (e.g., read-only)
3. Use the token in API requests or share for limited access

This is useful for:

* Providing read-only access to firmware downloads
* Allowing external services to upload files
* Sharing files without exposing full account credentials

***

## Best Practices

1. **Use descriptive storage IDs** - Choose meaningful names like `firmware`, `website`, `exports` rather than generic names
2. **Organize with directories** - Structure files in logical folders for easier management
3. **Set appropriate access levels** - Only enable public read when necessary
4. **Use access tokens** - Create scoped tokens instead of sharing account credentials
5. **Configure index files** - For web-accessible storages, set an index file for cleaner URLs
6. **Monitor storage usage** - Keep track of storage consumption, especially for large files

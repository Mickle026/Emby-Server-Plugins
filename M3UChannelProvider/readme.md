# 🎮 Emby Plugin — M3U Channel Provider

### Build IPTV and live channel libraries directly from any M3U playlist

---

## 📦 Overview

**M3U Channel Provider** is an Emby Server plugin that dynamically creates fully playable channel collections from standard `.m3u` or `.m3u8` playlists.

It supports:

* Local or remote M3U URLs (HTTP/HTTPS/file)
* Dynamic grouping by **`group-title`**
* Per-stream logos
* Optional proxy URL rewriting
* Embedded channel icon in the Emby dashboard
* Configurable channel display name (via Web UI)
* Fully compliant with Emby 4.8–4.9 SDK

---

## 🚀 Features

| Feature                       | Description                                                                             |
| ----------------------------- | --------------------------------------------------------------------------------------- |
| **M3U Playlist Support**      | Parses large `.m3u` or `.m3u8` files directly from disk or URL.                         |
| **Grouping Mode**             | Displays channels grouped by their `group-title` attribute (e.g. Sports, Movies, News). |
| **Direct Playback**           | Channels play instantly using embedded `MediaSourceInfo` (no resolver required).        |
| **Proxy Option**              | Automatically rewrite stream URLs through your own proxy endpoint.                      |
| **Configurable Channel Name** | Rename the channel directly from the plugin Web UI.                                     |
| **Embedded Logo**             | Uses a built-in `logo.png` from the plugin assembly for the channel tile.               |
| **Emby Compliant**            | Built with Emby SDK 4.8.11+, fully compliant with IChannel and plugin loading rules.    |

---

## ⚙️ Configuration

After installation, go to:

```
Emby Dashboard → Plugins → M3U Channel Provider → Configure
```

Available options:

| Setting                      | Description                                              |
| ---------------------------- | -------------------------------------------------------- |
| **M3U Playlist URL or Path** | Local file path or remote `.m3u` / `.m3u8` URL.          |
| **Proxy URL**                | Optional HTTP proxy endpoint for rewriting stream paths. |
| **Grouping Mode**            | Organize channels by `group-title` or show a flat list.  |
| **Channel Display Name**     | Custom name for how the channel appears in Emby.         |

---

## 🧩 How it Works

* On startup, Emby loads the plugin and registers a new `IChannel` provider.
* When opened, the plugin reads the configured M3U file.
* The root channel view shows folders for each `group-title`.
* Each folder lists playable `ChannelItemInfo` entries with `MediaSourceInfo` pointing to either:

  * The original stream URL (`http://.../playlist.m3u8`)
  * Or your proxy endpoint (`http://localhost:8096/emby/Plugins/M3uChannelProvider/Stream?url=...`).
* Clicking a stream begins playback immediately.

---

## 🧱 Technical Highlights

* Written in **C# / .NET 6**.
* Uses Emby’s native interfaces:

  ```csharp
  public class M3uChannel : IChannel
  ```
* Uses `IHttpClient` from Emby’s internal network API for playlist fetching.
* Embedded resource loading:

  ```csharp
  var type = this.GetType();
  return type.Assembly.GetManifestResourceStream(type.Namespace + ".logo.png");
  ```
* No ASP.NET attributes, no MVC controllers — pure Emby plugin pattern.

---

## 🧠 Design Notes

* Channel icons are embedded as `EmbeddedResource` in the DLL.
* Logos from M3U entries are preserved exactly (no escaping).
* The plugin avoids Emby caching issues by using HTTP URLs for logos.
* Grouping mode replaces pagination for faster browsing with clear categorization.

---

## 🖼️ Example Screenshot

```
Channels
 ├── Movies
 ├── Sports
 ├── Kids
 ├── News
 └── Ungrouped
```

Selecting **Movies** opens a playable list of M3U entries.

---

Copy `Emby.Plugins.M3uChannelProvider.dll` to:

```
%AppData%\Emby-Server\programdata\plugins\
```

Restart Emby Server.

---

## 📜 License

This plugin is released under the **MIT License**.
It is provided “as-is” for educational and integration purposes.

---

## 🧺 Credits

* **Plugin Author:** Michael Williams
* **SDK Compatibility:** Emby Server 4.8.11 → 4.9.x
* **Language:** C# (.NET 6)
* **Project Type:** `Class Library`

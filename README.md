<p align="center">
  <img src="https://img.shields.io/badge/maddyBlog-ff69b4?style=for-the-badge" alt="maddyBlog"/>
  <img src="https://img.shields.io/badge/7%20plugins-8b5cf6?style=for-the-badge" alt="7 Plugins"/>
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript"/>
  <img src="https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=next.js&logoColor=white" alt="Next.js"/>
</p>

<h1 align="center">🧩 maddyBlog Plugins</h1>

<p align="center">
  <b>Official plugin repository for <a href="https://github.com/Maddyrampant/maddyBlog">maddyBlog</a></b><br/>
  <sub>7 production-ready plugins · Hook-based architecture · Admin UI · Fully type-safe</sub>
</p>

<p align="center">
  <a href="#-available-plugins">Plugins</a>
  ·
  <a href="#-getting-started">Getting Started</a>
  ·
  <a href="#-creating-a-plugin">Create a Plugin</a>
  ·
  <a href="#-manifest-format">Manifest</a>
  ·
  <a href="#%EF%B8%8F-hook-system">Hook System</a>
</p>

<br/>

---

<br/>

## ✨ Available Plugins

| Plugin | Description | Permissions | Settings UI |
|--------|-------------|-------------|-------------|
| **seo** | Advanced SEO — meta tags, OG, Twitter Cards, JSON-LD, breadcrumbs, canonical | `WRITE_POST` | ✅ |
| **analytics-dashboard** | Visitor charts, top content, real-time stats widgets | `ACCESS_ANALYTICS` | ✅ |
| **social-login** | Google & GitHub OAuth authentication | `READ_USER`, `WRITE_USER` | ✅ |
| **newsletter** | Email subscription, SMTP, broadcast, welcome emails | `SEND_EMAIL`, `READ_USER` | ✅ |
| **related-posts** | AI-powered content recommendations via tag/category similarity | `READ_POST`, `USE_AI` | ✅ |
| **custom-code** | Inject CSS, JS, and meta tags — no theme editing needed | — | ✅ |
| **cache** | Full-page & fragment caching with TTL, auto-invalidation | `ACCESS_ANALYTICS` | ✅ |

Every plugin adds its own **settings tab** in the admin panel and registers API routes under `/api/plugins/*`.

<br/>

---

<br/>

## 🚀 Getting Started

```bash
# Add a plugin to your maddyBlog install
cd your-maddyblog-project

# Clone the plugins repo into src/plugins
git clone https://github.com/Maddyrampant/maddyblog-plugins.git temp-plugins
cp -r temp-plugins/* src/plugins/
rm -rf temp-plugins

# Activate via admin panel
# Navigate to /admin/plugins and toggle the plugin on
```

Or install directly from the **Marketplace** tab in the maddyBlog admin panel.

<br/>

---

<br/>

## 🛠️ Creating a Plugin

Each plugin is a directory under this repo containing:

```
my-plugin/
├── plugin.json         # Required — metadata, permissions, capabilities
├── index.tsx           # Plugin entry point (default export)
├── MyPluginSettings.tsx # Optional — admin settings UI component
└── ...
```

Every plugin must export a `Plugin` object (or default export) that implements the `Plugin` interface:

```typescript
import { BasePlugin } from "@/lib/plugin/basePlugin";

class MyPlugin extends BasePlugin {
  manifest = {
    name: "my-plugin",
    version: "1.0.0",
    description: "What my plugin does",
    author: "Your Name",
    license: "MIT",
    permissions: ["READ_POST"],
    hooks: ["registerRoutes"],
  };

  constructor() {
    super();
    // Register API routes, UI extensions, hooks, etc.
  }
}

export const plugin = new MyPlugin();
```

<br/>

---

<br/>

## 📋 Manifest Format

Every plugin **must** include a `plugin.json` at its root:

```json
{
  "name": "my-plugin",
  "version": "1.0.0",
  "description": "What my plugin does",
  "author": "Your Name",
  "license": "MIT",
  "permissions": ["READ_POST"],
  "hooks": ["registerRoutes"],
  "settings": true
}
```

### Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | `string` | ✅ | Kebab-case plugin identifier |
| `version` | `string` | ❌ | SemVer (defaults to `1.0.0`) |
| `description` | `string` | ❌ | Shown in marketplace & admin |
| `author` | `string` | ❌ | Author credit |
| `license` | `string` | ❌ | SPDX license identifier |
| `permissions` | `string[]` | ❌ | Required permissions |
| `hooks` | `string[]` | ❌ | Hooks the plugin registers |
| `settings` | `boolean` | ❌ | Whether plugin has an admin settings UI |

### Available Permissions

| Permission | Description |
|------------|-------------|
| `READ_POST` | Read blog posts |
| `WRITE_POST` | Create/update posts |
| `DELETE_POST` | Delete posts |
| `READ_USER` | Read user data |
| `WRITE_USER` | Modify user data |
| `MANAGE_COMMENTS` | Moderate comments |
| `SEND_EMAIL` | Send emails via configured provider |
| `ACCESS_ANALYTICS` | Read analytics data |
| `MANAGE_PLUGINS` | Enable/disable other plugins |
| `USE_AI` | Access AI provider APIs |
| `MANAGE_AI` | Configure AI providers |

<br/>

---

<br/>

## ⚙️ Hook System

The maddyBlog plugin engine supports these hook points:

| Hook | Type | Trigger |
|------|------|---------|
| `beforePostSave` | Data | Before a post is created/updated |
| `afterPostSave` | Data | After a post is saved |
| `afterPostDeleted` | Data | After a post is deleted |
| `beforeCommentCreate` | Data | Before a comment is created |
| `afterCommentCreated` | Data | After a comment is created |
| `beforeUserRegister` | Data | Before user registration |
| `afterUserRegister` | Data | After user registration |
| `beforeRenderPost` | Data | Before a post is rendered |
| `afterRenderPost` | Data | After a post is rendered |
| `injectAdminSidebar` | UI | Add items to admin sidebar |
| `injectPostView` | UI | Add content to post view |
| `injectPostHeader` | UI | Add content above post content |
| `injectPostFooter` | UI | Add content below post content |
| `registerRoutes` | API | Register additional API routes |

<br/>

---

<br/>

## 📦 Theme Compatibility

These plugins work with all 9 official maddyBlog themes out of the box. Theme components opt-in to plugin features via the `supportedFeatures` field in their `theme.json`.

<br/>

---

<br/>

## 📄 License

All plugins in this repository are licensed under **MIT** unless otherwise noted.

<br/>

---

<br/>

<p align="center">
  <sub>built with ❤️ by <a href="https://github.com/Maddyrampant">Maddyrampant</a></sub>
  <br/>
  <sub>Part of the <a href="https://github.com/Maddyrampant/maddyBlog">maddyBlog</a> ecosystem</sub>
</p>

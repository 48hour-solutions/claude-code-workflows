# Comprehensive Guide to Building Electron Applications with TypeScript

## Table of Contents
1.  [Introduction](#1-introduction)
    * [What is Electron?](#11-what-is-electron)
    * [Why Use TypeScript with Electron?](#12-why-use-typescript-with-electron)
2.  [Prerequisites](#2-prerequisites)
3.  [Project Setup](#3-project-setup)
    * [Initializing the Project](#31-initializing-the-project)
    * [Installing Dependencies](#32-installing-dependencies)
    * [Configuring TypeScript](#33-configuring-typescript)
    * [Configuring ESLint and Prettier](#34-configuring-eslint-and-prettier)
    * [Updating `package.json` Scripts](#35-updating-packagejson-scripts)
4.  [Recommended Project Structure](#4-recommended-project-structure)
5.  [Core Electron Concepts](#5-core-electron-concepts)
    * [The Main Process](#51-the-main-process)
    * [The Renderer Process](#52-the-renderer-process)
    * [The Preload Script](#53-the-preload-script)
    * [The Process Model: A Summary](#54-the-process-model-a-summary)
6.  [Inter-Process Communication (IPC)](#6-inter-process-communication-ipc)
    * [Security First: The `contextBridge`](#61-security-first-the-contextbridge)
    * [One-Way Communication: Renderer to Main (`invoke`)](#62-one-way-communication-renderer-to-main-invoke)
    * [Two-Way Communication: Main to Renderer (`send`)](#63-two-way-communication-main-to-renderer-send)
    * [Typing the IPC Bridge](#64-typing-the-ipc-bridge)
7.  [Security Best Practices](#7-security-best-practices)
    * [Security Checklist Summary](#71-security-checklist-summary)
    * [Enable Context Isolation](#72-enable-context-isolation)
    * [Disable the Remote Module](#73-disable-the-remote-module)
    * [Enable Sandboxing](#74-enable-sandboxing)
    * [Handle Session Permissions](#75-handle-session-permissions)
    * [Use `ses.setPermissionRequestHandler()`](#76-use-sessetpermissionrequesthandler)
    * [Validate Sender in IPC](#77-validate-sender-in-ipc)
    * [Content Security Policy (CSP)](#78-content-security-policy-csp)
    * [Use `shell.openExternal()` for Untrusted Links](#79-use-shellopenexternal-for-untrusted-links)
    * [Keep Dependencies Updated](#710-keep-dependencies-updated)
8.  [Development Workflow](#8-development-workflow)
    * [Hot Reloading for Main and Renderer](#81-hot-reloading-for-main-and-renderer)
    * [Using Electron DevTools](#82-using-electron-devtools)
9.  [Integrating Frontend Frameworks](#9-integrating-frontend-frameworks)
    * [General Approach](#91-general-approach)
    * [Example with React](#92-example-with-react)
10. [Building and Packaging](#10-building-and-packaging)
    * [Introduction to Electron Forge](#101-introduction-to-electron-forge)
    * [Setting up Electron Forge with TypeScript](#102-setting-up-electron-forge-with-typescript)
    * [Building for Production](#103-building-for-production)
    * [Code Signing and Notarization](#104-code-signing-and-notarization)
11. [Performance Best Practices](#11-performance-best-practices)
    * [Lazy Load Modules](#111-lazy-load-modules)
    * [Offload CPU-Intensive Tasks](#112-offload-cpu-intensive-tasks)
    * [Optimize Renderer Performance](#113-optimize-renderer-performance)
    * [Manage Memory Usage](#114-manage-memory-usage)
    * [Bundle Your Code](#115-bundle-your-code)
12. [State Management](#12-state-management)
    * [Where Should State Live?](#121-where-should-state-live)
    * [Using Libraries like Redux or Zustand](#122-using-libraries-like-redux-or-zustand)
    * [Using `electron-store`](#123-using-electron-store)
13. [Testing Your Application](#13-testing-your-application)
    * [Unit Testing](#131-unit-testing)
    * [End-to-End (E2E) Testing with Playwright or Spectron](#132-end-to-end-e2e-testing-with-playwright-or-spectron)
14. [Common Pitfalls and How to Avoid Them](#14-common-pitfalls-and-how-to-avoid-them)
    * [Blocking the Main Process](#141-blocking-the-main-process)
    * [Ignoring Security Warnings](#142-ignoring-security-warnings)
    * [Misunderstanding the Process Model](#143-misunderstanding-the-process-model)
    * [Large Application Size](#144-large-application-size)
15. [Recipes & Advanced Topics](#15-recipes--advanced-topics)
    * [Creating Custom Menus](#151-creating-custom-menus)
    * [Working with Native Node.js Modules](#152-working-with-native-nodejs-modules)
    * [Handling Application Updates](#153-handling-application-updates)
16. [Cheatsheet](#16-cheatsheet)
    * [Key Electron Modules](#161-key-electron-modules)
    * [Essential `BrowserWindow` Options](#162-essential-browserwindow-options)
    * [IPC Methods](#163-ipc-methods)
17. [Further Resources](#17-further-resources)

---

## 1. Introduction

### 1.1 What is Electron?

Electron is an open-source framework for building cross-platform desktop applications using web technologies: HTML, CSS, and JavaScript. It combines the Chromium rendering engine and the Node.js runtime environment, allowing developers to create desktop apps that run on Windows, macOS, and Linux from a single codebase.

* **Main Process**: Acts as the application's backend. It runs a full Node.js environment, manages application lifecycle events, and creates and controls renderer processes (`BrowserWindow` instances).
* **Renderer Process**: Represents a window in your application. It's essentially a web page running in a Chromium browser environment. It does **not** have direct access to Node.js APIs for security reasons.

### 1.2 Why Use TypeScript with Electron?

TypeScript is a superset of JavaScript that adds static typing. Using it with Electron provides significant benefits, especially for large and complex applications:

* **Enhanced Safety**: Catch type-related errors at compile time, not runtime. This is crucial for building stable desktop applications.
* **Improved Developer Experience**: Autocompletion, type inference, and interface definitions make navigating Electron's extensive APIs much easier.
* **Better Code Maintainability**: Types act as documentation, making the codebase easier to understand, refactor, and onboard new developers.
* **Access to Modern Features**: Use the latest ECMAScript features and compile them down to a compatible JavaScript version.

---

## 2. Prerequisites

Before you begin, ensure you have the following installed:

* **Node.js**: Version 18.x or later.
* **npm** or **yarn** or **pnpm**: A package manager for Node.js.

You can verify your installation by running:
```bash
node -v
npm -v
````

-----

## 3\. Project Setup

We will set up a robust Electron project from scratch using TypeScript, ESLint, and Prettier for a clean and maintainable codebase.

### 3.1 Initializing the Project

Create a new directory for your project and initialize a `package.json` file.

```bash
mkdir my-electron-app
cd my-electron-app
npm init -y
```

### 3.2 Installing Dependencies

Install the necessary dependencies.

**Core Dependencies:**

```bash
npm install electron
```

**Development Dependencies:**

```bash
npm install -D typescript @types/node electron-builder eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin prettier eslint-config-prettier eslint-plugin-prettier concurrently wait-on
```

  * `typescript`: The TypeScript compiler.
  * `@types/node`: Type definitions for Node.js.
  * `electron-builder`: A popular tool for packaging and building Electron apps (alternative: `electron-forge`).
  * `eslint` & plugins: For linting and code quality.
  * `prettier` & plugins: For consistent code formatting.
  * `concurrently`: To run multiple commands simultaneously (e.g., start Electron and the TS compiler).
  * `wait-on`: To wait for a file or port to be available before starting another command.

### 3.3 Configuring TypeScript

Create a `tsconfig.json` file in the root of your project. This file specifies the compiler options for TypeScript.

```json
// tsconfig.json
{
  "compilerOptions": {
    /* Base Options */
    "target": "ES2022",
    "module": "CommonJS",
    "lib": ["ES2022", "DOM"],
    "allowJs": true,
    "checkJs": false,
    "sourceMap": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "removeComments": true,
    "isolatedModules": true,
    "esModuleInterop": true,

    /* Strict Type-Checking Options */
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "strictBindCallApply": true,
    "strictPropertyInitialization": true,
    "noImplicitThis": true,
    "alwaysStrict": true,

    /* Module Resolution Options */
    "moduleResolution": "node",
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    },
    "resolveJsonModule": true,

    /* Advanced Options */
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

**Key `tsconfig.json` options explained:**

  * `"target": "ES2022"`: Compiles to a modern JavaScript version. Electron's underlying Node.js and Chromium support this.
  * `"module": "CommonJS"`: The standard module system for Node.js, used by Electron's main process.
  * `"outDir": "./dist"`: The directory where compiled JavaScript files will be placed.
  * `"rootDir": "./src"`: The root directory of your source TypeScript files.
  * `"strict": true`: Enables all strict type-checking options, which is highly recommended for robust code.
  * `"esModuleInterop": true`: Allows for better compatibility between CommonJS and ES modules.

### 3.4 Configuring ESLint and Prettier

**1. Create `.eslintrc.json`:**
This file configures ESLint to use the TypeScript parser and recommended rules.

```json
// .eslintrc.json
{
  "parser": "@typescript-eslint/parser",
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:prettier/recommended"
  ],
  "parserOptions": {
    "ecmaVersion": "latest",
    "sourceType": "module"
  },
  "env": {
    "browser": true,
    "es2021": true,
    "node": true
  },
  "rules": {
    // Add custom rules here if needed
  }
}
```

**2. Create `.prettierrc.json`:**
This file configures your code formatting rules.

```json
// .prettierrc.json
{
  "semi": true,
  "trailingComma": "all",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2
}
```

**3. Create `.prettierignore`:**
Ignore files that shouldn't be formatted.

```
# .prettierignore
node_modules
dist
build
```

### 3.5 Updating `package.json` Scripts

Modify the `scripts` section of your `package.json` to automate your development and build processes.

```json
// package.json
{
  "name": "my-electron-app",
  "version": "1.0.0",
  "description": "A robust Electron app with TypeScript",
  "main": "dist/main.js",
  "scripts": {
    "start": "npm run build && electron .",
    "build": "tsc",
    "watch": "tsc -w",
    "lint": "eslint --ext .ts,.tsx src",
    "format": "prettier --write \"src/**/*.ts\"",
    "dev": "concurrently \"npm:watch\" \"wait-on dist/main.js && electron .\""
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "electron": "^28.0.0"
  },
  "devDependencies": {
    // ... your dev dependencies
  }
}
```

  * **`"main": "dist/main.js"`**: Tells Electron where to find the entry point of your application *after* TypeScript compilation.
  * **`build`**: Compiles your TypeScript code from `src` to `dist`.
  * **`watch`**: Runs the TypeScript compiler in watch mode.
  * **`dev`**: The primary development script. It runs the TypeScript watcher and Electron concurrently. `wait-on` ensures Electron doesn't start until the initial compilation is complete.

-----

## 4\. Recommended Project Structure

A well-organized project structure is crucial for maintainability.

```
my-electron-app/
├── dist/                     # Compiled output (JavaScript files) - auto-generated
├── node_modules/             # Project dependencies
├── release/                  # Packaged application builds - auto-generated
├── src/                      # TypeScript source code
│   ├── main/                 # Main Process code
│   │   ├── main.ts           # Main process entry point
│   │   └── preload.ts        # Preload script
│   ├── renderer/             # Renderer Process code (UI)
│   │   ├── index.html        # Main HTML file for the UI
│   │   ├── renderer.ts       # Main script for the UI
│   │   └── style.css         # Styles for the UI
│   ├── shared/               # Code/types shared between processes
│   │   └── types.ts          # Shared TypeScript type definitions
│   └── assets/               # Static assets (icons, images)
│
├── .eslintrc.json            # ESLint configuration
├── .prettierrc.json          # Prettier configuration
├── package.json
├── package-lock.json
└── tsconfig.json
```

-----

## 5\. Core Electron Concepts

Understanding Electron's multi-process architecture is the most important step to becoming proficient.

### 5.1 The Main Process

  * **Role**: The application's backend and entry point.
  * **Environment**: Full Node.js environment.
  * **Responsibilities**:
      * Creating and managing `BrowserWindow` instances (your app windows).
      * Handling application lifecycle events (`ready`, `window-all-closed`, etc.).
      * Interacting with the operating system (creating menus, dialogs, tray icons).
      * Performing resource-intensive tasks.
      * Listening for and responding to IPC messages from renderer processes.

**Example `src/main/main.ts`:**

```typescript
// src/main/main.ts
import { app, BrowserWindow, ipcMain } from 'electron';
import path from 'path';

// This function creates the main application window.
function createWindow() {
  const mainWindow = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      // Best Practice: Isolate renderer context from Node.js APIs
      contextIsolation: true,
      // Best Practice: Load a preload script to securely expose APIs
      preload: path.join(__dirname, 'preload.js'),
    },
  });

  // Load the renderer's HTML file.
  mainWindow.loadFile(path.join(__dirname, '../renderer/index.html'));

  // Open the DevTools.
  // mainWindow.webContents.openDevTools();
}

// This method will be called when Electron has finished initialization.
app.whenReady().then(() => {
  createWindow();

  // Handle creating a new window on macOS when the dock icon is clicked.
  app.on('activate', () => {
    if (BrowserWindow.getAllWindows().length === 0) {
      createWindow();
    }
  });
});

// Quit when all windows are closed, except on macOS.
app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') {
    app.quit();
  }
});

// Example IPC handler in the main process
ipcMain.handle('get-app-version', () => {
  return app.getVersion();
});
```

### 5.2 The Renderer Process

  * **Role**: The application's frontend (the UI).
  * **Environment**: A standard web environment (Chromium), **without** direct Node.js access by default.
  * **Responsibilities**:
      * Displaying the user interface (HTML, CSS).
      * Handling user interactions.
      * Sending IPC messages to the main process to request OS-level actions or data.

**Example `src/renderer/index.html`:**

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>My Electron App</title>
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <h1>Hello from Electron!</h1>
    <p>App Version: <span id="app-version">Loading...</span></p>
    <script src="./renderer.js"></script>
  </body>
</html>
```

**Example `src/renderer/renderer.ts`:**

```typescript
// src/renderer/renderer.ts

// This function runs when the window is loaded.
window.addEventListener('DOMContentLoaded', () => {
  const versionElement = document.getElementById('app-version');
  
  if (versionElement && window.electronAPI) {
    // Use the exposed API from the preload script
    window.electronAPI.getAppVersion().then((version: string) => {
      versionElement.innerText = version;
    });
  }
});

```

### 5.3 The Preload Script

  * **Role**: A secure bridge between the renderer process and the main process.
  * **Environment**: Runs in the renderer's context but **has access to Node.js APIs**.
  * **Responsibilities**:
      * To selectively and securely expose specific Node.js or Electron main process functionality to the renderer process via the `contextBridge`.

This is the cornerstone of modern Electron security. **Never** expose entire modules like `fs` or `ipcRenderer` to the renderer.

**Example `src/main/preload.ts`:**

```typescript
// src/main/preload.ts
import { contextBridge, ipcRenderer } from 'electron';

// Expose a safe, limited API to the renderer process.
contextBridge.exposeInMainWorld('electronAPI', {
  // One-way from renderer to main
  getAppVersion: () => ipcRenderer.invoke('get-app-version'),
  // Two-way from main to renderer
  onUpdateMessage: (callback: (message: string) => void) => {
    ipcRenderer.on('update-message', (_event, message) => callback(message));
    // Return a function to remove the listener
    return () => ipcRenderer.removeAllListeners('update-message');
  },
});
```

### 5.4 The Process Model: A Summary

```
[ Main Process (Node.js) ]
  |
  |-- Manages app life, OS integration
  |-- Creates BrowserWindow
  |
  +-----> [ BrowserWindow ]
             |
             +-----> [ Renderer Process (Chromium) ] <--X-- [ Node.js APIs ]
             |          |
             |          |--- Renders HTML/CSS, handles UI logic
             |          |
             +-----> [ Preload Script (Privileged) ]
                        |
                        |--- Access to Node.js
                        |--- Securely exposes functions via contextBridge
                        |
                        +-----> [ window.electronAPI ] (available in Renderer)
```

-----

## 6\. Inter-Process Communication (IPC)

IPC is how your main and renderer processes communicate. Modern Electron IPC prioritizes security through the `contextBridge`.

### 6.1 Security First: The `contextBridge`

The `contextBridge` module ensures that the APIs you expose from your preload script cannot be modified by scripts running in the renderer process. It creates a separate, isolated world for your exposed APIs.

**Key Rule:** Always use `contextBridge.exposeInMainWorld` in your preload script. Do not attach things directly to the `window` object.

### 6.2 One-Way Communication: Renderer to Main (`invoke`/`handle`)

This pattern is for when the renderer needs to ask the main process to do something and get a response back (like a function call).

1.  **Renderer**: Calls an exposed function that uses `ipcRenderer.invoke()`.
2.  **Main**: Listens for the channel with `ipcMain.handle()` and returns a value (or a `Promise`).

**Preload Script (`src/main/preload.ts`):**

```typescript
contextBridge.exposeInMainWorld('myAPI', {
  doSomething: (data: string) => ipcRenderer.invoke('do-something', data),
});
```

**Main Process (`src/main/main.ts`):**

```typescript
import fs from 'fs/promises';

ipcMain.handle('do-something', async (_event, data: string) => {
  console.log(`Main process received: ${data}`);
  // Example: Perform a privileged action, like reading a file.
  try {
    const fileContent = await fs.readFile('path/to/some/file.txt', 'utf-8');
    return { success: true, content: fileContent };
  } catch (error) {
    console.error('Failed to read file:', error);
    return { success: false, error: 'File read failed' };
  }
});
```

**Renderer Process (`src/renderer/renderer.ts`):**

```typescript
async function handleAction() {
  const result = await window.myAPI.doSomething('Hello from Renderer');
  if (result.success) {
    console.log('File content:', result.content);
  } else {
    console.error('Error:', result.error);
  }
}
```

### 6.3 Two-Way Communication: Main to Renderer (`send`)

This pattern is for when the main process needs to push an event or data to the renderer without being asked (e.g., a download progress update).

1.  **Main**: Uses `webContents.send()` on a specific `BrowserWindow` instance.
2.  **Preload**: Exposes a function that registers a listener using `ipcRenderer.on()`.
3.  **Renderer**: Calls the exposed function with a callback to handle the incoming data.

**Main Process (`src/main/main.ts`):**

```typescript
// Assume 'mainWindow' is your BrowserWindow instance
function notifyRendererOfUpdate() {
  mainWindow.webContents.send('download-progress', { percent: 50 });
}
```

**Preload Script (`src/main/preload.ts`):**

```typescript
contextBridge.exposeInMainWorld('myAPI', {
  onDownloadProgress: (callback: (progress: { percent: number }) => void) => {
    const listener = (_event, progress) => callback(progress);
    ipcRenderer.on('download-progress', listener);
    // Return a cleanup function
    return () => ipcRenderer.removeListener('download-progress', listener);
  },
});
```

**Renderer Process (`src/renderer/renderer.ts`):**

```typescript
const cleanupListener = window.myAPI.onDownloadProgress((progress) => {
  console.log(`Download is ${progress.percent}% complete.`);
});

// To stop listening later:
// cleanupListener();
```

### 6.4 Typing the IPC Bridge

To get full TypeScript benefits, you need to type the API exposed on the `window` object.

**1. Create a shared types file (`src/shared/types.ts`):**

```typescript
// src/shared/types.ts
export interface IElectronAPI {
  getAppVersion: () => Promise<string>;
  onUpdateMessage: (callback: (message: string) => void) => () => void;
}
```

**2. Augment the global `Window` interface (`src/renderer/renderer.d.ts`):**
Create a new file `renderer.d.ts` in your renderer directory.

```typescript
// src/renderer/renderer.d.ts
import { IElectronAPI } from '../shared/types';

declare global {
  interface Window {
    electronAPI: IElectronAPI;
  }
}
```

Now, TypeScript in your renderer files will know about `window.electronAPI` and its methods, providing autocompletion and type checking.

-----

## 7\. Security Best Practices

Electron applications are powerful, but they also have a unique threat model. Adhering to security best practices is **not optional**.

### 7.1 Security Checklist Summary

1.  ✅ **Enable `contextIsolation`**: `webPreferences: { contextIsolation: true }`
2.  ✅ **Disable `nodeIntegration`**: This is the default (`nodeIntegration: false`).
3.  ✅ **Disable the `remote` module**: It's deprecated and insecure.
4.  ✅ **Enable Sandboxing**: `webPreferences: { sandbox: true }`
5.  ✅ **Use a `preload` script**: The only secure way to bridge worlds.
6.  ✅ **Use `contextBridge`**: Never attach APIs directly to `window`.
7.  ✅ **Validate IPC Senders**: Check `event.senderFrame` in IPC handlers.
8.  ✅ **Implement a Content Security Policy (CSP)**: Mitigates XSS attacks.
9.  ✅ **Use `shell.openExternal()` for external links**: Prevents the new window from having Node.js access.
10. ✅ **Keep Electron and dependencies up-to-date**: Use `npm audit` regularly.

### 7.2 Enable Context Isolation

**What it does:** Ensures that your preload script and the renderer's JavaScript code run in separate contexts, preventing the renderer from accessing internal Electron APIs or your preload script's privileged functions.
**How:**

```typescript
const mainWindow = new BrowserWindow({
  webPreferences: {
    contextIsolation: true, // This is true by default in recent Electron versions
    // ...
  },
});
```

### 7.3 Disable the Remote Module

The `@electron/remote` module provides a simple way to access main process objects from the renderer, but it is a significant security risk. It has been deprecated in favor of secure IPC.
**How:**
In your main process:

```typescript
// If you ever enabled it, remove this line:
// require('@electron/remote/main').initialize();

// And in your BrowserWindow constructor, ensure it's not enabled:
const mainWindow = new BrowserWindow({
  webPreferences: {
    // enableRemoteModule: false, // This is false by default
    // ...
  },
});
```

### 7.4 Enable Sandboxing

**What it does:** Puts the renderer process in a restricted environment, similar to a standard web browser tab. It prevents the renderer from performing most OS-level actions.
**How:**

```typescript
const mainWindow = new BrowserWindow({
  webPreferences: {
    sandbox: true, // Enable sandboxing
    // ...
  },
});
```

**Note:** When sandboxing is enabled, your preload script will not have access to Node.js modules. You must use IPC to request any Node-related actions from the main process.

### 7.5 Handle Session Permissions

Don't grant permissions (like microphone, camera access) by default. Prompt the user.

### 7.6 Use `ses.setPermissionRequestHandler()`

Intercept permission requests from a renderer and decide whether to grant them.

```typescript
// src/main/main.ts
import { session } from 'electron';

app.whenReady().then(() => {
  session.defaultSession.setPermissionRequestHandler((webContents, permission, callback) => {
    const url = webContents.getURL();

    // Example: Only allow camera access for a specific origin
    if (permission === 'media' && url.startsWith('[https://my-trusted-site.com/](https://my-trusted-site.com/)')) {
      // You could prompt the user here instead of automatically granting
      return callback(true);
    }

    // Deny all other requests
    return callback(false);
  });

  createWindow();
});
```

### 7.7 Validate Sender in IPC

In your `ipcMain.handle` or `ipcMain.on` listeners, you can check where the request is coming from to prevent untrusted origins from triggering actions.

```typescript
ipcMain.handle('sensitive-action', (event, ...args) => {
  const frame = event.senderFrame;
  const url = new URL(frame.url);

  if (url.protocol === 'file:') {
    // Request is from a local file, likely trusted.
    // Proceed with action...
  } else {
    console.warn(`Blocked IPC from untrusted origin: ${url.origin}`);
    throw new Error('IPC request from untrusted origin blocked.');
  }
});
```

### 7.8 Content Security Policy (CSP)

A CSP is a powerful defense against Cross-Site Scripting (XSS). It tells the browser which sources of content (scripts, styles, images) are trusted.

**How:** Add a `<meta>` tag to your `index.html` or set the `Content-Security-Policy` HTTP header using `session.defaultSession.webRequest.onHeadersReceived`.

**Example `index.html`:**

```html
<head>
  <meta http-equiv="Content-Security-Policy" content="script-src 'self'; style-src 'self' 'unsafe-inline';">
</head>
```

This policy allows scripts (`script-src`) only from the same origin (`'self'`) and styles (`style-src`) from the same origin or as inline styles. Be as restrictive as possible.

### 7.9 Use `shell.openExternal()` for Untrusted Links

If your app displays content with external links, don't let them open in a new `BrowserWindow`. A new window would inherit your app's privileges. Instead, open them in the user's default browser.

```typescript
// In your main process, after getting a link from IPC
import { shell } from 'electron';

ipcMain.on('open-external-link', (_event, url) => {
  // Validate the URL first
  if (url.startsWith('https://') || url.startsWith('http://')) {
    shell.openExternal(url);
  }
});
```

### 7.10 Keep Dependencies Updated

Vulnerabilities are often found in libraries. Regularly update your dependencies and check for known issues.

```bash
# Check for known vulnerabilities
npm audit

# Fix vulnerabilities if possible
npm audit fix
```

-----

## 8\. Development Workflow

### 8.1 Hot Reloading for Main and Renderer

A smooth development workflow is key. You want changes in your code to reflect in the running app without a manual restart. Our `dev` script in `package.json` already handles basic recompilation, but we can improve it.

For a more advanced setup, consider using tools like `electron-reloader` or configuring your frontend bundler (like Vite or Webpack) for Hot Module Replacement (HMR) in the renderer.

**Simple Main Process Reloading with `electron-reloader`:**

1.  `npm install -D electron-reloader`
2.  In `src/main/main.ts`:
    ```typescript
    // Add this at the top, but handle production builds
    try {
      if (process.env.NODE_ENV === 'development') {
        require('electron-reloader')(module);
      }
    } catch (_) {}
    ```

### 8.2 Using Electron DevTools

You can open the developer tools for any `BrowserWindow` instance to debug your renderer process just like a web page.

```typescript
// In your main process:
mainWindow.webContents.openDevTools();
```

You can also debug the main process using Node.js inspectors. Modify your `dev` script:

```json
// package.json
"scripts": {
  "dev": "concurrently \"npm:watch\" \"wait-on dist/main.js && electron --inspect=5858 .\""
}
```

Then, you can attach a debugger (like the one in VS Code or Chrome's `chrome://inspect`) to port `5858`.

-----

## 9\. Integrating Frontend Frameworks

Electron is UI-agnostic. You can use React, Vue, Svelte, Angular, or any other web framework in your renderer process.

### 9.1 General Approach

1.  **Set up your framework**: Use the framework's standard tooling (e.g., `create-react-app`, `create-vite`) inside the `src/renderer` directory or as the root of your project.
2.  **Configure the build**: Adjust the framework's build process to output its files (`index.html`, `bundle.js`, etc.) into a location Electron can find, like `dist/renderer`.
3.  **Load the output**: Point Electron's `mainWindow.loadFile()` or `mainWindow.loadURL()` to the HTML file generated by your framework's build process. In development, you can point `loadURL` to the dev server (e.g., `http://localhost:3000`).

### 9.2 Example with React (using Vite)

Vite is a modern and fast build tool that works wonderfully with Electron.

1.  **Set up Vite**: `npm create vite@latest src/renderer -- --template react-ts`
2.  **Install dependencies**: `cd src/renderer && npm install && cd ../..`
3.  **Configure `vite.config.ts`**:
    ```typescript
    // src/renderer/vite.config.ts
    import { defineConfig } from 'vite'
    import react from '@vitejs/plugin-react'

    export default defineConfig({
      plugins: [react()],
      base: './', // Use relative paths for Electron
      build: {
        outDir: '../../dist/renderer', // Output to the main dist folder
        emptyOutDir: true,
      }
    })
    ```
4.  **Update `main.ts` to load from Vite dev server**:
    ```typescript
    // src/main/main.ts
    if (process.env.NODE_ENV === 'development') {
      mainWindow.loadURL('http://localhost:5173'); // Default Vite port
    } else {
      mainWindow.loadFile(path.join(__dirname, '../renderer/index.html'));
    }
    ```
5.  **Update `package.json` scripts**:
    ```json
    "scripts": {
      "dev:renderer": "npm run dev --prefix src/renderer",
      "build:renderer": "npm run build --prefix src/renderer",
      "build:main": "tsc",
      "build": "npm run build:main && npm run build:renderer",
      "dev": "concurrently \"npm:build:main -- -w\" \"npm run dev:renderer\" \"wait-on http://localhost:5173 && electron .\""
    }
    ```

-----

## 10\. Building and Packaging

Once your app is ready, you need to package it into distributable formats (.exe, .dmg, .deb, etc.). **Electron Forge** and **Electron Builder** are the two most popular tools for this. We'll focus on Electron Forge.

### 10.1 Introduction to Electron Forge

Electron Forge is an all-in-one tool for packaging and publishing Electron applications. It handles icons, code signing, installers, and more.

### 10.2 Setting up Electron Forge with TypeScript

1.  **Install Forge CLI and import the project**:

    ```bash
    npm install -D @electron-forge/cli
    npx electron-forge import
    ```

    This will detect your setup and create a `forge.config.js` file.

2.  **Configure `forge.config.js` for TypeScript**:
    You'll need a Webpack or Vite plugin for Forge to handle bundling your TypeScript code. Here's a basic Webpack configuration.

    First, install Webpack dependencies:

    ```bash
    npm install -D @electron-forge/plugin-webpack ts-loader css-loader style-loader
    ```

    Then, update `forge.config.js`:

    ```javascript
    // forge.config.js
    module.exports = {
      packagerConfig: {},
      rebuildConfig: {},
      makers: [
        {
          name: '@electron-forge/maker-squirrel', // for Windows
          config: {},
        },
        {
          name: '@electron-forge/maker-zip',
          platforms: ['darwin'], // for macOS
        },
        {
          name: '@electron-forge/maker-deb', // for Debian/Ubuntu
          config: {},
        },
      ],
      plugins: [
        {
          name: '@electron-forge/plugin-webpack',
          config: {
            mainConfig: './webpack.main.config.js',
            renderer: {
              config: './webpack.renderer.config.js',
              entryPoints: [
                {
                  html: './src/renderer/index.html',
                  js: './src/renderer/renderer.ts',
                  name: 'main_window',
                  preload: {
                    js: './src/main/preload.ts',
                  },
                },
              ],
            },
          },
        },
      ],
    };
    ```

    You will also need to create `webpack.main.config.js` and `webpack.renderer.config.js`. You can find templates for these in the official Electron Forge documentation.

### 10.3 Building for Production

With Electron Forge configured, building is simple:

```bash
# Create a distributable package
npm run make
```

This command will:

1.  Bundle your application code.
2.  Package it into a platform-specific format (e.g., an executable).
3.  Create an installer.
4.  Place the final output in the `out` directory.

### 10.4 Code Signing and Notarization

For professional distribution, especially on macOS and Windows, you must sign your application.

  * **macOS**: Requires an Apple Developer ID certificate. After signing, the app must be "notarized" by Apple to pass Gatekeeper checks.
  * **Windows**: Requires an Authenticode certificate. This prevents the "Unknown Publisher" warning.

Electron Forge and Electron Builder have extensive documentation on how to configure code signing using environment variables for your certificates.

-----

## 11\. Performance Best Practices

### 11.1 Lazy Load Modules

Don't `require` or `import` modules until you need them. The startup time of your main process is critical.

**Bad:**

```typescript
import { dialog } from 'electron';
// ... app starts ...
```

**Good:**

```typescript
async function showSaveDialog() {
  const { dialog } = await import('electron');
  dialog.showSaveDialogSync();
}
```

### 11.2 Offload CPU-Intensive Tasks

Never block the main process or the renderer's UI thread. For heavy computations, use:

  * **Web Workers** in the renderer process.
  * **Worker Threads** (`worker_threads`) in the main process.
  * A hidden renderer process that acts as a background worker.

### 11.3 Optimize Renderer Performance

Treat your renderer process like a high-performance web application.

  * Use `requestAnimationFrame` for animations.
  * Virtualize long lists to only render visible items.
  * Profile your UI with Chrome DevTools to find performance bottlenecks.

### 11.4 Manage Memory Usage

Desktop apps run for a long time. Memory leaks are a serious problem.

  * Remove IPC listeners and other event listeners when a component or window is destroyed. Our `onDownloadProgress` example returned a cleanup function for this reason.
  * Use the DevTools Memory tab to hunt for leaks.

### 11.5 Bundle Your Code

For production, bundling your code with a tool like Webpack, Vite, or Parcel has several benefits:

  * Reduces the number of `require` calls, improving startup time.
  * Can perform tree-shaking to remove unused code, reducing app size.
  * Minifies the code, further reducing size.

-----

## 12\. State Management

For complex applications, managing state is a common challenge.

### 12.1 Where Should State Live?

  * **UI State**: Belongs in the renderer process (e.g., which tab is active).
  * **Application State**: Often best managed in the main process as the single source of truth. This prevents state desynchronization if you have multiple windows. The main process can then push state updates to all renderers via IPC.

### 12.2 Using Libraries like Redux or Zustand

You can use standard web state management libraries. A common pattern is to run the Redux store in the main process and use an IPC-based middleware to sync actions and state changes with the renderer processes.

### 12.3 Using `electron-store`

For simple, persistent key-value storage (like user settings), `electron-store` is an excellent choice.

```bash
npm install electron-store
```

```typescript
// In the main process
import Store from 'electron-store';

const store = new Store();

store.set('user.theme', 'dark');
console.log(store.get('user.theme'));
```

It's safe to use from both the main and renderer processes (via IPC).

-----

## 13\. Testing Your Application

### 13.1 Unit Testing

Use frameworks like **Jest** or **Vitest** to test individual functions and modules in both your main and renderer code, just as you would in a normal Node.js or web project.

### 13.2 End-to-End (E2E) Testing with Playwright or Spectron

E2E testing automates user interactions with your final, packaged application.

  * **Playwright**: A modern E2E testing framework from Microsoft. It has experimental support for Electron and is the recommended future-proof choice.
  * **Spectron**: The classic choice, but it's now in maintenance mode and doesn't support recent Electron versions well.

**Playwright Example Snippet:**

```typescript
import { test, expect, _electron as electron } from '@playwright/test';

test('App launches and displays version', async () => {
  const electronApp = await electron.launch({ args: ['dist/main.js'] });
  const window = await electronApp.firstWindow();
  
  await window.waitForSelector('#app-version');
  const versionText = await window.locator('#app-version').innerText();
  
  expect(versionText).not.toBe('Loading...');
  
  await electronApp.close();
});
```

-----

## 14\. Common Pitfalls and How to Avoid Them

### 14.1 Blocking the Main Process

The main process coordinates your entire app. If you block it with a long-running synchronous task (like `fs.readFileSync`), the entire application will freeze.

  * **Solution**: Always use asynchronous APIs (`fs.promises.readFile`) or offload work to worker threads.

### 14.2 Ignoring Security Warnings

Electron will print warnings to the console if you use insecure configurations. **Do not ignore these warnings.** They are there to protect you and your users.

### 14.3 Misunderstanding the Process Model

Trying to access `document` in the main process or `app` in the renderer process is a common beginner mistake. Remember the roles: main is for backend/Node, renderer is for UI/DOM. Use IPC to communicate between them.

### 14.4 Large Application Size

An empty Electron app can be 40-50MB. Be mindful of your dependencies.

  * **Solution**: Use tools like `webpack-bundle-analyzer` to see what's taking up space. Prune unused dependencies. Avoid bundling large assets directly; fetch them from a server if possible.

-----

## 15\. Recipes & Advanced Topics

### 15.1 Creating Custom Menus

```typescript
// src/main/main.ts
import { Menu } from 'electron';

const menuTemplate = [
  {
    label: 'File',
    submenu: [
      { role: 'quit' }
    ]
  },
  // ... other menus
];

const menu = Menu.buildFromTemplate(menuTemplate);
Menu.setApplicationMenu(menu);
```

### 15.2 Working with Native Node.js Modules

If you need to use a native Node.js module (one with C++ bindings), you might need to rebuild it against Electron's version of Node.js. Tools like `electron-rebuild` can help with this.

### 15.3 Handling Application Updates

The `update-electron-app` module provides a simple way to add auto-updating capabilities. For more control, use `electron-updater`, which integrates well with Electron Builder and Electron Forge.

-----

## 16\. Cheatsheet

### 16.1 Key Electron Modules

  * `app`: Manages application lifecycle.
  * `BrowserWindow`: Creates and controls application windows.
  * `ipcMain`: Handles IPC messages from renderers.
  * `ipcRenderer`: Sends IPC messages from renderers/preload.
  * `Menu`: Creates native application menus.
  * `dialog`: Displays native dialogs for opening/saving files, alerts, etc.
  * `shell`: Manages files and URLs in their default applications.
  * `session`: Manages browser sessions, cookies, cache, etc.

### 16.2 Essential `BrowserWindow` Options

```typescript
new BrowserWindow({
  width: 1280,
  height: 720,
  show: false, // Don't show the window until it's ready
  frame: false, // For custom window frames
  titleBarStyle: 'hidden', // macOS custom frame
  webPreferences: {
    preload: 'path/to/preload.js',
    contextIsolation: true,
    sandbox: true,
  }
})
```

### 16.3 IPC Methods

| Direction          | Method (Sender)             | Method (Receiver)  | Use Case                                  |
| ------------------ | --------------------------- | ------------------ | ----------------------------------------- |
| **Renderer → Main** | `ipcRenderer.invoke(channel)` | `ipcMain.handle(channel)` | Asynchronous request-response (like a fetch) |
| **Renderer → Main** | `ipcRenderer.send(channel)`   | `ipcMain.on(channel)`     | Fire-and-forget message                 |
| **Main → Renderer** | `win.webContents.send(channel)` | `ipcRenderer.on(channel)` | Pushing events/data to the UI           |

-----

## 17. Further Resources

  * **Official Electron Documentation**: [https://www.electronjs.org/docs/latest/](https://www.electronjs.org/docs/latest/)
  * **Electron Security Best Practices**: [https://www.electronjs.org/docs/latest/tutorial/security](https://www.electronjs.org/docs/latest/tutorial/security)
  * **Electron Forge Documentation**: [https://www.electronforge.io/](https://www.electronforge.io/)
  * **Electron Builder Documentation**: [https://www.electron.build/](https://www.electron.build/)
  * **Awesome Electron GitHub Repo**: [https://github.com/sindresorhus/awesome-electron](https://github.com/sindresorhus/awesome-electron)

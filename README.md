<img src="https://user-images.githubusercontent.com/74038190/212284100-561aa473-3905-4a80-b561-0d28506553ee.gif" width="100%"/>

<div align="center">

# 🛡️✨ @thehackingguard/mahiru-baileys ✨🛡️
**Mahiru-Baileys is a custom-modified fork of Whiskeysockets Baileys, built for developers who want a fast, lightweight, and no-nonsense WhatsApp Web API. Optimized for stability, pairing-code auth, plugin-based bots, and real-time message handling — perfect for automation, bots, and hacking around WhatsApp with style.**

<img src="https://camo.githubusercontent.com/78c0b59f3a0d244ceb7aec4496ae0003edf1b7cb91bb3d2cdc8a8c66c2f4fb1c/68747470733a2f2f66696c65732e636174626f782e6d6f652f7536697632672e706e67" width="420" alt="Mahiru Baileys Banner" style="border-radius: 16px; border: 1px solid rgba(255,255,255,.25);" />
<br/><br/>

<a href="https://www.npmjs.com/package/@thehackingguard/mahiru-baileys">
  <img src="https://img.shields.io/npm/v/@thehackingguard/mahiru-baileys?label=version&logo=npm" alt="npm version"/>
</a>
<a href="https://www.npmjs.com/package/@thehackingguard/mahiru-baileys">
  <img src="https://img.shields.io/npm/dt/@thehackingguard/mahiru-baileys?label=downloads&logo=npm" alt="npm downloads"/>
</a>
<a href="https://www.npmjs.com/package/@thehackingguard/mahiru-baileys">
  <img src="https://img.shields.io/npm/l/@thehackingguard/mahiru-baileys?label=license" alt="license"/>
</a>
<a href="https://bundlephobia.com/package/@thehackingguard/mahiru-baileys">
  <img src="https://img.shields.io/bundlephobia/min/@thehackingguard/mahiru-baileys?label=minified%20size" alt="bundle size"/>
</a>
<a href="https://bundlephobia.com/package/@thehackingguard/mahiru-baileys">
  <img src="https://img.shields.io/bundlephobia/minzip/@thehackingguard/mahiru-baileys?label=gzip%20size" alt="gzip size"/>
</a>
<img src="https://img.shields.io/node/v/@thehackingguard/mahiru-baileys?label=node" alt="node engine"/>
<img src="https://img.shields.io/npm/types/@thehackingguard/mahiru-baileys?label=types" alt="types"/>

<br/>

<a href="https://whatsapp.com/channel/0029VbBmZQP2v1IpwJpat63P">
  <img src="https://img.shields.io/badge/Join-WhatsApp%20Channel-25D366?logo=whatsapp&logoColor=white" alt="WhatsApp Channel" />
</a>

</div>

---

## ✨ What is this?
`mahiru-baileys` is a fork-style distribution built on top of **Baileys** (WhiskeySockets), focused on:
- cleaner developer experience (DX)
- additional message features (album, buttons, polls, etc.)
- newsletter/channel helpers
- pairing-code helpers
- small quality-of-life improvements

> Built on top of WhiskeySockets/Baileys. Core protocol logic credits go to the original Baileys maintainers.

---

## 📦 Install

### Normal install
```bash
npm i @thehackingguard/mahiru-baileys 
```

### Use as a drop-in replacement (alias)
Replace the Baileys dependency with this package:

**For `@whiskeysockets/baileys`:**
```json
{
  "dependencies": {
    "@whiskeysockets/baileys": "npm:@thehackingguard/mahiru-baileys"
  }
}
```

**Terminal alias install:**
```bash
npm i @whiskeysockets/baileys@npm:@thehackingguard/mahiru-baileys
```

---

## ⭐ Feature highlights
| Feature | Notes |
|---|---|
| 📣 Newsletter/Channel helpers | create/update/react & utilities |
| 🖼️ Album messages | send multiple media as an album |
| 🖱️ Interactive messages | buttons & quick replies |
| 👤 LID addressing | works with `@lid` flow |
| 🔐 Pairing code | custom alphanumeric pairing codes |
| 📷 HD profile pic upload | upload full-size pfp (no crop) |
| 🧰 DX tweaks | reduced log noise & cleaner output |

---

## 📬 3 ways to connect

<details>
<summary><strong>Pairing Code Simple</strong></summary>

```js
import pino from "pino"
import {
  makeWASocket,
  useMultiFileAuthState,
  fetchLatestBaileysVersion
} from "@thehackingguard/mahiru-baileys"

async function mahiruSocket() {
  const { state, saveCreds } = await useMultiFileAuthState("./session")
  const { version } = await fetchLatestBaileysVersion()

  const sock = makeWASocket({
    version,
    auth: state,
    logger: pino({ level: "silent" }),
    printQRInTerminal: false
  })

  sock.ev.on("creds.update", saveCreds)
  sock.ev.on("connection.update", ({ connection }) => connection === "open" && console.log("connected"))

  if (!state.creds?.registered) {
    if (sock.ws?.readyState !== 1) await new Promise(r => sock.ws.once("open", r))
    console.log("PAIRING CODE:", await sock.requestPairingCode("62xxxxxxxxx"))
  }

  return sock
}

mahiruSocket().catch(console.error)
```
</details>

<details>
<summary><strong>Pairing Code Costume</strong></summary>

```js
import pino from "pino"
import {
  makeWASocket,
  useMultiFileAuthState,
  fetchLatestBaileysVersion
} from "@thehackingguard/mahiru-baileys"

async function mahiruSocket() {
  const { state, saveCreds } = await useMultiFileAuthState("./session")
  const { version } = await fetchLatestBaileysVersion()

  const sock = makeWASocket({
    version,
    auth: state,
    logger: pino({ level: "silent" }),
    printQRInTerminal: false
  })

  sock.ev.on("creds.update", saveCreds)
  sock.ev.on("connection.update", ({ connection }) => connection === "open" && console.log("connected"))

  if (!state.creds?.registered) {
    if (sock.ws?.readyState !== 1) await new Promise(r => sock.ws.once("open", r))
    console.log("PAIRING CODE:", await sock.requestPairingCode("62xxxxxxxxx", "MAHIRUSYG"))
  }

  return sock
}

mahiruSocket().catch(console.error)
```
</details>

<details>
<summary><strong>QR Terminal</strong></summary>

```js
import pino from "pino"
import {
  makeWASocket,
  useMultiFileAuthState,
  fetchLatestBaileysVersion
} from "@thehackingguard/mahiru-baileys"

async function mahiruSocket() {
  const { state, saveCreds } = await useMultiFileAuthState("./session")
  const { version } = await fetchLatestBaileysVersion()

  const sock = makeWASocket({
    version,
    auth: state,
    logger: pino({ level: "silent" }),
    printQRInTerminal: true
  })

  sock.ev.on("creds.update", saveCreds)
  sock.ev.on("connection.update", ({ connection }) => connection === "open" && console.log("connected"))

  return sock
}

mahiruSocket().catch(console.error)
```
</details>

---

## 🧩 Notes & configuration
If you enable “auto-follow newsletter on connect” in your build, keep it **pointing to your own newsletter JID** and document it clearly for users.

---

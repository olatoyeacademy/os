# Home Assistant Setup — Olatoye Academy Reminders

This folder contains everything you need to get audio reminders broadcasting
to your Google Home speakers from the Olatoye Academy schedule.

---

## Step 1 — Flash Home Assistant onto your Raspberry Pi

1. Download **Raspberry Pi Imager** from https://www.raspberrypi.com/software/
2. Open Imager → Choose Device → Raspberry Pi 5
3. Choose OS → **Other specific purpose OS → Home assistants and home automation → Home Assistant**
4. Choose your SD card → click **Next** → **Write**
5. Insert SD card into Pi, plug in ethernet cable, plug in power
6. Wait ~5 minutes, then open **http://homeassistant.local:8123** in your browser
7. Follow the onboarding wizard — create your account, name your home

---

## Step 2 — Install the Google Assistant SDK integration

This allows Home Assistant to broadcast messages to your Google Home speakers.

1. In Home Assistant go to **Settings → Add-ons → Add-on Store**
2. Search for and install **Google Assistant SDK**
3. Go to **Settings → Devices & Services → Add Integration**
4. Search **Google Assistant SDK** and follow the OAuth login steps
5. Grant access to your Google account (the one linked to your Google Home)

---

## Step 3 — Add the Olatoye Academy automations

1. In Home Assistant go to **Settings → Add-ons → Add-on Store**
2. Install **File Editor** (so you can paste config files via the browser)
3. Copy the contents of `automations/reminders.yaml` into your
   Home Assistant `automations.yaml` file
4. Copy the contents of `config/configuration.yaml` additions into your
   Home Assistant `configuration.yaml`
5. Go to **Developer Tools → Check Configuration** to validate
6. Click **Restart** to apply

---

## Step 4 — Enable the webhook

The schedule page sends session reminders to Home Assistant via a webhook.

1. In Home Assistant go to **Settings → Automations**
2. Open **"Olatoye Academy — Incoming Reminder"**
3. Copy the **Webhook ID** shown (it will match what's in `config/configuration.yaml`)
4. In `schedule.html`, update the `HA_WEBHOOK_URL` variable at the top of the
   script section with your Home Assistant local IP, e.g.:
   `http://192.168.1.XXX:8123/api/webhook/olatoye-reminder`

---

## Step 5 — Enable remote access (optional but recommended)

So reminders work even when you're not on home WiFi:

1. Sign up for **Nabu Casa** at https://www.nabucasa.com — £5/month
2. In Home Assistant go to **Settings → Home Assistant Cloud** and log in
3. This gives you a secure remote URL like `https://xxxxx.ui.nabu.casa`
4. Replace the local IP webhook URL in `schedule.html` with this remote URL

---

## How it works once set up

When you tap **"Set Reminder"** on any session in the schedule:

1. The schedule page sends a webhook to Home Assistant with the session details
2. Home Assistant fires an automation
3. Google Assistant SDK broadcasts to all your Google Home speakers:
   *"Reminder for Elsie — Maths starts in 5 minutes"*

Scheduled automations also fire automatically at the start of each session
every week without needing to tap anything.

---

## Folder structure

```
home-assistant/
├── SETUP.md                  ← you are here
├── automations/
│   └── reminders.yaml        ← all reminder automations
├── scripts/
│   └── broadcast.yaml        ← reusable broadcast script
└── config/
    └── configuration.yaml    ← additions to HA config
```

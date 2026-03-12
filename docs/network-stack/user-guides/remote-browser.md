# NeXuS Remote Browser Setup
## Future-Proof Privacy Browsing

## Concept

```
Local Machine (Alpine)          Remote Server (Devuan VM/VPS)
┌─────────────────┐            ┌──────────────────────────┐
│ w3m (CLI)       │            │ Mullvad Browser          │
│ mpv             │            │    ↓                     │
│ Thin client     │◄───VNC─────┤ X11/Wayland              │
│                 │            │    ↓                     │
│ Fan: Quiet      │            │ SSH Tunnel               │
│ CPU: 10%        │            │    ↓                     │
└─────────────────┘            │ NeXuS Proxy (Tor+I2P)    │
                               └──────────────────────────┘
```

## Benefits

### Security
- ✅ JavaScript runs remotely (can't exploit local machine)
- ✅ Downloads isolated to remote server
- ✅ Browser exploits contained in VM/container
- ✅ Fingerprinting sees remote system, not yours

### Privacy
- ✅ Double-hop: Your IP → Server → Tor → Internet
- ✅ Browser state separate from daily machine
- ✅ Can destroy/rebuild remote browser anytime
- ✅ Access from multiple devices (phone, tablet, etc.)

### Performance
- ✅ Heavy sites render on server CPU/GPU
- ✅ Your laptop just displays results
- ✅ Screamin demon fan stays quiet!
- ✅ Can use powerful remote server

## Option 1: Devuan VM (Local)

**Run browser in your existing Devuan VM:**

```bash
# On Devuan VM:
sudo apt install mullvad-browser x11vnc matchbox-window-manager

# Start minimal X11
startx &

# Start VNC server
x11vnc -display :0 -forever -shared

# Launch Mullvad Browser
mullvad-browser &

# On Alpine host:
apk add tigervnc
vncviewer devuan-vm-ip:5900
```

**Pros:**
- ✅ Local (fast, no internet needed)
- ✅ Full control

**Cons:**
- ❌ Still runs on screamin demon (but lighter than webtop)
- ❌ Not accessible outside home network

## Option 2: Remote VPS (Cloud)

**Run on cheap VPS ($5/month):**

```bash
# On VPS (Debian/Ubuntu):
sudo apt install mullvad-browser x11vnc xvfb awesome

# Start virtual X display
Xvfb :99 -screen 0 1920x1080x24 &
export DISPLAY=:99

# Start window manager
awesome &

# Start VNC
x11vnc -display :99 -forever -shared -passwd nexus &

# SSH tunnel from Alpine to VPS
ssh -L 5900:localhost:5900 user@vps-ip

# Connect VNC through tunnel
vncviewer localhost:5900
```

**Pros:**
- ✅ Zero load on screamin demon
- ✅ Access from anywhere (phone, tablet, etc.)
- ✅ Powerful server hardware
- ✅ Can run 24/7

**Cons:**
- ❌ Costs $5/month
- ❌ Need internet connection
- ❌ VPS provider sees encrypted VNC traffic (use SSH tunnel)

## Option 3: Browserless/Playwright (Headless)

**Run headless browser, stream screenshots:**

```bash
# On server:
npm install -g browserless playwright

# Start browserless with NeXuS proxy
browserless --proxy-server=socks5://localhost:1080

# On Alpine:
# Access via web interface
firefox http://server-ip:3000
```

## Option 4: Guacamole (Web-based Remote Desktop)

**Apache Guacamole = Remote desktop in browser:**

```bash
# On server:
docker run -p 8080:8080 guacamole/guacamole

# Configure remote browser connection
# Access from Alpine:
w3m http://server-ip:8080
# Or links: links http://server-ip:8080
```

**Pros:**
- ✅ Access via w3m (stay in CLI!)
- ✅ No VNC client needed
- ✅ HTML5-based

## Recommended Setup

**For you (CLI-first + remote browser):**

### Daily (99%)
```bash
# Local CLI
w3m, mpv, newsboat
```

### Heavy browsing (1%)
```bash
# Connect to remote browser
ssh -L 5900:localhost:5900 vps
vncviewer localhost:5900

# Or even better:
ssh vps -t 'DISPLAY=:99 mullvad-browser'
# Forward X11 back to local minimal X server
```

## NeXuS Integration

**Remote browser → NeXuS routing:**

```bash
# Option A: NeXuS runs on VPS
# Remote browser connects to local NeXuS

# Option B: SSH tunnel NeXuS to VPS
ssh -R 1080:localhost:1080 vps
# Now VPS can connect to localhost:1080 → your NeXuS!

# Option C: VPN between laptop and VPS
# Remote browser sees NeXuS as local proxy
```

## Future Vision

**Your ultimate setup:**

```
Phone/Tablet/Laptop (anywhere)
    ↓ (SSH/VNC)
Remote Browser on VPS
    ↓ (SSH tunnel)
NeXuS on Home Server
    ↓ (Smart routing)
Tor + I2P + Yggdrasil + B.A.T.M.A.N.
    ↓
Internet (fully anonymous, multi-hop)
```

**Benefits:**
- Browse from phone → VPS → Home NeXuS → Tor/I2P
- Zero browser on local device (ultimate privacy)
- Heavy sites = VPS problem (not your device)
- Full NeXuS mesh network from anywhere

## Resources

- Apache Guacamole: https://guacamole.apache.org/
- Browserless: https://www.browserless.io/
- X11VNC: http://www.karlrunge.com/x11vnc/
- Mullvad Browser: https://mullvad.net/browser

🌀 **Together Everyone Achieves More - Remotely!**

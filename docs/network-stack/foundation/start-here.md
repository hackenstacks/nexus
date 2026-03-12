#  NeXuS - Start Here!

**For Grandma, Papa, and the kid down the street**

## What is This?

A privacy system that protects your internet browsing from snooping.

## How Do I Use It?

### Step 1: Start It
```bash
cd /home/user/git/medusa-proxy
chmod +x nexus-launch.sh
./nexus-launch.sh
```

### Step 2: Watch It (Optional)
```bash
./nexus-status.py --live
```
Press Ctrl+C to stop watching.

### Step 3: Use It
In your web browser settings, set:
```
Proxy: localhost
Port: 8118
```

That's it! Your browsing is now private.

## Something Broke?

Run the magic fix-it tool:
```bash
./nexus-doctor.sh
```

It will tell you what's wrong and fix it automatically.

## What Does It Do?

- 🔒 Makes your browsing anonymous (like Tor)
- 🚫 Blocks ads and trackers
- 🌐 Lets you visit .i2p sites
- 🛡️ Protects your privacy

## The Three Buttons

Think of these like TV remote buttons:

1. **Power Button** → `./nexus-launch.sh` (Turn it on)
2. **Info Button** → `./nexus-status.py --live` (See what's happening)  
3. **Fix Button** → `./nexus-doctor.sh` (Fix problems)

## Need Help?

Look at these files in order:
1. This file (you're reading it!)
2. `BEAST-SLAYER-GUIDE.md` (more details)
3. Run `./nexus-doctor.sh` (fixes most problems)

## Is It Working?

Visit: http://check.torproject.org

If it says "Congratulations", you're protected! 🎉

---

**Remember:** Sane • Simple • Secure
**Together Everyone Achieves More** 🌀

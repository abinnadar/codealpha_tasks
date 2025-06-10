# Suricata Installation and Custom Alert Setup

Suricata is an open-source network security monitoring tool that functions as a:

- Network Intrusion Detection System (NIDS)
- Network Intrusion Prevention System (NIPS)
- Network Security Monitoring tool

It is widely used to monitor and protect networks against malicious traffic and cyber threats.

---

## 📦 Step 1: Install Suricata

```bash
sudo apt install suricata
```

---

## 🧪 Step 2: Run Suricata

To explore options, run:

```bash
suricata
```

For this guide, we'll run Suricata in `AF_PACKET` mode on interface `eth0`:

```bash
sudo suricata -i eth0 --af-packet
```

> **AF_PACKET** allows capturing and analyzing packets directly from the NIC.

---

## 🛡 Step 3: Download Rulesets

By default, no rules are loaded. Use the built-in utility:

```bash
sudo suricata-update
```

Then re-run Suricata again:

```bash
sudo suricata -i eth0 --af-packet
```

---

## 🚨 Step 4: Test Alert Triggering

We will trigger a built-in rule with SID `2100498`. Use `curl` to trigger and `tail` to monitor:

```bash
curl http://testmyids.com
tail -f /var/log/suricata/fast.log
```

You should see an alert entry logged.

---

## ✏️ Step 5: Create a Custom Rule

1. Open the local rules file:

```bash
sudo nano /etc/suricata/rules/local.rules
```

2. Add your custom ICMP ping alert rule:

```suricata
alert icmp any any -> $HOME_NET any (msg: "You're being pinged mate!"; sid: 9000001; rev: 1;)
```

### Rule Explanation:

- **alert** – Action to take
- **icmp** – Protocol
- **any any** – Source IP and port
- **->** – Direction
- **$HOME_NET any** – Destination (home network)
- **msg** – Alert message
- **sid** – Unique rule ID (9000001 for custom rules)
- **rev** – Rule version

---

## ⚙️ Step 6: Configure Suricata to Use Custom Rules

1. Open config file:

```bash
sudo nano /etc/suricata/suricata.yaml
```

2. Navigate to the `rule-files:` section (bottom of the file) and add:

```yaml
  - local.rules
```

Ensure the full path is correct if required:

```yaml
  - /etc/suricata/rules/local.rules
```

---

## 🧪 Step 7: Test Your Custom Rule

1. Start Suricata:

```bash
sudo suricata -i eth0 --af-packet
```

2. From another machine, ping your system.

3. Check alerts log:

```bash
tail -f /var/log/suricata/fast.log
```

You should now see alerts like:

```
You're being pinged mate!
```

---

## ✅ Summary

You have now:

- Installed Suricata
- Updated its rules
- Triggered a built-in alert
- Created and tested your own custom rule


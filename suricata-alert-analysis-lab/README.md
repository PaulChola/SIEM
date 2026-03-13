# Suricata Alert Analysis Lab

A lightweight lab workspace for exploring **Suricata IDS/IPS** alerts, custom rule development, and alert analysis.

This repository contains:

- `rules/` – Custom Suricata rules (e.g., `custom.rules`).
- `pcaps/` – Packet capture files used to generate alerts.
- `logs/` – Alert/log output from Suricata runs (e.g., `eve.json`).
- `docs/` – Notes and rule breakdowns (currently placeholders).
- `screenshots/` – Example screenshots of the lab environment and analysis steps.

---

## 🔧 Getting Started

### Prerequisites

- Ubuntu / Debian Linux (tested)
- Suricata installed (e.g., `sudo apt install suricata`)
- Root or sudo privileges to run Suricata in packet-capture mode

### Running Suricata on a PCAP

1. Update Suricata config as needed (e.g., `suricata.yaml`).
2. Ensure custom rules are available in `rules/custom.rules`.
3. Run Suricata against a PCAP (example):

```sh
sudo suricata -c /etc/suricata/suricata.yaml -S rules/custom.rules -r pcaps/<your-file>.pcap --init-errors-fatal
```

4. Review alerts/output under the default log directory (typically `/var/log/suricata`):

```sh
ls -l /var/log/suricata
cat /var/log/suricata/eve.json | jq -C '. | select(.event_type=="alert") | {alert:.alert.signature, src:.src_ip, dst:.dest_ip}' | less -R
```

> ✅ Tip: `eve.json` is the most common output format for analysis and can be piped to `jq` for filtering.

---

## 🧩 Custom Rule Example

The repository includes a sample custom rule in `rules/custom.rules`:

```snort
alert http $HOME_NET any -> $EXTERNAL_NET any (msg:"GET on wire"; flow:established,to_server; content:"GET"; http_method; sid:12345; rev:3;)
```

You can add additional rules into `rules/custom.rules` and rerun Suricata with `-S rules/custom.rules`.

---

## 🧠 Notes / Investigation

This lab is intended for:

- Examining Suricata alerts generated from packet captures.
- Practicing rule creation and tuning (false positives/positives).
- Learning how Suricata formats its output (EVE JSON, fast.log, etc.).

Feel free to add notes to `investigation-notes.md` and expand `docs/suricata-rule-breakdown.md` as you explore.

---

## 📸 Screenshots

The `screenshots/` directory contains example captures of the analysis process. Use them as visual references while working through the lab.

### Included screenshots

- **after suricata is run**
  ![after suricata is run](screenshots/after%20suricata%20is%20run.png)
- **cat custom rules**
  ![cat custom rules](screenshots/cat%20custom%20rules.png)
- **cat eve json**
  ![cat eve json](screenshots/cat%20eve%20json.png)
- **cat fast**
  ![cat fast](screenshots/cat%20fast.png)
- **jq  . eve with | less**
  ![jq  . eve with | less](screenshots/jq%20.%20eve%20with%20%7C%20less.png)
- **jq flow id eve json 225338295235828**
  ![jq flow id eve json 225338295235828](screenshots/jq%20flow%20id%20eve%20json%20225338295235828.png)
- **ls -var-log-suricata**
  ![ls -var-log-suricata](screenshots/ls%20-var-log-suricata.png)
- **severity 3**
  ![severity 3](screenshots/severity%203.png)
- **sudo suricata -r sample -S**
  ![sudo suricata -r sample -S](screenshots/sudo%20suricata%20-r%20sample%20-S.png)


---

## 🧰 Useful Commands

```sh
# Validate Suricata config
sudo suricata -T -c /etc/suricata/suricata.yaml

# Run Suricata with a specific rules file
sudo suricata -c /etc/suricata/suricata.yaml -S rules/custom.rules -r pcaps/example.pcap

# Tail the EVE JSON log
sudo tail -F /var/log/suricata/eve.json | jq -C '.'
```

---

## Next Steps

1. Add or tweak rules in `rules/custom.rules`.
2. Run Suricata against `pcaps/` files.
3. Analyze alerts in `logs/eve.json` and document findings in `investigation-notes.md`.

---

> 📌 Note: If you want to inspect the raw packet traffic interactively, open the `.pcap` files in Wireshark.

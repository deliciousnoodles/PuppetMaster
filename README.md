
╔═══════════════════════════════════════════════════════════════════════════════╗
║                                                                               ║
║   ██████╗ ██╗   ██╗██████╗ ██████╗ ███████╗████████╗                          ║
║   ██╔══██╗██║   ██║██╔══██╗██╔══██╗██╔════╝╚══██╔══╝                          ║
║   ██████╔╝██║   ██║██████╔╝██████╔╝█████╗     ██║                             ║
║   ██╔═══╝ ██║   ██║██╔═══╝ ██╔═══╝ ██╔══╝     ██║                             ║
║   ██║     ╚██████╔╝██║     ██║     ███████╗   ██║                             ║
║   ╚═╝      ╚═════╝ ╚═╝     ╚═╝     ╚══════╝   ╚═╝                             ║
║                                                                               ║
║   ███╗   ███╗ █████╗ ███████╗████████╗███████╗██████╗                         ║
║   ████╗ ████║██╔══██╗██╔════╝╚══██╔══╝██╔════╝██╔══██╗                        ║
║   ██╔████╔██║███████║███████╗   ██║   █████╗  ██████╔╝                        ║
║   ██║╚██╔╝██║██╔══██║╚════██║   ██║   ██╔══╝  ██╔══██╗                        ║
║   ██║ ╚═╝ ██║██║  ██║███████║   ██║   ███████╗██║  ██║                        ║
║   ╚═╝     ╚═╝╚═╝  ╚═╝╚══════╝   ╚═╝   ╚══════╝╚═╝  ╚═╝                        ║
║                                                                               ║
║                                                                               ║
╚═══════════════════════════════════════════════════════════════════════════════╝

## SpiderFoot Sock Puppet Detector
*"if it looks like a sock, it might be a sock"*

PUPPETMASTER is an end-to-end pipeline for discovering if a list of web domains are a "Sock Puppet" network. 
It combines domain discovery, [SpiderFoot](https://github.com/smicallef/spiderfoot) OSINT scanning, and graph-based network analysis.  

---

## What Does This Tool Do?

### The Complete Pipeline

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  DISCOVER   │ ──▶ │    SCAN     │ ──▶ │   ANALYZE   │
│  Domains    │     │  SpiderFoot │     │   Network   │
└─────────────┘     └─────────────┘     └─────────────┘
```

1. **Discover** -  Scrape Google/DuckDuckGo for competitor or suspicious domains using keywords
2. **Scan**     -  Run SpiderFoot OSINT scans in batch mode or interactive Web GUI
3. **Analyze**  -  Detect shared infrastructure that proves common ownership

### What We Find

| Signal Type | Example | Meaning |
|-------------|---------|---------|
| Same Google Analytics ID | UA-12345678-1      | **Strongest proof** of same operator |
| Same AdSense Publisher ID | pub-1234567890    | **Strongest proof** of same operator |
| Same Google Site Verification | Token string  | **Strongest proof** of same operator |
| Same SSL certificate | SHA256 fingerprint     | **Strongest proof** of same operator |
| Same WHOIS registrant | John Doe, 123 Main St | Strong evidence |
| Same nameservers | ns1.customdns.com          | Strong evidence |


**One shared unique identifier = same operator.**
<<<NOTE:  there can be false positives, follow on analysis required>>

### Use Cases

- Detecting fake review networks
- Identifying coordinated disinformation campaigns
- Competitor intelligence
- Fraud investigation
- Any situation where you suspect multiple websites are secretly controlled by one entity

---

## Quick Start

### Prerequisites

- Python 3.8 or higher
- SpiderFoot (auto-installed if not present)

### Installation

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/puppetmaster.git
cd puppetmaster

# Run PUPPETMASTER (auto-installs dependencies)
python3 puppetmaster.py
```

That's it! The interactive menu guides you through everything.

### Platform-Specific Notes

**Linux (Ubuntu/Debian/Kali):**
```bash
# If needed, install Python and pip:
sudo apt update && sudo apt install python3 python3-pip python3-venv
```

**macOS:**
```bash
# Via Homebrew:
brew install python3
```

**Windows:**
- Download Python from [python.org](https://python.org)
- Check "Add Python to PATH" during installation
- Run from Command Prompt or PowerShell

**AWS EC2 / Cloud Servers:**
- Works great on t3.micro or larger
- Use Option 9 (tmux) to keep scans running after SSH disconnect
- Use Option 11 (Web GUI) with SSH tunnel for interactive access

---

## The Main Menu

```
╔═══════════════════════════════════════════════════════════╗
║   PUPPETMASTER - SpiderFoot Sock Puppet Detector          ║
╚═══════════════════════════════════════════════════════════╝

  DISCOVERY & SCANNING
  [1]  Scrape domains via keywords        - Google/DuckDuckGo search
  [2]  Load domains from file             - Import domain list
  [3]  Run SpiderFoot scans (CLI)         - Batch mode scanning
  [4]  Check scan queue status            - Monitor progress
  [11] SpiderFoot Web GUI                 - Interactive browser mode

  ANALYSIS
  [5]  Run Puppet Analysis                - Detect sock puppet networks
  [6]  View previous results              - Browse past analyses

  SETTINGS
  [7]  Configuration                      - Paths and settings
  [8]  Help & Documentation               - Full guide
  [9]  Launch in tmux                     - For long scans
  [10] System monitor                     - Resource usage
  [q]  Quit
```

---

## Step-by-Step Guide

### Option A: Full Pipeline (Discovery → Scan → Analyze)

1. **Start PUPPETMASTER:** `python3 puppetmaster.py`
2. **[1] Scrape domains** - Enter keywords like "competitor keyword site" to find domains
3. **[3] Run SpiderFoot** - Scans all discovered domains (takes hours for many domains)
4. **[5] Run Analysis** - Detects connections and generates reports

### Option B: Analyze Existing SpiderFoot Data

If you already have SpiderFoot CSV exports:

1. **Start PUPPETMASTER:** `python3 puppetmaster.py`
2. **[5] Run Puppet Analysis** - Point to your folder of CSV exports
3. **Review reports** in the output directory

### Option C: Interactive GUI Mode

For exploring SpiderFoot's full capabilities:

1. **[11] SpiderFoot Web GUI** - Launches SpiderFoot's web interface
2. **Open browser** to http://localhost:5001 (or use SSH tunnel for remote)
3. **Create scans manually**, export results when done
4. **[5] Run Analysis** on exported CSVs

---

## Remote Access (SSH Tunnels)
Running on a remote server (EC2, VPS)?
### For SpiderFoot Web GUI:

```bash
# On your local machine, create SSH tunnel:
ssh -L <<path to .pem/ssh keys>> 5001:localhost:5001 user@your-server

# Then open in your local browser:
http://localhost:5001
```

### For Long-Running Scans:

Use **Option 9** to launch in tmux. Your scan keeps running even if SSH disconnects:

```bash
# Detach from session:
Ctrl+b, then d

# Later, reattach:
tmux attach -t puppetmaster
```

---

## Understanding the Results

### Output Files

| File | Description |
|------|-------------|
| `executive_summary.md` | **Start here!** Human-readable overview |
| `smoking_guns.csv`     | All definitive connections with evidence |
| `clusters.csv`         | Domain cluster assignments |
| `hub_analysis.csv`     | Potential controller/C2 domains |
| `all_connections.csv`  | Complete connection list |
| `signals.csv`          | All extracted signals |
| `network.graphml`      | Graph file for visualization tools |

### Signal Classification

| Tier | Meaning | Examples |
|------|---------|----------|
| SMOKING_GUN    | One match = almost definitive connection | Same Google Analytics ID, Same email, Same SSL cert |
| STRONG         | 2+ matches = likely connected            | Same WHOIS registrant, Same phone number |
| NOISE          | Ignored (too common)                     | Same CDN, Same cloud hosting provider |

### Confidence Levels

- **CONFIRMED** - At least one smoking gun signal   !! false positives can happen  !!
- **LIKELY** - Multiple strong signals
- **POSSIBLE** - Some strong signals
- **WEAK** - Only weak/supporting signals

---

## Dependencies

Core dependencies are auto-installed on first run:

```
pandas          # Data manipulation
networkx        # Graph analysis
tqdm            # Progress bars
tldextract      # Domain parsing
matplotlib        # Visualization
python-louvain      # Community detection (REQUIRED for accurate clustering)
googlesearch-python  # Google search
ddgs                  # DuckDuckGo search
```

---

## FAQ

### How accurate is this?

**Smoking guns are extremely reliable.** 
__HOWEVER False posivites can occur.  TODO__

Strong signals are probabilistic. Two matching signals = high confidence.

### What if I get no results?
Could mean:
1. The domains aren't actually connected
2. The operators use different tracking/analytics per domain
3. SpiderFoot didn't capture the relevant data
4. Privacy protections hide connecting signals (WHOIS privacy, etc.)

### How many domains can I analyze?
Tested with 100+ domains / 13 million SpiderFoot rows. Larger datasets work but take longer.

### Can I use this without SpiderFoot?
Currently no. PUPPETMASTER is designed for SpiderFoot data. The tool can auto-install SpiderFoot for you.
Technically if your data is structred in spider foots 3 row / 7 row format then this pipeline can injest that
You can also just modify the expected data structure 

---

---

## License
MIT License - Use freely, attribution appreciated.

---

## Credits
- Vibe coded with         [Claude](https://claude.ai) 
- Powered by              [SpiderFoot](https://github.com/smicallef/spiderfoot)
- Network analysis via    [NetworkX](https://networkx.org/)
- Community detection via [python-louvain](https://github.com/taynaud/python-louvain)

---

## Issues & Contributing
Found a bug? Have an idea? Open an issue on GitHub.
Remember, this is mostly AI generated code :) 

---

thank you!

# AutoRecon Suite: enum_subs & recon 🚀

A powerful, automated Bash script suite designed for Bug Bounty Hunters to streamline the reconnaissance and enumeration process. This suite consists of two main scripts: `enum_subs` for deep subdomain enumeration, and `recon` for crawling, fuzzing, and vulnerability scanning.

## 🛠️ Tools Included

### 1. `enum_subs`
This script performs a comprehensive subdomain enumeration using a mix of passive and active techniques.
* **Passive Enumeration:** Gathers subdomains using `subfinder` and `assetfinder`.
* **Active Brute-Forcing:** Resolves and brute-forces subdomains using `puredns`.
* **Permutations:** Generates subdomain alterations using `dnsgen` and resolves them.
* **Output:** Consolidates all unique, live subdomains into a single file (`all_subs.txt`).

### 2. `recon`
This script takes a single subdomain or a list of subdomains and automates the web reconnaissance process.
* **Subdomain Validation:** Checks if the subdomain is alive using `httpx-toolkit` and tests for sub-domain takeovers using `subzy`.
* **Link Gathering:** Crawls and fetches URLs using `katana`, `ffuf`, and `waymore`.
* **Categorization:** Sorts the discovered links by status codes (2xx, 3xx, 4xx, 5xx) and isolates `.js` files.
* **Automated Scanning:** Runs `nuclei` with specific tags based on the status code (e.g., bypass for 40x, exposure for 50x).
* **Advanced Checks:** Uses `nomore403` for 40x bypass attempts and `linkfinder` to extract endpoints from JavaScript files.
* **Queue Mode:** Supports reading from a file, processing subdomains one by one with a 60-second delay between them.

---

## ⚙️ Prerequisites & Dependencies

Before running these scripts, ensure you have the following tools installed and accessible in your system's `$PATH`:

**For `enum_subs`:**
* [subfinder](https://github.com/projectdiscovery/subfinder)
* [assetfinder](https://github.com/tomnomnom/assetfinder)
* [puredns](https://github.com/d3mondev/puredns)
* [dnsgen](https://github.com/ProjectAnte/dnsgen)

**For `recon`:**
* [httpx-toolkit](https://github.com/projectdiscovery/httpx)
* [subzy](https://github.com/LukaSikic/subzy)
* [katana](https://github.com/projectdiscovery/katana)
* [ffuf](https://github.com/ffuf/ffuf)
* [waymore](https://github.com/xnl-h4ck3r/waymore)
* [nuclei](https://github.com/projectdiscovery/nuclei)
* [nomore403](https://github.com/devanshbatham/nomore403)
* [linkfinder](https://github.com/GerbenJavado/LinkFinder)

**⚠️ Important Note on Wordlists:**
You MUST update the paths of the wordlists and resolvers inside the scripts to match your local environment before running them:
* In `enum_subs`: Update `WORDLIST` and `RESOLVERS`.
* In `recon`: Update `WORDLIST`.

---

## 🎯 My Personal Hunting Methodology

I built these tools to fit my specific Bug Bounty workflow. Here is exactly how I use them:

1. **Subdomain Enumeration:** I start by running `enum_subs` on the main target (e.g., `enum_subs target.com`) to gather as many valid subdomains as possible.
2. **Automated Recon:** I feed the resulting `all_subs.txt` file directly into the `recon` script (e.g., `recon all_subs.txt`) to crawl links and run initial automated scans.
3. **Manual Dorking:** While the `recon` script is running in the background, I perform Google Dorking using `site:target.com`. I manually open interesting links one by one to test application logic and specific functions.
4. **Reviewing Results:** Once the `recon` script finishes, I manually go through the categorized output files (e.g., `2xx_links.txt`, `nuclei_js_results.txt`) one by one to analyze the findings and escalate potential vulnerabilities.

---

## 💻 Usage

Make sure both scripts are executable:

chmod +x enum_subs recon


**Run Subdomain Enumeration:**

./enum_subs target.com


**Run Recon on a Single Subdomain:**

./recon sub.target.com


**Run Recon in Bulk Mode (File):**

./recon all_subs.txt

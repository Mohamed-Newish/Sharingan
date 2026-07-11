# Sharingan

![Bash](https://img.shields.io/badge/Bash-script-green) ![Python](https://img.shields.io/badge/Python-3.x-blue) ![Type](https://img.shields.io/badge/type-recon-red)

An automated **reconnaissance pipeline** for bug-bounty and web pentest engagements. Feed it a scope file and Sharingan chains together subdomain enumeration, live-host probing, URL harvesting, wordlist generation, port scanning, screenshotting, and vulnerability triage — turning hours of manual recon into a single command.

## What it does

`sharingan.py` reads your scope file and runs the `recon.sh` pipeline against every in-scope target. For each one it produces an organised output folder under `~/targets/<target>/`:

| Stage | Tooling | Output |
| ----- | ------- | ------ |
| Subdomain enumeration | [SubEnum](https://github.com/bing0o/SubEnum) (amass, assetfinder, subfinder, findomain, …) | `domains` |
| Live-host probing | httprobe | `hosts` |
| URL harvesting | waybackurls, gau | `urls` |
| Custom wordlists | unfurl (paths + params) | `wordlist/` |
| Content discovery | fff, ffuf | `fff_results/` |
| Port scanning | naabu | `ports` |
| URL bucketing by type | grep | `wayback_links_exts/` (js/php/aspx/jsp) |
| Vuln-pattern triage | gf (xss, lfi, rce, redirect, idor, sqli, ssrf) | `urls_perhaps_vuln/` |
| Screenshots | EyeWitness | screenshots |
| Secrets / JS analysis | SecretFinder | JS findings |
| S3 buckets | slurp | bucket findings |
| Template scanning | nuclei | `scanner_nuclei/` |
| SQLi / blind XSS | dsss, kxss, dalfox | `dsss_res`, `xss_res` |

It also flags interesting URLs (password-reset, tokens, keys) for manual review.

## Scope file

Create a text file with one target per line. Wildcards are supported (`*.example.com` is stripped to `example.com`):

```
*.example.com
api.example.com
```

## Usage

```bash
python3 sharingan.py inscope.txt
```

## Install

```bash
git clone https://github.com/Mohamed-Newish/Sharingan.git
cd Sharingan
chmod +x recon.sh
sudo mv recon.sh /usr/bin/sharingan
pip3 install -r requirements.txt
```

## Required tools

Sharingan orchestrates the following; install the ones you want to use:

[SubEnum](https://github.com/bing0o/SubEnum) ·
[httprobe](https://github.com/tomnomnom/httprobe) ·
[waybackurls](https://github.com/tomnomnom/waybackurls) ·
[unfurl](https://github.com/tomnomnom/unfurl) ·
[anew](https://github.com/tomnomnom/anew) ·
[fff](https://github.com/tomnomnom/fff) ·
[gf](https://github.com/tomnomnom/gf) ·
[naabu](https://github.com/projectdiscovery/naabu) ·
[nuclei](https://github.com/projectdiscovery/nuclei) ·
[EyeWitness](https://github.com/FortyNorthSecurity/EyeWitness) ·
[gau](https://github.com/lc/gau) ·
[uro](https://github.com/s0md3v/uro) ·
[dalfox](https://github.com/hahwul/dalfox) ·
ffuf, hakrawler, secretfinder, slurp, dsss, kxss, qsreplace

## Configuration

A few values in `recon.sh` are environment-specific — adjust them before running:

- **nuclei templates path** — update the `~/.local/nuclei-templates/...` paths to your own.
- **Blind XSS collector** — set the `BLIND` URL in the `bxss()` function to your own callback host.
- **ffuf wordlist** — `raft-large-files-lowercase.txt` must be on your path.

## Disclaimer

For **authorized security testing and educational use only**. Run it exclusively against assets you own or are explicitly permitted (in-scope) to test.

## License

Mohamed Newish — [@kanike99](https://twitter.com/kanike99)

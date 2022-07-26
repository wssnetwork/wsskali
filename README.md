# Wsskali
This repository consist of Dockerfile to build Docker image for kali linux with open source tools that commonly use in bug bounty activity
## List of tools
* https://github.com/m3n0sd0n4ld/GooFuzz
* https://github.com/projectdiscovery/subfinder
* https://github.com/projectdiscovery/httpx
* https://github.com/projectdiscovery/nuclei
* https://github.com/projectdiscovery/notify
* https://github.com/tomnomnom/anew
* https://github.com/tomnomnom/waybackurls
* https://github.com/tomnomnom/meg
* https://github.com/tomnomnom/fff
* https://github.com/ffuf/ffuf
* https://github.com/hakluke/hakrawler
## List of wordlists
* http://ffuf.me/wordlist/common.txt
* http://ffuf.me/wordlist/parameters.txt
* http://ffuf.me/wordlist/subdomains.txt
# First setup for fresh server
1. Spin up new Linux server - prefer Debian
2. Update, upgrade & install git
   ```
   $ apt update && apt upgrade
   $ apt install git
   ```
3. Clone the repository & cd to wsskali directory
   ```
   $ git clone https://github.com/wssnetwork/wsskali
   $ cd wsskali
   ```
4. Run first-setup shell script
   ```
   $ bash first-setup
   ```
# How to setup and access container
1. Run cd to wsskali directory
2. If run `first-setup` shell script, it should auto build the docker image. run check as below:
   ```
   $ docker image ls
   ```
   If image already created, proceed to step no. 6. If no image created, follow step no. 3 to 5 as below.
3. To build the docker image run command as `$ docker build . -t <image-name>:<image-tag>`
   ```
   $ docker build . -t wsskali:1.0
   $ docker image ls
   ```
4. Better to create volume first `$ docker volume create <volume-name>` (easy to manage data in the future)
   ```
   $ docker volume create wsskali
   $ docker volume ls
   ```
5. Once image created, run with `$ docker run -v <vol-name-in-host>:<pwd-in-container> --name <prefered-container-name> -it <image-name>:<image-tag> /bin/bash`
   ```
   $ docker run -v wsskali:/home --name wss_kali
   ```
6. If already in running state, access container with cmd `$ docker exec -it <container-running> /bin/bash`
   ```
   $ docker exec -it wss_kali /bin/bash
   ```
   If access from Visual Studio Code, need to install `Extension: Remote - Containers` by Microsoft. Once installed and container is running, access container via:
   ```
   Ctrl+Shift+P > Select Remote-Containers...
   ```
# Once container up and running
If require to have full capabilities as kali linux, fresh install the `first-kali-setup` with run command (require some input during setup):
```
$ first-kali-setup
```
This script will run `kali-linux-headless` as define in https://www.kali.org/tools/kali-meta/#kali-linux-headless. Please choose other packages if require.
# Methodology
1. Get url, domain, subdomain
   1. Scan with `subfinder`
   2. Cat from `subfinder`, push to `waybackurls` - to gather info from archive
   3. Cat from `subfinder`, push to `hakrawler` - to crawl data
2. Scan either it alive
   1. Probe with `httpx`
   2. `To-Do:` how to use `meg`
3. Identify good spot - what technology behind it
   1. Analyze output from `httpx`
   2. Support analyze with `goofuzz` - gather info from search engine
   3. Use `fff` to gather header and response from target
   4. Use `ffuf` to fuzzing directory, subdomain, files, etc.
4. Scan for vulnerability
   1. Scan with `nuclei`
5. Found any low, medium, high, critical - use `notify` push to slack, telegram, etc.
6. `anew` useful to append output into existing files

# ~/just/another/bounty/flow/1.2.3
-  notes / reminders  

#

## subdomains
```
~/go/bin/assetfinder target.com -subs-only | tee -a assetfinder-target.com.log

echo "target.com"| ~/go/bin/haktrails subdomains | tee -a haktrails-sub-target.com.log

subfinder -d target.com -recursive -silent -o subfinder-target.com

~/go/bin/github-subdomains -d target.com -o gitsub-target.com -t GITHUB_TOKEN

~/go/bin/chaos --key CHAOS_TOKEN -d target.com -o chaos-target.com

findomain -t target.com -u findomain-target.com.log
```
#### cloudrecon  > https://github.com/g0ldencybersec/CloudRecon

##### scrape iprange  
    ~/go/bin/CloudRecon  scrape  -i 10.0.0.1/24
##### store result in local db
    # creates a db if no db exists or if not specified
    ~/go/bin/CloudRecon  store -i 192.168.1.1/24
##### read local db
    ~/go/bin/CloudRecon retr -all 
##### parse out domains from local db (tweaking may be needed)
    ~/go/bin/CloudRecon retr -all | gron | grep -iho "\".*\.target\.com" | cut -f2 -d'"'| tr "," "\\n" | sort -u | tee -a cloudrecon-target.com

#### sublert - alerts via slack hook if new subdomain get registerd @ crt.sh. Runs on vps.
- https://medium.com/@yassineaboukir/automated-monitoring-of-subdomains-for-fun-and-profit-release-of-sublert-634cfc5d7708
- https://github.com/yassineaboukir/sublert

#### dorkhunter
```
python3 ~/tools/dorkhunter/dorks_hunter.py -d target.com -o dorkhunter-target.com.log
```
#### way|/more|/backurls
```
~/go/bin/waybackurls target.com * | tee -a waybackurls-target.com
python3 ~/tools/waymore/waymore.py -i target.com -mode U --output-urls waymore-target.com.log
```
##### uncover
> https://github.com/projectdiscovery/uncover
```
echo 'target.com' | ~/tools/uncover/uncover -e censys -v -f ip,port,host 2>&1| tee -a uncover-target.com.log
```

##### check if a domain is alive
```
cat subdomains.log | httpx -o alive.log
```
## crawl domains

- gospider
    ```
    gospider -S alive.log -o gospider-alive.log
    ```
- katana
    ```
    katana -u domains -js-crawl -d 3  -H "X-Forwarded-host: 127.0.0.1" -H 'User-agent: null' -sc *target.com -output katana katana-target.com.log
    ```
- [crawley](https://github.com/s0rg/crawley) 
    ```
    crawley  -skip-ssl -silent  -js -brute  -depth -1 -all 'https://webtransfer.post.ch/' | grep -viE .".gif|.jpg|.gif|.css|.png|.jpeg"| grep -i '^http'
    ```
#### extract urls from /path 
```
egrep -rhaio "(http|https)://[a-zA-Z0-9./?=_-]*" * | grep target.com
```

#### extract urls from browser history
```
sqlite3 ~/.BurpSuite/pre-wired-browser/Default/History "select url,title from urls" | grep rosenberger.com | tee -a rosenberger.com-browser-history.log
sqlite3 ~/.config/google-chrome/Default/History "select url,title from urls" | grep rosenberger.com
```

#### replace domain/string
```
cat burp-requests.log | sed 's/someburp\.colob\.com/new\interact\.sh/m'
```
## enum parameters 
##### x8
```
# GET
~/tools/x8/target/release/x8 -u 'https://target.com/'  -w ~/.cache/kiterunner/wordlists/httparchive_parameters_top_1m_2022_12_28.txt

# POST
~/tools/x8/target/release./x8 -X POST  -u "https://target.com/" -w ~/.cache/kiterunner/wordlists/ht`parchive_parameters_top_1m_2022_12_28.txt -c 7
```
##### arjun
```
arjun -u https://target.com/ | tee -a arjun-target.log
```

##### xnLinkFinder
```
python3 ~/tools/xnLinkFinder/xnLinkFinder.py -i endpoints-target.com -sp https://target.com/ -sf target.com -d 3 -o linkf-target.com.log -op linkf-param-target.com.log -vv
```

#### bruteforce files/dirs
```
feroxbuster -k -A --smart -u https://target/ -w ~/tools/SecLists/Discovery/Web-Content/raft-large-directories.txt -o ferox-target.log

dirb http://target.com | tee -a dirb-target.com

echo "https://target.com" | ~/tools/feroxbuster -w ~/tools/SecLists/Discovery/Web-Content/dirsearch.txt --smart --silent -A -k --stdin -o feroxbuster-target.com --filter-status 403,401

~/tools/kiterunner/kr scan https://target.com/ -A php-221228 -o kr.target.com.log
```

##### uncover
    echo 'target.com' | ~/tools/uncover/uncover -e censys -v -f ip,port,host 2>&1| tee -a uncover-target.com.log
    > enginees options: censys, netlas, criminalip, fofa

###### robtex ip info 
curl https://freeapi.robtex.com/ipquery/8.8.8.8|gron

## vulnerbility scans

###### skipfish
```
skipfish -o skip-target.com http://target.com
```

###### nuclei
- https://github.com/projectdiscovery/nuclei

##### xsstrike 
```
 while read t;do echo -e "running $t\n";python3 ~/tools/XSStrike/xsstrike.py -u $t --crawl | tee -a xsstrike-target.com;done<endpoints-target.com
```

##### postMessage

- postMessage-tracker
- https://github.com/fransr/postMessage-tracker

## framework
#### [reconwtf](https://github.com/six2dez/reconftw)

##### tokens in use: securitytrails, fofa, gitlab, github, securitytrails, shodan, binaryedge, chaos, WHOISXML


## Online tools / resources
- httpbin.org 
    > eg genrate b64 payload 
    ```
    echo '<iframe src="http://10.10.0.1/server-status"></iframe>' | base64 | awk '{print "http://httpbin.org/base64/"$1}'
    
    ## output
    http://httpbin.org/base64/PGlmcmFtZSBzcmM9Imh0dHA6Ly8xMC4xMC4wLjEvc2VydmVyLXN0YXR1cyI+PC9pZnJhbWU+Cg==
    
    ## check 
    curl 'http://httpbin.org/base64/PGlmcmFtZSBzcmM9Imh0dHA6Ly8xMC4xMC4wLjEvc2VydmVyLXN0YXR1cyI+PC9pZnJhbWU+Cg=='
    ```

- uncoder.io

## reminders
#### golang install syntax ->go install repo@latest 
```go install github.com/hacktheworld/hacktheworld@latest ```

#### kiterunner / assetnotes -> save wordlist 
```
>> kr wordlist list
>> kr wordlist save 2m-subdomains
<< file saved name=2m-subdomains path=/home/nojjan/.cache/kiterunner/wordlists/2msubdomains.txt
```
## search engines 
- crt.sh 
- ivre.rocks 
- vulners.com 
- pulsedive.com 
- binaryedge.io 
- socradar.io 
- fullhunt.io 
- publicwwww.com 
- urlscan.io 
- searchcode.com 
- app.netlas.io 
- intelx.io 
- leakix.net 
- censys.io 
- hunter.io 
- fofa.info 
- zoomeye.org 
- grep.app 
- app.binaryedge 
- onyphe.io



## PDFs

- http://www.cgisecurity.com/lib/HTTP-Request-Smuggling.pdf
- http://www.ussrback.com/docs/papers/IDS/whiskerids.html
- https://media.defcon.org/DEF%20CON%2024/DEF%20CON%2024%20presentations/DEFCON-24-Regilero-Hiding-Wookiees-In-Http.pdf
- https://securityvulns.ru/advisories/content.asp
- https://dl.packetstormsecurity.net/papers/general/whitepaper_httpresponse.pdf
- https://cdivilly.wordpress.com/2011/04/22/java-servlets-uri-parameters/
- https://msdn.microsoft.com/en-us/library/system.text.encodinginfo.getencoding.aspx


### Acknowledgements
 - Bug Hunters are Awesome People! 
 
 
### License

[![CC0](https://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0)

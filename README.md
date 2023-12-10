# bountyflow
just another bug bounty 1 - 2 - 3
> Notes from @zionskompis ~ tools ~ syntax ~ examples ~ reminders ~ payloads
##### my paths -> tools / bins
```
~/tools/
~/go/bin/
```
#### tokens used in tool x
> gitlab
> github 
> securitytrails
> shodan 
> binaryedge
> chaos


#### search engines 
> crt.sh | ivre.rocks | vulners.com | pulsedive.com | binaryedge.io | socradar.io| fullhunt.io | publicwwww.com | urlscan.io | searchcode.com | app.netlas.io | intelx.io | leakix.net | censys.io | hunter.io | fofa.info | zoomeye.org | grep.app | app.binaryedge | onyphe.io



#### golang install syntax ->go install repo@latest 
```go install github.com/hacktheworld/hacktheworld@latest ```

#### kiterunner / assetnotes -> save wordlist 
```
>> kr wordlist list
>> kr wordlist save 2m-subdomains
<< file saved name=2m-subdomains path=/home/nojjan/.cache/kiterunner/wordlists/2m`subdomains.txt
```


#### enum subdomains
```
~/go/bin/assetfinder target.com -subs-only | tee -a assetfinder-target.com.log

echo "target.com"| ~/go/bin/haktrails subdomains | tee -a haktrails-sub-target.com.log

subfinder -d target.com -recursive -silent -o subfinder-target.com

~/go/bin/github-subdomains -d target.com -o gitsub-target.com -t GITHUB_TOKEN

~/go/bin/chaos --key CHAOS_TOKEN -d target.com -o chaos-target.com

```

###### dorkhunter
```
python3 ~/tools/dorkhunter/dorks_hunter.py -d target.com -o dorkhunter-target.com.log
```
##### way|/more|/backurls
```
~/go/bin/waybackurls target.com * | tee -a waybackurls-target.com
python3 ~/tools/waymore/waymore.py -i target.com -mode U --output-urls waymore-target.com.log
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
### enum parameters 
##### x8
```
~/tools/x8/target/release./x8 -X POST  -u "https://target.com/" -w ~/.cache/kiterunner/wordlists/ht`parchive_parameters_top_1m_2022_12_28.txt -c 7
```
##### arjun
```
arjun -u https://target.com/ | tee -a arjun-target.log
```

### bruteforce files/dirs
```
feroxbuster -k -A --smart -u https://target/ -w ~/tools/SecLists/Discovery/Web-Content/raft-large-directories.txt -o ferox-target.log

dirb http://target.com | tee -a dirb-target.com

echo "https://target.com" | ~/tools/feroxbuster -w ~/tools/SecLists/Discovery/Web-Content/dirsearch.txt --smart --silent -A -k --stdin -o feroxbuster-target.com --filter-status 403,401

~/tools/kiterunner/kr scan https://target.com/ -A php-221228 -o kr.target.com.log
```

### vulnerbility scans



### Online tools / resources
- httpbin.org 
    > eg genrate b64 payload 
    ```
    echo '<iframe src="http://10.10.0.1/server-status"></iframe>' | base64 | awk '{print "http://httpbin.org/base64/"$1}'
    
    ## output
    http://httpbin.org/base64/PGlmcmFtZSBzcmM9Imh0dHA6Ly8xMC4xMC4wLjEvc2VydmVyLXN0YXR1cyI+PC9pZnJhbWU+Cg==
    
    ## check 
    curl 'http://httpbin.org/base64/PGlmcmFtZSBzcmM9Imh0dHA6Ly8xMC4xMC4wLjEvc2VydmVyLXN0YXR1cyI+PC9pZnJhbWU+Cg=='
    ```


#### PDFs

- http://www.cgisecurity.com/lib/HTTP-Request-Smuggling.pdf
- http://www.ussrback.com/docs/papers/IDS/whiskerids.html
- https://media.defcon.org/DEF%20CON%2024/DEF%20CON%2024%20presentations/DEFCON-24-Regilero-Hiding-Wookiees-In-Http.pdf
- https://securityvulns.ru/advisories/content.asp
- https://dl.packetstormsecurity.net/papers/general/whitepaper_httpresponse.pdf
- https://cdivilly.wordpress.com/2011/04/22/java-servlets-uri-parameters/
- https://msdn.microsoft.com/en-us/library/system.text.encodinginfo.getencoding.aspx


## Acknowledgements
 - Bug Hunters are Awesome People! 

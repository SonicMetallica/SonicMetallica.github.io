TheHarvester is a great tool to dam harvest everything from a url.
You also can use shodan to get alot information about a website/company.


usage: theHarvester [-h] -d DOMAIN [-l LIMIT] [-S START] [-g] [-p] [-s] [-v]
                    [-e DNS_SERVER] [-t DNS_TLD] [-n] [-c] [-f FILENAME]
                    [-b SOURCE]

theHarvester is used to gather open source intelligence (OSINT) on a company
or domain.

optional arguments:
  -h, --help            show this help message and exit
  -d DOMAIN, --domain DOMAIN
                        company name or domain to search
  -l LIMIT, --limit LIMIT
                        limit the number of search results, default=500
  -S START, --start START
                        start with result number X, default=0
  -g, --google-dork     use Google Dorks for Google search
  -p, --port-scan       scan the detected hosts and check for Takeovers
                        (21,22,80,443,8080)
  -s, --shodan          use Shodan to query discovered hosts
  -v, --virtual-host    verify host name via DNS resolution and search for
                        virtual hosts
  -e DNS_SERVER, --dns-server DNS_SERVER
                        DNS server to use for lookup
  -t DNS_TLD, --dns-tld DNS_TLD
                        perform a DNS TLD expansion discovery, default False
  -n, --dns-lookup      enable DNS server lookup, default False
  -c, --dns-brute       perform a DNS brute force on the domain
  -f FILENAME, --filename FILENAME
                        save the results to an HTML and/or XML file
  -b SOURCE, --source SOURCE
                        baidu, bing, bingapi, certspotter, crtsh, dnsdumpster,
                        dogpile, duckduckgo, github-code, google, hunter,
                        intelx, linkedin, linkedin_links, netcraft, otx,
                        securityTrails, spyse(disabled for now), threatcrowd,
                        trello, twitter, vhost, virustotal, yahoo, all

# Shodan

–help

$ shodan –help
Usage: shodan [OPTIONS] COMMAND [ARGS]…

Options:
  -h, –help  Show this message and exit.

Commands:
alert       Manage the network alerts for your account.
convert     Convert the given input data file into a different format.
count       Returns the number of results for a search.
data        Bulk data access to Shodan.
domain      View all available information for a domain.
download    Download search results and save them in a compressed JSON file.
honeyscore  Check whether the IP is a honeypot or not.
host        View all available information for an IP address.
info        Shows general information about your account.
init        Initialize the Shodan command-line.
myip        Print your external IP address.
org         Manage your organization’s access to Shodan.
parse       Extract information out of compressed JSON files.
radar       Real-Time Map of some results as Shodan finds them.
scan        Scan an IP/ netblock using Shodan.
search      Search the Shodan database.
stats       Provide summary information about a search query.
stream      Stream data in real-time.
version     Print version of this tool.

info

If you have setup your API token, you can check the number of credits you have left:

$ shodan info 
Query credits available: 100 
Scan credits available: 100

Query credits are used to search Shodan and scan credits are used to scan IPs.

A search request consumes 1 query credit and scanning 1 IP consumes 1 scan credit.
version

When writing this article I was using shdoan 1.21.2:

$ shodan version 
1.21.2

count

Returns the number of results for a search query.

$ shodan count openssh 
23128 
$ shodan count openssh 7 
219

download

Search Shodan and download the results into a file where each line is a JSON banner.

By default it will only download 1,000 results, if you want to download more look at the –limit flag.

The download command lets you save the results and process them afterwards using the parse command.

So if you often search for the same queries it will help you save credits.

The export credits are used to download data from the website at the rate of: 1 export credit lets you download up to 10,000 results. They are single-use which means that once you use them they don’t automatically renew at the start of the month.

But if you don’t have export credits, you can use 1 query credit to save 100 results.

$ shodan download -h 
Usage: shodan download [OPTIONS] <filename> <search query> 

Download search results and save them in a compressed JSON file. 

Options: 
--limit INTEGER The number of results you want to download. -1 to download 
all the data possible. 
--skip INTEGER The number of results to skip when starting the download. 
-h, --help Show this message and exit.

For example here I will download 1000 results of the query openssh:

$ shodan download openssh-data openssh 
Search query: openssh 
Total number of results: 23128 
Query credits left: 100 
Output file: openssh-data.json.gz 
[###################################-] 99% 00:00:00 
Saved 1000 results into file openssh-data.json.gz

After the download you can check how many credits you have left:

$ shodan info 
Query credits available: 95 
Scan credits available: 100

host

See information about the host such as where it’s located, what ports are open and which organization owns the IP.

$ shodan host 1.1.1.1 
1.1.1.1 
Hostnames: one.one.one.one 
Country: Australia 
Organization: Mountain View Communications 
Updated: 2020-01-21T22:26:00.168041 
Number of open ports: 3 

Ports: 
53/udp 
80/tcp 
443/tcp 
|-- SSL Versions: -SSLv2, -SSLv3, TLSv1, TLSv1.1, TLSv1.2, TLSv1.3 

$ shodan host 138.201.81.199 
138.201.81.199 
Hostnames: apollo.archlinux.org 
Country: Germany 
Organization: Hetzner Online GmbH 
Updated: 2020-01-21T03:02:11.476262 
Number of open ports: 4 

Ports: 
22/tcp OpenSSH (8.1) 
25/tcp Postfix smtpd 
80/tcp nginx (1.16.1) 
443/tcp nginx (1.16.1) 
|-- SSL Versions: -SSLv2, -SSLv3, -TLSv1, -TLSv1.1, TLSv1.2, TLSv1.3

myip

Returns your Internet-facing IP address.

$ shodan myip 
199.30.49.210

parse

Use parse to analyze a file that was generated using the download command.

It lets you filter out the fields that you’re interested in, convert the JSON to a CSV and is friendly for pipe-ing to other scripts.

$ shodan parse -h 
Usage: shodan parse [OPTIONS] <filenames> 

Extract information out of compressed JSON files. 

Options: 
--color / --no-color 
--fields TEXT List of properties to output. 
-f, --filters TEXT Filter the results for specific values using key:value 
pairs. 
-O, --filename TEXT Save the filtered results in the given file (append if 
file exists). 
--separator TEXT The separator between the properties of the search 
results. 
-h, --help Show this message and exit.

The following command outputs filtered data for the previously downloaded openssh data:

$ shodan parse --fields location.country_code3,ip_str,hostnames -f port:2222 openssh-data.json.gz 
ITA 89.107.109.247 
HUN 193.6.173.187 
FRA 77.87.111.110 pro-sip1.srv.proceau.net 
USA 50.210.94.33 
USA 35.130.36.118 035-130-036-118.biz.spectrum.com 
AUT 80.120.19.180 
JPN 124.155.95.212 v095212.ppp.asahi-net.or.jp 
POL 83.144.70.114 83-144-70-114.static.chello.pl 
BGR 84.238.200.8 
AUT 80.120.19.168 
USA 162.211.126.140 
CAN 76.10.173.222 mail.nanoman.ca 
USA 24.172.82.71 rrcs-24-172-82-71.midsouth.biz.rr.com 
AUT 80.120.19.182 
ITA 188.14.96.151 host151-96-static.14-188-b.business.telecomitalia.it 
USA 216.67.111.198 216-67-111-198.static.acsalaska.net 
USA 73.179.238.221 c-73-179-238-221.hsd1.fl.comcast.net 
HKG 113.28.18.59 113-28-18-59.static.imsbiz.com 

$ shodan parse --fields port,ip_str,location.city,location.postal_code -f location.country_code:FR --separator , openssh-data.json.gz 
22,188.92.65.5,Hésingue,68220 
2222,77.87.111.110,, 
22,51.89.105.163,, 
22,5.135.218.249,, 
22,93.177.70.142,, 
2222,81.250.129.207,Paris,75116 
22,51.255.85.97,, 
22,193.52.218.40,Aix-en-provence,13090 
22,51.77.112.86,, 
22,149.202.19.41,, 
22,5.39.117.104,, 
22,195.154.53.223,Beaumont,95260 
22,37.71.132.198,, 
22,178.33.71.35,, 
22,212.83.188.179,Jouy-le-moutier,95280 
2222,195.200.166.216,Berre-l'etang,13130 
22,82.251.157.165,Paris,75004

search

This command lets you search Shodan and view the results in a terminal-friendly way.

By default it will display the IP, port, hostnames and data. You can use the –fields parameter to print whichever banner fields you’re interested in.

A simple query won’t consume any credits but if you use a search filter or request page 2 and beyond, credits will be consumed.

$ shodan search -h 
Usage: shodan search [OPTIONS] <search query> 

Search the Shodan database 

Options: 
--color / --no-color 
--fields TEXT List of properties to show in the search results. 
--limit INTEGER The number of search results that should be returned. 
Maximum: 1000 
--separator TEXT The separator between the properties of the search 
results. 
-h, --help Show this message and exit.

Example of query that won’t cost credits:

$ shodan search --fields ip_str,port,os smb 
156.226.167.81 445 Windows Server 2008 R2 Datacenter 7601 Service Pack 1 
156.243.104.194 445 Windows Server 2008 R2 Enterprise 7601 Service Pack 1 
91.230.243.89 445 Windows 10 Pro 16299 
85.3.170.18 445 Windows 6.1 
213.238.170.132 445 Windows Server 2012 R2 Standard 9600 
154.208.176.81 445 Windows Server 2008 R2 Enterprise 7601 Service Pack 1 
103.235.171.78 445 Windows Server 2016 Datacenter 14393 
102.130.40.85 445 Windows Server 2016 Standard 14393 
50.3.151.113 445 Windows Server 2012 R2 Standard 9600 
220.241.112.233 445 Windows Server 2019 Standard 17763 
100.27.15.229 445 WWindows Server 2012 R2 Standard 9600 
212.71.136.11 445 Unix 
156.255.174.225 445 Windows Server 2008 R2 Datacenter 7601 Service Pack 1 
156.232.162.239 445 WWindows Server 2008 R2 Enterprise 7601 Service Pack 1 
186.210.102.132 445 Unix 
154.94.153.34 445 Windows Server 2012 R2 Datacenter 9600 
213.130.28.31 445 Windows 6.1

Example of query that will cost 1 credit (because using a filter):

$ shodan search --fields ip_str,port,org,info product:mongodb 
165.22.3.203 27017 Digital Ocean 
213.159.208.76 27017 JSC The First 
209.6.48.11 27017 RCN 
23.239.0.110 27017 Linode 
52.220.230.134 27017 Amazon.com 
47.91.139.188 27017 Alibaba 
159.203.169.196 27017 Digital Ocean 
49.233.135.180 27017 Tencent cloud computing 
122.228.113.75 27017 WENZHOU, ZHEJIANG Province, P.R.China. 
106.14.42.66 27017 Hangzhou Alibaba Advertising Co.,Ltd. 
59.108.91.3 27017 Beijing Founder Broadband Network Technology Co.,L 
115.29.176.18 27017 Hangzhou Alibaba Advertising Co.,Ltd. 
148.251.46.75 27017 Hetzner Online GmbH 
3.121.222.150 27017 Amazon.com 
47.75.211.162 27017 Alibaba 
200.219.217.122 27017 Equinix Brazil

scan

Scan an IP/ netblock using Shodan.

$ shodan scan -h 
Usage: shodan scan [OPTIONS] COMMAND [ARGS]... 

Scan an IP/ netblock using Shodan. 

Options: 
-h, --help Show this message and exit. 

Commands: 
internet Scan the Internet for a specific port and protocol using the... 
list Show recently launched scans 
protocols List the protocols that you can scan with using Shodan. 
status Check the status of an on-demand scan. 
submit Scan an IP/ netblock using Shodan.

Launching a scan will cost credits:

1 scan credit lets you scan 1 IP

By default a scan result will be displayed to stdout but you can save it to a file to be able to parse it later.

$ shodan scan submit --filename 104.27.154.244_scan.json.gz 104.27.154.244

If the host has already been scanned in the last 24 hours, you won’t be able to scan it again without an Enterprise grade plan.

$ shodan scan submit --filename 104.27.154.244_scan.json.gz 104.27.154.244 

Starting Shodan scan at 2020-01-22 23:46 - 100 scan credits left 
No open ports found or the host has been recently crawled and cant get scanned again so soon.

You are also able to see the scans you previously launched with their ID and status:

$ shodan scan list 
# 2 Scans Total - Showing 10 most recent scans: 
# Scan ID Status Size Timestamp 
zmWj3RNgiPbiQjx9 PROCESSING 1 2020-01-22T22:49:39.037000 
8J9yu7jqTQO7AIiP PROCESSING 1 2020-01-22T22:46:34.790000

To save your scan results you are not forced to use –filename. You can simply launch a scan without saving it, and download the results later thanks to the scan ID:

$ shodan download --limit -1 scan-results.json.gz scan:zmWj3RNgiPbiQjx9

As scan are done asynchronously, you can check the status of a scan at any moment.

$ shodan scan status zmWj3RNgiPbiQjx9 
DONE

To see the scan ID when launching a scan you can use the verbose mode:

$ shodan scan submit --verbose 13.226.145.4 

Starting Shodan scan at 2020-01-23 00:00 - 97 scan credits left 
# Scan ID: 3z6Cqf1CCyVLtc6P
# Scan status: DONE

Customers with an Enterprise Data License will be allowed to request a scan of the entire Internet by simply specifying the port and protocol/module.

$ shodan scan internet 8080 wemo-http

Available protocols and modules can be listed with shodan scan protocols.
stats

Provide summary information about a search query

$ shodan stats -h 
Usage: shodan stats [OPTIONS] <search query> 

Provide summary information about a search query 

Options: 
--limit INTEGER The number of results to return. 
--facets TEXT List of facets to get statistics for. 
-O, --filename TEXT Save the results in a CSV file of the provided name. 
-h, --help Show this message and exit.

It seems that by default you will get only top 10 and not for all facets:

$ shodan stats nginx 
Top 10 Results for Facet: country 
US 13,598,596 
CN 6,013,993 
ZA 3,067,296 
DE 1,560,114 
HK 1,065,990 
RU 869,931 
FR 859,715 
GB 555,946 
NL 550,591 
JP 526,386 

Top 10 Results for Facet: org 
Amazon.com 1,897,943 
CloudInnovation infrastructure 1,288,280 
Leaseweb USA 1,200,146 
EGIHosting 1,131,973 
DXTL Tseung Kwan O Service 1,052,688 
Hangzhou Alibaba Advertising Co.,Ltd. 770,553 
Digital Ocean 749,221 
Asline Limited 680,364 
Power Line Datacenter 678,264 
Quantil Networks 585,935

But we can customize this behavior:

$ shodan stats --facets domain,port,asn --limit 5 nginx 
Top 5 Results for Facet: domain 
amazonaws.com 2,208,958 
scalabledns.com 435,980 
googleusercontent.com 308,114 
t-ipconnect.de 225,276 
your-server.de 180,711 

Top 5 Results for Facet: port 
80 10,019,366 
443 5,300,058 
5000 588,809 
5001 563,208 
8080 453,604 

Top 5 Results for Facet: asn 
as37353 2,447,679 
as35916 1,878,181 
as15003 1,508,786 
as16509 1,236,249 
as18779 1,132,180

Source: [https://community.turgensec.com/shodan-pentesting-guide](https://community.turgensec.com/shodan-pentesting-guide)
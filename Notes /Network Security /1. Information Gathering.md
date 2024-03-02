# 1. ✨Information Gathering
While there are many steps associated with an engagement, none are more important than the act of information gathering or footprinting a designated target.

> The Information gathering phase is focused on two essential aspects of all targets: Business and Infrastructure.
> 

The Business side of information gathering deals with collecting information regarding the type of business, its stakeholders, assets, products, services, employees and generally non-technical information.
The organization will probably operate its business purpose through an **Infrastructure** such as networks, systems, domains, IP addresses and so on.
The second phase of the Information Gathering process will focus on uncovering this type of information.

- Must have info at the end of the info gathering phase:
    
    ![image](https://github.com/0xbazooka/eCPPT-/assets/99322823/8ced0466-2e2e-4cc5-bef4-82bd3b6958b6)

    

- Our Process:
    
    ![image](https://github.com/0xbazooka/eCPPT-/assets/99322823/7d4658a8-7f4f-4276-8e4c-dd6855afa4ee)

    

- information gathering techniques can be classified into two main disciplines: **Passive** and **Active**
- Tools such as Dradis, Faraday and Magitree can be very useful due to the fact that they are specifically designed to keep track of networks/vulnerability scans.
    
    As you will see, these tools can facilitate the sharing of gathered information with your colleagues and, in addition, allow you to import scans and reports created with tools like Burp Suite, Nessus, Nexpose, Nmap and so on.
    
    https://github.com/infobyte/faraday
---

## 1. Business
### Search Engines

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f0f89f55-e1c4-438d-99c1-1dffaefdd0cb/bb019ebc-d5b0-4f3d-b9d9-1e9c85991f6a/Untitled.png)

### Google Dorks

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f0f89f55-e1c4-438d-99c1-1dffaefdd0cb/bda4b868-8194-4db2-9408-c3aaab467728/Untitled.png)


## 2. Infrastructure

As the Scope of Engagement (SOE) for your penetration test, your customer can give you:

1. The name of the organization (full scope test)
2. IP addresses or net blocks to test

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f0f89f55-e1c4-438d-99c1-1dffaefdd0cb/1188d096-2e2f-4f44-83a1-fa6fd6fd5bc3/Untitled.png)

## Full Scope

## DNS

### WHOIS

- whois, port 43

While using WHOIS databases, be sure to try different searching techniques on your target.

For example, be sure to search for just the name of the target company with no domain, then continue on to other searches leveraging different variations of domain names (i.e. target, [target.com](http://target.com/), target net, etc.).

[https://who.is](https://who.is/)

DNS is a key aspect of Information Security as it binds a ostname to an IP address and many protocols such as SSL are as safe as the DNS protocol they bind to.

### DNS Records

### DNS Enumeration

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f0f89f55-e1c4-438d-99c1-1dffaefdd0cb/068043e7-b261-42c8-a03f-bda796c3ac40/Untitled.png)

- A DNS Lookup is the simplest query a DNS server can receive. It asks the DNS to resolve a given hostname to the corresponding IP. You can do so with nslookup:
    
    ```python
    nslookup <targetorganization.com>
    ```
    
    In order to obtain the IP addresses of an organization, an attacker will first try to determine the hostnames and then try to resolve them.
    

- With Reverse DNS lookup, we will receive the IP address associated to a given domain name. This process queries for DNS pointer records (PTR). For this task you can use nslookup:
    
    ```python
    nslookup -type=PTR <IPaddress>
    ```
    
    Only domains with a PTR record set will respond to the above reverse lookup.
    

- With the MX(Mail Exchange) lookup, we retrieve a list of servers responsible for delivering e-mails for that domain. Once again you can use nslookup or online tools:
    
    ```python
    nslookup -type=MX <domain>
    ```
    
    https://www.dnsqueries.com/en/
    
    [https://mxtoolbox.com](https://mxtoolbox.com/)
    
    ```python
    nslookup -type=ns <domain>
    ```
    
    ```python
    nslookup -type=any <domain>
    ```
    

### `dig`

The command's we have seen so far were issued on a Windows machine. The Linux nslookup version has some limitations, therefore we suggest a more powerful tool called `dig`.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f0f89f55-e1c4-438d-99c1-1dffaefdd0cb/1a149701-6c43-433f-b6ce-5a7406443ec0/Untitled.png)

```python
dig +nocmd <domain> axfr +noall +answer @<dnsserver>
```

### **`Fierce`**

1. requests DNS Servers of our target domain
2. uses these dns servers
3. attempts to retrieve the zone files from the dns servers

if 3. fails:

1. bruteforce attack to guess subdomains based on the commonly used ones.
- scanning ip space using : reverse lookup

```python
fierce -dns <domain> 
```

```python
fierce -dns <domain> -dnsserver <dns server>
```

### `dnsenum`

1. looks for A records, reverse lookups
2. ns servers
3. mx servers
4. attempts to perform a zone transfer attack by sending an ASFR record

```python
dnsenum <domain> 
dnsenum <domain> --dnsserver <dns server>
dnsenum <domain> -F <brute-force list>
```

- more details in Tools..

### `dnsmap` <domain>

- for brureforcing subdomains, it also returns their ips.

```python
dnsmap—bulk.sh domains.txt /tmp/results
```

For brute forcing a list of target domains in a bulk fashion use the bash script provided.

### `dnsrecon`

THE ALL IN ONE

- + BIND
- SRV
- SEC recs

60-80

## IP

### `fping`

`-h`: help

`-a` : send few icmp packets

`-A` : only view alive ones

`-g` : for a range of ips

`-e` : return time required to recieve response

`-q` : be quiet, only view alive

```python
fping -q -a -g <ip range> -r <num of retries> -e
```

### `hping3`

`-1` : send icmp packets to an ip

`-c` : num of packets

`--icmp-ts` :  time-stamp icmp packet

`-2` : send UDP packet

even if it responds with unavailable port, its’ live w da elmohem

`-S` : tcp packet with SIN flag set

`-F` : send FIN flag

`-U` : URGent, prioritize the incoming data 

…

## Narrowed Scope

### Live Hosts

- Determine hosts (IP) that are alive.
- Determine if they have an associated host name/domain.

gather additional information and apply the information
gathering techniques on both host names and domains
that we have already studied.

**Identifying Live Hosts** ?

```bash
fping -a -g 192.168.1.0/24
```

where `-a` shows systems that are alive and `-g` generate a target list from a supplied IP netmask or a starting and ending IP address.

**Host Discovery Scan** ?

```bash
nmap -sn 10.0.0.0/24
```

The `-sn` option, also known as ping scan/ping sweep, tells Nmap not to run a port scan on the remote hosts, but instead return only the hosts that respond to the probes sent.

Nowadays though, ICMP is often disabled on perimeter routers and firewalls, and even on latest Windows clients (via Windows Firewall).

ICMP scans are then no longer reliable in determining whether a host is alive or not.

---

### Further DNS

(define scope → enum dns)

Now that we know how to discover live hosts, let us investigate more and see how we can find further DNS within the target network. 

This step deals with using Nmap to enumerate all the DNS servers that exist in the remote network.

- DNS runs on port 53 tcp & udp
- We can increase our surface by using Nmap to scan the entire network and find hosts that have these ports open. To do this, we can use the following two commands:
    
    ```python
    nmap -ss -p53 [NETBLOCK]
    nmap -sU -p53 [NETBLOCK]
    ```
    
- Once we retrieve more DNS servers, we can perform a reverse lookup to find out if they are serving any particular domain.
- Moreover, we can try zone transfer techniques on them as well as any of the techniques studied before.

Maltego uses what it calls transformations to discover information about specific targets.

For instance, you can begin with a server address and enumerate various information regarding that server, and then build on that information until you have a full map of
the entity's entire internet presence.


## Tools

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f0f89f55-e1c4-438d-99c1-1dffaefdd0cb/ae672188-65fb-4eae-ba0b-5ab5052a8bd2/Untitled.png)

### [dnsdumpster](https://dnsdumpster.com)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/f0f89f55-e1c4-438d-99c1-1dffaefdd0cb/816121c6-ea23-4dfd-a60a-2c246f81a9a6/Untitled.png)

- returns to us: the hosting behind the target domain, the location of the servers, the DNS records (MX, A, etc.) and it also creates a map with all the information obtained.
- good to start our investigation, not intrusive

### [dnsenum](https://github.com/fwaeytens/dnsenum)

- its purpose is gathering as much info as possible about a domain.

```python
dnsenum.pl [options] <domain>
dnsenum <domain> #3amal kda practically
```

- `--private` Show and save private IPs at the end of the file domain_ips.txt.
- `--subfile <file>` Write all valid subdomains to this file.
- `--threads <value>` The number of threads that will perform different queries.
- `-p, --pages <value>` The number of Google search pages to process when scraping names, the default is 20 pages, the -s switch must be pecified.
- `-s, --scrap <value>` The maximum number of subdomains that will be scraped from Google.
- `-f --file` Read subdomains from this file to perform brute force.

- It also comes with a wordlist file containing the most common DNS and sub domain names. This will be useful in running brute force attacks: `/usr/share/dnsenum`

```python
dnsenum --subfile <file to write subs in> -v -f /usr/share/dnsenum/dns.txt -u a -r <domain>
```

- `-u`  update any existing file
- `-r`  recursive subdomain discovery

[dnsmap](https://github.com/makefu/dnsmap)

[FOCA](https://www.elevenpaths.com/labstools/foca/index.html)




Shodan

Again, it is very important to be very meticulous about saving information from the tools for use in the later phases. This will ensure a complete and thorough test.






---

# 2. ✨Scanning

  

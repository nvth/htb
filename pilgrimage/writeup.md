## Pilgrimage - Linux - Easy
### Reproduction
**Vulnerability Exploited**: Misconfiguration - Exploitation  

**System Vulnerabilities**:Kernel - Linux 5.10.0-23-amd64 - 10.10.11.219

**Vulnerabilities Explaination**: Misconfiguration lead to file read (LFI) lead to RCE/ACE  

**Previlege Escalation Vulnerabilities**: Binwalk - CVE-2022-4510  

**Vulnerabilities Fix**: Update lasted version binwalk, Re-configuration information leaked.  

**Severity**: Critical

**Details**  
***dirsearch***  
![dirsearch](https://github.com/nvth/htb/blob/master/pilgrimage/imgs/dirsearch.PNG?raw=true)

Firstly, we using `dirsearch` tool to scan this server, we got `.git` link by misconfiguration leaked.  
From information we gathered by `dirsearch`, use `git dumper` tool dump all file from `pilgrimage.htb/.git`,  
![dumping]()




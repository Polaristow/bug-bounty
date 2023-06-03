# knock信息收集工具使用
## 1.工具地址：
https://github.com/guelfoweb/knock
## 2.安装：
推荐方法1：
```
git clone https://github.com/guelfoweb/knock.git
cd knock
pip3 install -r requirements.txt

python3 knockpy.py <DOMAIN>
```
推荐方法2：
```
git clone https://github.com/guelfoweb/knock.git
cd knock
python3 setup.py install

knockpy <DOMAIN>
```
## 3.使用教程
最常用的使用方法
`knockpy <DOMAIN>`
例如 knockpy baidu.com
具体可以参考工具介绍
```
usage: knockpy [-h] [-v] [--no-local] [--no-remote] [--no-scan] [--no-http] 
               [--no-http-code CODE [CODE ...]] [--dns DNS] [-w WORDLIST] 
               [-o FOLDER] [-t SEC] [-th NUM] [--silent [{False,json,json-pretty,csv}]]
               domain

--------------------------------------------------------------------------------
* SCAN
full scan:    knockpy domain.com
quick scan:   knockpy domain.com --no-local
faster scan:  knockpy domain.com --no-local --no-http
ignore code:  knockpy domain.com --no-http-code 404 500 530
silent mode:  knockpy domain.com --silent

* SUBDOMAINS
show recon:   knockpy domain.com --no-local --no-scan

* REPORT
show report:  knockpy --report knockpy_report/domain.com_yyyy_mm_dd_hh_mm_ss.json
plot report:  knockpy --plot knockpy_report/domain.com_yyyy_mm_dd_hh_mm_ss.json
csv report:   knockpy --csv knockpy_report/domain.com_yyyy_mm_dd_hh_mm_ss.json
--------------------------------------------------------------------------------

positional arguments:
  domain                target to scan

optional arguments:
  -h, --help            show this help message and exit
  -v, --version         show program's version number and exit
  --no-local            local wordlist ignore
  --no-remote           remote wordlist ignore
  --no-scan             scanning ignore, show wordlist and exit
  --no-http             http requests ignore
                        
  --no-http-code CODE [CODE ...]
                        http code list to ignore

  --dns DNS             use custom DNS ex. 8.8.8.8                        

  -w WORDLIST           wordlist file to import
  -o FOLDER             report folder to store json results
  -t SEC                timeout in seconds
  -th NUM               threads num

  --silent [{False,json,json-pretty,csv}]
                        silent or quiet mode, default: False
                        
```

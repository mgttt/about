personal tech learn/pratise log since 2019

### 2020-11

### 2019-10 ~ 2020-10

* Quantum Computation
* Big Data / Database tools
* Customs Project Tech Support
* ffic for piping 
```
purpose:
    piping big data(10m+) between mysql w+ selected db.tbl.fields with speed of 500k-records/s;
    set nameOfPipe=test && bin/mysql.exe | ffic namepipe.c $nameOfPipe | bin/mysqlimport.exe \\.\pipe\$nameOfPipe

ref:
https://github.com/psmay/windows-named-pipe-utils/blob/master/createAndReadPipe-main.c
void main()
{
    HANDLE hNamedPipe = Create_Some_Named_Pipe();

    RedirectIO(stdout, hNamedPipe);
    printf("Hello World."); //This arrived at the listening named pipe.
}

void RedirectIO(FILE *hFrom, HANDLE hTo)
{
    int fd = _open_osfhandle((intptr_t)hTo, _O_WRONLY | _O_TEXT);
    _dup2(fd, _fileno(hFrom));
    setvbuf(hFrom, NULL, _IONBF, 0); //Disable buffering.
}
```

### 2019-11

* VS Code for ES
* [HubSpot Intergration and Developement](https://app.hubspot.com/academy/6493533/tracks/71/593/2968)

### 2019-10

* [WrapEval](https://github.com/wanjochan/wrapeval)
* [EvalCli](https://github.com/wanjochan/evalcli)
* delete this.constructor;this.constructor.constructor('return this')()
* (function(){return this})()
* (new Function('return this'))()

### 2019-06

* [distributed-id algorithm](https://github.com/wanjochan/misctools/blob/master/mysql/duid.md)


### 2019-05

* design and build tiny framework for lambda, and make it work with our cluster database deployment; [shared src](https://github.com/wanjochan/misctools/tree/master/lambda), it works for SCF @ QCloud too ;)
* Offline IoT [does-the-iot-really-need-the-internet](https://uk.farnell.com/does-the-iot-really-need-the-internet)

### 2019-04

* [Notes about Mysql Cluster Deployment](http://blog.sequoiadb.com/cn/Detail-id-54)
* [max sqlite performance](https://blog.csdn.net/chenguanzhou123/article/details/9376537)
* design distributed sql-db and kvdb
* cloudflare enterprise worker and plugin for socket reverse to access the db backend
* taste OKTA and SalesForce
* [mysql cluster setup scripts](https://github.com/wanjochan/misctools/tree/master/mysql) and [wiki](https://github.com/wanjochan/misctools/wiki/mysql-cluster)

### 2019-03

* setup new Trading Acount for HSI, new journey
* nwjs-builder-phoneix for packing uChat win version (v0.7.0)
* 285mb size win7 [vdi image](https://github.com/wanjochan/PartnerNET.Software/blob/master/win7_000_d.7z?raw=true) for automation dev;
* review VMWARE [ESXi](https://www.vmware.com/products/esxi-and-esx.html), for future own server cluster for BizTrade
* review [vnpy](https://github.com/vnpy/vnpy), get some ideas for the algo-trading
* update sptrader_nodejs to adopt json-3.5.0.hpp when NODE_MODULE_VERSION>57
* learn [how to transfer domain between cloudflare accounts](https://support.cloudflare.com/hc/en-us/articles/204615358-How-to-move-domains-between-Cloudflare-accounts)
* review [JS on cloudflare workers](https://cloudflareworkers.com/) for more ServerLess Deployment Options, more [examples](https://developers.cloudflare.com/workers/writing-workers/blog-posts/)
* review [PHP Zend VM](http://joshuais.me/php-zend-vm/)
* review [Ethereum VM codes](https://ethervm.io)
* review [Tachyon](https://github.com/Tachyon-Team/Tachyon.git) - about JS-vm-JS
* review [AST](https://en.wikipedia.org/wiki/Abstract_syntax_tree) a must-learn for new lang impl.
* review [ASM 

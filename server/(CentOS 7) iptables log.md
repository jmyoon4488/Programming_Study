#### 참고 링크

https://unix.stackexchange.com/questions/423778/log-iptables-events-on-centos-7  
http://flowvalue.tistory.com/2

## [rsyslog 수정 (root)]
    # vi /etc/rsyslog.conf
    
    // add rule
    ...
    #### RULES ####
    ...
    kern.* /var/log/iptables.log 

## [rsyslog 재시작 (root)]
    # service rsyslog restart  
    or  
    # systemctl restart rsyslog

## [iptables 수정 (root)]
    # vi /etc/sysconfig/iptables

    *filter
    ...
    *nat
    ...
    -A PREROUTING ~~~
    -A INPUT ~~~
    -A OUTPUT ~~~
    ...
    -A INPUT -m limit --limit 5/min -j LOG --log-prefix "iptables denied: " 
    COMMIT

## [iptables 재시작 (root)]
    # systemctl restart iptables

> * log level - warn
> 1. iptables -A INPUT -j LOG --log-prefix "BAD_INPUT: " --log-level 4  
> 1. iptables -A FORWARD -j LOG --log-prefix "BAD_FORWARD: " --log-level 4  
> 1. iptables -A OUTPUT -j LOG --log-prefix "BAD_OUTPUT: " --log-level 4  

> * log level - debug  
> 1. iptables -A INPUT -j LOG --log-prefix "BAD_INPUT: " --log-level 7  
> 1. iptables -A FORWARD -j LOG --log-prefix "BAD_FORWARD: " --log-level 7  
> 1. iptables -A OUTPUT -j LOG --log-prefix "BAD_OUTPUT: " --log-level 7  

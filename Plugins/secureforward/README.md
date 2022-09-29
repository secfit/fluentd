<div align="center">
  <h3 align="center">Fluentd Secure Forward plugin</h3>
</div>

Fluentd input/output plugin to forward fluentd messages over SSL with authentication<br>
(this project is part from  [tagomoris/fluent-plugin-secure-forward](https://github.com/tagomoris/fluent-plugin-secure-forward))
This is an example of exchanging data between 2 servers (master & slave)

## Installation (master)
  ```sh
 [root@master test]# /opt/td-agent/embedded/bin/fluent-gem install fluent-plugin-secure-forward   
   ```
## Generate certificate file and transfer to slave<br> (master)
during file generation use this shared_key `0658545a2cf5aa3c4b1e081eb77436fe`
  ```sh
 [root@master test]# mkdir /etc/td-agent/cert
 [root@master test]# /opt/td-agent/embedded/bin/secure-forward-ca-generate /etc/td-agent/cert "0658545a2cf5aa3c4b1e081eb77436fe"
 [root@master test]# mkdir /etc/td-agent/cert
   ```  
 it will generate file named `ca_cert.pem`

## transfer `ca_cert.pem` to slave server to /etc/td-agent/cert

## set permission (master & &slave)
  ```sh
 [root@master&slave test]# chown td-agent:td-agent -R /etc/td-agent/cert
 [root@master&slave test]# /chmod 700 /etc/td-agent/cert
 [root@master&slave test]# chmod 400 /etc/td-agent/cert/* 
   ```  
   
## Integrate into td-agent.conf
  ```sh
<store>
    @type secure_forward
    shared_key 0658545a2cf5aa3c4b1e081eb77436fe
    secure yes
    ca_cert_path /etc/td-agent/cert/ca_cert.pem
    enable_strict_verification yes
  </store>
   ```  

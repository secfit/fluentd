<div align="center">
  <h3 align="center">Fluentd Installation Steps</h3>
</div>

## Preparation
### Increase the Maximum Number of File Descriptors
* Increase Max of File Descriptors<br>
   /etc/security/limits.conf
    ```sh
		root soft nofile 65536
		root hard nofile 65536
		* soft nofile 65536
		* hard nofile 65536
     ```
      
 * Optimize Network Kernel Parameters
     /etc/sysctl.conf
      ```sh
		    net.core.somaxconn = 65535
		    net.core.netdev_max_backlog = 5000
		    net.core.rmem_max = 16777216
		    net.core.wmem_max = 16777216
		    net.ipv4.tcp_wmem = 4096 12582912 16777216
		    net.ipv4.tcp_rmem = 4096 12582912 16777216
		    net.ipv4.tcp_max_syn_backlog = 8096
		    net.ipv4.tcp_slow_start_after_idle = 0
		    net.ipv4.tcp_tw_reuse = 1
		    net.ipv4.ip_local_port_range = 10240 65535
      ```
  * import repository<br>
    Add the following to /etc/yum.repos.d/td.repo
      ```sh
          [treasuredata]
	      name=TreasureData
	      baseurl=http://packages.treasuredata.com/3/redhat/\$releasever/\$basearch
	      gpgcheck=1
	      gpgkey=https://packages.treasuredata.com/GPG-KEY-td-agent
      ```
  * Checking for update & installing td-agent using YUM command<br>
      ```sh
          yum check-update
	      yum install -y td-agent
	      chkconfig td-agent on
      ```
  * systemd running permission<br>
  change values on this file : /usr/lib/systemd/system/td-agent.service <br>
  change 
      ```sh
        change 
              User=td-agent
	          Group=td-agent
        by
              User=root
              Group=root
      ```
   
  * Reload systemd file
      ```sh
        systemctl daemon-reload
      ``` 
  * Set dir permission
      ```sh
        chown -R root:td-agent /var/log/td-agent/
        chmod -R 775 /var/log/td-agent/
        mkdir /opt/fluent/
      ``` 
   * Start service
      ```sh
        service td-agent start
      ``` 

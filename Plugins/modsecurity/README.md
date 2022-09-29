<div align="center">
  <h3 align="center">Fluentd ModSecurity plugin</h3>
</div>

Fluentd output (filter) plugin for parsing a ModSecurity audit log<br>
(this project is part from  [bitsofinfo/fluentd-modsecurity](https://github.com/bitsofinfo/fluentd-modsecurity)
## Preparation
#### define fluentd ruby plugins location
  ```sh
 [root@admin test]# ls -d /opt/td-agent/embedded/lib/ruby/gems/2.4.0/gems/fluentd-*/lib/fluent/plugin/
    /opt/td-agent/embedded/lib/ruby/gems/2.4.0/gems/fluentd-1.11.5/lib/fluent/plugin/
   ```
#### copy this file "out_modsecurity-audit-format.rb" to last fluentd-ruby-plugins-location
  ```sh
 [root@admin test]# cp out_modsecurity-audit-format.rb /opt/td-agent/embedded/lib/ruby/gems/2.4.0/gems/fluentd-1.11.5/lib/fluent/plugin/
   ```
#### set permission
  ```sh
 [root@admin test]# chmod 664 /opt/td-agent/embedded/lib/ruby/gems/2.4.0/gems/fluentd-1.11.5/lib/out_modsecurity-audit-format.rb
   ```
#### copy plugin file fluent-plugin-modsecurity.gemspec to /etc/td-agent/
  ```sh
 [root@admin test]# cp fluent-plugin-modsecurity.gemspec /etc/td-agent/
   ```
#### set s.files value in /etc/td-agent/fluent-plugin-modsecurity.gemspec
  ```sh
 [root@admin test]# sed -i "s;__sfile__;/opt/td-agent/embedded/lib/ruby/gems/2.4.0/gems/fluentd-1.11.5/lib/out_modsecurity-audit-format.rb;g" /etc/td-agent/fluent-plugin-modsecurity.gemspec
   ```
 ## build the gem
  ```sh
 [root@admin test]# /opt/td-agent/embedded/bin/gem build /etc/td-agent/fluent-plugin-modsecurity.gemspec
   ```
## Integrate into td-agent.conf
  ```sh
<source>
  @type tail
  tag raw-modsec
  path /var/log/httpd/modsec_audit.log
  pos_file /opt/fluent/mod_security.log.pos
  <parse>
    @type multiline
    format_firstline /^--[a-fA-F0-9]{8}-A--$/
        format1 /(?<message>.*)/
  </parse>
</source>
   ```



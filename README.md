Role Name
=========

An Ansible role that installs mailgraph on Ubuntu servers.

[Mailgraph] (https://mailgraph.schweikert.ch/) is a very simple mail statistics RRDtool frontend for Postfix and Sendmail that produces daily, weekly, monthly and yearly graphs of received/sent and bounced/rejected mail. Mailgraph was developed by [David Schweikert] (http://david.schweikert.ch/).

Requirements
------------

This role also installs rrdtool which is needed by Mailgraph to draw the graphs.

A web server (eg. Apache) will be required to view mailgraph which will run at the following address:

http://[hostname]/cgi-bin/mailgraph.cgi

Role Variables
--------------

# Postfix delivers emails to integrated content filters like amavisd which, after successful spam/virus scanning, deliver the email back to Postfix. Setting this variable to true will avoid Mailgraph counting your emails twice 
`mailgraph_ignore_localhost` [default: `true`]

# Start the mailgraph service on boot
`mailgraph_start_on_boot` [default: `true`]

# Mail log location for mailgraph to read
`mailgraph_mail_log` [default: `/var/log/mail.log`]

# The location that the mailgraph CGI will be copied to after being configured
mailgraph_cgi_location: /var/www/cgi-bin

Dependencies
------------

This role requires a web server to serve the mailgraph.cgi. Jeff Geerling's `geerlingguy.apache` role is being used here. Please remember that the `mailgraph_cgi_location` may need to be changed depending on your vhost configuration in apache.

Example Playbook
----------------

# Simple implementation with no domain - URL: `http://[hostname]:8080/cgi-bin/mailgraph.cgi`

    - hosts: mailservers
      
      vars: 
        mailgraph_ignore_localhost: true
        mailgraph_start_on_boot: true
        mailgraph_mail_log: /var/log/mail.log
        mailgraph_cgi_location: /var/www/cgi-bin
        apache_listen_port: 8080

      roles:
        - geerlingguy.apache
        - LukasGibb.mailgraph

# More complex implementation with a domain - URL: `http://example.com:8080/cgi-bin/mailgraph.cgi`

    - hosts: mailservers
      
      vars: 
        mailgraph_ignore_localhost: true
        mailgraph_start_on_boot: true
        mailgraph_mail_log: /var/log/mail.log
        mailgraph_cgi_location: /var/www/example_com/cgi-bin
        apache_listen_port: 8080
        apache_vhosts:
          - servername: "example.com"
            documentroot: "/var/www/example_com"

      roles:
        - geerlingguy.apache
        - LukasGibb.mailgraph

License
-------

MIT

Author Information
------------------

This role was created in 2018 by [Lukas Gibb](https://github.com/LukasGibb), from [CloudJourneyman.com](http://www.cloudjourneyman.com/)

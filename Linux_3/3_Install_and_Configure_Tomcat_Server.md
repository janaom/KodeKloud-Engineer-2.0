# Instructions

The `Nautilus` application development team recently finished the beta version of one of their Java-based applications, which they are planning to deploy on one of the app servers in `Stratos DC`. After an internal team meeting, they have decided to use the `tomcat` application server. Based on the requirements mentioned below complete the task:

a.  Install `tomcat` server on `App Server 2`.

b. Configure it to run on port `6300`.

c. There is a `ROOT.war` file on `Jump host` at location `/tmp`.

Deploy it on this tomcat server and make sure the webpage works directly on base URL i.e `curl http://stapp02:6300`

# Solution

from thor@jump_host:

`scp /tmp/ROOT.war steve@stapp02:/tmp`

`ssh steve@stapp02`

`sudo yum  update`

`sudo yum install tomcat`

`sudo su -`

`vi /usr/share/tomcat/conf/server.xml`

```bash
   #change ports
    <Connector port="6300" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    <!-- A "Connector" using the shared thread pool-->
    <!--
    <Connector executor="tomcatThreadPool"
               port="6300" protocol="HTTP/1.1"

```
`cp /tmp/ROOT.war /usr/share/tomcat/webapps`

`systemctl restart tomcat`

`systemctl status tomcat`

`curl -i http://stapp02:6300`

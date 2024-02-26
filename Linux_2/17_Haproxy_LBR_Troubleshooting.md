# Instructions

`xFusionCorp Industries` has an application running on `Nautlitus` infrastructure in `Stratos Datacenter`. The monitoring tool  recognised that there is an issue with the `haproxy` service on `LBR` server. That needs to fixed to make the application work properly.

Troubleshoot and fix the issue, and make sure `haproxy` service is running on `Nautilus LBR` server. Once fixed, make sure you are able to access the website using `StaticApp` button on the top bar.

# Solution

`ssh loki@stlb01`

`sudo su -`

`systemctl status haproxy`

`haproxy -c -f /etc/haproxy/haproxy.cfg`

`cat /etc/haproxy/haproxy.cfg |grep haprox`

![image](https://github.com/janaom/KodeKloud-Engineer-2.0/assets/83917694/10ac1f30-6c8c-4b9e-814e-fe1495e77b1d)

If you check `id haprox`, you will get: `id: haprox: no such user`

However, there if you check `id haproxy`, the result will be: `uid=188(haproxy) gid=188(haproxy) groups=188(haproxy)`

Just change this line in `haproxy.cfg`

```python
user        haproxy
```

Check again: `haproxy -c -f /etc/haproxy/haproxy.cfg`

You should see: `Configuration file is valid`

`systemctl start haproxy`

`systemctl status haproxy`

You should see: `Active: active (running)`

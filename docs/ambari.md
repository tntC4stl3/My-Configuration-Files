Ambari
=======

1. Generate ssh key on Ambari server, it will be use latter.
	* ssh-keygen

2. Enable NTP on Cluster
	* `yum install ntp`
	* check NTP service will be automatically started: `systemctl is-enable ntpd`
	* set NTP servcie to auto-start on boot: `systemctl enable ntpd`
	* start NTP service: `systemctl start ntpd`

3. Configuring iptables, temporarily disable iptables.
	* `systemctl disable firewalld`
	* `service firewalld stop`
	
4. Disable SELinux
	* `setenforce 0`
	* Permanently disable SELinux: set `SELINUX=disabled` in `/etc/selinux/config`.

5. Use `hostname` command to set hostname to FQDN (e.g. master1.test.sample.net)

6. Install Ambari
	* `wget -nv http://public-repo-1.hortonworks.com/ambari/centos7/2.x/updates/2.4.0.1/ambari.repo -O /etc/yum.repos.d/ambari.repo`
	* `yum install ambari-server`

7. Setup and start Ambari server
	* `ambari-server setup`
	* `ambari-server start`
	* detail please refer to http://docs.hortonworks.com/HDPDocuments/Ambari-2.4.0.1/bk_ambari-installation/content/set_up_the_ambari_server.html

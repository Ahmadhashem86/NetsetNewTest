## On the primary server side:

1- added new key in named.conf.keys

\# nano -w named.conf.keys


     key dns2 {
        algorithm hmac-sha1;
        secret "+hItdqx5Q+jvuKVrrBEdp+kotR8=";
     };

     server 85.118.202.8 {
        keys { dns2 ;};
     };


1.1- restart service to activate the change

\# sudo jexec  nameserver rndc reconfig


2- added new line in acl recurseallow
   added new line in allow-transfer

\# nano -w named.conf.options


     85.118.202.8;

2.1- activate the changes:
\# sudo jexec  nameserver rndc reconfig

check the status: 
\# less /srv/jails/nameserver/var/log/messages


## On the secondary server side:

1- Create key:

\# tsig-keygen -a hmac-sha1 dns2 > dns2.key

2- use the key in the configuration:
\# nano -w /etc/named.conf.d/keys.conf


     key "dns2" {
        algorithm hmac-sha1;
        secret "+hItdqx5Q+jvuKVrrBEdp+kotR8=";
     };

     server 194.16.65.242 {
        keys { dns2; };
     };

3- Create Zones configuration:

\# nano -w /etc/named.conf.d/zones.conf

     zone "netset.se" {
        type slave;
        file "/etc/named.conf.d/zones/db.netset.se";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
     };


     zone "netset.us" {
        type slave;
        file "/etc/named.conf.d/zones/db.netset.us";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
     };
 
     zone "netset.nu" {
        type slave;
        file "/etc/named.conf.d/zones/db.netset.nu";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
     };

     zone "netset.eu" {
        type slave;
        file "/etc/named.conf.d/zones/db.netset.eu";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
     };

     zone "netset.asia" {
        type slave;
        file "/etc/named.conf.d/zones/db.netset.asia";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
     };

     zone "tradepoint.se" {
        type slave;
        file "/etc/named.conf.d/zones/db.tradepoint.se";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
     };

     zone "240-28.65.16.194.in-addr.arpa" {
        type slave;
        file "/etc/named.conf.d/zones/db.194.16.65";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
     };

     zone "cries.se" {
        type slave;
        file "/etc/named.conf.d/zones/db.cries.se";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
     };

     zone "nettailer.co.uk" {
        type slave;
        file "/etc/named.conf.d/zones/db.nettailer.co.uk";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
     };

     zone "nettailer.com" {
        type slave;
        file "/etc/named.conf.d/zones/db.nettailer.com";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
     };

	zone "nettailer.se" {
        type slave;
        file "/etc/named.conf.d/zones/db.nettailer.se";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
	};

	zone "netailer.se" {
        type slave;
        file "/etc/named.conf.d/zones/db.netailer.se";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
	};

	zone "my-secure-shop.eu" {
        type slave;
        file "/etc/named.conf.d/zones/db.my-secure-shop.eu";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
	};

	zone "nettailer.dk" {
        type slave;
        file "/etc/named.conf.d/zones/db.nettailer.dk";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
	};

	zone "netailer.dk" {
        type slave;
        file "/etc/named.conf.d/zones/db.netailer.dk";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };

	};

	zone "nettailer.no" {
        type slave;
        file "/etc/named.conf.d/zones/db.nettailer.no";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
	};

	zone "netailer.eu" {
        type slave;
        file "/etc/named.conf.d/zones/db.netailer.eu";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
	};

	zone "netset.io" {
        type slave;
        file "/etc/named.conf.d/zones/db.netset.io";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
	};

	zone "netshore.com" {
        type slave;
        file "/etc/named.conf.d/zones/db.netshore.com";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
	};

	zone "netset.co.uk" {
        type slave;
        file "/etc/named.conf.d/zones/db.netset.co.uk";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
	};

	zone "netset.com" {
        type slave;
        file "/etc/named.conf.d/zones/db.netset.com";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
	};

	zone "netsetdemo.com" {
        type slave;
        file "/etc/named.conf.d/zones/db.netsetdemo.com";
        masters { 194.16.65.242; };
        allow-transfer {  key dns2; };
	};

4- Check the default settings:

\# less /etc/named.conf

5- check settings is ok <br/>
\# named-checkconf <br/>
\# named-checkzone  unixmen.local  /var/named/forward.unixmen <br/>
\# named-checkconf /etc/named.conf <br/>


6- Port

\# firewall-cmd --permanent --add-port=53/udp <br/>
\# firewall-cmd --permanent --add-port=53/tcp <br/>
\# firewall-cmd --reload <br/>
+# firewall-cmd --list-all <br/>

####Note1: Cygrids DNS servers:

@dns01.cygrids.net <br/>
@dns02.cygrids.net <br/>
85.118.200.201 

####Troublshooting:

1- reverse dns finns hos IP ägaren
\#dig -x IP_ADDRESS

2-Network unreachable on IPv6:

add OPTIONS="-4"

to nano -w /etc/sysconfig/named


####Note2: You should add Nameserver: ns4.netset.se hos domän registraren.
netset.com hos godaddy <br/>
netset.se hos Loopia <br/>

#--------------------------------------------------------

#DNSSEC:


\# sudo dnssec-keygen -a RSASHA256 -b 2048 -n ZONE   netsetdemo.com

\# sudo dnssec-keygen -f KSK -a RSASHA256 -b 4096 -n ZONE netsetdemo.com

\# dnssec-signzone -3 f55345c0803300b4 -A -N INCREMENT -o  netsetdemo.com -t netsetdemo.com.hosts


     KSK=257  ZSK=256     RSASHA256= algorithm 8
     flags aa=authoritative name server   ra= recursive name server  ad= the DNSSEC is enabled and used

\# dig DNSKEY netsetdemo.com. +multiline <br/>
\# dig +trace +noadditional DS netsetdemo.com. @8.8.8.8 | grep DS <br/>
\# dig @8.8.8.8 netsetdemo.com. DS <br/>
\# dig @ns.netset.se netset119.netset.com A +cd <br/>


	#!/bin/sh
	PDIR=`pwd`
	ZONEDIR="/srv/jails/nameserver/usr/local/etc/namedb" #location of your zone files
	ZONE=$1
	ZONEFILE=$2
	DNSSERVICE="named" #On CentOS/Fedora replace this with "named"
	cd $ZONEDIR
	SERIAL=`sudo jexec  nameserver rndc zonestatus netsetdemo.com | grep signed |egrep -ho '[0-9]{10}'`
	sed -i 's/'$SERIAL'/'$(($SERIAL+1))'/' $ZONEFILE
	sudo dnssec-signzone -A -3 f55345c0803300b4  -N increment -o $1 -t $2
	cd $PDIR


\# sudo jexec  nameserver rndc reconfig

\# Add the keys to working directory named/working/


Test 1: lyckades

cd /srv/jails/nameserver/usr/local/etc/namedb
sudo jexec  nameserver rndc freeze netsetdemo.com
sudo nano -w  netsetdemo.com.hosts

% the keyś are in separate folder
sudo dnssec-signzone -g -d keys/netsetdemo_256.keys -3 $(head -c 1000 /dev/random | sha256 | cut -b 1-16) -A -o  netsetdemo.com -t netsetdemo.com.hosts keys/netsetdemo_256.keys/Knetsetdemo.com.+008+02763.private keys/netsetdemo_256.keys/Knetsetdemo.com.+008+59581.private

% If the keys are in the same folder as zonefile and the SOA serial is incremntal (not recommended)
sudo dnssec-signzone -3 $(head -c 1000 /dev/random | sha256 | cut -b 1-16) -A -N INCREMENT -o  netsetdemo.com -t netsetdemo.com.hosts
                   
sudo jexec  nameserver rndc reload netsetdemo.com
sudo jexec  nameserver rndc thaw netsetdemo.com

sudo jexec  nameserver rndc zonestatus netsetdemo.com
less /srv/jails/nameserver/var/log/messages
sudo jexec  nameserver rndc status
sudo jexec  nameserver rndc  signing -list netsetdemo.com

====================================================

### ZONE: netset.co.uk


\# cd /srv/jails/nameserver/usr/local/etc/namedb <br/>
\# sudo jexec  nameserver rndc freeze netset.co.uk <br/>
\# sudo nano -w  netset.co.uk.hosts <br/>

\# sudo dnssec-signzone -d keys/netset.co.uk.keys -3 $(head -c 1000 /dev/random | sha256 | cut -b 1-16) -A -o netset.co.uk -t netset.co.uk.hosts keys/netset.co.uk.keys/Knetset.co.uk.+008+32134.private keys/netset.co.uk.keys/Knetset.co.uk.+008+42756.private
<br/>
\# sudo jexec  nameserver rndc reload netset.co.uk <br/>
\# sudo jexec  nameserver rndc thaw netset.co.uk <br/>

\# sudo jexec  nameserver rndc zonestatus netset.co.uk <br/>
\# less /srv/jails/nameserver/var/log/messages <br/>
\# sudo jexec  nameserver rndc status<br/>
====================================================

Zone: nettailer.com

sudo dnssec-keygen -a RSASHA256 -b 2048 -n ZONE nettailer.com
sudo dnssec-keygen -f KSK -a RSASHA256 -b 4096 -n ZONE nettailer.com


sudo jexec  nameserver rndc freeze nettailer.com
sudo nano -w nettailer.com.hosts

sudo dnssec-signzone -d keys/nettailer.com.keys -3 $(head -c 1000 /dev/random | sha256 | cut -b 1-16) -A -o nettailer.com -t nettailer.com.hosts keys/nettailer.com.keys/Knettailer.com.+008+65085.private keys/nettailer.com.keys/Knettailer.com.+008+61729.private

sudo jexec  nameserver rndc reload nettailer.com
sudo jexec  nameserver rndc thaw nettailer.com


sudo jexec  nameserver rndc zonestatus nettailer.com
sudo jexec  nameserver rndc status
====================================================

Zone: netset.io
Edit zone
then 

sudo dnssec-signzone -d keys/netset.io.keys -3 $(head -c 1000 /dev/random | sha256 | cut -b 1-16) -A -o  netset.io -t netset.io.hosts keys/netset.io.keys/Knetset.io.+008+16549.private keys/netset.io.keys/Knetset.io.+008+44718.private

Then

sudo jexec  nameserver rndc reload 

sudo jexec  nameserver rndc zonestatus netset.io
less /srv/jails/nameserver/var/log/messages
sudo jexec  nameserver rndc status


====================================================

Zone: netset.com
5 8 * * Mon  /usr/home/nsadmin/sign-netset.com-zone


cd /srv/jails/nameserver/usr/local/etc/namedb
sudo jexec  nameserver rndc freeze netset.com
sudo nano -w  netset.com.hosts

sudo dnssec-signzone -d keys/netset.com.keys -3 $(head -c 1000 /dev/random | sha256 | cut -b 1-16) -A -o  netset.com -t netset.com.hosts keys/netset.com.keys/Knetset.com.+008+36214.private keys/netset.com.keys/Knetset.com.+008+64908.private

sudo jexec  nameserver rndc reload netset.com
sudo jexec  nameserver rndc thaw netset.com

sudo jexec  nameserver rndc zonestatus netset.com
less /srv/jails/nameserver/var/log/messages
sudo jexec  nameserver rndc status

====================================================
Zone: nettailer.co.uk

cd /srv/jails/nameserver/usr/local/etc/namedb

sudo nano -w nettailer.co.uk.hosts

sudo dnssec-signzone -d keys/nettailer.co.uk.keys -3 $(head -c 1000 /dev/random | sha256 | cut -b 1-16) -A -o nettailer.co.uk -t nettailer.co.uk.hosts  keys/nettailer.co.uk.keys/Knettailer.co.uk.+008+23700.private keys/nettailer.co.uk.keys/Knettailer.co.uk.+008+45094.private

sudo jexec  nameserver rndc reload

less /srv/jails/nameserver/var/log/messages
sudo jexec  nameserver rndc zonestatus nettailer.co.uk

====================================================

Zone: netset.se

cd /srv/jails/nameserver/usr/local/etc/namedb
sudo jexec  nameserver rndc freeze netset.se
sudo nano -w  netset.se.hosts

sudo dnssec-signzone -d keys/netset.se.keys -3 $(head -c 1000 /dev/random | sha256 | cut -b 1-16) -A -o  netset.se -t netset.se.hosts keys/netset.se.keys/Knetset.se.+008+60556.private keys/netset.se.keys/Knetset.se.+008+04146.private 

sudo jexec  nameserver rndc reload  netset.se
sudo jexec  nameserver rndc thaw  netset.se

less /srv/jails/nameserver/var/log/messages

sudo jexec  nameserver rndc zonestatus netset.se
====================================================

Zone: nettailer.se

cd /srv/jails/nameserver/usr/local/etc/namedb
sudo jexec  nameserver rndc freeze nettailer.se
sudo nano -w  nettailer.se.hosts

sudo dnssec-signzone -d keys/nettailer.se.keys -3 $(head -c 1000 /dev/random | sha256 | cut -b 1-16) -A -o  nettailer.se -t nettailer.se.hosts keys/nettailer.se.keys/Knettailer.se.+008+02454.private keys/nettailer.se.keys/Knettailer.se.+008+25465.private


sudo jexec  nameserver rndc reload nettailer.se
sudo jexec  nameserver rndc thaw nettailer.se

less /srv/jails/nameserver/var/log/messages

sudo jexec  nameserver rndc zonestatus nettailer.se
 
 =======================================================

 Zone:nettailer.dk

 sudo dnssec-signzone -d keys/nettailer.dk.keys -3 $(head -c 1000 /dev/random | sha256 | cut -b 1-16) -A -o nettailer.dk -t nettailer.dk.hosts  keys/nettailer.dk.keys/Knettailer.dk.+008+46938.private keys/nettailer.dk.keys/Knettailer.dk.+008+22066.private

 =======================================================

 Zone: netset.eu

sudo jexec  nameserver rndc freeze netset.eu
sudo nano -w  netset.eu.hosts

 sudo dnssec-signzone -d keys/netset.eu.keys -3 $(head -c 1000 /dev/random | sha256 | cut -b 1-16) -A -o netset.eu -t netset.eu.hosts  keys/netset.eu.keys/Knetset.eu.+008+54198.private keys/netset.eu.keys/Knetset.eu.+008+37339.private

 sudo jexec  nameserver rndc reload netset.eu
 sudo jexec  nameserver rndc thaw netset.eu

 less /srv/jails/nameserver/var/log/messages
 sudo jexec  nameserver rndc zonestatus netset.eu
 ========================================================

# fastdbl

Freifunk Nord fastd Blacklist

Public keys listed are no longer needed for connecting to our Networks, as a result of abuse, faulty interconnecting different Networks, and so on we need this blacklist.

Installation:

1 - cron-wget the blacklist to your fastd directory:

    crontab -e

then add 

    */5 * * * * wget -q -O /etc/fastd/fastd-blacklist.json https://raw.githubusercontent.com/Freifunk-Nord/fastdbl/master/fastd-blacklist.json

2 - download the Scipt and make it Xecutable:

    wget -O /etc/fastd/fastd-blacklist.sh https://raw.githubusercontent.com/Freifunk-Nord/fastdbl/master/fastd-blacklist.sh
    chmod +x /etc/fastd/fastd-blacklist.sh

3 - Add the following to your fastd.conf:

    on verify "
      /etc/fastd/fastd-blacklist.sh $PEER_KEY
    ";

4 - restart your fastd and you are ready to go:

    /etc/init.d/fastd restart

# Single script to add blacklist

If you want to add this with one script, use these lines:

```
FASTDCONFPATH=/etc/fastd/ffnord-mvpn

wget -q -O /etc/fastd/fastd-blacklist.json https://raw.githubusercontent.com/Freifunk-Nord/fastdbl/master/fastd-blacklist.json
wget -O /etc/fastd/fastd-blacklist.sh https://raw.githubusercontent.com/Freifunk-Nord/fastdbl/master/fastd-blacklist.sh
chmod +x /etc/fastd/fastd-blacklist.sh

sed -i 's~on verify "true";~on verify "/etc/fastd/fastd-blacklist.sh $PEER_KEY";~g' $FASTDCONFPATH/fastd.conf

service fastd stop
service fastd start
```

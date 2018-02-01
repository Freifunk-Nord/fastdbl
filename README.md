# fastdbl

ffrl fastd Blacklist

Public keys listed are no longer accepted for connecting to our Networks, as a result of abuse, faulty interconnecting different Networks, and so on

Installation:

1 - cron-wget the blacklist to your fastd directory:

    crontab -e

then add 

    */5 * * * * wget -q -O /etc/fastd/fastd-blacklist.json https://raw.githubusercontent.com/ffruhr/fastdbl/master/fastd-blacklist.json

2 - download the Scipt and make it Xecutable:

    wget -O /etc/fastd/fastd-blacklist.sh https://raw.githubusercontent.com/ffruhr/fastdbl/master/fastd-blacklist.sh
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
FASTDPATH=/etc/fastd/ffnord-mvpn

cat >>/etc/fastd/fastd-blacklist.sh << EOF
#!/bin/bash
  PEER_KEY=$1
  if /bin/grep -Fq $PEER_KEY /etc/fastd/fastd-blacklist.json; then
       exit 1
  else
       exit 0
  fi
EOF

chmod +x /etc/fastd/fastd-blacklist.sh
rm $FASTDPATH/fastd-blacklist.json
cat >>$FASTDPATH/fastd-blacklist.json << EOF
{
  "peers": 
  [
    {
      "pubkey": "78a61042764d1f3dae43c48a0978eaf6f8768fd1b263e27bf7299f6d47f66ac0",
      "comment": "dummy eintrag"
    },
    {
      "pubkey": "1cc1b4eec3a7bd81cff5c0677351a65de1b46132382ca4b590dee4203c9dbd99",
      "comment": "Omega2015 aus Bad Säckingen bei Basel bastelt gerne rum ;)"
    }
  ]
}
EOF

sed -i 's~on verify "true";~on verify "/etc/fastd/fastd-blacklist.sh $PEER_KEY";~g' $FASTDPATH/fastd.conf

service fastd stop
service fastd start
```

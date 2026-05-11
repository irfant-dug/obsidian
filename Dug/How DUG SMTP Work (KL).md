Every host (eg. kmon005, kmon0007) that is sending emails will have an Mail Transfer Agent (MTA) software to handle email. The most popular MTA is postfix, which is commonly used in DUG
```
[adm_irfant@kmon0005 ~]$ systemctl status postfix.service
● postfix.service - Postfix Mail Transport Agent
   Loaded: loaded (/usr/lib/systemd/system/postfix.service; enabled; vendor preset: disabled)
   Active: active (running) since Sat 2026-05-09 14:05:32 +08; 5h 41min ago
  Process: 16115 ExecStop=/usr/sbin/postfix stop (code=exited, status=0/SUCCESS)
  Process: 16133 ExecStart=/usr/sbin/postfix start (code=exited, status=0/SUCCESS)
  Process: 16131 ExecStartPre=/usr/libexec/postfix/chroot-update (code=exited, status=0/SUCCESS)
  Process: 16127 ExecStartPre=/usr/libexec/postfix/aliasesdb (code=exited, status=0/SUCCESS)
 Main PID: 16208 (master)
   CGroup: /system.slice/postfix.service
           ├─16208 /usr/libexec/postfix/master -w
           ├─16210 qmgr -l -t unix -u
           └─19948 pickup -l -t unix -u
```

The host will send their emails to an SMTP relay server, which is ksmtp1 for KL. ksmtp1 is also running postfix service. The reason why we used a smtp relay server as it make it  easier to set up PTR + SPF + DKIM for email authentication. Failure to do so may result in email going into spam folder, depending on the policy. 

Once ksmtp1 receives the email, it will check the email's recipient domain (dug.com) and send it to the domain's mail server (based on MX record). ksmtp1 will get a confirmation from the email server if the email is successfully received.
```
echo "This is a test from kmon0005" | mail -S mta=smtp://ksmtp1.dug.com -s "Test from kmon0005" irfant@dug.com
```

```
[root@ksmtp1 ~]# tail -50 /var/log/maillog | grep -i "irfant"                      
May  9 15:24:08 ksmtp1 postfix/cleanup[175304]: 4F49620D: message-id=<20260509072408.4bM_H%adm_irfant@kud42.dug.com>                                    May  9 15:24:08 ksmtp1 postfix/qmgr[150]: 4F49620D: from=<adm_irfant@kud42.dug.com>, size=412, nrcpt=1 (queue active)                       May  9 15:24:10 ksmtp1 postfix/smtp[175305]: 4F49620D: to=<irfant@dug.com>, relay=ASPMX.L.GOOGLE.com[142.251.12.27]:25, delay=1.8, delays=0.07/0.03/1/0.73, dsn=2.0.0, status=sent (250 2.0.0 OK                 
  DMARC:Quarantine 1778311450 41be03b00d2f7-c826788730esi7709065a12.371 - gsmtp) 
```
* status=sent + dsn=2.0.0 = Google accepted the message. ksmtp1 successfully delivered it.
* 250 2.0.0 OK = Google's SMTP "all good" response.
* DMARC:Quarantine = Google accepted it but routed it to the spam folder instead of the inbox, because the message failed DMARC checks.

Because we don't set up those PTR + SPF + DKIM email authentication, we have to hardcode the allow inbound IP address to not end up in spam.


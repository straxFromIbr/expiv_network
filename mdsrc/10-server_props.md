# サーバの設定事項

以下の表に示すようにドメイン名とホスト名を定めた。
それぞれのサーバの名前は麻雀の牌の種類（萬子、筒子、索子、字牌）から命名した。


## ネットワーク

|   ドメイン名 | ネットワークアドレス | サブネットマスク |
|-------------:|---------------------:|-----------------:|
| `mahjongnet` |        `192.168.1.0` |  `255.255.255.0` |

## コンピュータ

|             | コンピュータ名 |    IPアドレス |
|:------------|---------------:|--------------:|
| DNS  Server |      `pinsdns` | `192.168.1.1` |
| DHCP Server |     `sousdhcp` | `192.168.1.2` |
| SMTP Server |     `wanssmtp` | `192.168.1.3` |
| HTTP Server |     `charhttp` | `192.168.1.4` |


## 担当箇所
私は2週目にDHCPサーバ（`sousdhcp`）とSMTPサーバ（`wanssmtp`）の設定を担当した。

### DHCPサーバの設定

`/etc/dhcpd.conf`ファイルに次のように記述した。

```
ddns-update-style ad-hoc;
option domain-name "mahjongnet";
option domain-name 192.168.1.1;

subnet 192.168.1.0 netmask 255.255.255.0 {
    option broadcast-address 192.168.1.255;
    option subnet-mask 255.255.255.0;
}

host D-0152 {
    hardware ethernet 00:26:18:4E:F0:BD;
    fixed-address 192.168.1.5;
}

host D-0148{
    hardware ethernet 00:26:18:4E:F0:B3;
    fixed-address 192.168.1.6;
}
```

また、割り当てたプライベートIPアドレスを記憶しておくためのデータベースファイルを
`/var/db/dhcpd.leases`に作成した。

加えて`/etc/rc.conf`中でOS起動時にDHCPdを起動するように、
`dhcpd=YES`を追記した。


最後に、`sousdhcp`によりLANに接続した2台のWindowsPC(`D-152`,`D-148`)に上記で設定したIPアドレスが
割り当てられていることを確認した。


### SMTPサーバの設定
`/etc/postfix/main.cf`ファイルに次を追記した。

```
myhostname = wanssmtp.mahjongnet
mydomain = mahjongnet
myorigin = $mydomain
inet_interfaces = all
mydestination = $myhostname, localhost.$mydomain, $mydomain
mynetworks = 192.168.1.0/24, 127.0.0.0/8
alias_maps = hash:/etc/mail/aliases
mail_spool_directory = /var/mail
```

また`/etc/postfix/master.cf`に次の行を追加した。

```
smtp    inet    n   -   n   -   -   smtpd
```

加えてOS起動時にpostfixが起動するように、`/etc/rc.conf`に次の行を追記した。

```
sendmail=NO
postfix=YES
```

最後にtelnetで自身の25番ポートに接続し、`wanssmtp`の一般ユーザーに向けてメールを
送信し、受信できていることを確認した。








## 調査報告事項2
**DHCPサーバとの通信に用いられるブロードキャスト方式についてその利点と欠点を調べよ。**

ブロードキャストとは、同一のネットワークに接続されたすべての機器にデータを一斉送信
する機能である。
ブロードキャスト通信は、ネットワーク内のすべての機器と個別に一対一で通信するのに
比べ、流れるデータ量が少ない点、自身のIPアドレスを持たない場合でもブロードキャスト
通信によりDHCPサーバからデータを得られる点が優れている。
一方上述したようにブロードキャストはネットワーク内の全ての機器がデータを受信するが、
例えばDHCPによりIPアドレスを取得する場合、送信されたデータはDHCPサーバ以外にとって
不要な情報であるため、無関係な機器にとっては余分な負荷となってしまうことが欠点である。

### 参考
- [すべての相手に一斉送信、必殺技の「ブロードキャスト」を完全図解](https://xtech.nikkei.com/atcl/nxt/column/18/00780/062000008/)
- [ブロードキャスト](https://www.wdic.org/w/WDIC/%25E3%2583%2596%25E3%2583%25AD%25E3%2583%25BC%25E3%2583%2589%25E3%2582%25AD%25E3%2583%25A3%25E3%2582%25B9%25E3%2583%2588)

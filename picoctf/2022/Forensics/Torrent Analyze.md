# Torrent Analyze

SOS, someone is torrenting on our network. One of your colleagues has been using torrent to download some files on the company’s network. Can you identify the file(s) that were downloaded? The file name will be the flag, like `picoCTF{filename}`. [Captured traffic](https://artifacts.picoctf.net/c/165/torrent.pcap).

-----

We are given a PCAP, `torrent.pcap`, of UDP traffic.

Within torrent traffic, we can look for `info_hash` which is the SHA1 sum of the torrent file being downloaded.

With our source being `192.168.73.132`, we can filter on this and the BT-DHT packets:

```
bt-dht.bencoded.string contains info_hash and ip.src == 192.168.73.132
```

```
info_hash: e2467cbf021192c241367b892230dc1e05c0580e
```

Googling this reveals a [TorrentKitty](https://cn.torrentkitty.net/information/E2467CBF021192C241367B892230DC1E05C0580E) entry for `ubuntu-19.10-desktop-amd64.iso`:

```
picoCTF{ubuntu-19.10-desktop-amd64.iso}
```
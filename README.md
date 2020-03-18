




![](https://lh5.googleusercontent.com/o7TqWF6oMhFjabUwG0Z4eu0zpQVcfdE_17pOfh_r-E5DUMlFOSd4M2UnPtyVfEgXrq5ZpolauPsH0c-eS04zOvmC1oGBXBfI2BZWQCWqArVlZscg-_pyg8scj8BEDNe2ZOgKW_75)

Wireguard-Go userspace implementation is not the recommended way to use Wireguard, but sometimes we need to deal with what we have. The recommended way is to upgrade to a newer kernel version and use the classic kernel module. 

Let's see how to run Wireguard if you need to deal with a server with an old kernel version (I tried on Debian 7, Kernel 3.2.). 


## 1. Wireguard installation (Raspberry Pi 2 and above)



[Download the archive](https://golang.org/dl/) and extract it into /usr/local, creating a Go tree in /usr/local/go. For example:

```console
tar -C /usr/local -xzf go$VERSION.$OS-$ARCH.tar.gz
```

Download and build [Wireguard-Go](https://git.zx2c4.com/wireguard-go/about/):

```console
$ git clone https://git.zx2c4.com/wireguard-go
$ cd wireguard-go
$ make
$ sudo cp wireguard-go /usr/bin/
$ wireguard-go -f wg0
```

It works? Good.

We need to manually compile Wireguard tools:

```console
$ apt-get install autoconf automake build-essential libmnl-dev
$ git clone https://git.zx2c4.com/WireGuard
$ cd WireGuard/src/tools/
$ make
$ sudo make install
$ cat /etc/wireguard/wg0.conf

[Interface]
Address = 192.168.10.1/24
ListenPort = 51820
PrivateKey = 

[Peer]
PublicKey = 
AllowedIPs = 192.168.10.2/32

$ sudo wg-quick up wg0
```

Done.

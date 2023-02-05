x# bitcoin-full-node
this repository serves me as guide for installing bitcoin full node on home linux server<br>

I am writing this guide for myself in case I will need to go over bitcoin full node (and related services) again in the future. 
I am not an expert in bitcoin neither linux, but I haven't found all-in-one guide covering path "from 0 to hero" anywhere on 
the internet so I have decided to create one for myself. I would like to ask potential other readers (if any), open issue or 
even better open PR, in case you will find some mistake, I am in the just a simple guy.

## setup
- old laptop Lenovo Thinkpad X240 with 8GB RAM and 1TB SSD (server)
- old Macboook Pro (client)

I want to use old Lenovo as a server hosting all bitcoin related services and my Macbook Pro as client from which I will ssh 
into server and use other connected service through web browser.

## server OS 
**goal of this section:** to have clean installation of linux distribution

I want some reliable linux distribution so I did some quick research and came up with idea that Debian is the way. I found this 
[video](https://youtu.be/CJ41KZ0fBMc) where the guy explains installation fairly simply. After creating booting USB (16GB) with 
[balena etcher](https://www.balena.io/etcher) I installed:
- Debian bookworm with weekly updates
- GNOME windows environment

Luckily this Debian version doesn't require any additional tweaking and everything worked out of the box.<br>

To work mork effitiently I use combination of zsh and starship installed according 
to this [tuotiral](https://harshithashok.com/tools/oh-my-zsh-with-starship/).


## server initial setup
**goal of this section:**  daily user has sudo permissions, laptop works with closed lid, client can ssh to server

During installation I created root user and my daily user, which I added to sudoers group following this 
[guide](https://linuxize.com/post/how-to-add-user-to-sudoers-in-debian/).<br>

To disable sleep after closing laptop lid I update (uncomment and change) `HandleLidSwitch=ignore` in 
`/etc/systemd/logind.conf` file and privacy setup as:
![privacy setup after closed lid](./pics/screenshot-privacy-screen.png)
which ensures that after opening lid user password is required.<br>

To be able to connect from my client laptop to the linux server I had to setup `openssh-server` according to this 
[guide](https://phoenixnap.com/kb/how-to-enable-ssh-on-debian), which required to install `UFW` firewall via `sudo apt install 
ufw`. After installing **ufw** it is necessary to turn it on with `sudo ufw enable` 
and set **ufw** default to `sudo ufw default deny incoming` which denies every 
incoming connection except ssh. I faced a small issue, because this 
wasn't my first server setup I had to reset server's fingerprint on my client with 
command `ssh-keygen -R <IP address of the server>`. To ssh into server without inserting user password run `ssh-copy-id -i 
~/.ssh/<client-ssh-key>.pub <server-ip-address>`


## Bitcoin Core on the server
**goal of this section:** Bitcoin Core (mainnet, testnet, regtest) runs as daemon and starts automatically after server restart

### Bitcoin Core initial setup
As I plan to potentially develop features in Bitcoin Core I want to have version 
built from repository. For that I followed the notes for [unix build 
notes](https://github.com/bitcoin/bitcoin/blob/master/doc/build-unix.md) and 
specifically [ubuntu and 
debian](https://github.com/bitcoin/bitcoin/blob/master/doc/build-unix.md#ubuntu--debian) 
version.<br>

Once Bitcoin Core is setup I can run it as deamon with command `bitcoind -daemon` to 
start Bitcoin Core. In be able to do 
[IBD](https://btcinformation.org/en/glossary/initial-block-download), I have to 
allow incoming connetion on **Bitcoin port 8333** with `ufw allow 8333`

 



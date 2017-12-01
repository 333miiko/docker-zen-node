# Docker Zencash Secure Node

This repository will help you setup a zencash node with a single bash script.

The script will install Docker on a fresh Ubuntu VM and provision the following
containers:

- zend https://hub.docker.com/r/whenlambomoon/zend/
- Securenodetracker https://hub.docker.com/r/whenlambomoon/secnodetracker/
- neilpang/acme.sh https://hub.docker.com/r/neilpang/acme.sh

acme.sh will run as a docker container and auto-renew your SSL certificates when required.

## Requirements

You will need a server with at least 4GB ram (swap is genearlly OK) to run a ZenCash secure node. 
You may use my DigitalOcean referral link to get $10 free credit https://m.do.co/c/afbafb6012b6

The script has been tested with Ubuntu 16.04 but should work on any other Linux distribution.

You will also need a domain name that is pointing to your server (eg. zennode.example.com)

## Installation

Invoking the script is best done on a fresh installation. Re-running the install script should not
cause any issues.

```
curl -O https://raw.githubusercontent.com/WhenLamboMoon/docker-zen-node/master/install.sh \
sh ./install.sh <stakeaddr> <email> <fqdn> <region>
```

- Stakeaddr is the transparent address which has your 42 ZEN
- Email is your notification email address should your node have issues
- FQDN is the hostname which must point to the IP address of your server
- Region must either be eu, na or sea

Example:

`./install.sh ztjcr2DSYhMZZ3WFFeoK2hDxhmK4VP3QcgK email@example.com zennode.example.com sea`

The script will install the required dependencies and deploy the three containers on your host.

acme.sh is used to provision and maintain a valid SSL certificate on your domain.

The script should execute successfully when you see the output similar to this:

```
## Generating shield address for node...

ztSpgr2Yat7zSMiLNHtyDKGajzSesxabQsRwJnqtomDJU9wd6LzZppnQJyYiNE8sJDEy5MyTiMrSjf3bWcMKgtF9xcEY4eA
```

This will be the shield address you need to send 1 ZEN in order for your start accepting challenges. It is recommended you send this in 4 transactions of 0.25 ZEN. As of now, this can only be done using the swing wallet or CLI. 

After you send your 1 ZEN, you may check your node private balance:

```
docker exec zen-node gosu user zen-cli z_gettotalbalance
```

Your securenode tracker should now report its successful status, you may check by viewing the secnodetracker logs:

```
docker logs zen-secnodetracker
```

It should return something like this:

```
Secure node config found OK - linking...
CPU Intel Core Processor (Haswell, no TSX)  cores=1  speed=2394
Tracker app version: 0.1.0
MemTotal: 3.86GB  MemFree: 1.61GB  MemAvailable: 3.47GB  SwapTotal: 4.10GB  SwapFree: 4.10GB  
2017-12-01 07:29:58 GMT -- Connected to server ts1.na. Initializing...
2017-12-01 07:30:08 GMT -- Zend: Loading block index...
2017-12-01 07:30:18 GMT -- Zend: Verifying blocks...
Secure Node t_address (not for stake)=XXXX
Balance for challenge transactions is 1
Using the following address for challenges
xxxx
```

Finally, check the securenode tracker website to see if your node has been registered:

- NA: https://securenodes.na.zensystem.io/
- EU: https://securenodes.eu.zensystem.io/
- SEA: https://securenodes.sea.zensystem.io/

**Please Note:** It may take a few hours for your server to fully update to the latest block. Your node will not come online until this has been fully updated.

You may check the latest block status with the following command:

```
docker exec zen-node gosu user zen-cli getinfo
```

## Troubleshooting

If you need to execute commands with `zen-cli` you will need to append the following:

```
docker exec zen-node gosu user zen-cli
```

This is because zend is run as a non-root user using `gosu`. Removing `gosu user` will return
you an error due to missing config files.

If you run into any issues feel free to open an issue. Please include output from the following commands when opening your issue:

```
docker logs zen-secnodetracker
docker logs zen-node
```

## Donations

Donations are appreciated:

**BTC** 1FN8PRaY3CTNVZy2Fbix9fHK2ASwJ4zeys

**ETH** 0x4f78813f23f443deb75a5267bed8c282b74f571b

**ZEN** znZ2NFrmV6jaiq1hEnKxzApKzgGqptZNq7o

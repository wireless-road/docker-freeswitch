
## FreeSWITCH

[FreeSWITCH](http://www.freeswitch.org/) docker image

- Current version is `1.10.2`

### Fork
This is a fork of [docker-freeswitch](https://github.com/kovalyshyn/docker-freeswitch) with few changes allowing to simplify setup of SIP server and clients.

### Getting started


##### Run SIP server 

Build docker image:
```
$ docker build -t fs .
```
and run it:
```
$ docker run -it --privileged=true --net="host" --name=FS -v /home/al/Downloads/docker-freeswitch-wr/freeswitch_config_example:/etc/freeswitch fs
```

where `home/al/Downloads/docker-freeswitch-wr/freeswitch_config_example` is path to `freeswitch_config_example` folder of this repo.
This config defines three client's IDs: 1000, 1001 and 1002 with passwords same as ID. Server address set to 192.168.0.118. You have to change it to IP address of your server in `dialplan/default/20-interconnect.xml` and `directory/private.xml` files.

##### Run two or more SIP clients.
You can use [Baresip](https://github.com/baresip/baresip).
Install it first: 
```
$ sudo apt-get install baresip
```
Then open `~/.baresip/accounts`, comment everything and add to the end of file following: `<sip:1002@192.168.0.118>;auth_pass=1002` to configure your baresip client with ID 1002. Do the same on another machine but using another ID (1000 or 1002). Here `192.168.0.118` is IP address of server running freeswitch.

##### Make a call.
Run baresip by typing
```
$ baresip
```
without any arguments on one of your machine with baresip installed on previous step. You should see log message like this:
```
1002@192.168.0.118: {0/UDP/v4} 200 OK () [1 binding]
All 1 useragent registered successfully! (3 ms)
```
After that you can start a call. Type `d` for that and then address of abonent you are calling (1000 for example). 
To answer a call from machine that run `baresip` with address 1000 - type `a`.

##### Stop freeswitch server.

Type:
```
> shutdown
```
in freeswitch's command line to stop server.


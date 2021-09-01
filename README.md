# Equinix-Metal-Benchmarking
This repository will be a brief tutorial on how to run a few benchmarks on Equinix Metal machines.  
The benchmarks that will be shown are FIO and Netperf.

## Table of Contents 
- [Getting Started](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking/blob/main/README.md#getting-started)  
  - [New Users](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking/blob/main/README.md#new-users)
  - [Existing Users]()
  - [Setup](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking/blob/main/README.md#setup)
- [Lets Begin Benchmarking](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking/blob/main/README.md#lets-begin-benchmarking)
  - [FIO](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking/blob/main/README.md#fio)
    - [What is FIO](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking/blob/main/README.md#what-is-fio-ubuntu-fio-manual)
    - [Setup](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking/blob/main/README.md#setup-1)
    - [Running FIO](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking/blob/main/README.md#running-fio)
    - [Example](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking/blob/main/README.md#example)
      - [Flags](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking#flags) 
  - [Netperf](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking/blob/main/README.md#netperf) 
    - [What is Netperf](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking/blob/main/README.md#what-is-netperf-netperf-manual)
    - [Setup](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking/blob/main/README.md#setup-2)
    - [Running Netperf](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking#running-netperf)
    - [Example](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking#example-1)
      - [TPC Stream and OMNI Example](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking#tpc-stream-and-omni-example)
      - [Flags](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking#flags-1)
 - [Helpful Documentation](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking#helpful-documentation)
   - [Equinix Metal](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking#equinix-metal) 
   - [FIO](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking#fio-1)
   - [Netperf](https://github.com/JThomasEquinix/Equinix-Metal-Benchmarking#netperf-1)
   

## Getting Started 

### New Users
- New Metal Users must first [Sign up](https://console.equinix.com/signup).  
- Once you have created an account, you can begin using the [Equinix Metal Portal](https://console.equinix.com/login/).  

### Existing Users
- Go to [Equinix Metal Portal](https://console.equinix.com/login/).  
- Enter your user name and password.  
- Click Login.  

### Setup 
- Create a project.  
- Create your personal [SSH key](https://console.equinix.com/users/c3d60bb9-8ceb-4ac9-bd20-52d7e1538cfe/ssh) for the project.   
- For more in depth instructions on the SSH key setup check out the documentation on the official [Equinix Metal developer](https://metal.equinix.com/developers/docs/accounts/ssh-keys/) page.  
- Then deploy your desired machine you'd like to benchmark.
- To get SSH into the machine you just deployed go to its respective page and click SSH access and copy that into your terminal. ```ssh root@<your_public_ipv4>```

# Lets Begin Benchmarking 
This section will describe each benchmark breifly and also give instruction on how to run the test and provide links to each tests respective repositories so you can further experiment.

## FIO 

### What is FIO ([Ubuntu FIO Manual](http://manpages.ubuntu.com/manpages/bionic/man1/fio.1.html))
Fio was originally written to save the creator Jens Axboe the hassle of writing special test case programs when he wanted to test a specific workload, either for performance reasons or to find/reproduce a bug. The process of writing such a test app can be tiresome, especially if you have to do it often. Hence he needed a tool that would be able to simulate a given I/O workload without resorting to writing a tailored test case again and again.

Fio spawns a number of threads or processes doing a particular type of I/O action as specified by the user. fio takes a number of global parameters, each inherited by the thread unless otherwise parameters given to them overriding that setting is given. The typical use of fio is to write a job file matching the I/O load one wants to simulate.

### Setup 
- SSH into the machine you deployed. (OS recommended is Ubuntu because FIO comes with it)
- If you don't use Ubuntu you may need to download the [binary packages](https://fio.readthedocs.io/en/latest/fio_doc.html#binary-packages).
- Also know what disk you want to run the test on use the command of ```lsblk``` to see all block devices.
### Running Fio
Running fio is normally the easiest part - you just give it the job file (or job files) as parameters:  
```fio [options] [jobfile] ...```  
check out the official FIO repository's section on running fio [here](https://fio.readthedocs.io/en/latest/fio_doc.html#running-fio) for some great examples.
### Example
The way I did it was creating a configuration file which you can check out in the fio-configs directory of the repository. then running this command.  
```
fio <configuration file> --filename=/dev/sda --output=results.json --output-format=json+
 
```
#### Flags 
- --filename= location of disk you want to run test on 
- --output= name of the file you want the out put to go to 
- --output-format= the format type you'd like for the results
- More flags can be found in the [official FIO repo](https://fio.readthedocs.io/en/latest/fio_doc.html#command-line-options) 

## Netperf

### What is Netperf [(Netperf Manual)](https://hewlettpackard.github.io/netperf/doc/netperf.html#Top)
Netperf is a benchmark that can be use to measure various aspect of networking performance. The primary foci are bulk (aka unidirectional) data transfer and request/response performance using either TCP or UDP and the Berkeley Sockets interface. As of this writing, the tests available either unconditionally or conditionally include:

- TCP and UDP unidirectional transfer and request/response over IPv4 and IPv6 using the Sockets interface.
- TCP and UDP unidirectional transfer and request/response over IPv4 using the XTI interface.
- Link-level unidirectional transfer and request/response using the DLPI interface.
- Unix domain sockets
- SCTP unidirectional transfer and request/response over IPv4 and IPv6 using the sockets interface.
### Setup 
- SSH into the machine you deployed (OS recommended is Ubuntu because FIO comes with it)
- Download the needed packages to run netperf since I used Ubuntu I used ```apt install netperf ```
- for some test you may need to deploy two machines unless you already have another machine with a port listening.
- if you don't have a second machine deploy a second one and repeat the step of installing Netperf with your OS's respective package manager.    
### Running Netperf 
After deploying your Machine/s and doing the setup to run Netperf you just have to run the commamnd of ``` netperf ```  inside the machine this will run a basic TPC stream test. 
### Example
The way I did it was by doing a TPC-RR and an OMNI test and I used two Metal machine one as the remote system running the netserver.  
#### TPC Stream and OMNI Example 
  - run these commands for both machines after [connecting with SSH](https://metal.equinix.com/developers/docs/accounts/ssh-keys/#connecting-with-ssh)
  ```  
  apt update 
  apt install netperf 
  netperf -h  
  ```
  - Then on the machine you would like to be the remote host run this command to find the port the netserver is running on ```netstat -tulpn | grep netserver ```.
  - Output of command should look similar to this. 
  ``` 
  root@da-c3-small-x86-01-fio-test:~# netstat -tulpn | grep netserver
  tcp6       0      0 :::12865                :::*                    LISTEN      1713/netserver      
  ```
  - The number you want from this is __12865__ that is the port the netserver is listening on. 
  - Also retrieve the Public IPv4 address for this machine. 
    - Same address you used for SSH access earlier get rid of 'root@'. 
  - Then go back to the other machine and run this command for TCP_RR. 
  ``` 
  sudo netperf -H <your_public_ipv4> -p 12865 -P 1 -v -l 60 -I 99,5 -t TCP_RR -- -O min_latency,mean_latency,P90_LATENCY,P99_LATENCY,max_latency,stddev_latency,transaction_rate
  ```
  - Run this command for OMNI test. 
  ``` 
  sudo netperf -H <your_public_ipv4> -p 12865 -l 60 -I 99,5 -t OMNI -- -O LSS_SIZE_END,RSR_SIZE_END,LSS_SIZE,ELAPSED,THROUGHPUT,THROUGHPUT_UNITS
  ```
#### Flags 
- -H This option will set the name of the remote system and or the address family used for the control connection.
- -p This option tells netperf the port number at which it should expect the remote netserver to be listening for control connections.
- More flags can be found in the [official Netperf repo](https://hewlettpackard.github.io/netperf/doc/netperf.html#Global-Options)
## Helpful Documentation 
### Equinix Metal
- [Official Devloper Docs](https://metal.equinix.com/developers/docs/)
- [SSH Help](https://metal.equinix.com/developers/docs/accounts/ssh-keys/)
- [Account Help](https://metal.equinix.com/developers/docs/accounts/users/)
### FIO 
- [Fio Repo](https://fio.readthedocs.io/en/latest/fio_doc.html#running-fio)
- [Fio Ubuntu Manual](http://manpages.ubuntu.com/manpages/bionic/man1/fio.1.html)
### Netperf 
- [Netperf Repo](https://hewlettpackard.github.io/netperf/doc/netperf.html#Top)












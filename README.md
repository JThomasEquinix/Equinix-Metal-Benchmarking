# Equinix-Metal-Benchmarking
This repository will be a brief tutorial on how to run a few benchmarks on Equinix Metal machines.  
The benchmarks that will be shown are FIO, Netperf, and a TPC-C workload test from cockroachDB.

## Table of Contents 
- [Getting Started]()  
  - [New Users]()
  - [Existing Users]()
  - [Setup]()
- [Lets Begin Benchmarking]()
  - [FIO]()
    - [What is FIO]()
    - [Setup]()
    - [Running FIO]()
    - [Example]() 
  - [Netperf]() 
    - [What is Netperf]()
    - [Setup]() 

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
- To get SSH into the machine you just deployed go to its respective page and click SSH access and copy that into your terminal.

# Lets Begin Benchmarking 
This section will describe each benchmark breifly and also give instruction on how to run the test and provide links to each tests respective repositories so you can further experiment.

## FIO 

### What is FIO ([Ubuntu FIO Manual](http://manpages.ubuntu.com/manpages/bionic/man1/fio.1.html))
Fio was originally written to save the creator Jens Axboe the hassle of writing special test case programs when he wanted to test a specific workload, either for performance reasons or to find/reproduce a bug. The process of writing such a test app can be tiresome, especially if you have to do it often. Hence he needed a tool that would be able to simulate a given I/O workload without resorting to writing a tailored test case again and again.

Fio spawns a number of threads or processes doing a particular type of I/O action as specified by the user. fio takes a number of global parameters, each inherited by the thread unless otherwise parameters given to them overriding that setting is given. The typical use of fio is to write a job file matching the I/O load one wants to simulate.

### Setup 
- SSH into the machine you deployed (OS recommended is Ubuntu because FIO comes with it)
- If you don't use Ubuntu you may need to download the [binary packages](https://fio.readthedocs.io/en/latest/fio_doc.html#binary-packages)
- Also know what disk you want to run the test on use the command of ```lsblk``` to see all block devices
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
- more flags can be found in the [official FIO repo](https://fio.readthedocs.io/en/latest/fio_doc.html#command-line-options) 

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
- for some test you may need two machines to run some test. 







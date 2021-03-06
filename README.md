# Crust

[![](https://img.shields.io/badge/Project%20SAFE-Approved-green.svg)](http://maidsafe.net/applications) [![](https://img.shields.io/badge/License-GPL3-green.svg)](https://github.com/maidsafe/crust/blob/master/COPYING)


**Primary Maintainer:**     Chandra Prakash (prakash@maidsafe.net)

**Secondary Maintainer:**   Niall Douglas (niall.douglas@maidsafe.net)

Reliable p2p network connections in Rust with NAT traversal. One of the most needed libraries for any server-less, decentralised project.

|Crate|Linux|Windows|OSX|Coverage|Issues|
|:------:|:-------:|:------:|:------:|:------:|:------:|
|[![](http://meritbadge.herokuapp.com/crust)](https://crates.io/crates/crust)|[![Build Status](https://travis-ci.org/maidsafe/crust.svg?branch=master)](https://travis-ci.org/maidsafe/crust)|[![Build Status](http://ci.maidsafe.net:8080/buildStatus/icon?job=crust_win64_status_badge)](http://ci.maidsafe.net:8080/job/crust_win64_status_badge/)|[![Build Status](http://ci.maidsafe.net:8080/buildStatus/icon?job=crust_osx_status_badge)](http://ci.maidsafe.net:8080/job/crust_osx_status_badge/)|[![Coverage Status](https://coveralls.io/repos/maidsafe/crust/badge.svg)](https://coveralls.io/r/maidsafe/crust)|[![Stories in Ready](https://badge.waffle.io/maidsafe/crust.png?label=ready&title=Ready)](https://waffle.io/maidsafe/crust)|


| [API Documentation - Master branch](http://maidsafe.github.io/crust/crust/) | [SAFENetwork System Documention](http://systemdocs.maidsafe.net/) | [MaidSafe website](http://www.maidsafe.net) | [Safe Community site](https://forum.safenetwork.io) |

#Overview

![crusty] (https://github.com/dirvine/crust/blob/master/img/crust-diagram_1024.png?raw=true)

This library will allow p2p networks to establish and maintain a number of connections in a group when informed by users of the library. As connections are made they are passed up and the user can select which connections to maintain or drop. The library has a bootstrap handler which will attempt to reconnect to any previous "**direct connected**" nodes.

TCP connections are always favoured as these will be by default direct connected (until tcp hole punching can be tested). TCP is also a known reliable protocol. Reliable UDP is the fallback protocol and very effective.

The library contains a beacon system for finding nodes on a local network, this will be extended using a gossip type protocol for multi hop discovery.

Encryption of all streams will also allow for better masking of such networks and add to security, this is done also considering the possibility of attack where adversaries can send data continually we must decrypt prior to handling (meaning we do the work). There are several methods to mitigate this, including alerting upper layers of such activity. The user of the library has the option to provide a blacklisting capability per session to disconnect such nodes 'en masse'.

_direct connected == Nodes we were previously connected to. TCP nodes or reliable UDP nodes that allow incoming connections (i.e. direct or full cone nat that has been hole punched). This library also supports fallback endpoints being passed at construction that will allow a fallback should nodes from previous sessions become unavailable.

##Nat traversal/Handling

Several methods are used for NAT traversal, UpNP, hole punching [See here for TCP NAT traversal] (http://www.cmlab.csie.ntu.edu.tw/~franklai/NATBT.pdf) and [here for UCP/DHT NAT traversal
  ](http://maidsafe.net/Whitepapers/pdf/DHTbasedNATTraversal.pdf) etc. These methods will be added to by the community to allow a p2p network that cannot be easily blocked. By default this library spawns sockets randomly, enabling nodes to appear on several ports over time. This makes them very difficult to trace.

###Pre-requisite:
libsodium is a native dependency for [sodiumxoide](https://github.com/dnaq/sodiumoxide). Thus, install sodium by following the instructions [here](http://doc.libsodium.org/installation/index.html).

For windows, download and use the [prebuilt mingw library](https://download.libsodium.org/libsodium/releases/libsodium-1.0.2-mingw.tar.gz).
Extract and place the libsodium.a file in "bin\x86_64-pc-windows-gnu" for 64bit System, or "bin\i686-pc-windows-gnu" for a 32bit system.

##Todo Items

## [0.0.60]
- [x] [MAID-1075] (https://maidsafe.atlassian.net/browse/MAID-1075) Correct bug; listening on local port (127.0.0.1)
- [x] [MAID-1122] (https://maidsafe.atlassian.net/browse/MAID-1122) Windows ifaddr resolution

## [0.0.61]
- [x] [MAID-1124] (https://maidsafe.atlassian.net/browse/MAID-1124) Get a list of public IPs for others to connect to

## [0.0.62]
- [x] [MAID-1125] (https://maidsafe.atlassian.net/browse/MAID-1125) Update Bootstrap Handler to use Json format.

## [0.0.63]
- [x] [#134] (https://github.com/maidsafe/crust/issues/134) First node doesn't read its own bootstrap list

## [0.0.64]
- [x] Code clean up

## [0.0.65]
- [ ] [#1139] (https://maidsafe.atlassian.net/browse/MAID-1139) Remove Crust API’s start_listening2() and expose `get_own_endpoints()`
- [ ] [#1142] (https://maidsafe.atlassian.net/browse/MAID-1142) Add UTP protocol support to crust

## [0.0.66]
- [ ] [#1132] (https://maidsafe.atlassian.net/browse/MAID-1132) Integrate UPnP
- [ ] [#1136] (https://maidsafe.atlassian.net/browse/MAID-1136) Add a new event `NewBootstrapConnection`
- [ ] [#1140] (https://maidsafe.atlassian.net/browse/MAID-1140) Memory-mapped file I/O for bootstrap file

## [0.0.70]
- [ ] Have ConnectionManager guarantee at most one connection between any two nodes
- [ ] Utp Networking
  - [ ] Utp live port and backup random port selection
  - [ ] Create send/rcv channel from routing to connections object
  - [ ] Implement test for basic "hello world" two way communication
  - [ ] Add connection established/lost messages to be passed to routing (via channel)
  - [ ] Benchmark tx/rv number of packets
  - [ ] Benchmark tx/rc Bytes per second
  - [ ] NAT traversal  [See here for tcp NAT traversal] (http://www.cmlab.csie.ntu.edu.tw/~franklai/NATBT.pdf)
- [ ] Benchmark tx/rv number of packets
- [ ] Benchmark tx/rc Bytes per second
- [ ] Implement NAT hole punch (udp) for reliable udp

## [0.1.0]
- [ ] Tcp hole punching as per paper
- [ ] Tracer tcp (TCP with magic in clear [unencrypted])
- [ ] Wireshark module for tracer TCP

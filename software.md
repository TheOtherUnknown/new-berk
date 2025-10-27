---
layout: default
---

# Software to use
Obviously, selfhosting would be ideal. But what software could hold Valhalla? Here's some candidates

## Matrix
The default assumption. Matrix is an open standard communication platform, with several implementations. The fandoms chat experiment was done with Matrix Synapse, the mainline Matrix reference server. There are other implementations. 

### Pros
Very open, very FOSS, seems somewhat established with governments and things using it. Unlikely to go anywhere. Privacy first

### Cons
Not very user friendly. Synapse is a heavy application, and feature parity with Discord is not quite there. VC/screenshare/video functionality is not yet there, but MatrixRTC looks promising.

### Links 
* [Synapse](https://github.com/element-hq/synapse) - Mainline reference implementation
* [Conduit](https://gitlab.com/famedly/conduit) - Lightweight Rust beta implementation

## Spacebar
A Discord re-implementation and clone, with supposed backward compatability with the main Discord clients

### Pros
Supposed to be basically Discord, but FOSS and self-hostable. 

### Cons
Unsure how finished or active this project will be long-term. Seems not very active at the moment

### Links
* [Spacebar Server](https://github.com/spacebarchat/server)
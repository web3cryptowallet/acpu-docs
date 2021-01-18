# acpu-docs
Animation CPU Documentation

What is Formulas?
What is DOT syntax?

# FAQ

### Q: What is formula?
### A: FORMULA what is it

Formulas like magic. Formula is live particle.

Formula has Numeric ID, name and code.

Source block structure:

```python 
### <id> <name> 

<code>
```

Formula info:

```
formula.id
formula.name
formula.code
```

Each formula instance lives inside multiple nodes ACPU world, it's like program threads or DNA.

Single .acpul file can contain multiple formulas in a random order

```python
### 10000 formula.A

formula.A.code;

### 10001 formula.A.draw

formula.A.draw.code;

### ...
...

```

### Terms

```python
ACPU
ACPUL
Formula
Node instance
Logic formula
Draw formula
Register
```

### ACPU special predefined registers

```python
x,y,z
s
r
i,j,k  local
n,m    local
l0-l64 local
r0-r15 shared 
d0-d64 common
```

# ACPUL

```python
# define:

a 0;                    # a is 0

# extends, use, include:

b a;                    # b is a

c @1010;                # c is 1010 formula
_ @1010;                # import 1010 formula code

sys.include.def 1010;   # sys.include.def is 1010
_ @sys.include.def;     # import sys.include.def 1010 formula code

# operations:

r0:=1;                  # set r0 = 1
r0;                     # returns r0
r1+=r0;                 # r1 = r1 + r0
r1*=r0;                 # r1 = r1 * r0
r1:=sin(r0);            # r1 = sin(r0)
r1:=max(r0, 1);         # r1 = max(r0, 1)

r1:=if(r0, 0, 1);       # if r0 != 0 then 0 else 1

r0:=5;                  # loop 5 times
while(r0>0) {           # while r0 > 0
 r0-=1;                 # r0--
};                      #
```

# Formulas documentations

# Common formula modules

```
System formula modules link
User formula modules link
```

### ACPU builtins

```
draw
```

### draw
Set draw formula for node
```python
draw(r0, formulaId);
```

formulaId - draw formula id

Example:
```python
### 8000000 bootloader

r0:=k==0;

draw(r0, 8000001);

k:=1;

### 8000001 bootloader.draw

rect(u0, 3, 20,0,70,100);

i:=0;
while(i<5) {
 k i*10;
 rect(u0, 1, 0,20+k,20,5);
 rect(u0, 1, 70,20+k,20,5);
 i+=1;
};
```

## sys - System formulas

### 1010 sys.include.def

Define sys formulas basic and system primitives.
Include this formula module before use each other.

Include example:

```python
_ @1010; 
```

Key modules:
```python
sys.touch
sys.node
sys.box2d
sys.display
sys.dsp
sys.net
sys.obj
sys.storage
sys.events
sys.media
sys.geom
```

sys.include.def.acpul link

### 1011 sys.touch

### 1012 sys.node

```python
_ @1010; 
_ @sys.node;
```


### 1017 sys.box2d

### 1019 sys.display

### 1022 sys.dsp

### 1029 sys.stream

### 1030 sys.net

```
```

Functions:

```python
# Net event pool:

net.e.pool 501;
net.e { _;
 node.found 1; # Net node found
 node.lost 2;  # Net node lost
};

eid:=events.pop(net.e.pool,0);
if (eid==net.e.node.found) {}
if (eid==net.e.node.lost) {}

# scan on/off

net.scan.off;
net.scan.local.server;
net.scan.local.peer;

# get node props

net.node.props(buf);

 # buf -> output buffer
 # b: 0 type
 #    1 name
 #    2 ports
 #    3..n (ipv4|ipv6)*n

# peer

net.peer.on;
net.peer.off;

# connection

net.host(ipv4_1,_2,_3,_4, port);
net.host(192,168,0,1, 8077);
```


Net event pool process event:
```python
net.snippet.scan {
 _ @1010;
 _ @sys.net;
 _ @sys.event;
 if (r0) {
  net.scan.local.server();
 };
 eid l0;
 nid l1;
 eid:=events.pop(net.e.pool,0);
 if (eid==net.e.node.found) {
  nid:=o0;
  buf l2;
  buf:=net.node.props(nid);
  # type;name;ports;ips
 };
 if (eid==net.e.node.lost) {
  # remove
 };
};
```

### 1031 sys.obj

### 1033 sys.storage

### 1034 sys.events

### 1035 sys.media

### 1036 sys.geom

### 1037 sys.bullet.physics

### 1100 util

```python
_ @1100;
```

### ul.ui

Key modules:
```python
util.gl.std
util.media.font.obj
util.scroller.std
util.i18n
ul.ui
core.aline
core.gl3d
core.hud
```

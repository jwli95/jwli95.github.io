---
layout: default
title: Matlab Import Raw IQ Data
parent: USRP
nav_order: 3
---

```text
f = fopen('csi20.dat', 'rb');
v = fread(f, 'float');
fclose(f);

n = length(v);

di = v(1:2:(n - 1));
dq = v(2:2:n);

s_in = di + 1i * dq;
```

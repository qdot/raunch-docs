## Launch Protocol

The command protocol for the launch consists of two byte packets.

```
0xYY 0xZZ
```

- YY represents the desired position of the toy. Valid inputs are 0-99
  in [BCD format](https://en.wikipedia.org/wiki/Binary-coded_decimal).
- ZZ represents the desired speed the toy should move to the position
  listed at. Valid inputs are 0-99
  in [BCD format](https://en.wikipedia.org/wiki/Binary-coded_decimal).
  
Invalid inputs are ignored.

# JadventOfCode
Advent of Code 2019 J

## Day 1
### Part 1
```
  mass =. <.@((% 3:) - 2:)
  vals =. 120150 105328 70481 86674 112434 94883...
  +/ mass vals
3325342
```

`mass x` computes `floor((x/3)-2)`, and runs elementwise on vals. We then take the total with `+/`.

### Part 2
```
  mass =. (0 >. <.)@((% 3:) - 2:)
  tmass_t =. (([ + mass@]) $: mass@])`[@.(0 = ])"0
  tmass =. 0 tmass_t ]
  +/ tmass vals
4985158
```

`mass` is unchanged, except we now also cap it below by zero, computing `max(0, floor((x/3)-2))`. `tmass_t` is a recursively defined dyad, allowing us to compute a running sum. 
`x tmass_t y` returns `(x + (mass y)) tmass_t (mass y)`, unless `y = 0` in which case we just return the first argument. `tmass` then just calls `tmass_t` on its argument,
providing `0` for the left argument (since we start the running sum at 0). For example running `tmass 1969` would compute:

```
  tmass 1969
  0   tmass_t 1969
  654 tmass_t 654   NB. mass 1969 = 654
  870 tmass_t 216   NB. mass 654 = 216
  940 tmass_t 70    NB. mass 216 = 70 
  961 tmass_t 21    NB. mass 70 = 21
  966 tmass_t 5     NB. mass 21 = 5
  966 tmass_t 0     NB. mass 5 = 0
966
```
We have to specify that this runs elementwise, otherwise we will be checking for the whole array on the right to equal `0` to exit the recursion, which will always fail unless
the array has length 1 (i.e. is actually a number), hence the `"0` at the end of `tmass_t`. As before, we then take the total with `+/`.

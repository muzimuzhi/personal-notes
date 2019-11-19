# Cheatsheet for PDF Reference v1.7

## Color Operator

| Operands  | Operator        | Description                                      | Sec. |
| --------- | --------------- | ------------------------------------------------ | ---- |
| _gray_    | **g** / **G**   | gray, from black (0.0) to white (1.0)            |      |
| _r g b_   | **rg** / **RG** | rgb, from min (0.0) to max (1.0) intensity       |      |
| _c m y k_ | **k** / **K**   | cmyk, from zero (0.0) to max (1.0) concentration |      |

* Each operand accepts a number in range [0.0, 1.0].
* Lowercase operators set non-stroking color, while uppercase operators set stroking color.
* Example: `0.0 g` sets non-stroking color to black in gray color space.

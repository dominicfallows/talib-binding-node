# talib-binding

The [TA-Lib](http://ta-lib.org/) sync bindings.

## Install

```bash
yarn add talib-binding
```

## Usage

All the functions are exported, you can call it in the same form to [c api](https://ta-lib.org/d_api/d_api.html), but
there are some difference:

- the `startIdx` and `endIdx` is placed at the end of the function signature, and is optional.
- the return value is the computed result, if result is one array, will be it, else will be a nested array to contains all output arrays.
- if the ta-lib function returns a error code, will throw a error.

Additionally, you can pass a `Record[]` to extract its fields rather than input double array such as `inHigh`, `inLow`, etc.
And if the input field is a implicit field of the records, you need to input a string to specifying which one field will be extract
as it.

## Example

```typescript
import * as talib from './src/talib-binding.generated'

// Input double array directly:
let output = talib.SAR(
    [2, 3, 4, 5], /* inHigh */
    [1, 2, 3, 4], /* inLow */
    0.02, /* optAcceleration_Factor, optional */
    0.2, /* optAF_Maximum, optional */
    0, /* startIdx, optional */
    3 /* endIdx, optional */
)
console.log(output)

// Input a records array:
output = talib.SAR([
    {
        Time: 0,
        Open: 1,
        High: 2,
        Low: 1,
        Close: 2,
        Volume: 1,
    },
    {
        Time: 0,
        Open: 2,
        High: 3,
        Low: 2,
        Close: 3,
        Volume: 1,
    },
])

// Input implicit field name with Record[]
output = talib.COS([
    {
        Time: 0,
        Open: 1,
        High: 2,
        Low: 1,
        Close: 2,
        Volume: 1,
    },
    {
        Time: 0,
        Open: 2,
        High: 3,
        Low: 2,
        Close: 3,
        Volume: 1,
    },
], 'Volume')
```

## Notes

- Some API need to use `TA_MAType`, which is exported as `MATypes` in the binding. For example:
    ```typescript
    import * as talib from './src/talib-binding.generated'
    talib.MA([1, 2, 3], void 0, talib.MATypes.SMA)
    ```

## Contributing

Clone the repo at first:

```bash
git clone https://github.com/acrazing/talib-binding-node && cd talib-binding-node
```

All the files end with `generated.*` is generated by [src/generate.ts](./src/generate.ts). Maybe you need to view it
to get detailed information.

## License

- This project published under [MIT](./LICENSE) License.
- The `TA-Lib` is published under [LICENSE](./ta-lib/LICENSE.TXT).
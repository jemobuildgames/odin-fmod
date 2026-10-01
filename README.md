# odin-fmod

Odin bindings for [FMOD](https://www.fmod.com/).

Includes `core`, `fsbank` and `studio` APIs. The `dll` and `lib` files are downloaded from <https://www.fmod.com/download>.

[FMOD API documentation](https://www.fmod.com/docs/2.03/api/welcome.html)

Current FMOD version: `2.03.15` (build `168126`)

Latest tested Odin version: `dev-2026-09-nightly:a2fb372`

## Example

You can see example usage in [demo.odin](demo/demo.odin). Runs on raylib.

Run with:

```cmd
cd demo
odin run .
```

## TODO

- Add more examples
- Translate constants to enums and bit_sets.
- Implement odin wrappers for common procedures, to allow usage of slices and maybe allocators.

## Contributing

All contributions are welcome, I will try to merge them when I can!

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

## Migrating to `2.03.15`

Renames that are actually link fixes:

- `DSP_SetParameterf32`, `DSP_GetParameterf32`, `DSP_SetParameterb32` and `DSP_GetParameterb32` are
  now `DSP_SetParameterFloat`, `DSP_GetParameterFloat`, `DSP_SetParameterBool` and
  `DSP_GetParameterBool`. The old names emitted `FMOD_DSP_SetParameterf32`, and no such symbol
  exists in `fmod.dll`, so calling them from `main` failed with `LNK2019`.

Layout changes on win64:

- `fsbank.PROGRESSITEM.subSoundIndex` and `.threadIndex` are `i32`, not `int`. C `int` is 4 bytes
  while Odin `int` is 8, so the struct is 24 bytes instead of 32. Any code that walked the items
  returned by `FSBank_FetchNextProgressItem` with the old definition read past the end of every
  element.

Field name corrections, the layout was already right:

- In `ADVANCEDSETTINGS` the slot after `maxFADPCMCodecs` is `maxOpusCodecs` (it was named
  `maxPCMCodecs`), and the last field is `maxSpatialObjects` (it was named `maxOpusCodecs` a second
  time), following `fmod_common.h`.
- `fsbank.FORMAT_MAX` and `fsbank.FSBVERSION_MAX` were added, since the header uses them as bounds.

`SYSTEM_CALLBACK_*` was renumbered because FMOD dropped `SYSTEM_CALLBACK_MIDMIX`. All 17 values
match `fmod_common.h:115-131`, so anything that hard coded the old numbers needs a recompile.

Conventions to know when porting C samples:

- Anonymous unions are exposed through a group field named `data`, so `desc->floatdesc` becomes
  `desc.data.floatdesc` and `user_property->intvalue` becomes `user_property.data.i32value`.
- Where the C member is `type`, some structs use `type` directly (`USER_PROPERTY`) and others use
  `_type` (`DSP_PARAMETER_FLOAT_MAPPING`), because `type` is a predeclared identifier in Odin.

Both packages were checked on win64 against the 2.03.15 headers with MSVC: 586 procedures, their
return values, parameter counts and per parameter size, alignment and pointee width all match the C++
signatures, 63 comparable structs agree on size and field offsets (two anonymous unions are collapsed
into a `data` group on the Odin side, which is why the member counts differ), 464 enum members and
188 integer constants have 0 value errors, and the 12 shipped win64 binaries are byte identical to
the official ones by SHA256.

## TODO

- Add more examples
- Translate constants to enums and bit_sets.
- Implement odin wrappers for common procedures, to allow usage of slices and maybe allocators.

## Contributing

All contributions are welcome, I will try to merge them when I can!

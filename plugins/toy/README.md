# toy — OpenUSD `toy` file-format plugin

Scaffolded by `ost plugin new usd-fileformat toy --extension toy`.

## Layout

```
openstrata.plugin.yaml          bundle contract (identity, runtime range, provides, tests)
CMakeLists.txt                  builds libToyFileFormat.so into lib/
src/ToyFileFormat.{h,cpp}  the SdfFileFormat implementation
plugin/resources/toy/plugInfo.json   USD plugin registration
tests/fixtures/                 basic (valid) + invalid (negative) fixtures
```

## Workflow

```sh
ost plugin inspect toy     # Level 0: bundle structure
ost plugin build toy       # build the shared library into lib/
ost plugin doctor toy      # staged diagnostics (L0–L1; L2+ need a real runtime)
ost plugin test toy        # orchestrate the levels and write a report
```

Replace the body of `ToyFileFormat::Read` with your format's parser. The
scaffold emits an empty USDA layer so a `.toy` file opens as a valid
(if empty) stage out of the box.

## Co-hosting a Typed Schema

To compile a generated API schema into this same plugin, add `schema.usda` at the
bundle root and add `usd-schema:<TypeName>` to `provides` in
`openstrata.plugin.yaml`. `ost plugin build` runs `usdGenSchema` in the resolved
runtime environment, links the generated C++ API sources into this library, and
merges the generated `Types` into `plugin/resources/toy/plugInfo.json`.

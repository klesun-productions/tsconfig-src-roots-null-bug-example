See: https://github.com/microsoft/TypeScript/issues/43061

To reproduce the bug:

- Clone this repo
- Run `npm ci` to install typescript `v4.2.2`
- Run `npm run tsc`
- Profit

You should see following error:

```
klesun@kunkka:~/gits/tsconfig-src-roots-null-bug-example$ npm run tsc

> tsconfig-src-roots-null-bug-example@1.0.0 tsc /home/klesun/gits/tsconfig-src-roots-null-bug-example
> tsc

/home/klesun/gits/tsconfig-src-roots-null-bug-example/node_modules/typescript/lib/tsc.js:90526
                if (option.element.isFilePath && values.length) {
                                                        ^

TypeError: Cannot read property 'length' of undefined
    at convertToReusableCompilerOptionValue (/home/klesun/gits/tsconfig-src-roots-null-bug-example/node_modules/typescript/lib/tsc.js:90526:57)
    at convertToReusableCompilerOptions (/home/klesun/gits/tsconfig-src-roots-null-bug-example/node_modules/typescript/lib/tsc.js:90514:32)
    at getProgramBuildInfo (/home/klesun/gits/tsconfig-src-roots-null-bug-example/node_modules/typescript/lib/tsc.js:90452:22)
    at Object.newProgram.getProgramBuildInfo (/home/klesun/gits/tsconfig-src-roots-null-bug-example/node_modules/typescript/lib/tsc.js:90606:63)
    at Object.getProgramBuildInfo (/home/klesun/gits/tsconfig-src-roots-null-bug-example/node_modules/typescript/lib/tsc.js:87903:98)
    at emitBuildInfo (/home/klesun/gits/tsconfig-src-roots-null-bug-example/node_modules/typescript/lib/tsc.js:82111:32)
    at emitSourceFileOrBundle (/home/klesun/gits/tsconfig-src-roots-null-bug-example/node_modules/typescript/lib/tsc.js:82083:13)
    at forEachEmittedFile (/home/klesun/gits/tsconfig-src-roots-null-bug-example/node_modules/typescript/lib/tsc.js:81828:28)
    at Object.emitFiles (/home/klesun/gits/tsconfig-src-roots-null-bug-example/node_modules/typescript/lib/tsc.js:82057:9)
    at Object.emitBuildInfo (/home/klesun/gits/tsconfig-src-roots-null-bug-example/node_modules/typescript/lib/tsc.js:87913:33)
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! tsconfig-src-roots-null-bug-example@1.0.0 tsc: `tsc`
npm ERR! Exit status 1
npm ERR! 
npm ERR! Failed at the tsconfig-src-roots-null-bug-example@1.0.0 tsc script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /home/klesun/.npm/_logs/2021-03-03T15_22_07_045Z-debug.log
```

It is caused by combination of `rootDirs: null` and `composite: true` in `tsconfig.json`.

`null` is supposedly a valid value that can be used to remove overridden option from base config when you use `extends` - https://github.com/microsoft/TypeScript/pull/18058

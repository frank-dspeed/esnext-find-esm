# esnext-find-esm
trys to find a esm version of dependencys.


## What it does
It is using rollup under the hood to allow better tooling integration we could code the logic our self but rollup core already delivers the needed
logic around the es ast parser so we instrument that.

we take the rollup entrypoint (TypeScript support via rollup-typescript) and create a file with all dependencys that are cjs only and required via cjs require

## Why? 
to look which packages need upgrades and esm versions. As also allows you to know which packages should be imported

## Node Resolve Advanced Algo
the NodeJS resolve algo now gives import and export maps in the package.json priority over the main fild and package type
for us is only importent 

```js
const getImport = i => {
  // translates i to relative path if it is imported without extension
  // expected results package.json / index.js / tsconfig.json
}
const package = JSON.parse(readFile('./package.json'));
if (package.type === 'module') {
  // Eveything in here till the next deeper package.json is ESM
  // log the dir as esm
}

if (package.exports) {
  // show warning if not type module 
}

```

## Extras
The only solid path definition inside cjs and esm as also the browser file and url issues is the relativ path './' this gets handled right from
require as also import the absolute path for example is diffrent in the browser while a absolute browser path looks like '/path' that would be translated to domain.tld/path in nodejs it would lead to / the root of the filesystem with windows compat it would lead to C:\. so the browser absolute path is diffrent from the nodejs environment.

so as general rule of thumb always resolve to relative path before doing anything else. and most best would be to drop the nodejs require resolve algo with that method by simply referencing the right import.


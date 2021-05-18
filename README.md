# electron-builder-issue-3185

Simulates an environment with embedded submodules and their `node_modules`
folders not being copied into the build.

It's just the default Electron app created by `npx create-electron-app` but with
the renderer code moved into a nested `node_modules` folder.

## To Use

0. Clone and run `npm i`. It has the lastest verisons of `electron` and `electron-builder`.

1. Optionally test that everything works by running Electron normally. The repo has a
   `.vscode` for doing this in VSCode. The default `create-electron-app` should
   run fine.

2. Run `npm run build`. I only tested this on macOS Catalina, but it should be
   ok on Windows too. Note that my particular build gets automatically signed
   with my Apple developer ID.

3. Find the asar archive by navigating to `dist/mac/`, right clicking the `.app`
   and choosing "Show Package Contents" and going to `Contents/Resources/`.
   Unpack the asar by running

   `> npx asar extract app.asar out`

   I am not sure where the asar archive is on Windows, sorry.

4. See that `src/embedded-submodule/node_modules/` is missing

## package.json

In the package.json, under `build.files` I included two glob patterns. The first
is required when using custom `files`, and would ideally include nested
`node_modules` by default (like electron-forge does), but that would be a
big deviation from the current behaviour, which is now two years old.

The second pattern feels like it should work because it is so explicit in naming
the nested `node_modules`, but it does not. Hopefully this can be made to work.

Removing the `build.files` property altogether does not fix the issue.
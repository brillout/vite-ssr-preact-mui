Reproduction showcasing that Vite cannot be used with Preact + [MUI](https://mui.com/) because Vite fails to process CJS npm packages.

## Reproduction steps

```
git clone git@github.com:brillout/vite-ssr-preact-mui
cd vite-ssr-preact-mui/
pnpm install
pnpm run dev
```

Then go to [localhost:5173](http://localhost:5173) and observe this error:

```
11:42:09 AM [vite] Error when evaluating SSR module /node_modules/.pnpm/@mui+icons-material@5.14.3_@mui+material@5.14.5_react@18.2.0/node_modules/@mui/icons-material/Instagram.js:
|- ReferenceError: require is not defined
    at eval (/home/rom/tmp/vite-ssr-preact-mui/node_modules/.pnpm/@mui+icons-material@5.14.3_@mui+material@5.14.5_react@18.2.0/node_modules/@mui/icons-material/Instagram.js:5:30)
    at instantiateModule (file:///home/rom/tmp/vite-ssr-preact-mui/node_modules/.pnpm/vite@4.4.9/node_modules/vite/dist/node/chunks/dep-df561101.js:55974:15)

11:42:09 AM [vite] Error when evaluating SSR module /src/app.jsx: failed to import "/node_modules/.pnpm/@mui+icons-material@5.14.3_@mui+material@5.14.5_react@18.2.0/node_modules/@mui/icons-material/Instagram.js"
|- ReferenceError: require is not defined
    at eval (/home/rom/tmp/vite-ssr-preact-mui/node_modules/.pnpm/@mui+icons-material@5.14.3_@mui+material@5.14.5_react@18.2.0/node_modules/@mui/icons-material/Instagram.js:5:30)
    at instantiateModule (file:///home/rom/tmp/vite-ssr-preact-mui/node_modules/.pnpm/vite@4.4.9/node_modules/vite/dist/node/chunks/dep-df561101.js:55974:15)

11:42:09 AM [vite] Error when evaluating SSR module /src/entry-server.jsx: failed to import "/src/app.jsx"
|- ReferenceError: require is not defined
    at eval (/home/rom/tmp/vite-ssr-preact-mui/node_modules/.pnpm/@mui+icons-material@5.14.3_@mui+material@5.14.5_react@18.2.0/node_modules/@mui/icons-material/Instagram.js:5:30)
    at instantiateModule (file:///home/rom/tmp/vite-ssr-preact-mui/node_modules/.pnpm/vite@4.4.9/node_modules/vite/dist/node/chunks/dep-df561101.js:55974:15)

ReferenceError: require is not defined
    at /home/rom/tmp/vite-ssr-preact-mui/node_modules/.pnpm/@mui+icons-material@5.14.3_@mui+material@5.14.5_react@18.2.0/node_modules/@mui/icons-material/Instagram.js:3:30
    at instantiateModule (file:///home/rom/tmp/vite-ssr-preact-mui/node_modules/.pnpm/vite@4.4.9/node_modules/vite/dist/node/chunks/dep-df561101.js:55974:15)
```

## Additional context

To be able to use Preact with MUI, the user needs to add MUI to `ssr.noExternal` so that `vite.config.js#resolve.alias.react` applies to the MUI code (which is needed for Preact's compat layer to work).

This means that Vite needs to be able to process CJS npm packages for being able to use Preact with MUI.

See [this discussion](https://github.com/brillout/vite-plugin-ssr/discussions/1071#discussioncomment-6728731) with the original user reporting this issue.

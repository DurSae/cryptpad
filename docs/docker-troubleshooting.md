# CryptPad Docker troubleshooting notes

## Common causes of the registration page hanging

- The stock container expects `docker-entrypoint.sh` to create or update
  `/cryptpad/config/config.js` based on `CPAD_MAIN_DOMAIN`,
  `CPAD_SANDBOX_DOMAIN`, and `CPAD_CONF`. Overriding the entrypoint means those
  variables are not applied, and the config file may not be created or updated.
- The server must be built once (`npm run build`) before `node server.js` is
  started. Skipping the entrypoint also skips that build step.
- The main and sandbox origins must be distinct (typically `:3000` and `:3001`),
  and the URLs must match what clients use to access the instance.

## Recommendation

Use the default entrypoint (or invoke `docker-entrypoint.sh`) so the config and
build steps run, and only override the entrypoint if you manually create
`/cryptpad/config/config.js` and run `npm run build`.

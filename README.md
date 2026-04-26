# rightdocuments-client

Crystal SDK for the [RightDocuments](https://rightdocuments.com) API. Generated from `swagger/v1/swagger.yaml` via [openapi-generator](https://openapi-generator.tech/) on every upstream API change.

**This SDK is auto-generated.** Do not edit files under `src/rightdocuments-client/` — they are overwritten on each release.

## Install

```yaml
# shard.yml
dependencies:
  rightdocuments-client:
    github: aluminumio/rightdocuments-sdk-crystal-lang
    version: ~> 0.1
```

```sh
shards install
```

## Use (with `oauth-device-flow.cr` for auth)

```crystal
require "rightdocuments-client"
require "oauth-device-flow"

oauth = OAuth::DeviceFlow::Client.new(
  base_url:  "https://rightdocuments.com",
  client_id: ENV["RIGHTDOCUMENTS_CLIENT_ID"],
  store:     OAuth::DeviceFlow::NetrcStore.new(machine: "rightdocuments.com"),
)
oauth.authenticate(scope: "documents:read documents:write") unless oauth.authenticated?

config = RightDocuments::Configuration.default
config.host         = "rightdocuments.com"
config.access_token = oauth.access_token

documents = RightDocuments::DocumentsApi.new(config).list_documents
```

`config.access_token` is read fresh on each request, so reassigning it before each call (or wiring it as a property delegate to `oauth.access_token`) gives transparent auto-refresh.

## Release Cadence

Each push of `swagger/v1/swagger.yaml` to `main` on `aluminumio/rightdocuments` triggers a regen here. New patch tag (`v0.1.X`) cut on every successful regen with diffs. The CLI (`aluminumio/rightdocuments-cli`) is automatically dispatched to rebuild on each release.

Manual regeneration: `gh workflow run generate.yml`.

## License

MIT

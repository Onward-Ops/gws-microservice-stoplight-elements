# NestJS Module for Stoplight Elements OpenAPI

[Stoplight Elements](https://stoplight.io/open-source/elements) Interactive API Docs for your NestJS app.
Inspired by [NestJS-Redoc](https://github.com/mxarc/nestjs-redoc)

It's almost a drop in replacement for you current swagger UI, you only need to import this package and modify any settings you may want to change

> Maintained by Onward Ops as `@onwardops/nestjs-stoplight-elements`, a fork of [nestjs-stoplight-elements](https://github.com/nomansheikh/nestjs-stoplight-elements) by Noman Sheikh (MIT).

## Installation

`npm i @onwardops/nestjs-stoplight-elements`

## How to use

You need to install the [Swagger Module](https://github.com/nestjs/swagger) first if you want to get definitions updated with your project.

In future versions you will be able to pass a URL parameter as document, but for the moment you need this document object from the swagger module

```typescript
const options = new DocumentBuilder()
  .setTitle('Look, i have a title')
  .setDescription('A very nice description')
  .setBasePath('/api/v1')
  .build();
const document = SwaggerModule.createDocument(app, options);
```

Then add the following example code.

**Note**: All properties are optional, if you don't specify a title we will fallback to the one you used in your DocumentBuilder instance.

```typescript
// Instead of using SwaggerModule.setup() you call this module
const options = {
  layout: 'sidebar',
  // see available options below
};

const StoplightElements = new StoplightElementsModule(app, document, options);
await StoplightElements.start('/docs');
```

## Available options

From [Stoplight Elements Docs](https://meta.stoplight.io/docs/elements/ZG9jOjUxMDQ5MDY0-elements-configuration-options)

- `basePath` - Helps when using router: 'history' but docs are in a subdirectory like https://example.com/docs/api.
- `hideInternal` - Pass "true" to filter out any content which has been marked as internal with x-internal.
- `hideTryIt` - Pass true to hide the "Try It" panel (the interactive API console).
- `tryItCorsProxy` - Pass the URL of a CORS proxy used to send requests to TryIt. The provided url is prepended to the URL of an actual request.
- `tryItCredentialsPolicy` - Use to fetch the credential policy for the Try It feature. Options are: `omit` (default), `include`, and `same-origin`.
- `layout` - There are two layouts for Elements:
  - `sidebar` - (default) Three-column design.
  - `stacked` - Everything in a single column, making integrations with existing websites that have their own sidebar or other columns already.
- `logo` - URL to an image that will show as a small square logo next to the title, above the table of contents.
- `router` - Determines how navigation should work:
  - `history` - (default) uses the HTML5 history API to keep the UI in sync with the URL.
  - `hash` - uses the hash portion of the URL (i.e. window.location.hash) to keep the UI in sync with the URL.
  - `memory` - keeps the history of your "URL" in memory (does not read or write to the address bar).
  - `static` - renders using the StaticRouter which can help rendering pages on the server.

## Releasing

Releases are fully automated. Every merge to `main` runs
[`.github/workflows/publish.yml`](.github/workflows/publish.yml), which:

1. Builds and tests the package.
2. Reads the latest version published on npm and computes the next patch version.
3. Publishes it to npm as `@onwardops/nestjs-stoplight-elements` via
   [npm trusted publishing](https://docs.npmjs.com/trusted-publishers) (OIDC).

There is no manual step: no tags, GitHub Releases, or tokens to create or rotate.

**Notes for maintainers**

- **npm is the source of truth for versions.** The workflow does not commit a
  version bump or push a tag back to the repo, so the `version` in `package.json`
  is not authoritative; check the published version on npm.
- It does not write to the repository at all, because the organization enforces
  read-only `GITHUB_TOKEN` permissions. It derives the next version from npm, so
  it needs no elevated permission and no push credential.
- Every merge to `main` publishes a new patch version, including docs- or CI-only
  changes. Batch trivial commits if you want to avoid version churn.
- Publishing uses OIDC, so there is no `NPM_TOKEN`. It requires the trusted
  publisher on npm to be configured for this repository and the `publish.yml`
  workflow.

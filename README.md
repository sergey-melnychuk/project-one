project-one
===========

Install [wrangler](https://developers.cloudflare.com/workers/wrangler/get-started/):

```
$ wrangler --version
 ⛅️ wrangler 2.1.10
--------------------
```

Minimum *working* setup for CloudFlare Workers.

```
cat >> Cargo.toml << EOF
[package]
name = "project-one"
version = "0.1.0"
edition = "2021"

[lib]
crate-type = ["cdylib", "rlib"]

[dependencies]
wasm-bindgen = "0.2"
worker = "0.0.11"
EOF
```

```
cat >> wrangler.toml << EOF
name = "project-one"
compatibility_date = "2022-10-10"

main = "build/worker/shim.mjs"

[build]
command = "worker-build --release"
EOF
```

```
mkdir src
cat >> src/lib.rs << EOF
use worker::*;

#[event(fetch)]
pub async fn main(_req: Request, _env: Env, _ctx: Context) -> Result<Response> {
  Response::empty()
}
EOF
```

Prepare tooling and build:

```
cargo install worker-build
worker-build --release
```

Run locally and publish:

```
export CLOUDFLARE_API_TOKEN=<snip>
export CLOUDFLARE_ACCOUNT_ID=<snip>
wrangler dev
wrangler publish
```


# Scripts

Scripts are where the Workers code itself is stored and uploaded to the edge.

## Script object specification

<Definitions>

- `id`
  - The name of the script. Script names must:

    - start with a letter,
    - end with a letter or digit,
    - include only letters, digits, underscore, and hyphen,
    - and be 63 or fewer characters.

- `etag`
  - Hashed script content; can be used in an If-None-Match header on update.

- `script`
  - Raw script content, as a string

- `size`
  - [Size of script](/platform/limits) in bytes.

- `modified_on`
  - ISO_8601 timestamp of when the script was last modified.

</Definitions>

## Resource Bindings

Resource bindings allow you to attach resources, such as secrets, or KV namespaces to your Worker script. If you are including Resources in your Worker, you need to specify their Bindings as a part of your upload. This API uses a multipart form, rather than straight bytes, to send its data.

All bindings have the following properties: 
<Definitions>

- `name`
  - The name of the binding, which will be exposed as the name a global variable within the script. 
- `type`
  - The type of binding, can be one of: 
    - `namespace_id`
    - `secret`
    - `wasm_module`
    - `plain_text`

</Definitions>

### KV namespace bindings

If your Worker uses a [KV namespace](/storage/overview/), you will want to add a `kv_namespace` binding object to the `"bindings"` array in `metadata.json`.

KV bindings must include a `namespace_id`. The `namespace_id` value should correspond to the identifier associated with the namespace you want to use. 

### WebAssembly module bindings

If your Worker uses a [WebAssembly Module](/examples/boilerplates/rustwasm/), you will want to add a `wasm_module` binding object to the `bindings` array in `metadata.json`. 

You will also need to add your WebAssembly module as a file part to your request, and name it the same as the `part` field in the binding. 

### Secret bindings

If your Worker script uses [secrets](reference/apis/environment-variables#secrets), add a corresponding `secret_text` binding to the `bindings` array in `metadata.json`. 

Secret bindings must include a `text` property. The `text` value includes text, such as an API key you want to store.

<Aside>
Secrets are persisted between deploys of a Worker. You only need to include secrets in API calls when you are adding or changing the secret’s content.*
</Aside>

## Plain text bindings

If your Worker uses plain text environment variables, you will want to add a `plain_text` binding object for each one to the `bindings` array in `metadata.json`.

Secret bindings must include a `text`. The `text` value includes text, such as an API key you want to store.

## See also
- [Workers Scripts API](https://api.cloudflare.com/#worker-script-properties)
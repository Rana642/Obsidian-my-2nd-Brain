---
name: kb-mcp-upload-tools
description: "KB file intake for Claude sessions with a shell — kb_create_direct_upload (curl PUT + finish POST), kb_update_asset, kb_upsert_products (3274e00, 2026-10-01); why they exist"
metadata:
  node_type: memory
  type: project
  originSessionId: 07de83ef-3bef-4c52-b4bc-5471cb6b2146
  modified: 2026-10-01T12:10:23.995Z
---

On 2026-10-01 another Claude session, which was adding REEFCO's files, listed the KB file pain points:
- `kb_add_asset` only takes a public URL or base64 (≤3 MB).
- The remote MCP server can't read local paths.
- `kb_create_upload_link` needs a browser for every batch.

Shoaib asked for an MCP that makes this unblocked. Built in 3274e00:

- **`kb_create_direct_upload`** `{projectName, files[≤20]: {filename, kind, title, productName?, notes?, makePrimary?, sort?}}`
  - Returns one command per file: `curl -sSf -X PUT -H "Content-Type: …" -T "FILE" "<presigned R2 PUT, 30 min>" && curl -sS -X POST "https://adsbyshoaib.com/api/kb/direct-upload?t=<signed token>"`.
  - The finish route (`app/api/kb/direct-upload/route.ts`):
    - HEADs the upload and checks the size (25 MB cap);
    - sniffs the bytes against the kind;
    - moves the file from `knowledge/<p>/uploads/<uuid>-name` to `knowledge/<p>/assets/<uuid>.<ext>`;
    - registers it and returns JSON with the permanent URL.
  - Repeating the finish step returns the same asset.
  - The token is signed in the `kb-direct` domain with MCP_OAUTH_SIGNING_KEY. Kinds also include `literature_page` (with `sort`) and `literature_pdf`.
- **`kb_update_asset`** changes title, notes, sort, kind, product link or main photo.
  - Notes are appended by default, so "APPROVED by Shoaib" approval notes survive.
  - `makePrimary` keeps the old main photo as a "previous main photo" `product_image` row instead of deleting it. `registerAsset` *does* delete the old primary — beware.
- **`kb_upsert_products`**: a batch of up to 50 products, reported per item.
- `kb_get_product` now also lists `reference_image` links with their notes (pack photos, label print files).

Not built: server-side PDF page rendering or Arabic OCR. The session should render pages locally and upload them as `literature_page`.

Testing:
- The tools can't be exercised from a session whose ABS tool list was cached before the deploy; it needs a new chat or reconnect.
- They can't be tested locally either, because MCP_OAUTH_SIGNING_KEY isn't on the office PC.

Related: [[tadpharma-knowledge-base]], [[meezab-knowledge-base]].

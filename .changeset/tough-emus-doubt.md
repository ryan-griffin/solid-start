---
"@solidjs/start": patch
---

fix: guard handleShellCompleteRedirect against already-handled responses

When a redirect is triggered during SSR rendering (e.g., via navigate()
from a route component), both the inline redirect check and the
onCompleteShell callback (handleShellCompleteRedirect) may attempt to
set the Location header on the same response. If the response has
already been sent or headers have already been flushed, setHeader
throws ERR_HTTP_HEADERS_SENT. This commonly manifests in Bun due to
differences in its JavaScript runtime behavior.

Adds an e.handled guard to skip setting headers when the response
is already handled.

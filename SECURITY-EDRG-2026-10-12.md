# Research-demo security boundary — October 12, 2026

The composite demo's get_shell hook previously registered an arbitrary
model-requested Linux shell command. It is now unregistered and always raises
PermissionError, rather than exposing unrestricted host command execution.
Replace it only with operator-enrolled, fixed-command, sandboxed execution via
a governed adapter. Python/tool execution in other demo components is outside
the scope of this change and is NOT qualified for untrusted agent input.

The GLM4V server previously downloaded arbitrary image URLs with TLS
verification disabled. Arbitrary remote URLs are now rejected by the model
request path. JPEG and PNG data URLs remain supported subject to encoded byte,
decoded byte and pixel-count limits. Images obtained from an external source
must go through a separate SSRF-safe image proxy/broker with DNS/IP pinning,
redirect denial, MIME/size/dimension limits and explicit authorization before
being handed to the decoder. This change intentionally narrows compatibility.

Tests: tests/test_security_boundaries.py, syntax compilation.
GPU-model/server end-to-end behavior remains unverified without the research
dependencies; do not claim production readiness from these unit tests.

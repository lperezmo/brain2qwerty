# Security-maintained environment

The dependency pins now describe a security-maintained environment, not the
original published experiment environment. Model configurations are unchanged,
but dependency changes can affect numerics and performance. Revalidate metrics
before comparing results with the papers.

## Framework changes

- Torch 2.6.0 → 2.13.0 and torchvision 0.21.0 → 0.28.0.
- Torchaudio 2.6.0 → 2.11.0; CPU import and masking smoke checks pass.
- Transformers 4.52.4 → 5.16.1, with compatible Hub/tokenizer dependencies.
- Lightning and pytorch-lightning are aligned to 2.6.5.
- The Linux lock now resolves CUDA 13 runtime dependencies, replacing CUDA 12.4.
  Verify GPU driver/runtime compatibility before deploying.
- Vulnerable GitPython, NLTK, Pillow, filelock, fonttools, IDNA, orjson,
  protobuf, pyarrow, Requests, setuptools, ujson, urllib3, and Click pins are
  refreshed. psutil moves to 5.9.8 for Python 3.12 wheel availability.

## Validation and limits

The complete Linux/Python 3.12 dependency closure resolves successfully. The
existing 15 CPU tests and Ruff checks pass with the updated Python packages and
CPU builds of Torch, torchvision, and torchaudio. The optional KenLM package and
CUDA-only packages were omitted from that CPU test environment. Full training,
GPU execution, optional KenLM decoding, and published metrics were not validated.

The PyPI advisory scan still reports **PYSEC-2026-3624 / CVE-2026-58659** against
Lightning 2.6.5. No patched published release is listed. Its checkpoint loader
can import an attacker-controlled `_instantiator`; `weights_only=True` is not a
sufficient mitigation. Only load checkpoints created within a trusted workflow.
This dependency finding remains unresolved and has not been dismissed.

GitHub Dependabot/CodeQL alert endpoints were unavailable through the GitHub
plugin used for this change. Recheck the repository security tab after the PR is
merged and scanning completes; package-feed checks do not prove GitHub alert
closure.

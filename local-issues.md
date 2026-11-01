
## Issue - Default SDPA Attention Path on gfx1151

On my machine:
- default / SPDA -> CUDA error: invalid argument
- attn_implementation="eager" -> OK on cuda:0

Run without code changs

````
source .venv/bin/activate
export TRANSFORMERS_ATTN_IMPLEMENTATION=eager
python ingestion/build_index.py
````

Verify

````
TRANSFORMERS_ATTN_IMPLEMENTATION=eager python - <<'py'
from sentence_transformers import SentenceTransformer
m = SentenceTransformer(
    "BAAI/bge-base-en-v1.5",
    device="cuda",
    model_kwargs={"attn_implementation": "eager"},
)
print(m.device, m.encode(["hi"], normalize_embeddings=True).shape)
py
````


Permanent fix

In helix/retrieval/index.py and helix/memory/episodic.py

````
SentenceTransformer(
    EMBED_MODEL_NAME,
    device="cuda",
    model_kwargs={"attn_implementation": "eager"},
)
````
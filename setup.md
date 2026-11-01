````
cd /home/dmaxim/storage/github.com/mxinformatics/self-improving-agents
deactivate 2>/dev/null
rm -rf .venv
uv venv .venv --python /usr/bin/python3.13 --system-site-packages
source .venv/bin/activate
python -c "import torch; print(torch.__version__, torch.version.hip, torch.cuda.is_available())"
grep -v 'sentence-transformers' requirements.txt > requirements-notorch.txt
uv pip install -r requirements-notorch.txt
uv pip install 'sentence-transformers>=3.0.0' --no-deps
uv pip install transformers huggingface-hub tokenizers safetensors scikit-learn scipy tqdm numpy Pillow typing-extensions filelock packaging

uv pip uninstall -y torch torchvision torchaudio 2>/dev/null || true
uv pip freeze | grep -i '^nvidia-' | cut -d= -f1 | xargs -r uv pip uninstall -y
python -c "import torch; print(torch.__version__, torch.version.hip, torch.cuda.is_available(), torch.cuda.get_device_name(0) if torch.cuda.is_available() else None, torch.__file__)"
python -c "from sentence_transformers import SentenceTransformer; m = SentenceTransformer('BAAI/bge-base-en-v1.5'); print(m.device)"
````

Warnings from last line

````
uv pip install 'numexpr>=2.11.0' 'bottleneck>=1.5.0'

python -c "from sentence_transformers import SentenceTransformer; m = SentenceTransformer('BAAI/bge-base-en-v1.5'); print(m.device)"

````
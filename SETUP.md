# Setup Instructions for IBM AI Engineering Notebooks

This guide helps you set up your environment to run the AI Engineering notebooks, especially the Tokenization notebook which has specific dependencies.

## Quick Start

### Option 1: Using `uv` (Recommended)

```bash
# Install all dependencies from pyproject.toml
uv pip install -e .

# Download required spaCy language models
uv run python -m spacy download en_core_web_sm
uv run python -m spacy download de_core_news_sm
```

### Option 2: Using `pip`

```bash
# Install from requirements.txt
pip install -r requirements.txt

# Download required spaCy language models
python -m spacy download en_core_web_sm
python -m spacy download de_core_news_sm
```

### Option 3: Using `conda`

```bash
# Create a new conda environment
conda create -n ibm-ai python=3.12

# Activate the environment
conda activate ibm-ai

# Install PyTorch first (from conda-forge or pytorch channel)
conda install -c pytorch pytorch::pytorch -c pytorch

# Install remaining dependencies
pip install -r requirements.txt

# Download spaCy models
python -m spacy download en_core_web_sm
python -m spacy download de_core_news_sm
```

## Dependency Explanation

### Core NLP Libraries

- **nltk** (>= 3.9.2): Natural Language Toolkit for word tokenization
- **spacy** (>= 3.8.11): Industrial-strength NLP library
- **transformers** (== 4.42.1): Hugging Face transformers for BERT/XLNet

### PyTorch Stack (Critical for Compatibility)

- **torch** (>= 2.2.0, < 3.0.0): Deep learning framework
- **torchtext** (>= 0.18.0, < 1.0.0): PyTorch text utilities
  - ⚠️ **Important**: torchtext 0.18.0+ requires torch 2.2.0+
  - Earlier versions (0.17.2) are incompatible and cause C++ library errors

### Other Dependencies

- **numpy** (== 1.26): Numerical computing
- **scikit-learn** (== 1.6.0): Machine learning utilities
- **sentencepiece** (>= 0.2.1): Byte-pair encoding for tokenization
- **matplotlib** (== 3.9.3): Plotting and visualization

## Troubleshooting

### Issue: `OSError` with torchtext C++ library

**Error message:**
```
OSError: /path/to/torchtext/lib/libtorchtext.so: undefined symbol: ...
```

**Solution:**
This occurs when torch and torchtext versions are incompatible. Reinstall with compatible versions:

```bash
# Remove old versions
pip uninstall torch torchtext -y

# Install compatible versions
pip install torch>=2.2.0 torchtext>=0.18.0
```

### Issue: spaCy model not found

**Error message:**
```
OSError: [E050] Can't find model 'en_core_web_sm'
```

**Solution:**
Download the required spaCy models:

```bash
python -m spacy download en_core_web_sm
python -m spacy download de_core_news_sm
```

### Issue: Import errors for transformers or specific modules

**Solution:**
Ensure all packages are correctly installed:

```bash
# Check installed versions
pip list | grep -E "nltk|spacy|torch|transformers"

# If missing, reinstall
pip install -r requirements.txt
```

### Issue: Running on Apple Silicon (M1/M2/M3)

PyTorch on Apple Silicon requires special installation:

```bash
# Use conda for ARM64 compatibility
conda create -n ibm-ai python=3.12
conda activate ibm-ai
conda install pytorch::pytorch -c pytorch
pip install -r requirements.txt
```

## Verification

After installation, verify everything is set up correctly:

```python
# Test imports
import nltk
import spacy
import torch
import transformers
import torchtext

print(f"torch version: {torch.__version__}")
print(f"torchtext version: {torchtext.__version__}")

# Test spaCy model
nlp = spacy.load("en_core_web_sm")
doc = nlp("This is a test.")
print(f"Tokens: {[token.text for token in doc]}")
```

## Virtual Environment (Optional but Recommended)

If not using conda:

```bash
# Create virtual environment
python -m venv venv

# Activate (Linux/Mac)
source venv/bin/activate

# Activate (Windows)
venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

## Using the Notebooks

Once setup is complete, launch Jupyter:

```bash
# Using uv
uv run jupyter notebook

# Using pip
jupyter notebook
```

Navigate to the notebooks directory and open the desired notebook.

## Additional Resources

- [PyTorch Documentation](https://pytorch.org/docs/)
- [spaCy Documentation](https://spacy.io/)
- [NLTK Book](https://www.nltk.org/book/)
- [Hugging Face Transformers](https://huggingface.co/docs/transformers/)

## Need Help?

- Check the Troubleshooting section above
- Verify you're using Python 3.12 or higher
- Ensure all spaCy models are downloaded
- Check that torch and torchtext versions match (2.2+ for both)

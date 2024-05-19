# Hugging Face Repository Downloader

This script provides a convenient way to download Hugging Face models or datasets directly as a zip file, without the need for git. It also includes a progress bar to monitor the download. No need to worry about bigger files and git lfs.

## Features

- **Simplified Download:** Easily download Hugging Face repositories without Git.
- **Progress Tracking:** Visual progress bar for monitoring download status.
- **Error Handling:** Robust exception handling for common issues.
- **Flexible:** Works with both models and datasets.
- **Customizable:** Specify the output directory.
- **Handles Large Files:**  No need for Git LFS for large file downloads.

## Installation

1. **Install required libraries:**
   ```bash
   pip install huggingface_hub tqdm
   ```

## Usage

1. **Obtain your Hugging Face token:**
   - Go to your Hugging Face profile settings and generate a token with read access.
   - Replace `"YOUR_TOKEN_HERE"` in the script with your actual token.

2. **Run the script:**
   ```bash
   python huggingface-dl.py <repo_id> --type <model or dataset>
   ```

   - `<repo_id>`: The Hugging Face repository ID (e.g., "google/flan-t5-xxl").
   - `--type`: (Optional) Specify whether it's a model or dataset. Defaults to "model".

   **Example:**
   ```bash
   python huggingface-dl.py HuggingFaceFW/fineweb --type dataset
   ```

## Code

```python
from huggingface_hub import snapshot_download, login, HfApi
import os
import argparse
from tqdm.auto import tqdm

token = "YOUR_TOKEN_HERE"

login(token=token)

def download_with_progress(repo_id, local_dir, repo_type="model"):
    try:
        api = HfApi()
        repo_info = None
        
        # Fetch repo info based on the specified type
        if repo_type == "dataset":
            repo_info = api.dataset_info(repo_id=repo_id)
        elif repo_type == "model":
            repo_info = api.model_info(repo_id=repo_id)
        else:
            raise ValueError("Invalid repo_type. Please choose 'model' or 'dataset'.")

        files_to_download = repo_info.siblings

        # Calculate total size from files
        total_size_bytes = 0
        for file in files_to_download:
            if file.size is not None:
                total_size_bytes += file.size

        # Download files with progress
        with tqdm(total=total_size_bytes, unit="B", unit_scale=True, desc="Downloading") as t:
            for file in files_to_download:
                snapshot_download(repo_id=repo_id, local_dir=local_dir, repo_type=repo_type, local_dir_use_symlinks=False, allow_patterns=[file.rfilename])
                t.update(file.size if file.size is not None else 0)

    except ValueError as e:
        print(f"Error: {e}")

# --- Argument Parsing ---
parser = argparse.ArgumentParser(description="Download a Hugging Face model or dataset as a zip.")
parser.add_argument("repo_id", type=str, help="The Hugging Face repository ID (e.g., 'google/flan-t5-xxl')")
parser.add_argument("--type", choices=["model", "dataset"], default="model", help="The type of repository (model or dataset)")
args = parser.parse_args()

repo_id = args.repo_id
repo_type = args.type

# remove all / from repo_id
repo_name = repo_id.replace("/", "_")
local_dir = "path/to/output/folder"
local_path = os.path.join(local_dir, repo_name)

os.makedirs(local_path, exist_ok=True)

download_with_progress(repo_id, local_path, repo_type)

```

## Important Notes

- **Token Security:** Keep your Hugging Face token confidential. Never share it publicly or commit it to version control.
- **Rate Limits:** Hugging Face has rate limits on API usage. If you encounter issues, try using a valid token or wait for a while before retrying.

## Credits

This script is inspired by the `huggingface_hub` library, which provides the core functionality for interacting with the Hugging Face Hub. Please refer to the [huggingface_hub repository](https://github.com/huggingface/huggingface_hub) for more details.

## License

This script is licensed under the MIT License[LICENSE].
```


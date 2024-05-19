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
# ... (your script code here)
```

## Important Notes

- **Token Security:** Keep your Hugging Face token confidential. Never share it publicly or commit it to version control.
- **Rate Limits:** Hugging Face has rate limits on API usage. If you encounter issues, try using a valid token or wait for a while before retrying.

## Credits

This script is inspired by the `huggingface_hub` library, which provides the core functionality for interacting with the Hugging Face Hub. Please refer to the [huggingface_hub repository](https://github.com/huggingface/huggingface_hub) for more details.

## License

This script is licensed under the MIT License[LICENSE].
```


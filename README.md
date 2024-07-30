from google.colab import drive
import os
from collections import defaultdict

# Mount Google Drive
drive.mount('/content/drive')

def get_directory_size(path):
    """Calculate the total size of the given directory."""
    total_size = 0
    for dirpath, dirnames, filenames in os.walk(path):
        for filename in filenames:
            try:
                filepath = os.path.join(dirpath, filename)
                total_size += os.path.getsize(filepath)
            except FileNotFoundError:
                continue
    return total_size

def analyze_filesystem(path, size_threshold):
    """Analyze the filesystem structure and usage."""
    total_size = get_directory_size(path)
    print(f"Total size of '{path}': {total_size / (1024 * 1024):.2f} MB\n")

    large_files = []
    directory_sizes = defaultdict(int)

    for dirpath, dirnames, filenames in os.walk(path):
        dir_size = 0
        for filename in filenames:
            try:
                filepath = os.path.join(dirpath, filename)
                file_size = os.path.getsize(filepath)
                dir_size += file_size
                if file_size > size_threshold:
                    large_files.append((filepath, file_size))
            except FileNotFoundError:
                continue
        directory_sizes[dirpath] += dir_size

    print("Large Files:")
    for filepath, file_size in large_files:
        print(f"{filepath}: {file_size / (1024 * 1024):.2f} MB")

    print("\nDirectory Sizes:")
    for dirpath, dir_size in sorted(directory_sizes.items(), key=lambda x: x[1], reverse=True):
        if dir_size > size_threshold:
            print(f"{dirpath}: {dir_size / (1024 * 1024):.2f} MB")

if __name__ == "__main__":
    # Set the path to the directory you want to analyze
    # For example, to analyze the root of the Google Drive:
    directory_to_analyze = "/content/drive/My Drive/"
    
    # Set the size threshold for large files and directories (in bytes)
    size_threshold = 50 * 1024 * 1024  # 50 MB
    analyze_filesystem(directory_to_analyze, size_threshold)

import os
from collections import defaultdict

def get_size(start_path='.'):
    total_size = 0
    for dirpath, dirnames, filenames in os.walk(start_path):
        for f in filenames:
            fp = os.path.join(dirpath, f)
            # Skip if it is symbolic link
            if not os.path.islink(fp):
                total_size += os.path.getsize(fp)
    return total_size

def find_large_files_and_directories(start_path='.', num_results=10):
    file_sizes = []
    dir_sizes = defaultdict(int)

    # Walk through directory
    for dirpath, dirnames, filenames in os.walk(start_path):
        # Calculate total size for current directory
        dir_total = 0
        for f in filenames:
            fp = os.path.join(dirpath, f)
            try:
                size = os.path.getsize(fp)
            except OSError:
                size = 0
            file_sizes.append((size, fp))
            dir_total += size
        dir_sizes[dirpath] += dir_total

    # Sort and retrieve top results
    largest_files = sorted(file_sizes, key=lambda x: x[0], reverse=True)[:num_results]
    largest_dirs = sorted(dir_sizes.items(), key=lambda x: x[1], reverse=True)[:num_results]

    # Display results
    print("\nLargest Files:")
    for size, filepath in largest_files:
        print(f"{size / (1024 ** 2):.2f} MB\t{filepath}")

    print("\nLargest Directories:")
    for dirpath, size in largest_dirs:
        print(f"{size / (1024 ** 2):.2f} MB\t{dirpath}")

if __name__ == "__main__":
    start_path = input("Enter the directory to analyze (default is current directory): ").strip()
    num_results = input("Enter the number of top results to display (default is 10): ").strip()
    
    start_path = start_path if start_path else '.'
    num_results = int(num_results) if num_results else 10
    
    find_large_files_and_directories(start_path, num_results)

import os
from collections import Counter

def analyze_text_file(C:/Users/HI/Desktop/tanu):
    try:
        with open(file_path, 'r', encoding='utf-8') as file:
            content = file.read()
        
        # Number of lines
        num_lines = len(content.splitlines())
        
        # Number of words
        words = content.split()
        num_words = len(words)
        
        # Number of characters
        num_chars = len(content)
        
        # Frequency of each word (excluding case)
        word_freq = Counter(word.lower() for word in words)
        
        return {
            "lines": num_lines,
            "words": num_words,
            "characters": num_chars,
            "word_frequency": word_freq
        }
    
    except FileNotFoundError:
        return "File not found. Please provide a valid file path."
    except Exception as e:
        return f"An error occurred: {e}"

def analyze_directory(directory_path):
    if not os.path.isdir(directory_path):
        return "Invalid directory path."

    files_info = []

    for root, _, files in os.walk(directory_path):
        for file_name in files:
            file_path = os.path.join(root, file_name)
            file_size = os.path.getsize(file_path)
            file_info = {
                "file_name": file_name,
                "file_path": file_path,
                "file_size": file_size
            }

            # Analyze if the file is a text file
            if file_name.lower().endswith('.txt'):
                analysis = analyze_text_file(file_path)
                if isinstance(analysis, dict):
                    file_info.update(analysis)
                else:
                    file_info["error"] = analysis

            files_info.append(file_info)

    return files_info

# Example usage
directory_path = '/path/to/directory'  # Replace with your directory path
result = analyze_directory(directory_path)

for file_info in result:
    print(f"File: {file_info['file_name']}")
    print(f"Path: {file_info['file_path']}")
    print(f"Size: {file_info['file_size']} bytes")
    if 'lines' in file_info:
        print(f"Lines: {file_info['lines']}")
        print(f"Words: {file_info['words']}")
        print(f"Characters: {file_info['characters']}")
        print(f"Word Frequency: {file_info['word_frequency']}")
    if 'error' in file_info:
        print(f"Error: {file_info['error']}")
    print("\n")

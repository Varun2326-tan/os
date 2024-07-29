import platform
import os
import psutil
import cpuinfo

def get_system_info():
    # OS and kernel information
    os_info = platform.uname()
    system_info = {
        "System": os_info.system,
        "Node Name": os_info.node,
        "Release": os_info.release,
        "Version": os_info.version,
        "Machine": os_info.machine,
        "Processor": os_info.processor,
    }

    # Detailed processor information
    cpu_info = cpuinfo.get_cpu_info()
    cpu_details = {
        "CPU": cpu_info.get("brand_raw", "Unknown"),
        "Architecture": cpu_info.get("arch", "Unknown"),
        "Cores (Physical)": psutil.cpu_count(logical=False),
        "Cores (Logical)": psutil.cpu_count(logical=True),
        "Frequency": f"{psutil.cpu_freq().current:.2f} MHz",
    }

    # Memory information
    virtual_mem = psutil.virtual_memory()
    memory_info = {
        "Total Memory": f"{virtual_mem.total / (1024 ** 3):.2f} GB",
        "Available Memory": f"{virtual_mem.available / (1024 ** 3):.2f} GB",
        "Used Memory": f"{virtual_mem.used / (1024 ** 3):.2f} GB",
        "Memory Usage": f"{virtual_mem.percent}%",
    }

    # Disk information
    disk_info = psutil.disk_usage('/')
    disk_details = {
        "Total Disk Space": f"{disk_info.total / (1024 ** 3):.2f} GB",
        "Used Disk Space": f"{disk_info.used / (1024 ** 3):.2f} GB",
        "Free Disk Space": f"{disk_info.free / (1024 ** 3):.2f} GB",
        "Disk Usage": f"{disk_info.percent}%",
    }

    # Display information
    print("\nSystem Information")
    print("-" * 30)
    for key, value in system_info.items():
        print(f"{key}: {value}")

    print("\nProcessor Information")
    print("-" * 30)
    for key, value in cpu_details.items():
        print(f"{key}: {value}")

    print("\nMemory Information")
    print("-" * 30)
    for key, value in memory_info.items():
        print(f"{key}: {value}")

    print("\nDisk Information")
    print("-" * 30)
    for key, value in disk_details.items():
        print(f"{key}: {value}")

if __name__ == "__main__":
    get_system_info()

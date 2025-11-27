#!/usr/bin/env python3
# simulated_eternalblue_scanner.py
# Harmless demo: simulates scanning and then prints "vulnerable" IPs (documentation/test-net addresses).
# This script DOES NOT perform any network I/O.

import time
import random
import sys

targets = [
    "192.0.2.10",   # TEST-NET-1 (documentation)
    "192.0.2.25",
    "198.51.100.7", # TEST-NET-2 (documentation)
    "203.0.113.12"  # TEST-NET-3 (documentation)
]

def spinner(duration=3.5):
    chars = "|/-\\"
    end = time.time() + duration
    i = 0
    while time.time() < end:
        sys.stdout.write("\rScanning... " + chars[i % len(chars)])
        sys.stdout.flush()
        time.sleep(0.13)
        i += 1
    sys.stdout.write("\r" + " " * 40 + "\r")

def simulate_scan(target):
    print(f"Scanning {target} ...")
    spinner(random.uniform(0.8, 1.6))
    # randomize outcomes to look realistic for a demo
    score = random.random()
    if score < 0.18:
        return "VULNERABLE"
    elif score < 0.54:
        return "UNKNOWN - requires further check"
    else:
        return "NOT VULNERABLE"

def main():
    print("SIMULATED EternalBlue scan (demo only). No network activity performed.\n")
    results = {}
    for t in targets:
        status = simulate_scan(t)
        results[t] = status
        print(f"  -> {t}: {status}")
        time.sleep(0.25)

    print("\nSummary:")
    for t, s in results.items():
        if "VULNERABLE" in s:
            print(f"[!] {t}  — SIMULATED VULNERABLE (demo)")
    print("\nNote: These IPs are documentation/test-net addresses. This is a harmless simulation.")

if __name__ == "__main__":
    main()

import os
import hashlib
from datetime import datetime

class CyberShield:

    def __init__(self):
        self.suspicious_extensions = [
            ".exe", ".bat", ".cmd",
            ".vbs", ".scr", ".ps1"
        ]

    def calculate_hash(self, file_path):
        sha256 = hashlib.sha256()

        try:
            with open(file_path, "rb") as file:
                for block in iter(
                    lambda: file.read(4096), b""
                ):
                    sha256.update(block)

            return sha256.hexdigest()

        except Exception as error:
            return str(error)

    def scan_file(self, file_path):

        if not os.path.isfile(file_path):
            print("Error: File not found!")
            return

        filename = os.path.basename(file_path)
        extension = os.path.splitext(filename)[1].lower()

        print("\n========== CYBERSHIELD ==========")
        print("Security Scan Started")
        print("Date:", datetime.now())
        print("File:", filename)

        file_hash = self.calculate_hash(file_path)

        print("\nSHA-256 Hash:")
        print(file_hash)

        if extension in self.suspicious_extensions:
            print("\nWARNING: Executable file detected!")
            print("Manual security inspection recommended.")
        else:
            print("\nNo suspicious extension detected.")

        print("\nScan completed successfully.")
        print("=================================")


scanner = CyberShield()

path = input("Enter file path to scan: ")

scanner.scan_file(path)

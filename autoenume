#!/usr/bin/env python3
import argparse
import subprocess
import xml.etree.ElementTree as ET

def run_nmap(target):
    """Run nmap against target and save XML output."""
    xml_out = "scan.xml"
    print(f"[+] Running nmap on {target}...")
    subprocess.run(["nmap", "-sC", "-sV", "-oX", xml_out, target])
    return xml_out

def parse_nmap(xml_path):
    """Parse nmap XML and print results."""
    tree = ET.parse(xml_path)
    root = tree.getroot()
    print("\n[+] Open Ports Found:")
    for host in root.findall('host'):
        for port in host.findall(".//port"):
            state = port.find("state").get("state")
            portid = port.get("portid")
            service = port.find("service").get("name") if port.find("service") is not None else "unknown"
            if state == "open":
                print(f"  {portid}/tcp → {service}")

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="AutoEnum - basic recon automation")
    parser.add_argument("target", help="Target IP or hostname")
    args = parser.parse_args()

    xml_file = run_nmap(args.target)
    parse_nmap(xml_file)
  
# Run it with: python3 autoenum.py 10.10.10.1
# Takes an IP/hostname as input.
# Runs nmap with service/version detection.
# Saves XML → parses it → prints open ports + services.

# Splunk Universal Forwarder with Sysmon on Windows 10 VM

This guide provides step-by-step instructions to set up a Splunk Universal Forwarder and Sysmon on a Windows 10 virtual machine (VM) for a cybersecurity home lab. The Splunk Universal Forwarder collects Windows Event Logs and Sysmon logs, forwarding them to a Splunk indexer for analysis.

## Table of Contents
- Prerequisites
- Repository Structure
- Step 1: Install and Configure Sysmon
- Step 2: Download Splunk Universal Forwarder
- Step 3: Install Splunk Universal Forwarder
- Step 4: Configure Splunk Universal Forwarder
- Step 5: Configure Splunk Indexer
- Troubleshooting
- Contributing
- License

## Prerequisites
- Windows 10 VM: 64-bit Windows 10, with administrative privileges.
- Splunk Instance: Splunk Enterprise or Splunk Cloud instance configured to receive logs (note the IP address and port, default: 9997).
- Network Connectivity: VM must reach the Splunk indexer (e.g., port 9997 open).
- Splunk Account: Access to Splunk downloads.
- Sysmon: Download from Microsoft Sysinternals.
- Sysmon Configuration: sysmonconfig.xml from [Sysmon Olafconfig](https://github.com/olafhartong/sysmon-modular/blob/master/sysmonconfig.xml)
- Optional: Splunk deployment server (hostname/IP and port, default: 8089).

## Repository Structure
 ```
├── configs/
│   ├── inputs.conf              # Sample Splunk inputs configuration for Windows and Sysmon logs
│   ├── sysmonconfig-export.xml  # Sysmon configuration (sourced from SwiftOnSecurity/sysmon-config)
├── images/
│   ├── splunk_installer.png     # Screenshot of Splunk Universal Forwarder installer GUI
│   ├── sysmon_event_viewer.png  # Screenshot of Sysmon logs in Event Viewer
│   ├── splunk_search.png        # Screenshot of logs in Splunk web interface
├── docs/
│   ├── setup-guide.md           # Detailed setup guide (this document)
├── .gitignore                   # Ignores sensitive files (e.g., credentials, logs)
├── LICENSE                      # MIT License
├── README.md                    # This file
```

## Step 1: Install and Configure Sysmon
Sysmon logs detailed system activity (processes, network, files) for cybersecurity monitoring.

1. Download Sysmon:
   - Visit Microsoft Sysinternals.
   - Download and extract the Sysmon zip to C:\Sysmon.
   
2. Download Sysmon Configuration:
   - Get sysmonconfig.xml from [SysmonOlafconfig](https://github.com/olafhartong/sysmon-modular/blob/master/sysmonconfig.xml).
   - Select raw and save as sysmon config.
   - Save to C:\Sysmon.
   
3.  Install Sysmon:
    - Go to powershell and change directory to the path that contains Sysmon and the Sysmon config file.
         ``` 
         cd C:\Users\Esosa\Downloads\Sysmon>
         ```
    - Install Sysmon using:
        ```
        .\Sysmon64.exe -i sysmonconfig.xml
        ```
       Note that if the sysmon config file is in the downloads directory, use this:
        ```
       .\Sysmon64.exe -i ..\sysmon.config.xml
        ```
     - Verify Sysmon is running:
       - Open Services (Win + R, type services.msc).
       - Confirm Sysmon service is Running.
       
     - Check Sysmon logs:
       - Open Event Viewer (Win + R, type eventvwr.msc).
       - Navigate to Applications and Services Logs > Microsoft > Windows > Sysmon > Operational.


## STEP 2: Download Splunk Universal Forwarder
- Open a browser and go to Splunk’s download page.
- Select the Windows 64-bit .msi installer (e.g., Splunk Universal Forwarder 9.1.x).
- Save to C:\Downloads.

## STEP 3: Install Splunk Universal Forwarder
 - Run the Installer:
     - Double-click the .msi file in C:\Downloads.
     - Accept the license agreement and click Next.
     - Create credentials for administrator account.
       
 - Specify Receiving Indexer:
    - Enter the Splunk indexer’s hostname/IP and port (e.g., 192.168.19.15:9997 or 127.0.0.1:9997 for local).

 - Complete Installation:
    - Click Install to install to C:\Program Files\SplunkUniversalForwarder.
    - Verify the service:
         - Open Services (services.msc).
         - Confirm SplunkForwarder is Running.

## STEP 4: Configure Splunk Universal Forwarder
Configure the forwarder to collect Windows Event Logs and Sysmon logs.

### Step 4.2: Configure Inputs
- Locate inputs.conf:
   - Navigate to C:\Program Files\SplunkUniversalForwarder\etc\system\local in File Explorer.
   - If inputs.conf doesn’t exist, create a new text file named inputs.conf (no .txt extension).
     
- Edit inputs.conf:
   - Right-click inputs.conf, select Open with > Notepad (run as Administrator).
   - Use [configs/inputs.conf](.) from this repo
   - Save inputs.conf and close Notepad.
   - Restart the forwarder via Services (services.msc).


## Step 5: Configure Splunk Indexer
- Enable Receiving:
    - Log in to the Splunk indexer web interface.
    - Go to Settings > Forwarding and Receiving > Receive Data.
    - Add port 9997 if not configured.
- Create Indexes:
    - Go to Settings > Indexes.
    - Create "Endpoint" as index.
- Verify Connection:
    - Go to Settings > Forwarder Management or Clients to confirm the VM is connected.


## Troubleshooting
- Sysmon Logs Missing:
   - Confirm Sysmon service is running (services.msc).
   - Verify Sysmon events in Event Viewer (eventvwr.msc) under Microsoft > Windows > Sysmon > Operational.
   - Check inputs.conf for errors.

- Splunk Service Issues:
    - Ensure SplunkForwarder service is running (services.msc).
    - Check Event Viewer for errors.

- No Logs in Splunk:
   - Verify index (endpoint) exist on the indexer.
   - Check C:\Program Files\SplunkUniversalForwarder for errors.
   - Ensure port 9997 is open in Windows Defender Firewall.


## Contributing
Contributions are welcome! Please:
- Submit suggestions.
- Fork the repo, make changes, and submit a pull request.
- Follow the Code of Conduct.

## License
MIT License
  



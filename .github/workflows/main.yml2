name: Free Windows RDP

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: windows-latest
    timeout-minutes: 360
    steps:
      - name: Download Cloudflared
        run: |
          Invoke-WebRequest https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-windows-amd64.exe -OutFile cloudflared.exe

      - name: Set RDP Password
        run: |
          net user runneradmin P@ssw0rd123!
          echo "✅ Password: P@ssw0rd123!"

      - name: Enable RDP
        run: |
          Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server" -name "fDenyTSConnections" -Value 0
          Enable-NetFirewallRule -DisplayGroup "Remote Desktop"

      - name: Start Cloudflared Tunnel
        run: |
          Start-Process -NoNewWindow .\cloudflared.exe -ArgumentList "tunnel","--url","rdp://localhost:3389" -RedirectStandardOutput cloudflared.log
          Start-Sleep -Seconds 10
          Get-Content cloudflared.log -ErrorAction SilentlyContinue

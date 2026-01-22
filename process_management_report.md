# Process Management Report

## System info - Hostname: <hostnamectl> - Distro & version: <Ubuntu 22.04>
## 1. Listing processes - Commands run: `ps aux | less — full list with CPU/MEM columns.
                                        `ps -ef --forest | less — tree view to see parent/child relationships.
                                        'top — live view; note top consumers.
                        - Target nginx PIDs: pidof nginx or ps aux | grep -i nginx | grep -v grep.
                        - Observations to note: - High CPU/mem: which processes top shows at the top.
                                                - Process tree: which parent spawns nginx workers.
                                                - Foreground/background: anything unusual (e.g., many zombie processes).
## 2. Killing processes - Target process: sudo kill -1 122 -    (example)
                        - Signals used: - SIGTERM :  SIGTERM (15): polite stop—kill <PID>.
                                        - SIGHUP : SIGHUP (1): reload config—often used by daemons—kill -HUP <PID>.
                                        - SIGKILL : SIGKILL (9): force kill—kill -9 <PID> (last resort).
                                        - Outcome: exited/restarted/ignored
## 3. Service management (systemctl) - Service: nginx - Status output (snippet): 
                                     - Start: sudo systemctl start nginx,  
                                     - Stop: systemctl stop nginx, 
                                     - Restart: systemctl restart nginx and 
                                     - Reload: systemctl reload nginx.
## Code - Actions: start/stop/restart/reload - 
        - Logs: journalctl -u nginx --since "1 hour ago"
        - Jan 22 09:26:17 DESKTOP-IEMRB2F systemd[1]: Starting nginx.service - A high performance web server and a reverse proxy server... 
        - Jan 22 09:26:18 DESKTOP-IEMRB2F systemd[1]: Started nginx.service - A high performance web server and a reverse proxy server.      
        - Jan 22 09:26:51 DESKTOP-IEMRB2F systemd[1]: Stopping nginx.service - A high performance web server and a reverse proxy server... 
        - Jan 22 09:26:51 DESKTOP-IEMRB2F systemd[1]: nginx.service: Deactivated successfully.    
        - Jan 22 09:26:51 DESKTOP-IEMRB2F systemd[1]: Stopped nginx.service - A high performance web server and a reverse proxy server.    
        - Jan 22 09:27:09 DESKTOP-IEMRB2F systemd[1]: Starting nginx.service - A high performance web server and a reverse proxy server...   
        - Jan 22 09:27:09 DESKTOP-IEMRB2F systemd[1]: Started nginx.service - A high performance web server and a reverse proxy server. 
        - Jan 22 09:27:30 DESKTOP-IEMRB2F systemd[1]: Reloading nginx.service - A high performance web server and a reverse proxy server...   
        - Jan 22 09:27:30 DESKTOP-IEMRB2F nginx[1406]: 2026/01/22 09:27:30 [notice] 1406#1406: signal process started 
        - Jan 22 09:27:30 DESKTOP-IEMRB2F systemd[1]: Reloaded nginx.service - A high performance web server and a reverse proxy server.
## 4. Enable at boot - 
        - Enable: systemctl enable nginx` - 
        - Verify: systemctl is-enabled nginx → should say enabled.
        - Rationale: Explain why nginx should start at boot (e.g., it serves your site) or why you keep it disabled (lab-only).
## 5. Resource monitoring - CPU/memory: top — note %CPU and RES/MEM% for nginx workers at peak.  ps -o pid,comm,%cpu,%mem --pid $(pidof nginx) — snapshot per PID. 
                          - I/O (optional): sudo apt-get install -y sysstat (if needed) ,  iostat -xz 1 5 — summarize disk I/O; note any high utilization. 
                          - Network: sudo ss -tulpn | grep nginx — show listening ports (e.g., 0.0.0.0:80, :443) 
                          - Notes: Mention anomalies (e.g., unexpected port, high CPU spikes, many connections).
  

Lets say my service name is hotflash.

---

### ✅ Step-by-Step: Run `server.py` as a `systemd` service

#### 1. **Create a systemd service file**

```bash
sudo nano /etc/systemd/system/hotflash.service
```

#### 2. **Paste the following (edit paths if needed)**

```ini
[Unit]
Description=Hotflash Remote Control Panel
After=network.target

[Service]
User=ec2-user
WorkingDirectory=/home/ec2-user/hotflash
ExecStart=/usr/bin/python3 server.py
Restart=always
RestartSec=5
StandardOutput=file:/home/ec2-user/hotflash/server.log
StandardError=file:/home/ec2-user/hotflash/server.err

[Install]
WantedBy=multi-user.target
```

> ✅ Ensure:
>
> * `/usr/bin/python3` is the correct Python path (`which python3`)
> * Replace `/home/ec2-user/hotflash` with your actual project path

---

#### 3. **Reload systemd daemon**

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
```

---

#### 4. **Enable the service (auto-start on boot)**

```bash
sudo systemctl enable hotflash
```

---

#### 5. **Start the service now**

```bash
sudo systemctl start hotflash
```

---

#### 6. **Check status**

```bash
sudo systemctl status hotflash
```

You should see: `Active: active (running)`

---

#### 7. **View logs**

```bash
tail -f /home/ec2-user/hotflash/server.log
```

or

```bash
journalctl -u hotflash -f
```

---

Let me know if you’re using a virtualenv—you’ll need to point `ExecStart` to the `.venv/bin/python`.

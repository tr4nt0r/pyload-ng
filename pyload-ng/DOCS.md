# Home Assistant Add-on: pyLoad-ng

## Web Interface

To access pyLoad's web interface, open your web browser and navigate to:
[http://homeassistant:8000](http://homeassistant:8000)

**Default Login Credentials:**

- **Username**: `pyload`
- **Password**: `pyload`

<ha-alert alert-type="info">**Important:** For security reasons, it is strongly recommended to change the default login credentials immediately after your first login.</ha-alert>

**Default Download Location:**

`/media/downloads`

To change the download folder:

1. Open the pyLoad web interface.
2. Go to **⚙️ Settings → Download**.
3. Set your preferred path under **Download folder**.

<ha-alert alert-type="warning">Avoid downloading directly to an SD card. The high frequency of write operations can quickly degrade it.</ha-alert>

## Using Click'N'Load from a Remote PC

To use pyLoad's **Click'N'Load** feature from your local machine, you need to forward the Click'N'Load port (`9666`) from your local system to your Home Assistant host running the pyLoad Add-on. This allows your browser to forward Click'n'Load links to pyLoad as if it were running locally.

<ha-alert alert-type="info">Replace `homeassistant.local` with the hostname or IP of your Home Assistant device.</ha-alert>

### 🐧 Linux (or any system with SSH)

#### 📌 Prerequisites
- An SSH server must be running on your local system.

You can set up port forwarding with the following command:

```bash
ssh -L 127.0.0.1:9666:homeassistant.local:9666 -N 127.0.0.1
```

### 🪟 Windows (using `netsh portproxy`)

On Windows, you can achieve the same result using the built-in `netsh` tool to forward the Click'N'Load port.

#### 📌 Prerequisites:
- Run commands in an **elevated Command Prompt** (right-click → *Run as Administrator*).

#### ✅ Create Port Forward Rule:

```cmd
netsh interface portproxy add v4tov4 listenport=9666 listenaddress=127.0.0.1 connectport=9666 connectaddress=homeassistant.local
```
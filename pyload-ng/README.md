# Home Assistant App: pyLoad-ng

_The free and open-source Download Manager written in pure Python._

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

## App extras

This app is packaged with optional dependencies to use with Plugins.

- **unrar:** Used for Plugin ExtractArchive
- **7zip:** Used for Plugin ExtractArchive
- **tesseract-ocr:** Used for Captcha solving Plugins

---

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]

[aarch64-shield]: https://img.shields.io/badge/aarch64-yes-green.svg
[amd64-shield]: https://img.shields.io/badge/amd64-yes-green.svg

# Installation Troubleshooting Guide

This guide helps resolve common issues when installing TalkHeal dependencies.

## Issue: Timeout Error When Downloading Packages

If you're experiencing timeout errors like `HTTPSConnectionPool(host='files.pythonhosted.org', port=443): Read timed out`, try these solutions:

### Solution 1: Increase Pip Timeout (Recommended First Try)

```bash
pip install --default-timeout=100 -r requirements.txt
```

Or for even longer timeout:
```bash
pip install --default-timeout=1000 -r requirements.txt
```

### Solution 2: Use a Different PyPI Mirror

If PyPI's main server is slow or blocked in your region, use a mirror:

**Using Aliyun mirror (good for Asia-Pacific):**
```bash
pip install -r requirements.txt -i https://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com
```

**Using Tsinghua mirror:**
```bash
pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple --trusted-host pypi.tuna.tsinghua.edu.cn
```

**Using Tencent mirror:**
```bash
pip install -r requirements.txt -i https://mirrors.cloud.tencent.com/pypi/simple --trusted-host mirrors.cloud.tencent.com
```

**Using Douban mirror:**
```bash
pip install -r requirements.txt -i https://pypi.doubanio.com/simple --trusted-host pypi.doubanio.com
```

### Solution 3: Configure Pip to Always Use a Mirror

Create or edit `pip.ini` (Windows) or `pip.conf` (Linux/Mac):

**Windows:** `%APPDATA%\pip\pip.ini`
```ini
[global]
index-url = https://mirrors.aliyun.com/pypi/simple/
trusted-host = mirrors.aliyun.com
timeout = 120
```

**Linux/Mac:** `~/.pip/pip.conf`
```ini
[global]
index-url = https://mirrors.aliyun.com/pypi/simple/
trusted-host = mirrors.aliyun.com
timeout = 120
```

### Solution 4: Update Pip, Setuptools, and Wheel

```bash
python -m pip install --upgrade pip setuptools wheel
```

Then try installing again:
```bash
pip install -r requirements.txt
```

### Solution 5: Clear Pip Cache

```bash
pip cache purge
```

Then try installing again.

### Solution 6: Install Packages One by One

This helps identify which specific package is causing issues:

```bash
pip install streamlit
pip install streamlit-lottie
pip install langchain-google-genai
# ... continue with other packages
```

### Solution 7: Install with No Cache

```bash
pip install --no-cache-dir -r requirements.txt
```

### Solution 8: Use a VPN or Different Network

Sometimes network restrictions or regional blocks can cause timeouts. Try:
- Using a VPN
- Switching to mobile data/different WiFi
- Using a different DNS (like Google DNS: 8.8.8.8)

### Solution 9: Install Specific Problematic Packages Separately

If `jsonschema_specifications` keeps timing out:

```bash
pip install --default-timeout=1000 jsonschema-specifications
```

Then install the rest:
```bash
pip install -r requirements.txt
```

### Solution 10: Use Conda Instead of Pip (Alternative)

If you have Anaconda/Miniconda:

```bash
conda create -n talkheal python=3.10
conda activate talkheal
pip install -r requirements.txt
```

## Common Error Messages and Solutions

### "Read timed out"
- **Cause:** Slow internet or PyPI server issues
- **Solution:** Use Solutions 1, 2, or 8

### "Could not find a version that satisfies the requirement"
- **Cause:** Package name typo or package doesn't exist for your Python version
- **Solution:** Check your Python version, update pip

### "SSLError" or Certificate errors
- **Cause:** SSL certificate issues
- **Solution:** Use `--trusted-host` flag or update certificates

## Still Having Issues?

1. Check your Python version: `python --version` (TalkHeal requires Python 3.8+)
2. Check your internet connection
3. Try running with verbose mode to see more details:
   ```bash
   pip install -v -r requirements.txt
   ```
4. Report the issue on [GitHub Issues](https://github.com/eccentriccoder01/TalkHeal/issues) with:
   - Your OS and Python version
   - Full error message
   - Which solutions you've tried

## Quick Start Command (Recommended for Most Users)

For most connectivity issues, this command works best:

```bash
pip install --default-timeout=1000 -r requirements.txt -i https://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com
```

## Verification

After successful installation, verify all packages are installed:

```bash
pip list
```

Then try running the app:

```bash
streamlit run app.py
```

---

**Note:** If you're contributing to TalkHeal, make sure to also check [CONTRIBUTING.md](CONTRIBUTING.md) for development setup instructions.

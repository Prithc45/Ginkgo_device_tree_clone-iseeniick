# 📱 AxionOS ROM Build Guide (Beginner Friendly)

This guide will walk you through **everything from cloning the source → adding device trees → building your ROM** using `ax -b`.

---

# ⚡ Requirements

### 🖥️ Hardware (recommended)

* **CPU:** 6 cores / 12 threads or higher
* **RAM:** 16GB minimum (24GB+ recommended)
* **Storage:**

  * 200GB minimum
  * 400GB+ recommended for Android 16 builds

---

### 🐧 Software (Ubuntu / Arch / Debian)

Install dependencies:

```bash
sudo apt update
sudo apt install git curl python3 openjdk-17-jdk repo unzip bc bison build-essential zip flex ccache libssl-dev
```

---

# 🚀 Step 1 — Initialize AxionOS Source

Create a working directory:

```bash
mkdir AxionOSGinkgo
cd AxionOSGinkgo
```

Initialize repo:

```bash
repo init -u https://github.com/AxionAOSP/android.git -b axion-2.5
```

---

# 🔄 Step 2 — Sync Source

```bash
repo sync -c -j8
```

### What this does:

* `-c` → sync current branch only (faster, less storage)
* `-j8` → parallel download threads

⏳ This will take time depending on your internet.

---

# 📦 Step 3 — Clone Device Trees

Place your `clone_repos.sh` script in the **root of source**:

```
Sourcefiles/
├── android/
├── art/
├── build/
├── bootable/
├── clone_repos.sh  ← here
```

---

### Run the script:

```bash
bash clone_repos.sh
```

This will clone:

* device tree
* kernel
* vendor blobs
* hardware repos
* additional dependencies

---

# ⚠️ Important Fix (if script came from Windows)

If you get weird errors like `\r`:

```bash
dos2unix clone_repos.sh
```

---

# 🧠 Step 4 — Setup Build Environment

```bash
source build/envsetup.sh
```

---

# 🎯 Step 5 — Select Device

```bash
axion <device_codename> [user|userdebug|eng] [gms [core] | vanilla]
```

---

# 🔥 Step 6 — Build the ROM

```bash
ax -b
```

### Optional (manual threads):

```bash
ax -b -j10
```

---

# 📂 Output Location

Your final ROM will be in:

```
out/target/product/ginkgo/
```

---

# 💣 Common Issues & Fixes

## ❌ Soong path error

```
panic: unexpected relative path outside directory
```

👉 Fix:

```bash
rm -rf out
```

---

## ❌ Windows line ending issue

```
invalid module name / \r errors
```

👉 Fix:

```bash
dos2unix clone_repos.sh
```

---

## ❌ Build killed (RAM issue)

```
Killed
```

👉 Fix:

* Increase swap(40GB+ recommended)
* Reduce threads:

```bash
ax -b -j8
```

---

## ❌ Disk full

👉 Fix:

* Ensure **at least 50–100GB free**
* Clean build:

```bash
rm -rf out
```

---

# ⚡ Pro Tips

* Use **ccache** for faster rebuilds:

```bash
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
```

* Keep everything in **one directory tree**

* Avoid manually setting:

```bash
OUT_DIR
OUT_DIR_COMMON_BASE
```

---

# 🧪 Rebuilding After Changes

```bash
m installclean
ax -b
```

---

# 🏁 Done!

If everything goes right, you’ll get your flashable ROM zip 🎉

---

# 🔥 Final Advice

* First successful build = biggest milestone
* Don’t rush optimization
* Fix errors step-by-step

---

Happy building 🚀

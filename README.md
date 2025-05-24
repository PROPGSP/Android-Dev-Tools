

# 🔧 Android Dev Tools  
🚀 **A collection of powerful tools designed to kickstart Android ROM development and customization.** 🚀  

**🔹 Beginner-friendly** – As a learner in Android ROM development, I've gathered tools along my journey to make them easily accessible for new developers.

📁 **Get the Files from Releases:**  
[🔗 Android Dev Tools Release](https://github.com/PROPGSP/Android-Dev-Tools/releases/tag/release)  

---

## 🛠️ Important Advice  
✔️ **Strictly use Linux** – It's essential for ROM development.  
❌ **Avoid WSL** – Too many limitations for this process.  
💻 **Bare-metal Linux is best** – If you can't install Linux natively, use a **Linux VM**.  
🛠️ **Oracle VirtualBox is a better alternative** to WSL, but expect slower builds (several hours to days).  
📌 **Check out minimum system requirements:**  
[🔗 Android Build Requirements](https://source.android.com/docs/setup/start/requirements)  

---

## 🛠️ Usage Overview  
Each tool contains a **README.md** inside its respective folder with **detailed usage instructions**.  
Below is a **quick overview** for advanced users who prefer a **fast reference**.  

---

## 📦 Extracting OTA Firmware (payload.bin)  
If you have an **OTA update** containing `payload.bin`, use **payload-dumper-go** inside `payload.bin-unpackor`:

1️⃣ **Set execution permissions:**  
```bash
chmod +x payload-dumper-go 
```
2️⃣ **Add the tool to your PATH:**  
```bash
export PATH=$PATH:/path/to/payload-dumper-go
```
3️⃣ **Extract `payload.bin`:**  
```bash
payload-dumper-go /path/to/payload.bin
```
🚀 You now have unpacked images (`boot`, `dtbo`, `system`, `vendor`, etc.).  
⚠️ Not all payloads contain the same images – it depends on the device brand/model.

---

## 📦 Extracting Sparse Images (`super.img`)  
If you **don't have payload.bin** but instead have **sparse chunks**, follow these steps:

### 1️⃣ Convert Sparse Images to `super.img`  
Inside `super.img-extractor`, run:  
```bash
./simg2img system.img_sparsechunk.* super.img
```

### 2️⃣ Unpack `super.img` into partitions  
```bash
./lpunpack super.img /super_unpacked
```
✔️ You will get `system`, `vendor`, `system_a`, `system_b`, `vendor_a`, `vendor_b` images.  
⚠️ If an error occurs (`vendor_b.img not found`), create an empty file to bypass:  
```bash
mkdir super_unpacked; cd super_unpacked; touch vendor_b.img
```
Now you have **all images extracted** from the firmware.

---

## 🛠️ Extracting Device Tree, Kernel, and Other Files  
### 📌 Using **Android_boot_editor** (inside `image-unpack-repack`)
Before extracting, **check file formats**:  
```bash
file *.img
```

✔️ If **Erofs format**, use:  
```bash
./erofsUnpackRust_x64linux system_a.img
```
🚀 You now have unpacked files!  
Alternatively, **use Binwalk for deep extraction**:  
```bash
binwalk -Me name.img
```
🚀 Extracted data is stored inside auto-generated folders.

✔️ If you want to analyze an image without extracting:  
```bash
binwalk -Me -r name.img
```

⚠️ If the **device tree & kernel files** aren't inside standard partitions, **try:**  
```bash
./gradlew unpack
```
✔️ You’ll find extracted files inside `image-unpack-repack/build/`.  
📌 **After extracting each `.img` file, clear the build folder** to prevent issues.

---

## 📜 License  
📌 This project is a **collection of open-source tools**.  
Each tool contains its **own respective license file**.

---

I've incorporated the **credits section** into your README with proper formatting and styling for clarity. Here's the enhanced version:

---

## 🏆 Credits  
🙏 **Thanks to the amazing developers** who created these essential tools!  

| 🔹 **Tool** | 🏷️ **Developer** | 🔗 **Repository** |
|------------|-----------------|------------------|
| **Binwalk** | @ReFirmLabs | [🔗 GitHub Repo](https://github.com/ReFirmLabs/binwalk) |
| **DTC (Device Tree Compiler)** | @dgibson | [🔗 GitHub Repo](https://github.com/dgibson/dtc) |
| **Payload Dumper Go** | @ssut | [🔗 GitHub Repo](https://github.com/ssut/payload-dumper-go) |
| **Android Boot Image Editor** | @cfig | [🔗 GitHub Repo](https://github.com/cfig/Android_boot_image_editor) |

💙 **A huge shoutout to these developers for building such powerful tools that make ROM development easier for everyone!**  




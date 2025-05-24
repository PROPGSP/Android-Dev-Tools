<p align="center">
  <img src="https://img.shields.io/github/stars/PROPGSP/Android-Dev-Tools?color=FFD700&label=Stars" alt="GitHub stars">
  <img src="https://img.shields.io/github/forks/PROPGSP/Android-Dev-Tools?color=00BFFF&label=Forks" alt="GitHub forks">
  <img src="https://img.shields.io/github/watchers/PROPGSP/Android-Dev-Tools?color=32CD32&label=Watchers" alt="GitHub watchers">
  <img src="https://img.shields.io/github/contributors/PROPGSP/Android-Dev-Tools?color=8A2BE2&label=Contributors" alt="GitHub contributors">
  <img src="https://img.shields.io/github/license/PROPGSP/Android-Dev-Tools?color=DC143C&label=License" alt="GitHub license">
</p>




<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=30&duration=2000&pause=500&color=0088FF&center=true&vCenter=true&width=600&lines=Android+Dev+Tools;Firmware+Extraction;ROM+Customization;Kernel+Tweaks">
</p>



# 🔧 Android Dev Tools  



🚀 **A collection of powerful tools designed to kickstart Android ROM development and customization.** 🚀  

**🔹 Beginner-friendly** – As a learner in Android ROM development, I've gathered tools along my journey to make them easily accessible for new developers.

📁 **Get the Files from Releases:**  
[🔗 Android Dev Tools Release](https://github.com/PROPGSP/Android-Dev-Tools/releases/tag/release)  


---

## 🏆 **A Kind Request to Expert Android ROM Developers**  

🙏 **Calling all experienced Android ROM developers!**  
As a beginner in **Android ROM development**, I have collected some tools to make learning journey easier. But I know that there are **many more powerful tools** that experts like you use daily.  

📢 **If you know any must-have tools that simplify ROM development, firmware extraction, kernel tweaking, or device tree modifications, kindly share them!**  
Your knowledge can help **new developers** kickstart their journey more efficiently.  

---

## 💡 **How You Can Contribute**  
✔️ **Share the tools** you use for ROM development.  
✔️ **Provide GitHub or download links** for open-source tools.  
✔️ **Add a brief description** of how the tool helps in ROM customization.  

Every suggestion **helps beginners** grow and explore **Android development** more effectively.  
Your support makes the **Android ROM community** stronger! 💙  

🔥 **Drop your favorite tools below** or **open a Pull Request** with new additions! 
🔥 **Start an discussion in this repo** or **open an issue** and drop the tools to attcach

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

When extracting **device tree blobs (DTB)**, **kernel files**, and **embedded partitions**, different methods apply based on the image format. Here’s a **step-by-step guide covering all approaches**, including the **manual `dd` method** for stubborn files.

---

### 📌 **Step 1: Install Required Libraries & Tools**  
Before extracting, install essential tools:  
```bash
sudo apt update && sudo apt install -y binwalk simg2img lz4 squashfs-tools mount
```
✔️ **Additional Dependencies:**  
```bash
pip install protobuf
```
🔹 Some extraction tools may require Python dependencies like `protobuf`.

---

### 📌 **Step 2: Identify Image Format**  
First, check the format of `.img` files:  
```bash
file *.img
```
✔️ If **Erofs**, **SquashFS**, **Ext4**, or another format, use the relevant method below.

---

### 📌 **Step 3: Extracting EROFS Files**  
For **EROFS-formatted partitions**, use:  
```bash
./erofsUnpackRust_x64linux system_a.img
```
🚀 Files will be unpacked.

Alternatively, **mount EROFS manually** on Linux:  
```bash
mkdir mount_point  
sudo mount -t erofs system_a.img mount_point/
```
Now browse the extracted files inside `mount_point/`.

---

### 📌 **Step 4: Extracting Generic Files Using Binwalk**  
If the image contains embedded files (such as kernel or DTB blobs), use **Binwalk** for deep extraction:  
```bash
binwalk -Me name.img
```
🚀 Extracted data is stored inside auto-generated folders.  

✔️ If you only need a **content list**, without extracting:  
```bash
binwalk -Me -r name.img
```

📌 **Additional Extraction with Binwalk:**  
If Binwalk does not properly extract the data, use **`dd`** to manually carve out sections based on offsets.

---

### 📌 **Step 5: Extracting Kernel & Device Tree (DTB)**  
Some devices store **kernel and DTB files** inside `boot.img`, `vendor_boot.img`, or `dtbo.img`.  

#### ✔️ **Using `split_bootimg.pl`**
Install the tool:  
```bash
git clone https://github.com/xblax/bootimg-tools.git  
cd bootimg-tools  
chmod +x split_bootimg.pl  
```
Extract boot image components:  
```bash
./split_bootimg.pl boot.img
```
🚀 Generates extracted files such as:  
- `kernel`  
- `ramdisk`  
- `dtb`  

📌 **If DTB is missing, extract manually using `dd`:**  
```bash
dd if=boot.img of=dtb.img bs=1 skip=<offset>
```
_(Replace `<offset>` with actual DTB location found using Binwalk.)_

---

### 📌 **Step 6: Extracting Android Boot Image Components**  
For extracting boot partitions (`boot.img`, `vendor_boot.img`, etc.), use **Android Boot Image Editor**:

✔️ Clone and install:  
```bash
git clone https://github.com/cfig/Android_boot_image_editor.git  
cd Android_boot_image_editor  
chmod +x gradlew  
```
🚀 **Unpack boot image:**  
```bash
./gradlew unpack
```
✔️ Extracted files appear in `image-unpack-repack/build/`.  
📌 **After extracting each `.img`, clear the build folder** to avoid conflicts.

---

### 📌 **Step 7: Extracting Vendor Blobs**  
For extracting vendor binary blobs:  
```bash
./extract-proprietary-files.sh
```
🚀 This copies vendor blobs into the correct AOSP directory.

✔️ **Manually extracting `vendor.img`:**  
```bash
simg2img vendor.img vendor.raw.img  
mkdir vendor_mount  
sudo mount -o loop vendor.raw.img vendor_mount/
```
✔️ You can now browse vendor files inside `vendor_mount/`.

---

### 📌 **Step 8: Manual Extraction Using `dd` (If Nothing Else Works)**  
When **other methods fail**, manually extract files using `dd`.  
1️⃣ **Find offsets using Binwalk:**  
```bash
binwalk -E name.img
```
✔️ This displays embedded file locations.

2️⃣ **Extract data manually:**  
```bash
dd if=name.img of=extracted_file bs=1 skip=<offset> count=<size>
```
- **`skip=<offset>`** = Starting byte (found using Binwalk).  
- **`count=<size>`** = Size of the file (estimate based on previous extractions).  

🚀 This method **works when automated extractors fail**!

---

## 🎯 **Final Notes**  
✔️ Always **check the image format first**.  
✔️ Use the **appropriate extraction tool** (`Binwalk`, `simg2img`, `dd`, etc.).  
✔️ If extraction **fails**, **try a combination of multiple methods**.  
✔️ **Clear build folders** after unpacking to keep things organized.  


---

## 📜 License  
📌 This project is a **collection of open-source tools**.  
Each tool contains its **own respective license file**.


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


<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=30&duration=2000&pause=500&color=0088FF&center=true&vCenter=true&width=600&lines=Help+Others+With+A+Smile😊😉">
</p>




**Below are FAQs to improve google indexing**

Everything i made here is to make sure its very easily searchable to kick start the beginners journey in rom development.


🔍 Frequently Searched Questions – SEO Optimized for GitHub & Google 🚀  

🛠️ **Firmware Extraction & Partition Handling**  
1️⃣ How to extract vendor blobs from Android firmware?  
2️⃣ How to unpack kernel images from boot.img?  
3️⃣ How to extract DTB (Device Tree Blob) from Android boot.img?  
4️⃣ How to extract ramdisk from an Android boot.img?  
5️⃣ How to unpack system and vendor partitions from super.img?  
6️⃣ Best tools for extracting Android firmware partitions?  
7️⃣ How to repack boot.img after modification?  
8️⃣ How to convert sparse images to raw format?  
9️⃣ How to modify and repackage vendor_boot.img?  
🔟 What is payload.bin, and how to unpack it?  
1️⃣1️⃣ How to extract proprietary binaries from an Android update package?  
1️⃣2️⃣ How to fix errors while unpacking dynamic partitions?  

🔧 **Android ROM Development & Customization**  
1️⃣3️⃣ How to build a custom Android ROM from scratch?  
1️⃣4️⃣ How to apply patches and modifications to AOSP?  
1️⃣5️⃣ What are the essential tools for ROM customization?  
1️⃣6️⃣ How do I add GApps to a custom ROM?  
1️⃣7️⃣ How to decompile and edit system APKs?  
1️⃣8️⃣ How do I extract and modify bootloader images?  
1️⃣9️⃣ How to integrate vendor blobs into custom ROM development?  
2️⃣0️⃣ What is the difference between slot A and slot B in Android firmware?  
2️⃣1️⃣ How to check if a firmware package supports dynamic partitions?  

⚙️ **Kernel Tweaking & Device Tree Configuration**  
2️⃣2️⃣ How to extract and modify a device tree blob (DTB)?  
2️⃣3️⃣ What tools can I use to unpack and repack boot.img?  
2️⃣4️⃣ How to add custom kernel features to an Android ROM?  
2️⃣5️⃣ How to patch a kernel for a specific device?  
2️⃣6️⃣ How to enable root access in a custom ROM build?  
2️⃣7️⃣ How do I customize kernel parameters for better performance?  
2️⃣8️⃣ How to extract init_boot.img and modify boot parameters?  
2️⃣9️⃣ How to troubleshoot boot.img repacking issues?  
3️⃣0️⃣ What is the role of vendor_dlkm.img in Android firmware?  

🛠️ **Advanced Debugging & Reverse Engineering**  
3️⃣1️⃣ How to debug boot loops in custom ROMs?  
3️⃣2️⃣ What tools can I use for analyzing firmware images?  
3️⃣3️⃣ How to identify the partition layout of an Android device?  
3️⃣4️⃣ How to perform a deep analysis of system.img using Binwalk?  
3️⃣5️⃣ How to unpack and analyze vendor_dlkm.img?  
3️⃣6️⃣ How to extract modem firmware from a payload.bin?  
3️⃣7️⃣ How do I find the correct offsets for unpacking proprietary partitions?  
3️⃣8️⃣ How to manually extract bootloader partitions from a firmware package?  

🚀 **Android File System Management**  
3️⃣9️⃣ How to mount EROFS partitions manually?  
4️⃣0️⃣ What is the difference between ext4, SquashFS, and EROFS in Android?  
4️⃣1️⃣ How do I extract read-only partitions in Android firmware?  
4️⃣2️⃣ How to troubleshoot mount failures for extracted firmware images?  
4️⃣3️⃣ How to recover files from a corrupted Android partition?  

📦 **OTA Firmware Extraction & Payload Dumping**  
4️⃣4️⃣ How to unpack payload.bin from an Android OTA update?  
4️⃣5️⃣ How to extract images from super.img using lpunpack?  
4️⃣6️⃣ How to extract sparse system.img files?  
4️⃣7️⃣ How do I convert sparse images to raw format for mounting?  

🔍 **Bootloader Unlocking & Security Bypass**  
4️⃣8️⃣ How to unlock a bootloader for custom ROM installation?  
4️⃣9️⃣ How do I bypass bootloader restrictions on locked devices?  
5️⃣0️⃣ What are the security implications of unlocking a bootloader?  
5️⃣1️⃣ How to extract and modify fastboot partitions?  

📌 **Recovery & Rooting Tools**  
5️⃣2️⃣ How to install a custom recovery like TWRP?  
5️⃣3️⃣ How to root an Android device without unlocking the bootloader?  
5️⃣4️⃣ How to create a Magisk module for Android customization?  
5️⃣5️⃣ How do I patch a boot.img for root access?  

📢 **Android Debugging & Developer Tools**  
5️⃣6️⃣ How to use ADB to extract and modify firmware partitions?  
5️⃣7️⃣ How to troubleshoot fastboot flashing errors?  
5️⃣8️⃣ How do I capture logs for debugging boot failures?  
5️⃣9️⃣ What’s the best way to debug SELinux policy issues in Android?  

🌐 **Cross-Device Compatibility**  
6️⃣0️⃣ How to port a custom ROM to a different Android device?  
6️⃣1️⃣ What challenges exist when porting ROMs across vendors?  
6️⃣2️⃣ How to adapt a device tree for different chipset architectures?  
6️⃣3️⃣ How to extract Qualcomm firmware blobs for custom ROM development?  

🛠️ **ROM Optimization & Battery Performance**  
6️⃣4️⃣ How do I tweak kernel settings for better battery life?  
6️⃣5️⃣ How to identify power-consuming services in Android firmware?  
6️⃣6️⃣ What’s the best way to reduce wake-locks in custom ROMs?  

📜 **Firmware Modification & Repackaging**  
6️⃣7️⃣ How to repackage boot.img after modifications?  
6️⃣8️⃣ How to add additional vendor files in an Android ROM build?  
6️⃣9️⃣ How to adjust partition sizes for custom firmware installation?  

🔹 **Compatibility & System Fixes**  
7️⃣0️⃣ How do I fix compatibility issues when flashing a ROM?  
7️⃣1️⃣ How to resolve bootloader mismatches in Android firmware?  
7️⃣2️⃣ How do I fix system.img mounting errors?  
7️⃣3️⃣ How to troubleshoot vendor.img extraction failures?  

🚀 **Performance Tuning & Tweaks**  
7️⃣4️⃣ How to modify kernel parameters for better Android performance?  
7️⃣5️⃣ How do I tune I/O schedulers for speed optimization?  
7️⃣6️⃣ How to tweak Android memory management for smoother performance?  
7️⃣7️⃣ How do I adjust LMK settings for better RAM allocation?  

🔎 **Vendor Blob Extraction & Analysis**  
7️⃣8️⃣ How do I extract proprietary vendor blobs from stock firmware?  
7️⃣9️⃣ How to analyze vendor blobs for missing drivers?  
8️⃣0️⃣ What’s the best way to integrate vendor blobs into AOSP ROMs?  

📦 **ROM Security & Hardening Techniques**  
8️⃣1️⃣ How to improve SELinux policies for custom ROM security?  
8️⃣2️⃣ How do I strip unnecessary binaries for lightweight ROM builds?  
8️⃣3️⃣ How to secure proprietary vendor blobs in custom ROM development?  

📢 **Android Build System & Compilation**  
8️⃣4️⃣ How to set up an Android build environment?  
8️⃣5️⃣ How do I compile a custom kernel for an Android device?  
8️⃣6️⃣ How do I fix missing dependencies in an AOSP build?  

📌 **Troubleshooting & Debugging Firmware Flashing Errors**  
8️⃣7️⃣ How do I fix fastboot flashing failures?  
8️⃣8️⃣ How do I troubleshoot OTA update installation errors?  
8️⃣9️⃣ How to recover a bricked device after failed flashing?  

🔹 **Device-Specific Extraction & Customization**  
9️⃣0️⃣ How to extract firmware from a Samsung device?  
9️⃣1️⃣ How to unpack boot.img on a Qualcomm-based phone?  
9️⃣2️⃣ How to modify a device tree for MediaTek chipsets?  

🔧 **How-To Guides for ROM Developers**  
9️⃣3️⃣ How to create a minimal GSI (Generic System Image)?  
9️⃣4️⃣ How do I optimize firmware extraction for low-powered devices?  
9️⃣5️⃣ How to repack vendor_boot.img with new kernel patches?  

🔥 **Final Optimization Techniques**  
9️⃣6️⃣ How do I test a custom ROM before flashing it?  
9️⃣7️⃣ How to verify boot.img integrity before flashing?  
9️⃣8️⃣ How to optimize an Android build system for faster compilation?  
9️⃣9️⃣ How to remove unnecessary partitions from a ROM build?  
1️⃣0️⃣0️⃣ How to generate flashable ZIP files for custom ROM updates?  

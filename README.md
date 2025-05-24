

# 🔧 Android Dev Tools  
🚀 **A collection of powerful tools designed to kickstart Android ROM development and customization.** 🚀  

**🔹 Beginner-friendly** – As a learner in Android ROM development, I've gathered tools along my journey to make them easily accessible for new developers.

📁 **Get the Files from Releases:**  
[🔗 Android Dev Tools Release](https://github.com/PROPGSP/Android-Dev-Tools/releases/tag/release)  


---

## 🏆 **A Kind Request to Expert Android ROM Developers**  

🙏 **Calling all experienced Android ROM developers!**  
As a beginner in **Android ROM development**, I have collected some tools to make learning easier. But I know that there are **many more powerful tools** that experts like you use daily.  

📢 **If you know any must-have tools that simplify ROM development, firmware extraction, kernel tweaking, or device tree modifications, kindly share them!**  
Your knowledge can help **new developers** kickstart their journey more efficiently.  

---

## 🚀 **Existing Tools in This Repository**  
| 🔹 **Tool** | 🏷️ **Developer** | 🔗 **Repository** |
|------------|-----------------|------------------|
| **Binwalk** | @ReFirmLabs | [🔗 GitHub Repo](https://github.com/ReFirmLabs/binwalk) |
| **DTC (Device Tree Compiler)** | @dgibson | [🔗 GitHub Repo](https://github.com/dgibson/dtc) |
| **Payload Dumper Go** | @ssut | [🔗 GitHub Repo](https://github.com/ssut/payload-dumper-go) |
| **Android Boot Image Editor** | @cfig | [🔗 GitHub Repo](https://github.com/cfig/Android_boot_image_editor) |

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




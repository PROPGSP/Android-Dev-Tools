🔧 Android Dev Tools 🔧

A collection of powerful tools for made to init the Android ROM development and customization.

Im an Beginner in Android Rom Development.

I just uploaded the tools that i meet in my learning journey to make sure its easily reachable to new learners.


📁 Get the files from releases [https://github.com/PROPGSP/Android-Dev-Tools/releases/tag/release]

An Advice:
Striclty use linux for all the process for rom development.
Never use wsl because it has too many limitations.
Bare metal linux is the best choice.
If you cant install an linux or hire an linux vm, in this case use Oracle Virtual Box [https://www.virtualbox.org/]. This option is far more better than using wsl. But you will get very slow build that can take time from several hours to days. 
Check out the minimum requirements [https://source.android.com/docs/setup/start/requirements]

🛠️ Usage :
You can find the apropriate tool's readme.md which will have an detailed use cases.
Well this below is a very very short overview of use cases of tools for nerds.
Please read the readme.md file inside each tool folder to get better understanding if you are an beginner.

If you have OTA update firmware package with payload.bin :

Use payload dumper go inside the folder payload.bin-unpackor.
1) Set the permission to the tool:
```
chmod +x payload-dumper-go 
```
2) Add payload-dumper-go . replace the /path/to/payload-dumper-go with appropriate path
```
export PATH=$PATH:/path/to/payload-dumper-go
```
3) unpack payload.bin
```
payload-dumper-go /path/to/payload.bin
```
Now you have unpacked the payload.bin . 
You may see files like this boot, dtbo, gcf, gsa, gsa_bll, init_boot, Idfw, modem, pbl, product, pvmfw, system, system_dlkm, system_ext, tzsw, vbmeta, vbmeta_system, vbmeta_vendor, vendor, vendor_ boot ,vendor_dlkm, vendor_kernel_boot images.
Not all payload have all this above images. It varies for different phones and models and brands.
Now skip the below step to unpack sparse image.


If you dont have OTA payload.bin Instead you have some images and some sparsechunks .
![image](https://github.com/user-attachments/assets/e927faec-a74d-47e6-b863-3e4fd7f99b71)

Then you can unpack the super.img from the sparse chunks and then unpack the super.img into its partitions and then mount or unpack the erofs images

use the simg2imgtool inside this folder super.img-extractor:
replace "system.img_sparsechunk.*" with your appropriate sparsechunk names
```
./simg2img system.img_sparsechunk.* super.img
```

use lpunpack to unpack the super.img into its partitions
```
./lpunpack super.img /super_unpacked
```
now inside the super_unpacked folder you will get the android partitons images like system , vendor or system_a , system_b , vendor_a ,vendor_b images 

In some cases it will give some error like vendor_b not found or system_b not found.
This happens because the firmware file doesnt have such partitions. If you know about slot A/B then you will understand.

To fix just touch to create an dump file with that name inside the /super_unpacked folder
```
mkdir super_unpacked; cd super_unpacked; touch vendor_b.img
```
Replace the vendor_b,img with the appropriate name that you got in the error message.

now will get the unpacked images inside the super_unpacked folder.


Now you all have the all img files of firmware unpacked into .img file format.

If you want to unpack kernel, device tree blobs, prop files, partitions data and more:
Now use Android_boot_editor indise the folder image-unpack-repack.
The Android_boot_editor probably the best for unpacking and repacking boot.img , venndor.img ,system.img

before using Android_boot_editor run 
```
file *.img
```
To get the format of files .

If you see erofs files then skip the below step to erofs unpacking.
copy the img file you want to extract into the image-unpack-repack folder and then run
```
./erofsUnpackRust_x64linux system_a.img
```
you will get the unpacked files. or probably you just mount if you are in bare metal linux.

you can use binwalk to search and unpack those files.
```
binwalk -Me  name.img
```
you will get the extracted files inside the the folder like extracted_2389235.
if you want you can specify the output directory also. but this is and very short overview so you guys refer readme inside the image-unpack-repack

In case you need to examine what the other .img files have but no need extract, just use 
```binwalk -Me -r name.img```
You will see the list of files in it.

the kernel and device tree blob might be located in different partittions for different model and brand phones.
the correct use of binwalk will get you extract those files.

If you cant find those with binwalk 

```
./gradlew unpack
```
now you will get the unpacked files inside /image-unpack-repack/build/ folder
after extracting each img files delete that img file and clear the build folder to avoid any problems.
Android_boot_editor has only limited and enough file format support to unpack and repack . but this supported files formats are enought for our rom development process.




📜 License

This project is is just an collections of tools. Each tool in this project has the Liscense copy of it.


🏆 Credits



Thank You To Developer for making these Tools

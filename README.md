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
Well this below is overview of use cases of tools for nerds.

If you have OTA update firmware package with payload.bin :

Use payload dumper go inside the folder payload.bin-extractor.
1) Set the permission to the tool:
```
chmod +x payload-dumper-go 
```
2) Add payload-dumper-go . replace the /path/to/payload-dumper-go with appropriate path
```
export PATH=$PATH:/path/to/payload-dumper-go
```
3) Extract payload.bin
```
payload-dumper-go /path/to/payload.bin
```
Now you have extracted the payload.bin . 
You may see files like this boot, dtbo, gcf, gsa, gsa_bll, init_boot, Idfw, modem, pbl, product, pvmfw, system, system_dlkm, system_ext, tzsw, vbmeta, vbmeta_system, vbmeta_vendor, vendor, vendor_ boot ,vendor_dlkm, vendor_kernel_boot images.
Not all payload have all this above images. It varies for different phones and models and brands.

If you dont have OTA payload.bin Instead you have some images and some sparsechunks .
![image](https://github.com/user-attachments/assets/e927faec-a74d-47e6-b863-3e4fd7f99b71)

Then you can extract the super.img from the sparse chunks and then extract the super.img into its partitions and then mount or extract the erofs images


If you want to extract kernel, device tree blobs, prop files, partitions data and more:




📜 License

This project is is just an collections of tools. Each tool in this project has the Liscense copy of it.


🏆 Credits



Thank You To Developer for making these Tools

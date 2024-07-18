- Downloaded latest version
- Started it up
- When I selected a linux flavour, I got an error saying it couldn't find the ISO image
- So, searched for Rocky 9 image
- This article said I need to download it:
	- https://medium.com/@selvarajk/install-redhat-enterprise-linux-9-in-virtual-box-d98edb7daa68
- But I google it and and downloaded it from
	- https://rockylinux.org/download
	- NOTE: There are three:
		- **Minimal ISO** - is typically used to install a minimal Rocky Linux environment without downloading the entire DVD image or using the `boot` ISO to do so. - ***This is the one I used.***
		- **DVD ISO** - also known as the "everything" or "BaseOS" media, contains everything needed to do a custom installation of Rocky Linux without needing an internet connection.
		- **Boot ISO** - also known as the "net install" media, is used to perform Rocky Linux installations over the internet.
		
- **Next error:** when booting it said: 
```
isolinux 6.04 etcdisolinux image checksum error sorry
```
- Googled it, and this seemed to work:
	- https://sangkyu519.medium.com/install-rhel-with-virtualbox-fix-checksum-error-daba1bf566b0
- The VM then booted up properly.
- NOTE: **dnf** is *Dandified Yum*, a package manager for Rocky Linux:
	- https://docs.rockylinux.org/guides/package_management/dnf_package_manager/
- 
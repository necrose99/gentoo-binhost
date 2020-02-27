# gentoo binhost

Providing [gentoo](https://gentoo.org/) binary packages using [github](https://github.com/) infrastructure.

<div style="display: inline"><img src="https://raw.githubusercontent.com/wiki/spreequalle/gentoo-binhost/images/gentoo-logo.png" alt="gentoo-logo" width="160" /></div>

This repo provides various gentoo [binary packages](https://wiki.gentoo.org/wiki/Binary_package_guide) for a variety of different architectures (checkout branches for details). This branch contains the script that is used for GitHub upload.

Concept
-------

The package upload is realized using a small upload script thats executed via portage [hooks](https://wiki.gentoo.org/wiki//etc/portage/bashrc). For every package that is being merged via portage the Gentoo _Packages_ manifest file is committed to Git. The binary packages itself are not stored into repository there are uploaded as [GitHub release](https://developer.github.com/v3/repos/releases) artifacts.

To make everything work the following nomenclature has to apply:

Gentoo idiom|GitHub entity
------------|-------------
[CATEGORY](https://wiki.gentoo.org/wiki//etc/portage/categories)|GitHub release
[PF](https://devmanual.gentoo.org/ebuild-writing/variables/)|GitHub release asset
[CHOST](https://wiki.gentoo.org/wiki/CHOST)|Git branch name
[CHOST](https://wiki.gentoo.org/wiki/CHOST)/[CATEGORY](https://wiki.gentoo.org/wiki//etc/portage/categories)|Git release tag

## Usage
-----

Setup a gentoo binhost Github and provide the following.

### Dependencies

The upload script uses Python3 and [PyGithub](https://github.com/PyGithub/PyGithub) module.

    emerge dev-python/PyGithub
    

### Configuration

github upload can be easily configured.

#### make.conf

Enable gentoo binhost by adding the following lines.

    # enable binhost
    PORTAGE_BINHOST_HEADER_URI="https://github.com/necrose99/gentoo-binhost/releases/download/${CHOST}"
    Our local fork.
    
    PORTAGE_BINHOST_HEADER_URI="https://github.com/spreequalle/gentoo-binhost/releases/download/${CHOST}"
    FEATURES="${FEATURES} buildpkg"
    USE="${USE} bindist"
    ACCEPT_LICENSE="-* @BINARY-REDISTRIBUTABLE"

  

* * *

### bashrc

Add the [/etc/portage/bashrc](https://wiki.gentoo.org/wiki//etc/portage/bashrc) file below, if you use your own file make sure to call the **gh-upload.py** script during **postinst** phase.

    #!/bin/env bash
    
    if [[ ${EBUILD_PHASE} == 'postinst' ]]; then
    # FIXME come up with a more sophisticated approach to detect if binary package build is actually requested
    # commandline args like -B or --buildpkg-exclude and other conditionals are not supported right now.
    grep -q 'buildpkg' <<< {$PORTAGE_FEATURES}
    if [ $? -eq 0 ]; then
    /etc/portage/binhost/gh-upload.py
    fi
    fi
    

#### gh-upload.py

Add the [/etc/portage/binhost/gh-upload.py](/etc/portage/binhost/gh-upload.py) script and add your github settings accordingly. You need to create a [github access token](https://help.github.com/en/articles/creating-a-personal-access-token-for-the-command-line) that is able to access repository and create releases.

    gh_repo = 'spreequalle/gentoo-binhost'
    gh_token = '<your github access token>'

* * *

    Overides gh_branch = 'master' aarch64-coretexa53-linux-gnu or aarch64-coretexa53-linux-gnu_systemd since i havent an optimized compiler yet made.. 

* * *

Disclaimer
----------

Although this software is released under [JSON](/LICENSE) license, the binary packages come with their respective license according to _Packages_ Manifest file. Refer to [gentoo license](https://devmanual.gentoo.org/general-concepts/licenses/index.html) for details.  

* * *

 <table cellspacing="2" cellpadding="2" height="368" width="566"
border="1">
<caption><b>Active Branches</b></caption> <tbody>
<tr>
<td valign="top"><font size="+1"><a
href="https://github.com/necrose99/gentoo-binhost/tree/aarch64-coretexa53-linux-gnu"><b>aarch64-coretexa53-linux-gnu</b></a><br>
<b>generic arm64 but generally optomized for</b><br>
</font></td>
<td valign="top">Necrose99 <br>
Suited to RPI/Rockpro64/pine-book-Pro <a
href="gihub.com/pentoo">gihub.com/pentoo</a> <br>
<b><u><i>unofficial aarch64<br>
respins of <br>
</i></u></b><a
href="https://github.com/sakaki-/gentoo-on-rpi-64bit">https://github.com/sakaki-/gentoo-on-rpi-64bi</a>
</td>
</tr>
<tr>
<td valign="top">else upstream I may add more my own. latter.
<br>
</td>
<td valign="top"><a
href="https://github.com/spreequalle/gentoo-binhost">https://github.com/spreequalle/gentoo-binhost</a><br>
</td>
</tr>
<tr>
<td valign="top"><br>
</td>
<td valign="top"><br>
</td>
</tr>
<tr>
<td valign="top"><br>
</td>
<td valign="top"><br>
</td>
</tr>
<tr>
<td valign="top"><br>
</td>
<td valign="top"><br>
</td>
</tr>
<tr>
<td valign="top"><br>
</td>
<td valign="top"><br>
</td>
</tr>
<tr>
<td valign="top"><br>
</td>
<td valign="top"><br>
</td>
</tr>
<tr>
<td valign="top"><br>
</td>
<td valign="top"><br>
</td>
</tr>
</tbody>
</table  

  

  

  

  

  

  

  

  

  

  

  

  

###   
**`SIDE notes.`**

"bin-host- url-1 +2 " can also be done , on amd64 i offten have sabayon's devs , or Redcore and @Pentoo on binhosts, to get base packages before spec'ing them to my like , tens too be easiey just just add a few features  
  
binhost.conf and sourcing it as an arry into make.conf, has come to mind.  
can have multiple urls in "Quotes"  
  
PORTAGE\_BINHOST="${BINHOST}"  
BINHOST"${BINHOST}" +N  
BINHOST2=["https://isshoni.org/pi64pie/"](https://isshoni.org/pi64pie/)  
\# RPI64 Gentoo [https://github.com/sakaki-/gentoo-on-rpi-64bit](https://github.com/sakaki-/gentoo-on-rpi-64bit) binhost , i may add or adlib to upstream on my branch ie add pentesting or more features, or binpkg-multi-instance to have more of the same package , but with differing flags.  
i had before on the SATA HDD or "dropbox binhost of sneaker-net.." however the xpack feature has pro;s cons, namely it sometimes portage wont see remotely .  
however if networkmanger is rebuilt for openrc/then systemd package manager will offer the systemd one to systemd users.. that's be the pro , or differing use flags based on users tastes.  
  
3= etc helps with many many ...  
ie nested arrays. old Sabayon make.conf used these to keep uses in make.conf easy to find and edit.  
  

* * *

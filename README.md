# ppscsi
ppSCSI 0.92 modifications for newer kernels (6.5)

# Some notes from Dave Gilbert
Starting with Emilio Moretti's version, I've hacked this forward
to the 6.5 kernel.  I've disabled all except epst for the moment
(although the changes to the individual drivers are small).

I've tested the epst set with my HP5100C scanner on PCIe parport
cards (both wch_ch382 and MosChip/Netmos/Asix 9912).  They work
OK, but neither manages anything more than Mode 2 (PS/2) with the
scanner (although the 9912 identifies itself as being able to do
more in the parport driver).

dave@treblig.org

# Steps to compile

```
make
sudo make install
sudo make load
```
You may want to add this rule to udev:
```
sudo /bin/sh -c 'echo "SUBSYSTEM==\"scsi_generic\",ATTRS{type}==\"3\", SYMLINK=\"scanner%n\", MODE=\"0777\", GROUP=\"scanner\"" >> /etc/udev/rules.d/45-libsane.rules'
sudo service udev restart
```

# Scan
```
sudo sane-find-scanner
scanimage -d hp:/dev/sg1 --mode Color > scanImage
```
OR
```
xsane
```
OR
```
simple-scan
```

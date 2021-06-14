# freeathome_root
HowTo "rooting" ABB / B+J Free@Home SysAP

After You have Installed Old Firmware < 2.1.3 (=2.1.3 not tested)

Todo:

Vorbedingungen:
- Wir brauchen:
	- aktuelle Ziel-Firmware von ABB
	- Firmware mit einen Stand mutmaßlich > 2.1.4, sicher geht die 2.1.2
	- eigenes PGP Schlüsselpaar 
	- eigenes SSH Schlüsselpaar

- Aktuelle Firmware bearbeiten:
	- mkdir firmware
	- mkdir root_tgz
	- mkdir rescue_tgz
	- cp $firmware.img firmware/
	- cd firmware
	- unzip firmware.img
	- cd images
	- rm checksum2*
	- mv root.tgz ../../root_tgz/
	- mv rescue.tgz ../../rescue_tgz
	- cd ../../root_tgz/
	- gzip -d root.tgz
	- tar -vxpzf root.tar usr/sbin/check-key.sh etc/gnupg/pubring.gpg root/.ssh/authorized_keys

	-> pubring austauschen mit eigenen; check-key anpassen für self-test Aktivierung, authorized_keys anpassen

	- tar -vupf root.tar usr/sbin/check-key.sh etc/gnupg/pubring.gpg root/.ssh/authorized_keys
	- gzip -9 root.tar
	- cp root.tar.gz ../firmware/images/root.tgz
	- cd ../rescue_tgz/
	- gzip -d rescue.tgz
	- tar -vxpzf rescue.tar etc/gnupg/pubring.gpg
	- cp ../root_tgz/etc/gnupg/pubring.gpg  etc/gnupg/pubring.gpg
	- tar -vupf rescue.tar etc/gnupg/pubring.gpg
	- gzip -9 rescue.tar
	- cp rescue.tar.gz ../firmware/images/rescue.tgz
	- cd ../firmware/images/
	- gpg --detach-sig --armor --output checksum2.sig checksum2
	- cd .. ; zip -r -9 firmware_hacked.img META-INF images sbin



t.b. documented ....

# freeathome_root
HowTo "rooting" ABB / B+J Free@Home SysAP 1.0 UND SysAP 2.0

Todo:

Vorbedingungen - ziehmlich keine
- Wir brauchen:
	- aktuelle Ziel-Firmware von ABB/B+J	- 
	- eigenes SSH Schlüsselpaar
	- Linux Shell mir root

- Optionale Dinge:
	- eigenes PGP Schlüsselpaar (dann können wir unsere zukünftige Software gleich selbst signieren wobei das bis 2.6.3 keinen Sinn macht) 
	- AutoUpdate ausgeschalten (sonst macht die Arbeit auf Dauer keinen Sinn ;)
	- Alternativ, kann man die Pubring auch so tauschen das B+J Software nicht mehr akzeptiert wird, 
	  im Sinne von Schlüsselverlust, Verkauf, Downgrade, etc. empfehle ich das nicht.

- Aktuelle Firmware bearbeiten:
	- mkdir firmware
	- mkdir root_tgz
	- mkdir rescue_tgz
	- cp $firmware.img firmware/
	- cd firmware
	- unzip $firmware.img
	- cd images
	- rm checksum2*
	- mv root.tgz ../../root_tgz/
	- mv rescue.tgz ../../rescue_tgz
	- cd ../../root_tgz/
	- gzip -d root.tgz
	- weiter als "root"
	=== ab hier ist root erforderlich ===
	- tar -vxpzf root.tar usr/sbin/check-key.sh etc/gnupg/pubring.gpg root/.ssh/authorized_keys

			- authorized_keys anpassen
			- Optional: pubring austauschen mit eigenen 
			- Optional: check-key anpassen ( Test auf /static/locked auskommentieren ) aber nur wenn man aus irgendwelchen Gründen telnet per Tastendruck haben will)

	- tar -vupf root.tar usr/sbin/check-key.sh etc/gnupg/pubring.gpg root/.ssh/authorized_keys
	=== ende root erforderlich ===
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
	- cp ../META-INF/* .
	- cd .. ; zip -r -9 firmware_hacked.img META-INF images sbin

- auf dem SysAp
	- Update starten mit der neuen "firmware_hacked.img"

- Have Fun!
	- ssh -l root $BOXIP

Laubi

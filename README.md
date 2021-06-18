# freeathome_root
HowTo "rooting" ABB / B+J Free@Home SysAP 1.0

Todo:

Vorbedingungen:
- Wir brauchen:
	- aktuelle Ziel-Firmware von ABB
	- Firmware mit einen Stand mutmaßlich > 2.1.4, sicher geht die 2.1.2
	- eigenes PGP Schlüsselpaar 
	- eigenes SSH Schlüsselpaar
	- AutoUpdate ausgeschalten (sonst macht die Arbeit keinen Sinn ;)
	- Alternativ, kann man die Pubring auch so tauschen das B+J Software nicht mehr akzeptiert wird, 
	  im Sinne von Schlüsselverlust, Verkauf, Downgrade, etc. empfehle ich das nicht.

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

		- pubring austauschen mit eigenen; 
		- check-key anpassen ( Zeilen mit test auf /static/locked auskommentieren ) 
		- authorized_keys anpassen

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

- Sysap downgraden auf < 2.1.3, 
	- siehe https://github.com/markuslaube/freeathome_downgrade

- Aktivieren Telnet:
        - Ich empfehle die Verwendung eines LAN Kabels an der Box
	- Box Öffnen, Reset Button drücken, rechte Taste lange drücken
	- sleep 360
	- telnet 192.168.10.99 (dein rechner muss dafür  im netz 192.168.10.0/24 sein)
	- login: root
	- passwort ... fragt gar niemand erst ;)

- /root/.ssh/authorized_keys manuell anpassen

- ssh dienst starten 
 	- (im Browser einstellbar)
- Pubring tauschen kopieren
	- /etc/gnupg/pubring.gpg  

- Update auf eigene Firmware starten
        - normal wie immer über Browser, nur ab sofort mit der eigenen

- Restore Backup
	- ggf. SSH wieder aktivieren

- Have Fun!
	- ssh -l root $BOXIP

Laubi

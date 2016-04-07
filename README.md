# lxc-e-deklaracje
Jak utworzyć kontener zawierający program do rozliczeń podatkowych 2015

- Instalujemy lxc

```
sudo apt-get install lxc
```

- Tworzymy kontener

```
sudo  lxc-create -n e-deklaracje -t ubuntu
```

- Uruchamiany kontener

```
sudo lxc-start -n e-deklaracje
```

-  Weryfikujemy adres IP kontenera

```
lxc-info --name e-deklaracje  | grep IP
```

- Logujemy się przez ssh na maszynę - domyślne hasło `ubuntu`

```
ssh [cnt_ip] -l ubuntu
```

- Opcjonalnie zmieniamy hasło użytkownika

```
passwd
```

- Przechodzimy na konto root na kontenerze

```
sudo su -
```

- Dodajemy architekturę i386

```
dpkg --add-architecture i386
```

- Aktualizujemy system

```
apt update && apt upgrade -y
```

-  Instalujemy wymagane pakiety

```
apt install libxslt1.1 libgtk2.0-0:i386 libstdc++6:i386 libnss3-1d:i386 lib32nss-mdns libxml2:i386 \
               libxslt1.1:i386 libcanberra-gtk-module:i386 gtk2-engines-murrine:i386 libqt4-qt3support:i386 \
               libgnome-keyring0:i386 wget xorg -y
```

- Ściągamy paczkę Adobe Reader

```
wget ftp://ftp.adobe.com/pub/adobe/reader/unix/9.x/9.5.5/enu/AdbeRdr9.5.5-1_i386linux_enu.deb
```

- Ściągamy Adobe Air

```
wget http://airdownload.adobe.com/air/lin/download/latest/AdobeAIRInstaller.bin
```

- Instalujemy Adobe Reader

```
dpkg -i AdbeRdr9.5.5-1_i386linux_enu.deb
```

- Instalujemy Adobe Air

```
chmod +x AdobeAIRInstaller.bin
```

- Wylogowujemy się z maszyny

- Logujemy się jeszcze raz z Forward X

```
ssh [cnt_ip] -l ubuntu -X
```

- Przechodzimy na konto root

```
sudo su - 
```

- Przekazujemy uprawnienia do X-Servera

```
xauth merge /home/ubuntu/.Xauthority
```

- Instalujemy Adobe AIR

```
./AdobeAIRInstaller.bin
```

- Instalujemy e-Deklaracje

```
/opt/Adobe\ AIR/Versions/1.0/Adobe\ AIR\ Application\ Installer  `pwd`/e-DeklaracjeDesktop.air
```

- Wyłączamy aplikacje i opuszczamy konto roota

- Na użytkowniku ubuntu uruchamiany e-Deklaracje

```
/opt/e-Deklaracje/bin/e-Deklaracje
```

- W przyszłości, aby uruchomić program wystarczy polecenie z hosta - przy włązonym kontenerze

```
ssh [cnt_ip] -l ubuntu -X /opt/e-Deklaracje/bin/e-Deklaracje
```

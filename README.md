.appdata/e-Deklaracje# lxc-e-deklaracje
Jak utworzyć kontener zawierający program e-Deklaracje 2016 do rozliczeń podatkowych za rok 2015.

Autor nie ponosi odpowiedzialności za nieprawidłową instalację programu do rozliczeń (w szczególności błędów rozliczeń wynikłych z nieprawidłowego działania). Wykorzystanie instrukcji na własną odpowiedzialność.

## Stworzenie kontenera
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
sudo lxc-info --name e-deklaracje  | grep IP
```

- Logujemy się przez ssh na maszynę - domyślne hasło `ubuntu`

```
ssh [cnt_ip] -l ubuntu
```

- Opcjonalnie zmieniamy hasło użytkownika

```
passwd
```

## Alternatywa: Wykorzystanie skryptu
Należy umieścić [skrypt](download-and-install-adobe) w kontenerze i uruchomić go z uprawnieniami root 

```
sudo ./download-and-install
```

## Alternatywa: Ręczne wykonanie wszystkich komend
- Przechodzimy na konto root w kontenerze

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

- Tworzymy dowiązania symboliczne

```
ln -s /usr/lib/i386-linux-gnu/libgnome-keyring.so.0 /usr/lib/libgnome-keyring.so.0
ln -s /usr/lib/i386-linux-gnu/libgnome-keyring.so.0.2.0 /usr/lib/libgnome-keyring.so.0.2.0
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

- Nadajemy uprawnienia wykonywalne do Adobe Air

```
chmod +x AdobeAIRInstaller.bin
```

## Finalizacja instalacji
- Wylogowujemy się z maszyny
- Ustawiamy w ~/.ssh/config (zamiast [cnt_ip] mozna wpisać *)
```
  Host [cnt_ip]
    ForwardX11 yes
```


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

- [Opcjonalnie jeśli mamy z lat poprzednich katalog z ustawieniami ~/.appdata/e-Deklaracje* można go teraz skopiować na maszynę lxc w to samo miejsce, to jest ~/.appdata]

- Na użytkowniku `ubuntu` uruchamiany e-Deklaracje

```
/opt/e-Deklaracje/bin/e-Deklaracje
```

- W przyszłości, aby uruchomić program wystarczy polecenie z hosta - przy włączonym kontenerze

```
ssh [cnt_ip] -l ubuntu -X /opt/e-Deklaracje/bin/e-Deklaracje
```

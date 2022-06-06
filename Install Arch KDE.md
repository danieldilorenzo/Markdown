## **_Instalar ArchLinux_**<br><br>

> Essa é a configuração que funciona para mim, baseado em meu HD e meu uso dos programas. sendo necessário adaptar de acordo com tua necessidade. Cada linha deve ser executada uma por vez, a não ser que esteja num bloco (ver sessão "instalar KDE").<br>

<br><br>

---

<br>

## Teclado em Pt-Br<br>

<br>

`loadkeys br-abnt2`

<br>

---

<br>

## Verificar conexão

<br>

`ping -c 3 archlinux.org`

<br>

---

<br>

## Verificar o particionamento

<br>

`fdisk -l`
<br><br>

---

<br>

## Particionar o disco

<br>Dar o comando<br><br>
`cfdisk /dev/sda`<br><br>
Particularmente, eu deixo minhas partições assim:

```
sda1	   500m	        EFI
sda2	   10g	        Swap
sda3	   resto	    Linux Filesystem
```

<br>

De acordo com a [ArchWiki](https://wiki.archlinux.org/title/EFI_system_partition#Create_the_partition), o tamanho mínimo da partição EFI é de 300m, mas é recomendado 512m

<br>

---

<br>

## Verificar se o particionamento está correto

<br>

`fdisk -l`

<br>

---

<br>

## Criar as partições

<br>
Executar uma linha por vez:<br><br>

`mkfs.fat -F32 /dev/sda1`<br>
`mkswap /dev/sda2`<br>
`swapon /dev/sda2`<br>
`mkfs.ext4 /dev/sda3`

<br>

---

<br>

## Iniciar a instalação do sistema

<br>

`pacman -Syy`<br>
`mount /dev/sda3 /mnt`<br>
`pacstrap /mnt base base-devel linux linux-firmware sudo nano`<br>

---

<br>

## Configurar o sistema instalado

<br>

`genfstab -U /mnt >> /mnt/etc/fstab`<br>
`arch-chroot /mnt`
<br><br>

---

<br>

## Configurar Timezone

<br>

`ln -sf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime`
<br><br>

---

<br>

## Configurar Locale.gen

<br>

`nano /etc/locale.gen`
<br><br> Descomentar as linhas:

```
#en_US.UTF-8 UTF-8
#en_US ISO-8859-1
#pt_BR.UTF-8 UTF-8
#pt_BR ISO-8859-1<br>
```

<br>
Executar depois:
<br><br>

`locale-gen`<br>
`echo LANG=pt_BR.UTF-8 > /etc/locale.conf`<br>
`export LANG=pt_BR.UTF-8`<br><br>

---

<br>

## Configurar nome da máquina

<br>

`echo arch > /etc/hostname`

<br><br>

---

<br>

## Configurar hosts

<br>

`nano /etc/hosts`
<br><br>

```
127.0.0.1      localhost
::1            localhost
127.0.1.1      arch
```

<br><br>

---

<br>

## Configurar senha

<br>

`passwd`
<br><br>

---

<br>

## Instalar Grub

<br>

`pacman -S grub efibootmgr os-prober mtools`<br>
`mkdir /boot/efi`<br>
`mount /dev/sda1 /boot/efi`<br>
`grub-install --target=x86_64-efi --bootloader-id=grub_uefi`<br>
`grub-mkconfig -o /boot/grub/grub.cfg`<br>
<br>

---

<br>

## Instalar kde

<br>

```
pacman -S xorg-server xorg-apps xf86-video-intel plasma sddm dolphin kwalletmanager packagekit-qt5 neofetch wget git plasma-wayland-session konsole spectacle power-profiles-daemon gnome-boxes partitionmanager ktorrent kate ffmpegthumbs acpi telegram-desktop vlc android-tools ttf-fira-code ntfs-3g ffmpegthumbnailer gst-libav gst-plugins-ugly noto-fonts-emoji pulseaudio-alsa pulseaudio-bluetooth bluez-utils
```

<br><br>

---

<br>

## Habilitar gerenciador de janelas, rede e bluetooth

<br>

`nano /etc/bluetooth/main.conf`
<br><br>
(mudar autoenable para true)<br><br>

`sudo systemctl enable sddm`<br>
`systemctl enable NetworkManager`<br>
`sudo systemctl enable bluetooth.service`<br><br>

---

<br>

## Criar Usuário

<br>

`useradd -m -G wheel daniel`<br>
`passwd daniel`<br>
`EDITOR=nano visudo`<br><br>
Descomentar a linha abaixo<br>

```
# %wheel ALL=(ALL) ALL
```

(fechar e salvar o documento com `ctrl+O` e `ctrl+X` )

<br><br>

---

<br>

## Finalizar a instalação e reiniciar a máquina

<br>

`exit`<br>
`umount -R /mnt`<br>
`reboot`
<br><br>

## Notas:

<br>
- Talvez após isso, dê erro ao subir o sistema. Você terá que entrar na BIOS e mudar o boot para a entrada que você instalou o grub, de acordo com a sessão "Instalar Grub", no meu caso<br><br>

```
\EFI\grub_uefi\grubx64.efi
```

<br>

---

<br>

## Referências (material de apoio Markdown)

<br>

[Sintaxe básica de escrita e formatação no GitHub](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)<br>
[Github - Dominando o Markdown](https://docs.github.com/pt/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)<br>
[MarkdownGuide](https://www.markdownguide.org/cheat-sheet/)<br>
[Guia básico de Markdown](https://docs.pipz.com/central-de-ajuda/learning-center/guia-basico-de-markdown#open)<br>

# 🔑 How to setup Yubico Manager

## GUI

* Download [Appimage]
* Create file: `sudo touch /etc/udev/rules.d/70-u2f.rules`
* Copy paste [rule]

## Terminal

* Add [PPA] and `sudo apt install yubikey-manager`

Related:

* <https://support.yubico.com/hc/en-us/articles/360016648939-Troubleshooting-Failed-connecting-to-the-YubiKey-Make-sure-the-application-has-the-required-permissions-in-YubiKey-Manager>

[PPA]: <https://support.yubico.com/hc/en-us/articles/360016649039-Enabling-the-Yubico-PPA-on-Ubuntu>
[Appimage]: <https://github.com/Yubico/libu2f-host/blob/master/70-u2f.rules>
[rule]: <https://www.yubico.com/support/download/yubikey-manager/>

Tags:

    #linux #security #yubikey #setup

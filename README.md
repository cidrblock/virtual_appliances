## vyos prep

```

cp vyos-1.2.0-rolling+201905140337-amd64.iso vyos.iso
sudo rm vyos.img
qemu-img create vyos.img 2G

/usr/libexec/qemu-kvm -nographic -m 6144 \
                                 -smp 2 -enable-kvm \
                                 -serial telnet:localhost:8888,server,nowait \
                                 -netdev user,id=net0 \
                                 -device e1000,netdev=net0 \
                                 -drive file=vyos.img,if=ide,format=raw \
                                 -cdrom vyos.iso

install image

/usr/libexec/qemu-kvm -nographic -m 6144 \
                                 -smp 2 -enable-kvm \
                                 -serial telnet:localhost:8888,server,nowait \
                                 -netdev user,id=net0 \
                                 -device e1000,netdev=net0 \
                                 -drive file=vyos.img,if=ide,format=raw

configure
set system login user admin authentication plaintext-password password
set system login user bthornto authentication plaintext-password password
set system login user admin level admin
set system login user bthornto level admin
set service ssh
delete interfaces ethernet eth0 hw-id
set service lldp
commit
save



sudo rm vyos.qcow2
qemu-img convert -f raw -O qcow2 vyos.img vyos.qcow2

```

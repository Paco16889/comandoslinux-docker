# Solución — Red y paquetes

## Ejercicio 1

```bash
ping -c 3 127.0.0.1
ping -c 3 8.8.8.8
hostname
ip addr
# o: ip a
```

## Ejercicio 2

```bash
ss -tulpn
ss -tulpn | grep -E ':22|:80|:443'
```

## Ejercicio 3

```bash
mkdir -p ~/practica-linux
curl -I https://example.com
# HTTP/… 200 (o redirección)

curl -L https://example.com -o ~/practica-linux/descarga.html
# o: wget -O ~/practica-linux/descarga.html https://example.com

ls -l ~/practica-linux/descarga.html
```

## Ejercicio 4

```bash
# Debian/Ubuntu/Mint
sudo apt update
apt search '^tree$'
sudo apt install tree

# Fedora
# sudo dnf install tree

# Arch
# sudo pacman -S tree

tree -L 1 ~/practica-linux
```

Flatpak (Mint): apps gráficas / sandboxes; `apt` para lo del sistema.

# Deployment Flow

Tai lieu nay tong hop luong thao tac toi gian duoc giu lai tu command history. Cac buoc thu nghiem that bai nhu chay `headscale` bang Docker hoac tai binary sai URL khong duoc dua vao flow chinh.

## 1. Cai dat goi he thong can thiet

```bash
sudo apt update -y
sudo apt install -y nginx certbot python3-certbot-nginx
```

Command history cung ghi nhan viec cai dat them `curl`, `wget`, `git`, `ufw`, `ca-certificates`, `gnupg`, `lsb-release`.

## 2. Cau hinh Nginx

File cau hinh duoc luu trong repo la `config/headscale.conf`.

Nhung diem xac thuc duoc:

- redirect HTTP sang HTTPS
- reverse proxy toi `127.0.0.1:8080`
- bat `proxy_http_version 1.1`
- chuyen tiep `Upgrade` va `Connection "upgrade"`

Lenh kiem tra va nap lai da xuat hien trong command history:

```bash
sudo nginx -t
sudo systemctl restart nginx
sudo systemctl reload nginx
```

## 3. Cap chung chi

Command history ghi nhan mot lan cap chung chi that bai do `NXDOMAIN`, sau do thanh cong:

```bash
sudo certbot --nginx -d headscale.example.com
```

## 4. Cai dat Headscale

Flow duoc command history xac nhan:

```bash
HEADSCALE_VERSION="0.28.0"
HEADSCALE_ARCH="amd64"

wget -O headscale.deb \
  "https://github.com/juanfont/headscale/releases/download/v${HEADSCALE_VERSION}/headscale_${HEADSCALE_VERSION}_linux_${HEADSCALE_ARCH}.deb"

sudo apt install -y ./headscale.deb
sudo systemctl enable --now headscale
```

Command history ghi nhan dich vu sau do chay duoi `systemd` va lang nghe tren `127.0.0.1:8080`.

## 5. Tao user va preauth key

Lenh duoc xac nhan trong command history:

```bash
sudo headscale namespaces create cuongpm
sudo headscale users list
sudo headscale preauthkeys create --user 1 --reusable --expiration 24h
```

Lenh `sudo headscale preauthkeys create -n cuongpm --reusable --expiration 24h` da that bai va khong thuoc flow chinh.

## 6. Debug duoc giu lai

Nhung lenh debug co xuat hien trong command history:

```bash
sudo journalctl -u headscale -f
ss -tulnp | grep 8080
tailscale status --json
```

## 7. Diem chua duoc xac nhan

- Command history co lenh `tailscale up --login-server https://headscale.example.com --authkey <REDACTED>`, nhung tren may da ghi log thi `tailscale` khong ton tai.
- Repo khong co bang chung ve `sudo headscale nodes list` sau khi client join thanh cong.

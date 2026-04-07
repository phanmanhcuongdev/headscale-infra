# Debugging Report

Bao cao nay duoc lam sach lai tu command history va cau hinh hien co trong repo. Day la incident record, khong phai review.

## Incident scope

- Nginx duoc cau hinh reverse proxy cho domain cong khai da duoc sanitize thanh `headscale.example.com`.
- Headscale chay bang `systemd` sau khi cai bang goi `.deb`.
- Headscale log lap lai canh bao `No Upgrade header in TS2021 request`.
- Trong command history, thao tac `tailscale` tren may dang ghi log tra ve `command not found`.

## Verified evidence

### Config evidence

- `config/config.yaml` dat `listen_addr: 127.0.0.1:8080`.
- `config/config.yaml` dat `metrics_listen_addr: 127.0.0.1:9090`.
- `config/config.yaml` dat `grpc_listen_addr: 127.0.0.1:50443`.
- `config/config.yaml` su dung SQLite tai `/var/lib/headscale/db.sqlite`.
- `config/headscale.conf` proxy toi `http://127.0.0.1:8080`.
- `config/headscale.conf` co `proxy_set_header Upgrade $http_upgrade;`.
- `config/headscale.conf` co `proxy_set_header Connection "upgrade";`.

### Command evidence

- Mot lan `certbot --nginx` fail voi loi `NXDOMAIN`.
- Lan chay `certbot --nginx` sau do thanh cong va deploy chung chi vao site `headscale`.
- `headscale` version `v0.28.0` duoc cai dat thanh cong.
- `systemctl status headscale` ghi nhan service `active (running)`.
- `headscale namespaces create cuongpm` thanh cong.
- `headscale preauthkeys create --user 1 --reusable --expiration 24h` thanh cong.
- `journalctl -u headscale -f` ghi nhan nhieu dong `No Upgrade header in TS2021 request`.

## Confirmed noisy paths

Nhung muc duoi day co trong command history nhung khong nam trong failure path cuoi cung:

- thu chay `headscale` bang Docker
- thu tai binary `headscale` bang URL khong hop le va nhan `404`
- lenh tao preauth key voi `-n cuongpm` bi sai cu phap

## Root cause supported by repo evidence

Bang chung manh nhat trong repo la:

1. Headscale chay tren `127.0.0.1:8080`, nen reverse proxy la duong vao can thiet cho domain cong khai.
2. Command history ghi nhan canh bao lap lai `No Upgrade header in TS2021 request`.
3. Cau hinh Nginx dang duoc giu trong repo da bo sung cac header `Upgrade` va `Connection "upgrade"`.

Tu cac bang chung tren, co the xac nhan repo da ghi nhan mot su co lien quan den WebSocket upgrade qua reverse proxy. Repo khong chua bang chung ve ket qua dang ky node sau khi cau hinh da duoc sua.

## Minimal retained fix record

Ban cau hinh dang luu trong `config/headscale.conf` cho thay location block da duoc giu lai theo huong:

```nginx
location / {
    proxy_pass http://127.0.0.1:8080;
    proxy_http_version 1.1;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

Command history cung ghi nhan:

```bash
sudo nginx -t
sudo systemctl reload nginx
```

## What the repo does not prove

- Khong co bang chung trong repo ve node nao da join thanh cong.
- Khong co ket qua `headscale nodes list`.
- Khong co log client de xac nhan phia Windows hoac Linux da thuc hien `tailscale up` thanh cong.

# Architecture

Tai lieu nay chi mo ta nhung gi co the xac thuc tu `config/config.yaml`, `config/headscale.conf` va command history da duoc tong hop.

## Thanh phan duoc xac thuc

- `headscale` duoc cai dat bang goi `.deb` va duoc chay boi `systemd`.
- `headscale` phuc vu HTTP tren `127.0.0.1:8080`.
- Endpoint metrics/debug duoc cau hinh tren `127.0.0.1:9090`.
- gRPC admin duoc cau hinh tren `127.0.0.1:50443`.
- Database duoc cau hinh la `sqlite` voi file `/var/lib/headscale/db.sqlite`.
- `nginx` duoc cau hinh lam reverse proxy va redirect HTTP sang HTTPS.
- `nginx` proxy toi `http://127.0.0.1:8080`.
- Cau hinh `nginx` dang luu trong repo co cac header `Upgrade` va `Connection "upgrade"` cho WebSocket.

## Quan he giua cac thanh phan

1. Client duoc ghi nhan trong command history su dung `--login-server https://headscale.example.com`.
2. `nginx` nhan traffic HTTPS cho domain cong khai da duoc sanitize thanh `headscale.example.com`.
3. `nginx` chuyen tiep traffic vao `headscale` tren `127.0.0.1:8080`.
4. `headscale` su dung SQLite tai `/var/lib/headscale/db.sqlite`.

## Cac diem chi xac thuc mot phan

- Command history ghi nhan `certbot --nginx` da fail mot lan vi `NXDOMAIN`, sau do cap chung chi thanh cong.
- Command history ghi nhan user `cuongpm` da duoc tao va preauth key da duoc tao thanh cong.
- Command history ghi nhan canh bao lap lai `No Upgrade header in TS2021 request`.
- Repo khong chua bang chung ve mot node da dang ky thanh cong sau khi cap nhat proxy.

## Khong du bang chung de mo ta them

- Khong co so do mang day du trong repo.
- Khong co du lieu xac nhan luong ket noi cua client sau khi sua proxy.
- Khong co du lieu trong repo de mo ta them ve ACL, DERP thuc te, OIDC, hoac van hanh sau trien khai.

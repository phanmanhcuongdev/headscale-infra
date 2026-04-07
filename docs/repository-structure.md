# Repository Structure

Tai lieu nay mo ta cau truc repo sau khi da duoc chuan hoa de push GitHub.

## Thu muc goc

- `LICENSE`: giay phep MIT.
- `README.md`: tong quan repo va gioi han tai lieu.
- `config/`: luu lai cac file cau hinh duoc trich xuat tu qua trinh thiet lap.
- `docs/`: tai lieu hoa duoc viet lai tu du lieu thuc trong repo.

## Thu muc `config/`

- `config/config.yaml`: cau hinh `headscale`; trong file hien co `server_url`, `listen_addr`, `database`, `dns`, `noise`, `derp` va cac muc mac dinh khac.
- `config/headscale.conf`: cau hinh `nginx` cho site `headscale`; file nay da duoc sanitize de khong lo domain cong khai that.

## Thu muc `docs/`

- `docs/architecture.md`: mo ta cac thanh phan va moi lien he da duoc xac thuc.
- `docs/deployment-flow.md`: flow toi gian tu command history, da loai bo buoc that bai va noise khoi duong chinh.
- `docs/debugging-report.md`: incident report da duoc lam sach lai.
- `docs/repository-structure.md`: file nay.

## Nguon du lieu da duoc su dung de viet docs

- `config/config.yaml`
- `config/headscale.conf`
- command history truoc do nam trong `commands.sh`, da duoc chuyen hoa thanh cac tai lieu Markdown va khong con giu file log tho trong repo sau khi trich xuat noi dung can thiet

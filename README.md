# headscale-infra

Repo nay luu lai cau hinh va tai lieu hoa qua trinh thiet lap `headscale` va `nginx` duoc trich xuat tu cac file hien co trong repo.

Pham vi hien tai cua repo:

- `config/config.yaml`: cau hinh `headscale` dang duoc luu trong repo.
- `config/headscale.conf`: cau hinh `nginx` reverse proxy da duoc sanitize de phu hop voi repo public.
- `docs/architecture.md`: nhung thanh phan va moi lien he co the xac thuc tu config va command log.
- `docs/deployment-flow.md`: luong thiet lap va debug toi gian duoc tong hop tu command history.
- `docs/debugging-report.md`: bao cao su co duoc lam sach lai tu du lieu command log.
- `docs/repository-structure.md`: mo ta cau truc repo thuc te.

Cach doc repo:

1. Doc `docs/repository-structure.md` de biet repo gom nhung gi.
2. Doc `docs/architecture.md` de thay cac thanh phan duoc xac thuc.
3. Doc `docs/deployment-flow.md` neu can theo lai luong thao tac da duoc giu lai.
4. Doc `docs/debugging-report.md` neu can boi canh su co va huong xu ly da duoc ghi nhan.

Gioi han:

- Tai lieu chi phan anh du lieu thuc co trong repo nay.
- Khong co phan nao trong docs duoc bo sung bang suy doan ngoai config va command history da co.
- Cac gia tri nhay cam trong tai lieu cong khai da duoc sanitize, bao gom auth key, IP dang nhap va domain cong khai.

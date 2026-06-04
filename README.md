# SOA STOCK AI Dashboard — CPAXTRA

Dashboard วิเคราะห์ผลการตรวจ SOA Stock แบบ Real-time บน GitHub Pages

## โครงสร้าง Repo

```
soa-stock/
├── index.html          ← Dashboard (แก้ไฟล์นี้เมื่อต้องการอัปเดต UI)
├── data.json           ← ข้อมูลล่าสุด (อัปเดตอัตโนมัติเมื่อ push xlsx)
├── convert_xlsx.py     ← Script แปลง xlsx → data.json (ใช้บน PC)
└── .github/
    └── workflows/
        └── update-data.yml  ← GitHub Actions (รันอัตโนมัติ)
```

## วิธีอัปเดตข้อมูล

### วิธีที่ 1: GitHub Actions (แนะนำ)
1. ลาก xlsx ไฟล์ล่าสุดวาง root ของ repo
2. Commit + Push → Actions จะแปลงและอัปเดต `data.json` ให้อัตโนมัติ (~1 นาที)
3. ทุกคนที่เปิด URL จะเห็นข้อมูลใหม่เมื่อ refresh (หรือรอ auto-refresh 5 นาที)

### วิธีที่ 2: แปลงบน PC แล้ว Push
```bash
# 1. แปลง xlsx เป็น json
python convert_xlsx.py SOA_Data_Latest.xlsx

# 2. Push ขึ้น GitHub
git add data.json
git commit -m "อัปเดตข้อมูล $(date)"
git push
```

## ตั้งค่า GitHub Pages

1. ไปที่ **Settings → Pages**
2. Source: **Deploy from a branch**
3. Branch: **main**, Folder: **/ (root)**
4. Save → รอ ~2 นาที → ได้ URL

## คอลัมน์ที่รองรับใน Excel

| คอลัมน์ระบบ | ชื่อคอลัมน์ที่รองรับ |
|---|---|
| regional | Regional, regional |
| store | Store, store |
| zone | Zone, zone |
| dept | Dept, dept |
| item | Item, item |
| desc | Description, description, ชื่อสินค้า |
| price | ราคาสินค้า (฿), Price, price |
| diff | Diff, diff |
| value | มูลค่า Diff (฿), Value, value |
| remark | Remark, remark |
| status | Status, status (Match/Short/Over) |
| date | วันที่บันทึก, Date, date |

## หมายเหตุ

- Dashboard จะ auto-refresh ทุก 5 นาที
- ไฟล์ xlsx ที่ push ขึ้น repo ถูกเก็บใน Git History — **ไม่ควร push ข้อมูลลับ**
- ถ้าต้องการ Private ให้ใช้ Private Repo + GitHub Pages (ต้องการ GitHub Pro)

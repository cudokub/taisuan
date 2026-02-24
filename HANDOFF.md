# Handoff — ไต่สวนสาธารณะ

> สรุปการทำงานและสถานะปัจจุบัน สำหรับ session ใหม่

---

## วิธีการทำงาน

### Phase 1: Brainstorm → เอกสาร
- เริ่มจาก Cudo เล่าภาพรวมอีเวนต์ — Claude ถามคำถามเพื่อดึงรายละเอียด
- ถาม-ตอบเรื่อง: คาแรคเตอร์, เวที, ลำดับการแสดง, props, คำตะโกน, เวลา
- สรุปทั้งหมดลง `EVENT.md` เป็น source of truth

### Phase 2: Style Discovery → Presentation
- ใช้ skill `/frontend-slides` สร้าง presentation
- สร้าง 3 style previews ใน `.claude-design/slide-previews/` ให้เลือก
- Cudo เลือก Style C (People's Voice) แล้วขอปรับสี:
  - ประชาชน = **neon green #39ff14**
  - กกต. = **neon red #ff2020**
- สร้าง `presentation.html` 14 slides จากเนื้อหาใน EVENT.md

### Phase 3: Concept Art
- ใช้ `imggen` CLI (Gemini API) สร้างภาพ concept art ลาน BACC
- **Banana Pro** ผ่าน API → 503 ตลอด ใช้ไม่ได้
- **gemini-2.5-flash-image** ใช้ได้ผ่าน `--model gemini-2.5-flash-image --raw`
- **Banana Pro ผ่าน Gemini app** (browser) → ใช้ได้ ผลดีกว่า Flash
- **Workflow:** Claude เตรียม prompt → ลอง Flash ก่อน → Cudo เอา prompt ไปลองใน Gemini app → ได้ภาพที่ชอบ → Claude ใส่ลง presentation
- **Prompt lesson:** สั้นดีกว่ายาว, แนบ ref photo ของ BACC แล้วไม่ต้องอธิบายตึกเยอะ
- ภาพ convert เป็น JPEG + resize ด้วย `sips` ก่อนใส่ (เพื่อขนาดไฟล์เล็ก)

### Phase 4: รูปถ่ายผู้แสดง
- Cudo ส่งรูปมา → Claude crop 1:1 + resize 400px + convert JPEG ด้วย `sips`
- เซฟใน `photos/` ตั้งชื่อ romanized (เช่น nuhring.jpg, pao.jpg)
- ใส่ใน presentation เป็น `.card-photo` วงกลมมีกรอบสี neon ตาม team
- รูปป้ายห้อยคอ (badge-ggt.jpg, badge-jotok.jpg) → resize 200x200

### Phase 5: Feedback Loop
- Cudo ส่ง feedback มาเป็น list (page X: แก้อะไร)
- ถ้า feedback ซับซ้อน → Claude ถามก่อนแก้ (walk through)
- ถ้าชัดเจน → แก้เลย
- ทุกรอบ: แก้ → เปิด browser preview → Cudo เช็ค → push to main

### Deployment
- **GitHub repo:** `cudokub/taisuan` (public)
- **Live:** https://cudokub.github.io/taisuan/presentation.html
- **Deploy:** push to `main` → GitHub Actions auto-deploy (`.github/workflows/pages.yml`)
- **ขนาดไฟล์:** ระวังรูปใหญ่เกินจะ push ไม่ผ่าน → convert JPEG + resize ก่อนเสมอ

---

## สถานะปัจจุบัน

### เสร็จแล้ว
- [x] EVENT.md — เอกสารภาพรวมอีเวนต์ครบ
- [x] presentation.html — 14 slides, มีรูปผู้แสดง 10 คน + concept art + ป้ายห้อยคอ
- [x] Concept art ลาน BACC ไม่มีเวที (stage-concept.jpg)
- [x] Deploy บน GitHub Pages ใช้งานได้
- [x] Feedback รอบแรก (เวลา, ชื่อ, บทบาท, sequence, ตัด มิ้น Vote 62)

### ยังไม่เสร็จ (จาก EVENT.md TODO)
- [ ] เนื้อหา/คำถามเตรียมไว้ในแต่ละก้อน
- [ ] โทน/คาแรคเตอร์ของแต่ละตัวละครเจาะลึก

---

## Technical Notes

### Image Processing (sips)
```bash
# Crop to 1:1 square (center crop)
sips -c <shortest> <shortest> input.jpg --out output.jpg

# Resize to 400x400
sips -z 400 400 output.jpg

# Convert PNG to JPEG
sips -s format jpeg input.png --out output.jpg

# Resize max dimension (keeps aspect ratio)
sips -Z 1400 large-image.jpg
```

### imggen
```bash
# Flash model with ref image (reliable)
imggen "prompt" -r ref.jpg --raw --model gemini-2.5-flash-image

# Banana Pro (often 503 via API, use Gemini app instead)
imggen "prompt" -r ref.jpg --raw
```

### File Structure
```
ไต่สวนสาธารณะ/
├── CLAUDE.md              # Project instructions
├── EVENT.md               # Event details (source of truth)
├── HANDOFF.md             # This file
├── presentation.html      # 14-slide team briefing (zero-dependency)
├── stage-concept.jpg      # Concept art (JPEG, ~380KB)
├── photos/
│   ├── nuhring.jpg        # พี่หนูหริ่ง สมบัติ บุญงามอนงค์
│   ├── seen.jpg           # ซีน ThumbRights
│   ├── tum.jpg            # อาจารย์ตั้ม จิรวุธ
│   ├── nimit.jpg          # พี่นิมิตร์ เครือข่ายฯ
│   ├── mai.jpg            # มาย ภัสฯ (MC ร่วม)
│   ├── pao.jpg            # เป๋า iLaw
│   ├── ou.jpg             # อู๋ DayBreaker (MC ร่วม)
│   ├── neng_tune.jpg      # เหน่ง Tune & Co.
│   ├── neng_taxi.jpg      # พี่เหน่ง Taxi
│   ├── jiw.jpg            # พี่จิ๋ว Call
│   ├── cha.jpg            # ทนายชา รุ่งโรจน์เรืองฉาย
│   ├── badge-ggt.jpg      # ป้ายห้อยคอ กกต.
│   └── badge-jotok.jpg    # ป้ายห้อยคอ โจทก์
├── .github/workflows/
│   └── pages.yml          # GitHub Pages deploy
└── .claude-design/        # Style previews (not committed)
```

---

## Presentation Slides Overview

| # | ชื่อ | เนื้อหา |
|---|------|---------|
| 1 | Title | ประชาชน VS กกต. ไต่สวนสาธารณะ |
| 2 | เป้าหมาย | 3 ข้อ |
| 3 | ประเด็นไต่สวน | 3 ข้อกล่าวหา (ลองวางไว้) |
| 4 | ทีมโจทก์ | 5 คน + รูป |
| 5 | ทีมจำเลย | 5 คน + รูป |
| 6 | พิธีกร | อู๋ + มาย + รูป |
| 7 | เวที & อุปกรณ์ | Concept art + props list + ป้ายห้อยคอ |
| 8 | ลำดับการแสดง | Timeline bar chart 7 ก้อน |
| 9 | ช่วงเปิด | ก้อน 1-3 (15 นาที) |
| 10 | ช่วงถกกัน | ก้อน 4 (40 นาที) |
| 11 | ช่วงประชาชน | ก้อน 5-6 (33 นาที) |
| 12 | การมีส่วนร่วมผู้ชม | 3 cards |
| 13 | คำตะโกนปิดท้าย | 3 ตัวอย่าง (all green) |
| 14 | Closing | ประชาชน VS กกต. + info |

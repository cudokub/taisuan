# ไต่สวนสาธารณะ

## Overview
อีเวนต์ role-play กึ่งแสดง กึ่งจริง — จำลองการไต่สวน กกต. ต่อหน้าประชาชน
เป้าหมายคือสร้างภาพรวมงาน ทำไฟล์ presentation ให้ทีมงานและผู้เข้าร่วมเห็นภาพเดียวกัน

## Event Details
- **วันที่:** 25 กุมภาพันธ์ 2026 | 18:30 น.
- **สถานที่:** ลาน BACC (ไม่มีเวที — นั่งพื้นลาน)
- **ความยาว:** 1.5 ชั่วโมง (90 นาที)
- **ผู้ชม:** ~800 คน นั่งเบาะปูพื้นล้อมรอบด้านหน้า+ข้าง
- **ผู้แสดง:** 11 คน (โจทก์ 6 + จำเลย 5)

## Key Files
- `EVENT.md` — ภาพรวมอีเวนต์ทั้งหมด (คาแรคเตอร์, เวที, ลำดับ, props)
- `presentation.html` — Team briefing presentation (zero-dependency HTML)
- `stage-concept.jpg` — Concept art ลาน BACC (Mood & Tone เท่านั้น)
- `photos/` — รูปถ่ายผู้แสดงทั้ง 11 คน (1:1 square, JPEG) + รูปป้ายห้อยคอ
- `CLAUDE.md` — ไฟล์นี้ คำแนะนำโปรเจกต์

## Deployment
- **GitHub repo:** `cudokub/taisuan`
- **Live URL:** https://cudokub.github.io/taisuan/presentation.html
- **Deploy:** Push to `main` → GitHub Pages auto-deploy

## Design
- **Style:** Neon green (#39ff14) = ประชาชน/โจทก์, Neon red (#ff2020) = กกต./จำเลย
- **Fonts:** Noto Sans Thai + Sarabun (Google Fonts)
- **Background:** #0a0a0a (near-black)

## Working Notes
- งานนี้เน้น brainstorm ร่วมกัน → สร้างชุดข้อมูล → ทำ presentation
- ภาษาหลักของเอกสาร: ไทย
- Concept art ใช้ Gemini (Banana Pro ผ่าน app, Flash ผ่าน imggen CLI)
- รูปถ่ายผู้แสดง crop 1:1 ด้วย sips → JPEG 400x400

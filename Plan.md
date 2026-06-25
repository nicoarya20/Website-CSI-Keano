# Plan — cogniti Landing Page MVP 1

---

## North Star

> Website ini harus menciptakan **rasa penasaran, otoritas, dan dorongan untuk menghubungi Cogniti.**

Setiap keputusan desain dan konten harus berkontribusi ke salah satu dari tiga tujuan ini.

---

## Brand

| | |
|---|---|
| **Brand facing** | cogniti (lowercase) |
| **Legal** | Cognitiva Solusi Indonesia |
| **Positioning** | Intelligence Infrastructure Company |
| **Typography** | Inter |
| **Logo (dark bg)** | `asset/Logo-Final.png` (gray) |
| **Logo (light bg)** | `asset/Logo-Final 2.png` (navy) |

---

## Stack (MVP 1 — Single HTML)

- GSAP 3.12.5 + ScrollTrigger (CDN)
- Three.js r128 (CDN)

> Lenis dan Tailwind tidak dipakai di implementasi akhir.
> Next.js dipakai saat migrasi ke production (post MVP 1)

---

## Color Palette

Filosofi: **accent bukan warna, tapi kontras.** Monokromatik murni.

| Role | Hex |
|---|---|
| Background | `#0A0A0A` |
| Surface | `#141414` |
| Border | `#1A1A1A` |
| Text primary | `#F0F0F0` |
| Text secondary | `#888888` |
| Text muted | `#444444` |
| Accent (cold white) | `#FFFFFF` — sparingly |
| Logo mark | Merah — logo only, tidak masuk UI |

Feel referensi: Linear, Vercel.

---

## Global Design Decisions

- **Tidak ada section labels** — semua label uppercase kecil di atas section (e.g. "Living Architecture", "Selected Deployment Environments", "Careers", "Contact") dihapus. Konten berbicara sendiri.
- **Scroll restoration** — Chrome mengabaikan `history.scrollRestoration = 'manual'` dan tetap restore scroll secara internal saat refresh. Fix: tambah class `page-loading` ke `<html>` via head script, CSS `html.page-loading { overflow: hidden }` + `html.page-loading main { visibility: hidden }` menyembunyikan seluruh main content. Saat entry animation selesai, `scrollTo(0,0)` + `scrollTop=0` dipanggil sebelum dan sesudah `classList.remove('page-loading')` untuk memastikan posisi kembali ke 0 sebelum konten terlihat.

---

## Struktur Section

| # | Section | ID | Fungsi |
|---|---|---|---|
| 1 | **Hero** | `#hero` | Curiosity — WebGL sphere full-screen |
| 2 | **Manifesto** | `#manifesto` | Worldview building — word-by-word reveal |
| 3 | **Founder** | `#founder` | Otoritas personal — Fami Maliki |
| 4 | **Selected Deployments** | `#deployments` | Implied authority — arc-spine |
| 5 | **Living Architecture** | `#living-arch` | Interactive 3D simulation — centerpiece |
| 6 | **Careers** | `#careers` | Talent acquisition |
| 7 | **CTA / Contact** | `#cta` | Dorongan konversi |
| 8 | **Footer** | `#footer` | — |

> Minimal project, klien, dan produk — credibility dibangun lewat cara berpikir dan siapa di baliknya, bukan portfolio. Pendekatan McKinsey/Stripe.
> Loading Screen ditunda, bukan bagian dari MVP 1 awal.
> "Who We Serve" section dihapus — digantikan Living Architecture.

---

## Section 1 — Hero ✅ Implemented

### Headline
```
We build the infrastructure that thinks.
```

### WebGL — "The Core"

IcosahedronGeometry (detail 5) dengan tiga layer:
- Wireframe mesh `opacity: 0.06` — struktur
- Points (vertex colors) — glow individual
- Wireframe glow (AdditiveBlending) — pulse ripple

**Entry Animation — "Collapse into Text":**
1. WebGL full-screen saat load — immersive
2. First scroll/click → canvas shrinks ke `orbSlot` inline dalam headline
3. Canvas fade out → hero text reveal → navbar swap (minimal → full)
4. Scroll animations aktif setelah entry selesai

**Scroll:** `#hero` fade out saat `#manifesto` masuk (ScrollTrigger scrub).

---

## Section 2 — Manifesto ✅ Implemented

**Copy (final — 4 block):**
1. Software connected information. Intelligence connects decisions.
2. Organizations are drowning in data. Yet struggling to act.
3. The future belongs not to those who collect, but to those who act.
4. Intelligence should exist across every interaction. Every workflow. Every decision.

**Interaksi:**
- Section pinned, total scroll `4 × 100vh`
- Word-by-word reveal via GSAP ScrollTrigger `scrub: 2`
- **Reveal:** `opacity 0→1`, `y: 14→0`, `rotation: +6°→0°` (power3.out)
- **Keluar:** `opacity 1→0`, `y: 0→14` tanpa rotate (power3.in)
- Progress bar 4 segmen di bottom center
- Ambient trail visible di background

---

## Section 3 — Founder ✅ Implemented

**Layout:**
- Kiri: card — thumbnail foto + "Fami Maliki" + "FOUNDER & CEO"
- Kanan: deskripsi pencapaian (36px) + metadata bar
- Divider `1px #1A1A1A`, padding `88px 0`

**Interaksi:**
- Hover `.item-desc` → floating founder photo mengikuti cursor (kanan cursor, 20px offset)
- Pop-in: scale `0.05→1`, spring easing `cubic-bezier(0.34, 1.56, 0.64, 1)`, 0.7s
- Momentum tilt: `velocityX * 0.4`
- Text scramble 420ms saat hover
- Cursor `+` crosshair saat hover (custom cursor, bukan native)
- Custom cursor muncul di item-level hover, bukan hanya desc-level

**Konten pending:**
- Foto founder (placeholder `fp-placeholder`)
- Konten pencapaian nyata (saat ini placeholder)

---

## Section 4 — Selected Deployments ✅ Implemented

**Konsep:** Implied authority — coverage sektor tanpa nama klien. Pola McKinsey/Deloitte.

**Visual:** Arc-spine — lingkaran besar (center di luar viewport kiri), 5 sector sebagai titik di kurva. Teks dirotasi mengikuti tangen kurva.

**Interaksi:** GSAP ScrollTrigger scrub, section di-pin `5 × 100vh`.

**Sectors:** Public Services · Infrastructure · Logistics · Hospitality · Communities

**Rules:** No client logo, no project name, no screenshots, no pricing.

---

## Section 5 — Living Architecture ✅ Implemented

**Konsep:** Interactive 3D network simulation — centerpiece website. Menjelaskan kemampuan Cogniti tanpa membuka IP produk.

**Layout:** Grid 2 kolom — kiri (copy panel) | kanan (Three.js canvas)

**Copy:**
- Headline: "A Living Architecture For Decisions."
- Sub: "We connect signals, context, knowledge, and workflows into adaptive systems that help organizations move from awareness to action."

### Nodes (7)

| Node | Microcopy |
|---|---|
| Citizen | Every interaction becomes a signal. |
| Operations | Workflows gain visibility. |
| Knowledge | Information becomes structured context. |
| Infrastructure | Assets become part of the intelligence layer. |
| Intelligence | Patterns become recommendations. |
| Decision | Leaders act with better context. |
| Action | Decisions move into execution. |

### Interaksi

**Hover:**
- Node → state `h` (brighter), connected edges brightened
- Cursor `grab`
- Hint text update: "Drag to reposition — Click to activate"

**Drag (reposition):**
- `mousedown` pada node → drag di 3D plane tegak lurus kamera
- Mouse velocity ditrack via EMA (Exponential Moving Average) selama drag
- `mouseup` → velocity ditransfer ke node sebagai **throw physics**:
  - Drag cepat = terlempar jauh, drag pelan = terlempar pelan
  - Diam > 100ms sebelum lepas = tidak terlempar
  - Friction: `vel *= 0.88` per frame
- **Invisible walls:** Y bounds ±1.8, cylindrical XZ radius 3.0 — node memantul (restitution 0.5)
- Orbit kamera berhenti saat drag untuk stabilitas
- Edge geometry update realtime mengikuti posisi node baru

**Click (trigger signal):**
- Click tanpa drag (threshold < 5px) → BFS propagation dari node tersebut
- Tier delay: 950ms per tier
- Partikel travel dari node ke node via edge
- Pulse rings spawn di setiap node yang aktif
- Completion ("From awareness to action") hanya muncul jika semua 7 node aktif (eksklusif dari Citizen)

**Floating animation:**
- Setiap node: oscillasi X/Y/Z dengan fase berbeda (sin dengan frekuensi 0.73, 1.0, 0.55)
- Period ~17–30 detik, amplitude ±0.025–0.042 units
- Floating berbasis `basePos` — drag mengupdate `basePos`, floating berlanjut dari sana

**Camera:**
- Orbit lambat (ORBIT_S = 0.0006) sekitar Y axis
- Mouse parallax (laTX/laTY)
- Orbit berhenti saat node di-drag

**Auto-demo:** `triggerSignal(0)` setelah 4500ms jika user belum berinteraksi

**Panel states:**
- Default: headline + hint
- Active: node number + scramble name + micro text + progress dots
- Complete: "Signal Complete" + "From awareness to action"

---

## Section 6 — Careers ✅ Implemented

**Headline:** Build What Comes Next.

**Open Positions:**
- Innovation & Growth Manager
- Technical Lead
- Product Builder
- Full Stack Engineer

**Interaksi — dua layer:**

*Scan (hover):* Preview image di posisi cursor, curtain reveal/close animation.

*Read (click):* Accordion expand, role lain `opacity: 0.2`.

**Catatan:** Preview images masih gradient placeholder — ganti dengan foto editorial.

---

## Section 7 — CTA / Contact ✅ Implemented

**Headline:** Let's Start A Conversation.

**Mechanic:** Conversational sentence-form:
- "My name is _______, and I represent _______."
- "Reach me at _______."
- Inquiry type: pill selector (Partnership / Government / Enterprise / Investor / Career / General)
- Message textarea
- Submit → `mailto:info@cogniti.id`

---

## Section 8 — Footer ✅ Implemented

- Logo + nav links (Founder · Deployments · Careers · Contact)
- © 2026 Cognitiva Solusi Indonesia · "Intelligence Infrastructure"

---

## Global Background — Ambient Trail ✅ Implemented

- 20 rantai node, tiap trail 60 node, spring physics
- `lighter` blend mode, warna `hsla(210–230, 10%, 95%, 0.035)`
- `position: fixed`, `z-index: 11`, `pointer-events: none`
- Visible di semua section termasuk manifesto dan founder

---

## Referensi Visual

| Elemen | Inspirasi |
|---|---|
| Immersive 3D/WebGL | Active Theory |
| Smooth scroll + typografi bold | Locomotive |
| Stack & scalability | Basement Studio |
| Entry animation mechanic | Make Reign |
| Trust signals + enterprise tone | Stripe |

---

## Pending / To Do

- [ ] Foto founder (ganti `fp-placeholder` dengan foto editorial Fami Maliki)
- [ ] Konten pencapaian founder yang nyata (3 item — dikonfirmasi ke founder)
- [ ] Preview images Careers (ganti gradient placeholder dengan foto editorial)

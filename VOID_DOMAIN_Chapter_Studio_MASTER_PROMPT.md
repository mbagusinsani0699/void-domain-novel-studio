# VOID DOMAIN Chapter Studio: MASTER PROMPT untuk Codex

Dibuat berdasarkan paket `VOID_DOMAIN_S1_Consolidated_Release_R3.zip` (release `VOID-DOMAIN-S1-CONSOLIDATED-R3`). Seluruh fakta paket di bawah sudah dicek langsung ke isi ZIP, bukan ditebak.

***

## BAGIAN 0. CARA PAKAI (untuk penulis, bukan untuk Codex)

1. Buat dua repo GitHub:
   - `void-domain-studio` : kode aplikasi saja. Boleh public (GitHub Pages gratis hanya untuk repo public di akun Free).
   - `void-domain-data` : **PRIVATE**. Tempat naskah bab, keputusan, dan state aktual. Dibuat kosong, dikelola oleh aplikasi.
2. Hubungkan `void-domain-studio` ke Codex. Tempel **BAGIAN 1** (master prompt) bersama **Task Prompt Phase 0** (Bagian 2) ke task pertama. Phase 0 menyimpan master prompt ini ke repo sebagai `AGENTS.md` dan `project-docs/MASTER_PROMPT.md`, jadi task berikutnya cukup menempel Task Prompt fase terkait.
3. **Jangan upload ZIP rilis ke GitHub.** ZIP 211 MB, ledger di dalamnya 842 MB, sedangkan batas file GitHub 100 MB (upload lewat browser 25 MB). ZIP kamu pilih langsung di browser lewat halaman Import; datanya masuk IndexedDB browser, tidak keluar dari komputermu.
4. Satu fase = satu task Codex = satu PR. Review, merge, baru lanjut. Jangan minta Codex membangun semuanya sekali jalan.
5. Codex tidak punya ZIP aslimu, jadi test-nya memakai fixture mini buatan (berlabel `TEST_FIXTURE`). Verifikasi pada rilis asli terjadi di browsermu lewat halaman Production Gate (Phase 6). Hasilnya kamu laporkan kembali ke Codex bila ada yang gagal.
6. Biaya: tidak ada. Pembuatan bab memakai **Manual Bridge** (aplikasi menyusun paket prompt, kamu tempel ke chat AI yang sudah kamu punya, lalu tempel hasilnya kembali). Adapter API gratis bersifat opsional (Phase 7).

***

## BAGIAN 1. MASTER PROMPT (tempel ke Codex)

### 1. PERAN DAN TUJUAN

Kamu adalah senior full-stack engineer sekaligus narrative-systems engineer. Bangun **VOID DOMAIN Chapter Studio**: web app statis, gratis, berbahasa Indonesia, untuk memproduksi bab Season 1 novel VOID DOMAIN (1500 bab, 18 Arc) **satu bab per kali**.

Aplikasi menjalankan kontrak S1 sampai S6 dari `integration/Novel_Generator_Integration_R3.md` (Brief, Prerequisites, Draft, Audit, Patch, Commit) dan menambahkan satu lapisan interaksi yang diminta penulis: **S2B Decision Gate**, yaitu aplikasi bertanya dan memberi rekomendasi kepada penulis setiap kali ada adegan penting atau hal penting saat bab dibuat.

Prinsip utama: aplikasi **menjaga kanon dan state**, bukan menciptakan kanon. Penulis adalah pemilik keputusan. Aplikasi tidak pernah mengisi celah kanon secara diam-diam.

### 2. BATAS KERAS (tidak boleh dilanggar dalam fase mana pun)

| ID | Aturan |
| :-- | :-- |
| Z1 | **Nol biaya.** Tanpa backend, server, database berbayar, API berbayar, key yang terhubung billing, analytics, telemetry. Tidak ada CDN, font, atau skrip eksternal saat runtime. Semua library di-vendor ke `vendor/` dengan versi, lisensi, dan hash dicatat di `vendor/VENDOR.md`. |
| Z2 | **Tanpa build step.** HTML + ES modules + Web Worker. Node hanya untuk test (`node --test`, tanpa paket npm). Jangan menambah dependensi tanpa alasan tertulis di `PROGRESS.md`. |
| Z3 | **Hosting:** GitHub Pages "deploy from branch" (root repo). Sertakan `.nojekyll`. Aplikasi juga harus jalan lokal dengan `python3 -m http.server`. |
| Z4 | **Privasi.** Repo aplikasi TIDAK PERNAH berisi kanon, outline, ledger, prosa, atau token. Release pack hidup hanya di IndexedDB browser. Naskah dan keputusan disimpan ke repo privat `void-domain-data` lewat GitHub REST API dengan fine-grained PAT (hanya repo itu, izin Contents read/write) yang disimpan di `localStorage`. Tampilkan peringatan jelas, jangan pernah log atau tampilkan token penuh. |
| Z5 | **Batas GitHub.** Aplikasi menolak mengirim file lebih dari 50 MB. Jangan pernah mencoba commit ZIP rilis atau ledger mentah. |
| Z6 | **Sandbox Codex.** Codex tidak punya ZIP asli. Semua test memakai fixture mini buatan (`tests/fixtures/`), berlabel `TEST_FIXTURE`, tidak pernah dianggap kanon. **Dilarang mengarang** nama karakter, lokasi, aturan power, atau fakta Void Domain apa pun di kode, UI, atau test, kecuali yang dikutip dari fixture berlabel test. |
| Z7 | **Bahasa.** UI Bahasa Indonesia. Kode dan identifier Bahasa Inggris. Prosa novel Bahasa Indonesia sesuai Narrative Bible. Label System UI di dalam prosa tetap Bahasa Inggris (Narrative §06.1). |
| Z8 | **Fail closed.** Source hilang, hash tidak cocok, indeks tidak cocok, bukti aktual bab sebelumnya tidak ada, protected reveal terdeteksi, atau blocker P0 berlaku: **blokir** dan tampilkan alasan beserta ID blocker. Jangan auto-regenerate, auto-clear, atau mengisi diam-diam. |
| Z9 | **Tanpa batch.** Satu bab satu waktu. Tidak ada tombol "generate semua" atau antrean banyak bab. |
| Z10 | **Paket rilis read-only.** Aplikasi tidak menulis ke ZIP. Ledger mentah tidak pernah dikirim ke LLM. Target (rencana) tidak pernah disalin menjadi aktual. |
| Z11 | **Zero Em-Dash.** Karakter U+2014 dan tiga tanda hubung beruntun dilarang di semua teks yang dihasilkan aplikasi (prompt template, prosa, dokumen), sesuai Narrative §04.3. Tambahkan pengecekan di test repo. |

### 3. FAKTA PAKET RILIS R3 (parser harus memverifikasi, bukan menebak)

**Identitas.** `release_id` = `VOID-DOMAIN-S1-CONSOLIDATED-R3`; status `AUTHOR_APPROVED_PLAN_NOT_COMMITTED`; `production_activation` = `BLOCKED_PENDING_RUNTIME_AND_APPLICABLE_GATES`; 1500 bab, 18 Arc, 28 Stage, 108 fase; 10.000 alokasi lantai + 5 baris carryover; `actual_commits` = 0; `runtime_tests` = `NOT_RUN`; 128 file di manifest (manifest tidak memuat hash dirinya sendiri).

**Isi ZIP.** Root: `README.md`, `CHANGELOG_R3.md`, `Release_Manifest_R3.json`, `Source_Allowlist_R3.json`. Folder: `canon/` (11 Bible pemilik + `Register Resolusi R1.md` + `Technical Model - approved R1.md`), `production/`, `integration/`, `audit/`.

**Allowlist.** Deny by default. 23 path diizinkan (13 file `canon/` + 10 file `production/`). `deny_globs`: `audit/**`, `legacy/**`, `**/*Revised_R1*`, `**/*Granular_R2*`, `**/*Proposed_Patch*`, `**/*Source_Archive*`. Isi `audit/` boleh di-hash untuk verifikasi integritas tetapi **tidak boleh** diindeks, di-retrieve, atau ditampilkan sebagai pengetahuan. Baca `precedence` (lima tingkat) dari `Source_Allowlist_R3.json` dan tampilkan di panel "Otoritas"; jangan hardcode.

**Chapter Index** (`production/S1_Chapter_Index_R3.csv`): UTF-8 BOM, CRLF, 24 kolom: `chapter, chapter_id, arc, title, outline_offset_bytes, outline_length_bytes, ledger_offset_bytes, ledger_length_bytes, stage, phase, target_life, target_deaths, target_realm, target_job_history, earth_job_effect, target_personal_max_floor, target_C, target_F, target_dormant, target_consumed, actual_committed, status, canonical_stage, body_context`. Offset adalah **byte UTF-8**, bukan indeks karakter. Jangan mengubah newline atau encoding.

**Slice outline satu bab** berisi 15 field berlabel backtick: `CHAPTER_ID`, `LOCATION_&_FLOOR_MODE`, `KAEL_CYCLE_&_TIMELINE`, `POV_TYPE`, `SCENE_OBJECTIVE`, `CONFLICT`, `STORY_PROGRESSION`, `CHARACTER_PROGRESSION`, `MYSTERY_CLUES_PLANTED`, `CHAPTER_TYPE`, `ENDING_HOOK`, `CANON_STATUS_LABEL`, `ARC_STAGE_PHASE`, `CONTINUITY_GATE`, `STATE_LEDGER`. Slice bisa diawali heading `### BAB nnn: ...` dan **berakhir dengan sisa** `***` atau heading `#### Fase ...` milik bab berikutnya. Parser membuang sisa itu, memverifikasi `CHAPTER_ID` sama dengan indeks, dan tidak boleh membawa konten bab tetangga.

**Ledger** (`production/S1_Master_State_Ledger_R3.jsonl`): sekitar 842,6 MB, 1500 baris, lima bentuk top-level. Satu baris bab bisa berukuran dari beberapa KB (B1: 3,8 KB) hingga 1,3 MB (B1500) karena riwayat kumulatif. Field minimal: `chapter_id, continuity_gate, ending_hook, event_proof, evidence_scene, phase, pre_state, post_state, reader_clue, release_meta, source_legacy, stage, status`. **Seluruh isinya RENCANA** (`historical_fields_are_planned: true`, `actual_proof: null`). Pengukuran: riwayat `event_history`, `knowledge_history`, `evidence_history`, serta daftar kumulatif seperti `cleared_kael` menyumbang mayoritas ukuran; membuang field `*_history` saja masih menyisakan sekitar 260 MB, jadi **proyeksi ringkas (slim)** wajib lebih agresif (lihat §5).

**Aturan baca khusus.** Gunakan `S1_Lifecycle_Events_R3.json` (kunci `planned_deaths`, `planned_rebirths`, `planned_job_grants`) untuk riwayat D1 sampai D4, karena array `death_history` pada ledger sumber tidak memuat D4. Gunakan kolom `canonical_stage` (bukan label sumber "18a"). Gunakan `release_meta.effective_earth_job` untuk Earth (nilai HOLD berarti tidak diketahui; jangan dibaca sebagai aktif atau nol).

**Blocker registry** (`production/S1_Master_Blocker_Registry_R3.csv`): 57 baris (33 P0, 17 P1, 7 P2). Kolom: `id, category, priority, chapter_scope, finding, source_locator, owner, closure_criteria, gate_scope, status`. `gate_scope` = `IMPORT` (9) atau `AFFECTED_CHAPTERS` (48). `chapter_scope` bentuknya campuran dan parser wajib mendukung semuanya, dengan unit test: `1290–1500` (en dash), `625–700;730–1490` (beberapa rentang), `299–345+` (terbuka ke atas), `ALL`. Status yang ada: `OPEN`, `RESOLVED_IN_R3_*_RUNTIME_PENDING`, `APPROVED_EXCEPTION_PROOF_PENDING`, `OWNER_RULE_CONFIRMED`. Status "resolved in R3" **belum** berarti lulus: runtime masih harus membuktikannya.

**Lantai.** `S1_Master_Floor_Clearance_Plan_R3.csv` (10.000 baris unik, kolom `actual_*` kosong) dan `S1_Floor_Carryover_R3.csv` (5 baris). Carryover tidak menambah alokasi lantai. Floor personal, group, dan prisoner adalah tiga status independen.

**Persetujuan.** `S1_Author_Approvals_R3.json`: semua 18 Arc disetujui sebagai rencana; patch `A02-C105-L250-P01`; target L1500 di B345, L4500 di B715, L7500 di B1147 (retry B1167), L10000 pada A18 (B1490), pengecualian Echo Lantern B369. Semua `actual_proof: null`. Persetujuan berlaku untuk rencana, bukan bukti.

### 4. ARSITEKTUR

```
void-domain-studio/
  index.html  .nojekyll  AGENTS.md  PROGRESS.md
  src/
    app.js  router.js  ui/            (layar, tanpa logika domain)
    core/                             (logika murni, tanpa DOM, wajib punya unit test)
      release/   import-zip.js verify.js outline-parser.js ledger-slim.js
                 blocker-scope.js canon-index.js access-labels.js
      pipeline/  s1-brief.js s2-prereq.js s2b-decisions.js s3-package.js
                 s4-audit.js s5-patch.js s6-commit.js
      context/   packer.js retrieval-bm25.js token-estimate.js
      lint/      prose-linter.js
      store/     idb.js github-store.js backup.js
      gate/      production-gate.js acceptance-runner.js
    workers/     import.worker.js
  vendor/        (fflate untuk ZIP streaming, SHA-256 inkremental) + VENDOR.md
  tests/         unit/*.test.js  fixtures/  (generator: tests/fixtures/make-fixture.mjs)
  project-docs/  MASTER_PROMPT.md  ARCHITECTURE.md  DECISION_CARDS.md
```

**Empat lapisan data yang tidak boleh tercampur:**

1. **Release Pack** (read-only, IndexedDB): hasil impor R3. Berisi RENCANA.
2. **Actual Story State** (store terpisah): hanya diubah oleh S6 setelah audit lulus dan persetujuan penulis.
3. **Working Drafts**: revisi bab (`rev-0001`, `rev-0002`, ...), tiap revisi punya SHA-256 atas teks UTF-8 persis.
4. **Decision Register**: keputusan penulis, immutable, hanya bisa digantikan oleh keputusan baru yang merujuknya.

Aturan tak tertulis yang wajib ditegakkan kode: `intended_delta` tidak pernah otomatis menjadi `actual_delta`; nilai tidak diketahui disimpan sebagai `null` atau `HOLD`, bukan `0`; tidak ada default implisit ke "full clear".

### 5. IMPORT RELEASE (di browser, Web Worker, tanpa upload)

1. Penulis memilih ZIP lewat `<input type="file">`. Baca central directory tanpa memuat seluruh file ke memori; dekompresi **streaming** per entri (fflate `Unzip`), dengan progres per file dan tombol batal.
2. Baca dulu `Release_Manifest_R3.json` dan `Source_Allowlist_R3.json`. Hitung SHA-256 **inkremental** setiap entri dan cocokkan dengan manifest. Ada dua mode: *Verifikasi cepat* (file yang dipakai aplikasi) dan *Verifikasi penuh* (semua 128 entri, termasuk `audit/`). Production Gate mensyaratkan verifikasi penuh sekali per rilis. Hash tidak cocok atau file hilang = **blokir** (test T02).
3. Hanya file allowlist yang di-decode untuk dipakai. File lain hanya di-hash. File yang cocok `deny_globs` tidak pernah di-decode ke store retrieval.
4. Bangun store IndexedDB: `release_meta`, `chapter_index` (1500 baris), `outline_bytes` (simpan sebagai ArrayBuffer agar offset byte tetap valid), `gates`, `approvals`, `blockers` (dengan scope sudah di-parse), `lifecycle`, `floor_plan_by_chapter`, `carryover`, `canon_sections`, `slim_ledger`.
5. **Slim ledger.** Baca JSONL secara streaming per baris. Untuk tiap baris, verifikasi panjang byte dan offset terhadap `ledger_offset_bytes` dan `ledger_length_bytes` pada indeks, cek `chapter_id`, lalu simpan proyeksi ringkas: semua field skalar; list atau objek di bawah 2 KB disimpan utuh; yang lebih besar disimpan sebagai `{count, last_25, sha256_of_full}`; semua `*_history` hanya `{count}`. Catat `projection_version`. Ukur total hasilnya dan tampilkan di UI (peringatkan bila lebih dari 60 MB). Data mentah tidak disimpan dan tidak pernah dikirim ke LLM.
6. **Indeks section kanon.** Pecah tiap file `canon/` menurut heading Markdown menjadi `{owner, path, heading_path, text, access_label, approval_label, acd_refs, tokens_est}`. Deteksi label akses dari penanda di teks: `PUBLIC`, `RESTRICTED`, `PROTECTED`, `AUTHOR_ONLY`, `reader-only interlude`; dan label persetujuan: `AUTHOR_LOCKED`, `DERIVED_CANON`, `PROPOSED`, `CONTROLLED_OPEN`, `SUPERSEDED`. Section tanpa penanda diberi `UNLABELED` (diperlakukan tertutup untuk S3, lihat §9).
7. **Style Contract.** Bangun otomatis saat impor dari section Narrative Bible berdasarkan `heading_path`: §00.2, §01.1 sampai §01.3, §03 (nada, komedi, pergeseran kalimat), §04.1 sampai §04.4 (anti-slop), §05.1 (diksi), §06.1 (format System UI). Jangan menyalin teks Bible ke kode.
8. Pengaman impor: bila rilis lain dimuat ke project yang sama, bandingkan `release_id` + hash manifest dan **tolak** tanpa konfirmasi (project terikat ke satu `release_id` + manifest hash).

### 6. PIPELINE: S1 sampai S6 PLUS S2B

Prinsip pembagian kerja: yang bisa diperiksa secara **deterministik** (data terstruktur) dikerjakan kode; yang **semantik** (POV, kebocoran, kualitas adegan) dikerjakan LLM lewat Bridge lalu **diverifikasi kode** (mis. kutipan temuan harus benar-benar ada tepat satu kali di teks). UI selalu melabeli mana yang deterministik dan mana yang bantuan LLM.

| Tahap | Dikerjakan | Keluaran dan gate |
| :-- | :-- | :-- |
| **S1 Brief** | Kode (tanpa LLM) | Brief JSON dari **satu** slice outline + slim ledger bab itu + blocker yang berlaku + approval + gate + baris lantai + peristiwa lifecycle yang jatuh di bab ini. Memisahkan jelas: `PLAN/TARGET` vs `ACTUAL` (state committed bab sebelumnya) vs `HOLD` vs `MODEL_BUDGET`. Tidak pernah memuat isi bab lain. Maksimum menampilkan judul bab berikutnya kepada penulis, tidak ke S3. |
| **S2 Prerequisites** | Kode | Validator aturan terstruktur: bab sebelumnya berstatus COMMITTED (kecuali B1), blocker P0 yang berlaku, approval/patch yang disyaratkan, jendela lifecycle (T09: efek Job usia 9 sebelum grant B54 ditolak), consumed/dormant/locked (T08), lantai personal vs group (T07), bukti aktual yang disyaratkan. Hasil: `PASS`, `HOLD` (dengan ID blocker), atau `BLOCKED`. Bukti yang memang akan terjadi di bab ini **direncanakan**, bukan disyaratkan sudah ada sebelum draft (T06). HOLD yang tidak relevan tidak diisi massal dan tidak menahan bab (T30). |
| **S2B Decision Gate** (baru) | LLM + penulis | "Director Pass": LLM menerima Brief aman dan mengembalikan **Beat Plan** + **Decision Cards** (§7). Penulis menjawab kartu dan boleh menyunting beat. Hasil: `author_decision_refs[]`. |
| **S3 Draft** | LLM lewat Bridge | Paket prompt (§9, §14) memakai Brief, Beat Plan, keputusan, Style Contract, POV packet, dan section kanon yang lolos akses S3. Keluaran: teks penuh satu revisi di antara sentinel. Bila menemui titik keputusan baru, LLM **berhenti** dan mengeluarkan `VD_NEEDS_DECISION` (§7). |
| **S4 Audit** | Kode + LLM | (a) *Linter deterministik* (§10) selalu jalan. (b) *Audit LLM* dengan konteks author-only (boleh menyentuh kebenaran terlindungi, hasilnya tidak masuk S3). Temuan P0 sampai P4 dengan kutipan exact, lokasi, expected vs actual, owner, gate, status. Kode menolak temuan yang kutipannya tidak ada atau ambigu (ditandai "tidak terverifikasi"). Tidak ada PASS pada teks kosong dan tidak ada inferensi "aktual" dari rencana. |
| **S5 Patch** | Kode | Patch hanya berbentuk `{old_exact, new}` dengan anchor unik. Tolak: kutipan absen (T18), kutipan ambigu atau patch tumpang tindih (T19), revisi sumber sudah berubah sejak S4 (T20). Hasil selalu **revisi baru**; setelah itu S4 diulang. Revisi lama tidak diubah. |
| **S6 Commit** | Kode + penulis | Prasyarat: revisi saat ini lulus S4 (tidak ada P0/P1 terbuka), keputusan penulis tercatat, `proposed_actual_delta` disetujui penulis, versi/hash cocok. Commit atomik (§11): revisi + `actual_delta` + ringkasan + tautan bukti + keputusan. Tolak bab kosong, audit tidak lulus, audit dari revisi lama, atau transaksi stale (T21, T22). Event ID idempoten: ulang commit tidak menggandakan lantai atau kematian (T24). Bila revisi lama diubah, cache turunan ditandai stale dan bab hilir ditinjau (T25). |

**Kebijakan lulus (usulan aplikasi, bukan kanon; bisa diubah penulis di Pengaturan):** PASS berarti nol P0 dan nol P1 terbuka; P2 sampai P4 harus diakui penulis satu per satu.

**`proposed_actual_delta`.** Dihasilkan dari teks bab yang sudah lulus, setiap perubahan wajib disertai kutipan bukti (locator revisi + kutipan). Bila hasil aktual menyimpang dari target rencana: simpan revisi aktual apa adanya, **berhenti**, klasifikasikan penyebab, tunjuk owner kanon, ajukan keputusan penulis, propagasikan, audit ulang sebelum bab hilir (Integration §2). Tidak ada tombol yang menyalin target ke aktual.

**Mode kerja.** `SANDBOX` (fixture), `PILOT` (hanya B1 sampai gate berikutnya dibuka), `PRODUCTION` (setelah Production Gate disetujui penulis, §12). Mulai dari B1.

### 7. DECISION GATE: INTERAKSI TANYA DAN REKOMENDASI

Inilah fitur yang diminta penulis. Tujuannya: penulis tidak kebobolan keputusan penting yang diam-diam diambil AI.

**7.1 Pemicu deterministik (dari metadata, tanpa LLM)**

| Pemicu | Sumber data | Kartu |
| :-- | :-- | :-- |
| Kematian sejati (D1 sampai D4), kelahiran ulang, grant Job | `S1_Lifecycle_Events_R3.json` (bab jatuh di jendela peristiwa) | Momen Lifecycle |
| Tonggak lantai dan pengecualian disetujui: L250 (`A02-C105-L250-P01`, B105), L1500 (B345), L4500 (B715), L7500 (B1147/B1167), L10000 (B1490), Echo Lantern (B369) | `S1_Author_Approvals_R3.json`, floor plan, carryover | Tonggak Lantai |
| Cabang bersyarat di outline ("jika validasi tidak benar-benar muncul, tahan") | teks `STORY_PROGRESSION` / `CONTINUITY_GATE` | Pilih jalur: sukses terencana, gagal-retry, atau HOLD |
| Bab membawa clue/misteri: ID `SF-M..` atau `MY-..` di `MYSTERY_CLUES_PLANTED`; bab bertipe Misteri; wave reveal berganti | outline + Narrative §09.1 | Batas Reveal |
| Pengecualian POV: Macro/Cinematic, interlude Bumi/Luna, transisi reinkarnasi | `POV_TYPE`, `body_context` | Izin POV (ACD-019) |
| Kondisi tertutup Comedy Valve (kematian, pengorbanan, System Freeze) | `CHAPTER_TYPE`, lifecycle | Nada dan Humor |
| Bab membuka atau menyentuh `OPEN-01..15` (Narrative §10.1) | blocker `CANON_REVIEW`/`INTENTIONAL_HOLD`, klaim di brief | HOLD Dibutuhkan? |
| Romansa | OPEN-15 | Batas Romansa |
| Blocker P0 atau P1 berlaku dan butuh keputusan owner | blocker registry | Keputusan Blocker |
| Bab pembuka Arc dan bab penutup Arc, Arc 17 sampai 18 | indeks + `ARC_STAGE_PHASE` | Fungsi Bab |

**7.2 Pemicu oleh LLM (S2B, S3, S4)**
Karakter, tempat, benda, atau aturan baru yang belum ada di brief; kontradiksi antara rencana dan logika adegan; perlu kebenaran terlindungi agar adegan masuk akal; perubahan tidak dapat dibalik (kematian NPC, pengungkapan, kehilangan Skill); target outline tidak dapat dibuktikan lewat tindakan di adegan; panjang bab melampaui anggaran; pilihan gaya yang mengubah rasa bab (tragis vs hangat, aksi vs investigasi).

**7.3 Skema kartu (JSON, divalidasi di kode)**

```json
{
  "card_id": "DEC-S01-A02-C105-03",
  "chapter_id": "S01-A02-C105",
  "kind": "FLOOR_MILESTONE | LIFECYCLE | BRANCH | REVEAL_LIMIT | POV_EXCEPTION | TONE | OPEN_DEPENDENCY | NEW_ENTITY | PLAN_CONFLICT | IRREVERSIBLE | LENGTH | STYLE | CANON_CHANGE_PROPOSAL",
  "severity": "BLOCKING | RECOMMENDED | INFO",
  "trigger": {"source": "deterministic|llm", "ref": "mis. blocker ID, approval ID, field outline"},
  "question": "satu kalimat konkret",
  "why_it_matters": "satu sampai dua kalimat: apa yang berubah di cerita/state bila salah",
  "options": [
    {"id": "A", "label": "...", "consequence": "...", "state_effect": "...", "reversible": true, "canon_refs": ["owner §nomor"]}
  ],
  "recommendation": {"option_id": "A", "reason": "alasan dengan rujukan outline/kanon, bukan selera"},
  "default_if_skipped": "HOLD"
}
```

**7.4 Perilaku dan UX**
- Antrean kartu berbentuk modal/panel berurutan, maksimal **7 kartu sebelum draft**; kartu serupa digabung. Hanya buat kartu untuk keputusan yang mengubah hasil cerita, kanon, atau state (hindari kelelahan keputusan).
- Tiap kartu: opsi dengan konsekuensi, lencana **Rekomendasi** beserta alasannya, dan lima tindakan: *Pilih opsi*, *Setuju rekomendasi*, *Tahan (HOLD) dan lanjut tanpa mengklaim hasil*, *Tanya lebih lanjut* (membuat prompt lanjutan untuk Bridge), *Tulis jawabanku sendiri*.
- Keparahan: `BLOCKING` tidak bisa dilewati dan tidak pernah auto-resolve; `RECOMMENDED` ditanya di S2B; `INFO` otomatis memakai rekomendasi lalu tampil di ringkasan, bisa diurungkan. Kartu yang terkait `OPEN-xx` atau blocker P0 **tidak pernah** di-auto-resolve. Pengaturan "kepadatan kartu": *Ketat* (tanya semua RECOMMENDED), *Seimbang* (default), *Ringan* (INFO dan sebagian RECOMMENDED otomatis).
- **Keputusan di tengah penulisan.** S3 diinstruksikan: bila ia menemui titik keputusan yang belum dicakup keputusan terdahulu, ia **tidak memilih**; ia berhenti di batas adegan terakhir yang aman dan mengeluarkan blok `<<<VD_NEEDS_DECISION>>>` berisi kartu JSON. Aplikasi menampilkan kartu, lalu menyusun prompt lanjutan ("lanjutkan tepat setelah kalimat terakhir ini") dengan keputusan baru. Tidak pernah mengisi bagian yang hilang dengan tebakan.
- **Pasca-draft.** Hasil audit S4 yang butuh keputusan (bukan sekadar perbaikan prosa) juga menjadi kartu.
- Jawaban disimpan sebagai `decisions/DEC-<chapter>-<n>.json` (immutable). Mengubah keputusan = keputusan baru yang `supersedes` yang lama, lalu revisi terdampak ditandai stale.
- **Keputusan tidak boleh mengubah kanon diam-diam.** Bila opsi pilihan penulis bertentangan dengan aturan pemilik Bible, buat kartu `CANON_CHANGE_PROPOSAL`, simpan di `proposals/canon_proposals.json`, tahan bab, dan **jangan** mengedit rilis. Perubahan kanon adalah keputusan penulis di luar aplikasi.
- Ringkasan keputusan bab ditampilkan lagi sebelum S6.

### 8. GENERATION BRIDGE (cara menghasilkan bab tanpa biaya)

Antarmuka tunggal `generate(package) -> chunks` dengan beberapa adapter. **Jangan** mengklaim penyedia mana pun "gratis".

1. **ManualBridge (default, wajib, selalu gratis).** Aplikasi menyusun *Prompt Package*, menampilkannya dengan tombol salin. Bila lebih panjang dari batas karakter yang diatur penulis, bagi menjadi bagian bernomor ("Bagian 1/3, jangan menjawab sampai Bagian 3 diterima"). Penulis menempelnya ke chat AI yang ia punya, lalu menempel hasil kembali. Penerimaan hasil mendukung **multi-bagian**: tambah bagian demi bagian, hitung kata dan SHA-256 berjalan, deteksi sentinel awal/akhir hilang atau teks terpotong, dan buat prompt "lanjutkan" yang meminta teks lanjutan persis dari kalimat terakhir. Kegagalan di tengah menghentikan alur dan tidak pernah menghasilkan lanjutan kosong atau dugaan (T10).
2. **FreeApiAdapter (opsional, nonaktif secara default, Phase 7).** Memakai key milik penulis lewat `fetch` langsung dari browser, endpoint dan model dikonfigurasi di `providers.json` buatan penulis (tidak ada penyedia bawaan). Peringatan wajib di UI: pastikan akun/proyek tanpa billing, ketentuan tier gratis dan penggunaan data oleh penyedia dapat berubah dan harus dicek penulis sendiri. Hanya konteks lolos-S3 yang boleh dikirim. Dukung streaming dan perakitan teks persis; kegagalan tengah-stream menghentikan alur. Tangani batas laju dengan backoff.
3. **Tidak termasuk:** model lokal di browser (kualitas prosa Indonesia untuk kanon sebesar ini tidak memadai), server proxy apa pun.

Konfigurasi di Pengaturan: **target panjang bab** (kata) dan anggaran konteks (token). Panjang bab adalah keputusan terbuka di Bible (NV-O09), maka kolom ini **wajib diisi penulis** sebelum S3; boleh tampilkan saran awal sebagai placeholder, tetapi jangan diterapkan diam-diam. Estimasi token memakai pendekatan karakter (mis. 3,2 karakter per token untuk Bahasa Indonesia), ditampilkan sebagai Context Meter dengan batas awal 24.000 token untuk paket S3 (dapat diubah).

### 9. CONTEXT PACKER DAN KONTROL AKSES

Retrieval pada Allowlist berbunyi `SECTION_SCOPED_AUTHOR_ONLY_IF_PROTECTED; no direct full-Bible prompt`. Maka:

- **Dua tingkat konteks.** `AUTHOR_CONTEXT` (dipakai S1, S2, S4; boleh memuat kebenaran terlindungi bila perlu memeriksa) dan `S3_CONTEXT` (dipakai penulisan; **tidak** boleh memuat kebenaran `PROTECTED`/`AUTHOR_ONLY` masa depan). "Kebenaran yang diketahui penulis bukan kebenaran yang diketahui karakter."
- **Isi S3_CONTEXT:** (a) Style Contract; (b) POV packet: apa yang diketahui karakter POV saat ini, diturunkan dari `knowledge_*` pada slim ledger dan brief, ditandai rencana atau aktual; (c) slice outline bab ini; (d) section kanon ber-label `PUBLIC` atau `RESTRICTED` yang relevan dan sesuai wave reveal; (e) ringkasan dan ekor (sekitar 400 kata) bab sebelumnya yang sudah COMMITTED; (f) keputusan penulis.
- **Retrieval tanpa biaya:** routing pemilik berdasarkan field outline (lokasi ke WORLD/TOWER, karakter ke CHAR/DOSSIER, istilah power ke POWER, faksi ke FACTION, ras ke RACE, misteri ke STORY/CORE untuk author-only) lalu BM25 buatan sendiri di atas indeks section, dengan kuota per pemilik. Tidak ada embedding, tidak ada layanan luar. Selalu per section, tidak pernah satu Bible utuh.
- **Context Inspector:** daftar section terpilih, label akses, perkiraan token, tombol sertakan/keluarkan. Setiap pengecualian tercatat.
- **Section `UNLABELED`:** bila retrieval memilih satu, tampilkan kartu "Izinkan section ini untuk S3?". Jawaban disimpan di `access/access_overrides.json` pada repo data (keputusan penulis, bukan tebakan aplikasi). Default fail closed.
- **Protected Term Watchlist:** `access/protected_terms.json`. Saat impor, usulkan istilah dari heading section `PROTECTED`/`AUTHOR_ONLY`; **penulis mengonfirmasi** daftarnya. Dipakai linter untuk mendeteksi kebocoran lintas-wave (T04, T12). Jangan hardcode istilah kanon di kode.

### 10. LINTER DETERMINISTIK (S4a) DAN CHECKLIST AUDIT LLM (S4b)

**Linter (kode, selalu jalan, hasil dengan kutipan dan posisi):**

| Cek | Tingkat |
| :-- | :-- |
| Karakter U+2014 atau tiga tanda hubung beruntun di prosa | FAIL |
| Frasa dan pola terlarang dari Narrative §04.1 (diparse dari Bible saat impor, bisa ditinjau di Pengaturan; pola seperti "Bukan sekadar X, melainkan Y" memakai regex; ambang pengulangan sesuai §04.4) | WARN, FAIL bila melewati ambang |
| Tiga kalimat berturut-turut diawali subjek yang sama (§04.2 no. 1) | WARN |
| Kebocoran metadata ke prosa: ID bab (`S01-A..-C...`), `HOLD`, `OPEN-nn`, `P0`, nama field ledger/outline, istilah analitis Power System yang dilarang disalin (§00.2) | FAIL |
| Pola terlarang kanon yang bisa dicek regex: `R10`, `D5` (NV-G12, NV-G13) | FAIL |
| Istilah di Protected Term Watchlist muncul sebelum jendela reveal | FAIL |
| Blok System UI tidak memakai format blockquote dwibahasa §06.1 (header Inggris, deskripsi Indonesia) | WARN |
| Kata ganti gaul (`gue/lu`) pada narasi (§05.1) | WARN |
| Jumlah kata di luar toleransi target panjang bab (Pengaturan) | WARN |
| Sentinel awal/akhir revisi tidak utuh, teks kosong, atau terpotong | FAIL |

**Audit LLM (S4b)** memeriksa hal semantik yang tidak bisa diregex: head-hopping dan batas POV (NV-G02), akses informasi dan spoiler (NV-G03), fair-play (NV-G04), Visual Bridge (NV-G05), Comedy Valve (NV-G08), konsistensi karakter dan romansa (NV-G09, NV-G17), System Freeze sebagai kondisi psikologis (NV-G10), Skill/Fusion/dormant (NV-G11), Earth dan Luna (NV-G14), mode dan bukti clearance (NV-G15), pacing perubahan state (NV-G16), finale (NV-G18), penyelesaian terselubung atas data terbuka (NV-G19), serta skenario T11 sampai T17 pada matriks akseptansi. **Detail aturan diambil dari section kanon yang di-retrieve, bukan ditulis ulang di kode atau prompt.** Setiap temuan wajib memuat: `severity` (P0 sampai P4), kutipan exact, locator, expected vs actual, owner kanon, gate terkait, status `PASS|FAIL|HOLD`, `evidence_map` untuk setiap target perubahan.

### 11. PENYIMPANAN

**Lokal (IndexedDB):** store rilis (§5), `drafts`, `audits`, `decisions`, `actual_state`, `commit_queue`, `settings` (tanpa rahasia), `acceptance_results`.

**GitHub (repo privat `void-domain-data`), tata letak:**

```
project.json                       (project_id, release_id, release_manifest_sha256, prompt_version)
chapters/<CHAPTER_ID>/rev-0001.md  (teks persis, UTF-8, tanpa normalisasi newline)
chapters/<CHAPTER_ID>/commit.json  (commit_id, revision_sha256, actual_delta, summary, evidence_refs, decision_refs)
state/actual_state.json            (state aktual berjalan; mulai kosong)
state/snapshots/<CHAPTER_ID>.json
decisions/DEC-<CHAPTER_ID>-<n>.json
proposals/canon_proposals.json
access/access_overrides.json  access/protected_terms.json
audits/<CHAPTER_ID>-rev-0001.json
```

- **Commit atomik (S6)** memakai Git Data API: buat blob untuk semua file, buat tree dari `base_tree`, buat commit dengan parent = head yang dibaca di awal, lalu update ref **tanpa force**. Bila head sudah bergeser: berhenti, tampilkan perbedaan, penulis memutuskan. Jangan pernah force push. Mirror ke IndexedDB baru dilakukan setelah remote sukses; bila offline, simpan di `commit_queue` dengan `event_id` yang sama dan coba ulang secara idempoten (T23, T24).
- Skema data mengikuti Integration §3: `project_id, chapter_id, release_id, release_manifest_sha256, source_outline_slice_sha256, previous_committed_revision, revision_id, revision_sha256, stage, prior_actual_proofs[], author_decision_refs[], applicable_blocker_ids[], intended_delta, actual_delta, evidence_refs[], audit_status, commit_id`. Ini usulan skema dari dokumen integrasi; penulis menyetujui pemakaiannya di sini, perubahan lain harus lewat persetujuan penulis.
- **Backup/restore:** ekspor seluruh data proyek (tanpa release pack) ke satu file JSON, dan impor dengan *dry-run* yang membandingkan jumlah dan hash sebelum menimpa apa pun (T26).
- Lantai: kunci unik (project, participant, floor, event); status group dan prisoner independen; carryover tidak menambah alokasi. Hitungan kematian aktual naik tepat sekali per kematian biologis sejati; kelahiran ulang adalah peristiwa terpisah.

### 12. PRODUCTION GATE DAN UJI AKSEPTANSI

Rilis ini sendiri menyatakan `RELEASED_PLAN` bukan `PRODUCTION_READY`. Aplikasi wajib menegakkannya:

- Halaman **Production Gate** menampilkan: hash lokal dan parse (lulus/tidak), blocker `IMPORT` yang berlaku (9 baris, termasuk P0 yang masih `OPEN` seperti `RUN-002`), konflik sumber yang butuh tinjauan penulis, hasil uji runtime, dan status per bab.
- **Acceptance Runner** menjalankan skenario `T01` sampai `T30` dari `integration/Runtime_Acceptance_Test_Matrix_R3.csv` (kolom `id, stage, scenario, expected, website_result, actual_evidence`) terhadap rilis yang dimuat dan terhadap fixture berlabel test untuk skenario yang butuh data fabrikasi. Status: `NOT_RUN | PASS | FAIL | MANUAL_REVIEW`, plus tautan bukti. Fixture tidak pernah di-commit ke state kanon.
- Tambahan uji keputusan: **D01** kartu BLOCKING tidak bisa dilewati; **D02** kartu INFO otomatis dan bisa diurungkan; **D03** kartu terkait OPEN/P0 tidak pernah auto-resolve; **D04** `CANON_CHANGE_PROPOSAL` menahan bab dan tidak mengubah rilis; **D05** `VD_NEEDS_DECISION` di tengah draft berlanjut tepat setelah kalimat terakhir; **D06** maksimal 7 kartu pra-draft; **D07** keputusan immutable dan supersede menandai revisi stale; **D08** section `UNLABELED` butuh otorisasi sebelum masuk S3.
- Membuka mode `PRODUCTION` butuh persetujuan eksplisit penulis di halaman ini; tidak pernah otomatis. Per bab: bukti aktual sebelumnya harus ada dan blocker yang berlaku harus tertutup. HOLD yang tidak relevan tidak dipaksa diisi.
- Kesiapan **pilot**: impor lulus, B1 diuji dahulu, lalu uji batas bab terpilih dengan fixture berlabel test. Jangan memalsukan bukti aktual untuk B105 dan seterusnya.

### 13. LAYAR (Bahasa Indonesia, responsif, mode terang/gelap, bisa dipakai di ponsel)

1. **Beranda**: status rilis, hash, mode kerja, bab terakhir yang COMMITTED, peringatan blocker.
2. **Impor dan Verifikasi**: pilih ZIP, progres, hasil hash, ukuran slim ledger.
3. **Studio Bab**: pemilih bab (pohon Arc, Fase, Bab), stepper `S1, S2, S2B, S3, S4, S5, S6`, panel tengah per tahap, panel kanan berisi Context Inspector, Blocker yang berlaku, dan Keputusan.
4. **Kartu Keputusan**: antrean dan riwayat.
5. **Editor Revisi**: diff antar revisi, penerapan patch S5, hasil audit dengan lompat ke kutipan.
6. **Register Keputusan dan Proposal Kanon.**
7. **Production Gate dan Acceptance Runner.**
8. **Pengaturan**: repo data dan token, Bridge, target panjang bab, anggaran token, kepadatan kartu, kebijakan lulus, template prompt (berversi).
9. **Backup dan Restore.**

### 14. TEMPLATE PROMPT YANG HARUS DIKIRIM APLIKASI KE LLM

Simpan sebagai file teks berversi (`prompt_version`) yang bisa diedit di Pengaturan. Placeholder berbentuk `{{...}}`. Isi di bawah adalah kerangka perilaku; **jangan menambahkan fakta kanon ke dalam template**.

**14.1 PREAMBLE (dipakai semua template)**

```
Kamu penulis fiksi Bahasa Indonesia yang bekerja di bawah kontrak kanon ketat untuk novel VOID DOMAIN.
Aturan mutlak:
1. Hanya pakai informasi di PAKET KONTEKS. Jika yang kamu butuhkan tidak ada, jangan mengarang. Ajukan keputusan (VD_NEEDS_DECISION) atau tulis tanpa mengklaimnya.
2. RENCANA/TARGET bukan fakta. Yang terjadi hanyalah yang kamu tuliskan lewat tindakan dan konsekuensi di adegan. Jangan memaksakan hasil bila logika adegan gagal.
3. Karakter hanya tahu yang ada di POV PACKET. Kebenaran penulis bukan pengetahuan karakter.
4. Jangan memasukkan metadata ke prosa: ID bab, label status, HOLD, nama field, istilah analitis dari Bible.
5. Jangan memakai karakter em dash (U+2014) maupun tiga tanda hubung berurutan.
6. Ikuti STYLE CONTRACT. Patuhi daftar frasa terlarang di dalamnya.
7. Keluaran hanya dalam format yang diminta.
```

**14.2 S2B Director Pass**

```
{{PREAMBLE}}
TUGAS: Rencanakan bab {{CHAPTER_ID}}. JANGAN menulis prosa.
PAKET KONTEKS (S3_CONTEXT): {{BRIEF_JSON}} {{POV_PACKET}} {{CANON_PACKETS}} {{PREV_SUMMARY}}
KARTU YANG SUDAH ADA (jangan diduplikasi): {{EXISTING_CARDS}}
Hasilkan:
1. beat_plan: 6 sampai 14 beat. Tiap beat: tujuan konkret, hambatan, tindakan, perubahan keadaan, POV, lokasi, dan apakah perubahan itu TARGET (belum terbukti).
2. key_moments: momen penting yang butuh perhatian penulis dan alasannya.
3. new_cards: maksimal 5 kartu keputusan baru sesuai skema, hanya untuk hal yang tidak tercakup kartu yang sudah ada. Setiap kartu wajib punya rekomendasi beralasan yang merujuk bagian paket konteks.
4. canon_risks: hal di paket yang berpotensi bertentangan atau kurang data.
Keluaran: satu objek JSON di antara <<<VD_JSON_BEGIN>>> dan <<<VD_JSON_END>>>.
```

**14.3 S3 Draft**

```
{{PREAMBLE}}
STYLE CONTRACT: {{STYLE_CONTRACT}}
PAKET KONTEKS (S3_CONTEXT): {{BRIEF_JSON}} {{POV_PACKET}} {{CANON_PACKETS}}
BAB SEBELUMNYA (committed): ringkasan {{PREV_SUMMARY}}; ekor {{PREV_TAIL}}
BEAT PLAN YANG DISETUJUI: {{BEAT_PLAN}}
KEPUTUSAN PENULIS: {{DECISIONS_RESOLVED}}
TARGET PANJANG: sekitar {{TARGET_WORDS}} kata.
Prinsip yang dirujuk dokumen integrasi: Fantasy First, Design Light, Love Without Possession (definisi lengkap hanya dari section kanon di paket; jangan mendefinisikan sendiri).
Tulis seluruh bab sebagai satu revisi. Tindakan dan konsekuensi di adegan harus membuktikan perubahan target; jangan menyatakan hasilnya lewat narasi ringkasan.
Jika kamu menemui titik keputusan yang belum dicakup KEPUTUSAN PENULIS (entitas baru, aturan baru, kebenaran terlindungi, perubahan tak terbalikkan, kontradiksi rencana), JANGAN memilih. Berhenti di batas adegan terakhir yang aman, lalu keluarkan:
<<<VD_NEEDS_DECISION>>> {kartu JSON sesuai skema} <<<VD_END_DECISION>>>
dan berhenti menulis.
Format keluaran persis:
<<<VD_CHAPTER_BEGIN id={{CHAPTER_ID}} rev={{REV}}>>>
(teks bab)
<<<VD_CHAPTER_END>>>
```

**14.4 S3 Lanjutan (multi-bagian atau setelah keputusan)**

```
{{PREAMBLE}} Lanjutkan penulisan bab {{CHAPTER_ID}} rev {{REV}} PERSIS setelah kalimat terakhir berikut. Jangan mengulang teks sebelumnya, jangan meringkas, jangan menulis ulang.
KALIMAT TERAKHIR: {{LAST_SENTENCE}}
KEPUTUSAN BARU: {{NEW_DECISIONS}}
Selesaikan dengan <<<VD_CHAPTER_END>>>.
```

**14.5 S4 Audit (AUTHOR_CONTEXT)**

```
{{PREAMBLE}}
TUGAS: Audit revisi berikut. Kamu BOLEH memakai konteks author-only di bawah hanya untuk memeriksa; jangan menyalinnya ke temuan kecuali perlu bukti.
KONTEKS: {{BRIEF_JSON}} {{AUTHOR_CANON_PACKETS}} {{PRIOR_ACTUAL_STATE}} {{DECISIONS_RESOLVED}} {{LINTER_RESULT}}
REVISI (sha256 {{REV_SHA}}): {{FULL_TEXT}}
Keluaran JSON di antara <<<VD_JSON_BEGIN>>> dan <<<VD_JSON_END>>>:
{ "findings": [ {"id","severity":"P0..P4","quote":"salinan PERSIS, unik di teks","locator","expected","actual","owner","gate","status":"PASS|FAIL|HOLD","fix_hint"} ],
  "evidence_map": [ {"target_change","quote","verdict":"PROVEN|NOT_PROVEN"} ],
  "proposed_actual_delta": { "<field>": {"value","evidence_quote"} },
  "questions_for_author": [ kartu sesuai skema ] }
Aturan: tanpa kutipan exact dan unik, temuan tidak berlaku. Teks kosong tidak pernah PASS. Nilai yang tidak terbukti ditulis null atau HOLD, bukan 0. Jangan menyimpulkan keadaan aktual dari rencana.
```

**14.6 S5 Saran Patch (opsional, hasilnya tetap divalidasi kode)**

```
Untuk setiap temuan FAIL yang bisa diperbaiki di prosa, usulkan patch {"finding_id","old_exact","new","reason"}. old_exact harus kutipan persis dan unik. Jangan mengubah fakta kanon atau state; bila perbaikan butuh keputusan penulis, keluarkan kartu, bukan patch.
```

### 15. RENCANA FASE DAN ATURAN KERJA CODEX

**Aturan tiap task:** (1) baca `AGENTS.md` dan `PROGRESS.md` dulu; (2) kerjakan **hanya** fase yang diminta; (3) tulis test untuk logika inti sebelum atau bersama kode; `node --test` harus hijau; (4) tanpa dependensi baru; (5) jangan mengarang kanon (Z6); (6) bila butuh keputusan penulis, catat di `PROGRESS.md` bagian `QUESTIONS_FOR_AUTHOR`, jangan menebak; (7) perbarui `PROGRESS.md`: selesai, belum, risiko, hasil test; (8) laporan akhir berformat: ringkasan, file diubah, hasil test, hal yang tidak bisa diuji di sandbox (mis. rilis asli), pertanyaan untuk penulis.

| Fase | Isi | Selesai bila |
| :-- | :-- | :-- |
| 0 | Scaffold, `AGENTS.md`, `PROGRESS.md`, vendor, generator fixture mini-release, runner test, cek U+2014, shell UI, siap Pages | `node --test` hijau; halaman terbuka lokal; fixture dapat dibuat; docs tersimpan |
| 1 | Impor ZIP streaming, verifikasi hash, allowlist/deny, parser outline 15 field, slim ledger, indeks section, parser `chapter_scope`, lifecycle, Style Contract | T01, T02, T03, T27, T28 lulus pada fixture; ukuran slim dilaporkan; tidak ada decode file deny |
| 2 | S1 Brief, S2 Prerequisites, Context Packer, Context Inspector, kontrol akses, watchlist | T04 sampai T09, T30 lulus pada fixture |
| 3 | S2B Decision Gate: kartu deterministik, skema, UI, register, proposal kanon | D01 sampai D08 lulus |
| 4 | ManualBridge multi-bagian, template prompt, S3 penerimaan, linter, S4 audit | T10 sampai T17 (bagian yang dapat diuji) lulus |
| 5 | S5 patch, S6 commit atomik, GitHub store, `commit_queue`, backup/restore | T18 sampai T26 lulus (GitHub dengan mock) |
| 6 | Production Gate, Acceptance Runner, hardening, dokumentasi pengguna | Seluruh matriks terhubung; penulis menjalankan terhadap rilis asli |
| 7 (opsional) | FreeApiAdapter, wizard migrasi prosa yang sudah ada, sinkron slim pack ke repo privat, PWA offline | Disetujui penulis terlebih dahulu |

***

## BAGIAN 2. TASK PROMPT PER FASE (tempel salah satu per task)

**Phase 0**
```
Baca master prompt terlampir. Kerjakan HANYA Phase 0: buat scaffold repo sesuai §4, simpan master prompt ke project-docs/MASTER_PROMPT.md dan ringkasannya (Bagian 1 §1, §2, §6, §15) ke AGENTS.md, buat PROGRESS.md, vendor library (fflate dan SHA-256 inkremental) dengan VENDOR.md, buat generator fixture mini-release berlabel TEST_FIXTURE (3 sampai 5 bab, struktur folder dan kolom identik dengan rilis R3, termasuk indeks CSV dengan offset byte UTF-8 yang benar, manifest dengan hash, allowlist, blocker registry berisi scope "1290-1500", "625-700;730-1490", "299-345+", "ALL"), runner node --test, test yang memeriksa tidak ada U+2014 di repo, dan shell UI Bahasa Indonesia yang berjalan di GitHub Pages dan di python3 -m http.server. Jangan implementasikan fase lain. Laporkan sesuai format §15.
```

**Phase 1**
```
Kerjakan HANYA Phase 1 (impor dan verifikasi rilis) sesuai §3 dan §5. Mulai dengan test untuk: pembacaan slice outline berdasarkan offset byte UTF-8 (termasuk karakter multibyte), pembuangan sisa heading Fase dan ***, mismatch CHAPTER_ID, hash tamper (T02), deny_globs (T03), parser chapter_scope, proyeksi lifecycle D1 sampai D4, slim ledger streaming, dan memori terkendali (T27). Implementasikan di Web Worker dengan progres dan pembatalan. Laporkan ukuran slim ledger pada fixture dan perkiraan untuk 842 MB.
```

**Phase 2**
```
Kerjakan HANYA Phase 2: S1 Brief, S2 Prerequisites, Context Packer (§9), Context Inspector, label akses dan kartu otorisasi UNLABELED, Protected Term Watchlist. Pastikan Brief hanya memuat satu bab (T01), S3_CONTEXT tidak memuat section PROTECTED/AUTHOR_ONLY/UNLABELED (T04), S2 memblokir dependensi bab sebelumnya yang belum COMMITTED (T05), mengizinkan draft terkendali bila prasyarat ada tetapi bukti bab ini belum ada (T06), menolak operasi consumed/dormant/locked (T08), menolak efek Job sebelum grant (T09), dan tidak mengisi massal HOLD yang tidak relevan (T30).
```

**Phase 3**
```
Kerjakan HANYA Phase 3: S2B Decision Gate sesuai §7. Implementasikan pemicu deterministik, skema kartu dengan validasi, antrean UI, tindakan lima jenis, keparahan BLOCKING/RECOMMENDED/INFO, kepadatan kartu, register keputusan immutable dengan supersede, CANON_CHANGE_PROPOSAL, dan penanganan VD_NEEDS_DECISION. Tulis test D01 sampai D08. Dokumentasikan semua jenis kartu di project-docs/DECISION_CARDS.md dengan contoh yang diberi label fixture.
```

**Phase 4**
```
Kerjakan HANYA Phase 4: ManualBridge multi-bagian (§8), template prompt versi (§14), penerimaan hasil S3 dengan sentinel, linter deterministik (§10) yang membaca aturan dari Bible (fixture) saat impor, dan S4 dengan verifikasi kutipan unik. Test: T10 sampai T17 untuk bagian yang bisa dijalankan tanpa LLM nyata (gunakan respons LLM palsu berlabel TEST_FIXTURE). Tambahkan Context Meter dan peringatan sebelum menyalin paket yang berisi materi author-only.
```

**Phase 5**
```
Kerjakan HANYA Phase 5: S5 patch, S6 commit atomik lewat Git Data API (§11) dengan mock GitHub di test, commit_queue idempoten, cache stale, backup/restore dengan dry-run. Test T18 sampai T26. Jangan menyimpan token di log atau di repo.
```

**Phase 6**
```
Kerjakan HANYA Phase 6: Production Gate dan Acceptance Runner (§12) yang membaca Runtime_Acceptance_Test_Matrix_R3.csv, menampilkan blocker IMPORT, menyimpan hasil dan bukti, hardening, aksesibilitas, dan panduan pengguna Bahasa Indonesia. Mode PRODUCTION tidak boleh terbuka tanpa persetujuan eksplisit penulis.
```

**Phase 7 (hanya setelah penulis setuju)**
```
Kerjakan HANYA Phase 7 bagian yang disebutkan penulis: [FreeApiAdapter | wizard migrasi | sinkron slim pack | PWA offline]. Patuhi Z1 dan Z4 tanpa pengecualian.
```

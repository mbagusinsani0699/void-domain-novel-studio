# RELEASE SCHEMA DIGEST R3 (metadata struktural saja)

Dokumen ini hanya berisi **nama path, nama kolom, nama kunci JSON, dan jumlah baris** dari `VOID_DOMAIN_S1_Consolidated_Release_R3.zip`. Tidak ada isi cerita, kanon, atau teks outline. Aman ditaruh di repo aplikasi. Simpan sebagai `project-docs/RELEASE_SCHEMA_DIGEST_R3.md`.

Pakai ini untuk membuat fixture dan parser yang cocok dengan rilis asli. Bila rilis asli ternyata berbeda dari digest ini, **rilis asli yang benar**: laporkan selisihnya, jangan menyesuaikan rilis.

***

## 1. Path allowlist (23) dan peran

Semua path lain di ZIP hanya di-hash (lihat manifest), tidak di-decode untuk dipakai.

| Path | Peran di allowlist |
| :-- | :-- |
| `canon/Character Bible v3.1.md` | CANON_OWNER (owner `CHAR`) |
| `canon/Core Concept v3.1.md` | CANON_OWNER (`CORE`) |
| `canon/Faction & Political System Bible v2.1.md` | CANON_OWNER (`FACTION`) |
| `canon/Master Character Dossier & Conflict Engine Bible v2.1.md` | CANON_OWNER (`DOSSIER`) |
| `canon/Master Outline Bible v2.1.md` | CANON_OWNER (`OUTLINE`) |
| `canon/Narrative, POV, Style & Diction Bible v3.1.md` | CANON_OWNER (`NARR`) |
| `canon/Power System Bible v3.1.md` | CANON_OWNER (`POWER`) |
| `canon/Race Bible v3.1.md` | CANON_OWNER (`RACE`) |
| `canon/Story Foundation v3.1.md` | CANON_OWNER (`STORY`) |
| `canon/Tower & System Bible v3.1.md` | CANON_OWNER (`TOWER`) |
| `canon/World Bible v3.1.md` | CANON_OWNER (`WORLD`) |
| `canon/Register Resolusi R1.md` | AUTHOR_DECISION_MODEL (tanpa owner) |
| `canon/Technical Model - approved R1.md` | AUTHOR_DECISION_MODEL (tanpa owner) |
| `production/S1_Author_Approvals_R3.json` | APPROVED_PLAN_DATA |
| `production/S1_Chapter_Gates_R3.jsonl` | APPROVED_PLAN_DATA |
| `production/S1_Chapter_Index_R3.csv` | APPROVED_PLAN_DATA |
| `production/S1_Chapter_Outline_1-1500_Granular_R3.md` | APPROVED_PLAN_DATA |
| `production/S1_Floor_Carryover_R3.csv` | APPROVED_PLAN_DATA |
| `production/S1_Lifecycle_Events_R3.json` | APPROVED_PLAN_DATA |
| `production/S1_Master_Blocker_Registry_R3.csv` | APPROVED_PLAN_DATA |
| `production/S1_Master_Floor_Clearance_Plan_R3.csv` | APPROVED_PLAN_DATA |
| `production/S1_Master_State_Ledger_R3.jsonl` | APPROVED_PLAN_DATA |
| `production/S1_State_Changes_R3.jsonl` | APPROVED_PLAN_DATA |

Catatan: nama file bisa mengandung spasi, koma, dan `&`. Jangan mengasumsikan path aman untuk URL atau shell; gunakan pengutipan yang benar.

Path di `integration/` (`Novel_Generator_Integration_R3.md`, `Runtime_Acceptance_Test_Matrix_R3.csv`, `preflight_release.py`) dan root (`README.md`, `CHANGELOG_R3.md`) ada di manifest sebagai `REFERENCE_OR_PLAN`. `Runtime_Acceptance_Test_Matrix_R3.csv` **tidak** ada di allowlist, tetapi Production Gate memerlukannya: baca sebagai file referensi yang di-hash dan diverifikasi manifest, bukan sebagai pengetahuan cerita.

## 2. Manifest dan allowlist (JSON)

`Release_Manifest_R3.json`: kunci level atas `release_id, base_canon, status, production_activation, chapter_count, arc_count, stage_count, phase_count, floor_allocations, carryover_rows, actual_commits, author_approved_arcs, source_allowlist, author_approvals, blockers, source_hashes, state_schema, runtime_tests, static_checks, files, self_hash`.
`files` = 128 objek, tiap objek berkunci `path, bytes, sha256, role`. Peran: `AUDIT_ONLY` (99), `REFERENCE_OR_PLAN` (29). `sha256` huruf kecil heksadesimal 64 karakter. Manifest tidak memuat hash dirinya.

`Source_Allowlist_R3.json`: kunci `release, deny_by_default, precedence, allowed, deny_globs, note`. Tiap objek `allowed` berkunci `path, sha256, role, owner, retrieval`; `owner` hanya ada pada CANON_OWNER. `precedence` adalah array string berurutan (lima tingkat).

## 3. CSV (semua UTF-8 dengan BOM, akhir baris CRLF, sel berkutip bila perlu)

**S1_Chapter_Index_R3.csv** (1500 baris), 24 kolom berurutan:
`chapter, chapter_id, arc, title, outline_offset_bytes, outline_length_bytes, ledger_offset_bytes, ledger_length_bytes, stage, phase, target_life, target_deaths, target_realm, target_job_history, earth_job_effect, target_personal_max_floor, target_C, target_F, target_dormant, target_consumed, actual_committed, status, canonical_stage, body_context`
`chapter_id` berpola `S01-Axx-Cnnn` (nomor bab dengan tiga digit atau lebih). `actual_committed` bernilai teks `False`/`True`. `phase` bisa berupa angka (`1`) atau label seperti `A02.3`.

**S1_Master_Floor_Clearance_Plan_R3.csv** (10.000 baris; semua `plan_status` = `CONDITIONAL_PLANNED_NOT_COMMITTED`; kolom `mode` bernilai 1, 2, atau 3), 20 kolom:
`floor, mode, allocation_arc, source_arc, planned_chapter, participant, objective_plan, expected_evidence, dependency, actual_clearance, actual_objective_proof, actual_cost, source_row_json, planned_first_personal_clear_chapter, plan_status, branch_scene, actual_protection_state, actual_prisoner_state, actual_group_clearance, actual_reward`
Kolom `actual_*` kosong. `source_row_json` berisi JSON dalam sel berkutip dengan tanda kutip ganda yang di-escape (`""`). `participant` bukan hanya satu ID: ada nilai `CH-nnn` polos (1500 baris), `CH-nnn LIFE-nn; <catatan peserta lain>` (berbagai catatan), dan `Kael LIFE-nn; <catatan>`. Jadi parse `participant` sebagai teks bebas dengan token ID di awal, jangan sebagai enum.

**S1_Floor_Carryover_R3.csv** (5 baris), 20 kolom:
`floor, mode, allocation_arc, source_arc, planned_chapter, participant, objective_plan, branch_scene, expected_evidence, dependency, actual_clearance, actual_objective_proof, actual_cost, actual_protection_state, actual_prisoner_state, actual_group_clearance, actual_reward, source_row_json, plan_status, planned_first_personal_clear_chapter`
(Urutan kolom berbeda dari floor plan. Parse berdasarkan nama kolom, bukan posisi.)

**S1_Master_Blocker_Registry_R3.csv** (57 baris), 10 kolom:
`id, category, priority, chapter_scope, finding, source_locator, owner, closure_criteria, gate_scope, status`
`priority`: `P0` (33), `P1` (17), `P2` (7). `gate_scope`: `IMPORT` (9), `AFFECTED_CHAPTERS` (48). `category`: `EXECUTION_BLOCKER, FEASIBILITY, INTENTIONAL_HOLD, DATA_CONFLICT, CANON_REVIEW, RUNTIME_BLOCKER, PACING`. `status` yang ada: `OPEN`, `RESOLVED_IN_R3_PROJECTION_RUNTIME_PENDING`, `RESOLVED_IN_R3_INDEX_RUNTIME_PENDING`, `RESOLVED_IN_R3_PATCH_RUNTIME_PENDING`, `APPROVED_EXCEPTION_PROOF_PENDING`, `OWNER_RULE_CONFIRMED`. Sel `finding` dan `closure_criteria` bisa panjang dan berkoma (berkutip).
Bentuk `chapter_scope` yang terbukti ada: `1290–1500` (**en dash**, bukan hubung biasa), `625–700;730–1490` (beberapa rentang dipisah titik koma), `299–345+` (terbuka ke atas), `ALL`. Parser juga harus menerima hubung biasa `-` sebagai toleransi, tetapi melaporkan bila menemukan bentuk lain yang tidak dikenal (fail closed).

**integration/Runtime_Acceptance_Test_Matrix_R3.csv** (30 baris, `T01` sampai `T30`), 6 kolom: `id, stage, scenario, expected, website_result, actual_evidence`. Nilai `stage` bervariasi: `S1`, `S2`, `S3`, `S4`, `S5`, `S6`, `Storage`, `S1–S6` (en dash). `website_result` semuanya `NOT_RUN` dan `actual_evidence` kosong.

## 4. Outline Markdown (`S1_Chapter_Outline_1-1500_Granular_R3.md`, sekitar 3,5 MB)

- Slice bab dimulai pada heading `### BAB nnn: <judul>` (1500 heading seperti ini). Panjang slice dari indeks mencakup field bab dan sisa penutup.
- Berisi 15 field berlabel `* \`NAMA_FIELD:\` nilai` per bab, berurutan: `CHAPTER_ID, LOCATION_&_FLOOR_MODE, KAEL_CYCLE_&_TIMELINE, POV_TYPE, SCENE_OBJECTIVE, CONFLICT, STORY_PROGRESSION, CHARACTER_PROGRESSION, MYSTERY_CLUES_PLANTED, CHAPTER_TYPE, ENDING_HOOK, CANON_STATUS_LABEL, ARC_STAGE_PHASE, CONTINUITY_GATE, STATE_LEDGER`. Nilai satu field bisa berupa satu paragraf panjang.
- Setelah field ada pemisah `***` dan kadang heading fase `#### Fase Axx.n: ...` (30 buah) milik bab berikutnya. Ada juga heading `## RELEASE ARC n | ...` (18 buah) dan heading lain (`# VOID DOMAIN | ...`, `## Chapter Outline`, `### Bn`, dan seterusnya) di tempat lain dalam file. **Jangan memindai heading untuk membatasi bab; gunakan hanya offset byte dari indeks** lalu validasi `CHAPTER_ID`.
- Teks outline mengandung karakter di luar ASCII (huruf beraksen dan tanda baca tipografis). Offset adalah byte, jadi simpan sebagai bytes dan dekode setelah slice. Fixture harus menguji ini, termasuk karakter multibyte.

## 5. JSON dan JSONL

**S1_Chapter_Gates_R3.jsonl** (1500 baris): kunci `actual_evidence, chapter_id, effective_dependency, runtime_state, source_gate`. `actual_evidence` = `null`; `runtime_state` = `NOT_EXECUTED`; `effective_dependency` bisa berupa string hanya spasi.

**S1_State_Changes_R3.jsonl** (178 baris, **tidak** per setiap bab): kunci `actual_proof, changes, chapter, chapter_id`. `changes` adalah objek per nama field state dengan `post` (dan mungkin kunci lain). `actual_proof` = `null`.

**S1_Author_Approvals_R3.json**: kunci `all_arcs, status, approval_source, patch_L250, L1500, L4500, L7500, A18, S01_Echo_Lantern_exception, no_actual_commit`. Bentuk tiap kunci berbeda-beda (ada `chapter`, `approved`, `verification_retry`, `source_sha256`, `actual_proof`, dan lain-lain). Parser jangan mengasumsikan skema seragam; simpan apa adanya dan petakan hanya kunci yang diketahui.

**S1_Lifecycle_Events_R3.json**: kunci `planned_deaths` (4: D1 bab 299, D2 bab 699, D3 bab 1149, D4 bab 1290), `planned_rebirths` (4: RB1 bab 300, RB2 bab 700, RB3 bab 1150, RB4 bab 1291), `planned_job_grants` (4, pada bab 307, 706, 1159, 1313), `source_note`.
- Elemen `planned_deaths` memakai gabungan kunci (tidak semua elemen punya semuanya): `actual_fatal_proof, actual_injury_proof, actual_proof, category, cause, chapter, earth_elapsed_hours, event_id, fatal_biological, floor, group_clearance, intervention_requested, life_before, override, override_condition, personal_clearance, planned_injury_cause, protection_actual_proof, reincarnation_delay_local_seconds, soul_material, status, void_anchor_year`.
- Elemen `planned_rebirths`: `actual_proof, after_death, chapter, delay_void_local_seconds, event_id, status`.
- Elemen `planned_job_grants`: `actual_proof, chapter, grant, job, life`.
- Semua `actual_proof` = `null`, status `PLANNED_NOT_COMMITTED`.

## 6. Ledger (`S1_Master_State_Ledger_R3.jsonl`): 1500 baris, **lima bentuk top-level**

| Rentang bab | Jumlah | Kunci top-level |
| :-- | :-- | :-- |
| 1 sampai 60 | 60 | `chapter_id, continuity_gate, ending_hook, event_proof, evidence_scene, phase, post_state, pre_state, reader_clue, release_meta, source_legacy, stage, status` |
| 61 sampai 140 | 80 | bentuk 1 **ditambah** `conditional_dependency, new_floor_records` |
| 141 sampai 700 | 560 | bentuk 2 **ditambah** `state_delta` |
| 701 sampai 965 | 265 | `actual_proof, chapter_id, event_proof, gate, phase, post_state, pre_state, release_meta, source_legacy, stage, state_delta, status` (tidak ada `continuity_gate`, `ending_hook`, `evidence_scene`, `reader_clue`; ada `gate`, `actual_proof`) |
| 966 sampai 1500 | 535 | `actual_proof, chapter_id, event, phase, post_state, pre_state, release_meta, source_legacy, stage, status` (kolom `event` menggantikan `event_proof`) |

Implikasi parser: **jangan** mengasumsikan kunci tertentu ada; semua akses harus toleran dan mencatat bentuk yang terdeteksi. Fixture wajib mencakup minimal dua bentuk berbeda.

`release_meta` (ada di semua 1500): `actual_proof, actual_state_commit, approval, arc, chapter, effective_earth_job, historical_fields_are_planned, patch_dependencies, source_ledger_sha256`; opsional: `L250_dependency` (1396 bab), `L1500_dependency` (1156), `L4500_dependency` (786), `approved_floor_plan` (1), `approved_patch_fields` (1).

`pre_state` dan `post_state`: jumlah kunci membesar seiring bab (40 kunci pada bentuk 1, 273 kunci gabungan pada seluruh ledger). Kunci inti stabil (skalar/daftar kecil): `body, civic, cleared_kael, consumed, contract_devon, creation, creation_capacity, cycle, deaths, dormant, earth_body, energy_kael, evidence, evidence_history, event_history, faction, floor_current, fusion, fusion_capacity, hold, injury_kael, inventory_kael, kael_awareness, kael_location, knowledge_history, knowledge_kael, knowledge_public, known_resources, language, life, npc, npc_locations, open_threads, party, primary_job, realm, secondary_job, system, time, trust`. Kunci lain bertambah per Arc dengan awalan `arcNN_` (mis. `arc06_*` sampai `arc18_*`), `cluster*_status`, `floor_1500_*`, `floor_4500_*`, `death_event`, `death_history`, `rebirth_event`, `job_grant`, `age_window_life0N_years`, dan lain-lain.

**Kontrak proyeksi ringkas** (tidak boleh hardcode daftar 273 kunci): perlakukan kunci secara generik berdasarkan **ukuran dan tipe**, bukan nama. Skalar dan struktur kecil disimpan utuh; struktur besar menjadi `{count, last_N, sha256}`; setiap kunci berakhiran `_history` hanya `{count}`. Daftar kunci inti di atas boleh dijadikan "kunci yang selalu ditampilkan di Brief bila ada", bukan syarat keberadaan.

Ukuran terukur: baris B1 sekitar 3,8 KB; baris bab-bab akhir hingga sekitar 1,3 MB. Kunci kumulatif terbesar (urut): `event_history`, `cleared_kael`, `knowledge_history`, `evidence_history`, `axis_transactions_planned`, `arc13_actor_presence`, `knowledge_kael`, `evidence`. Menghapus hanya `*_history` masih menyisakan sekitar 260 MB di seluruh ledger, jadi batas ukuran per kunci pada proyeksi wajib agresif.

Offset dan panjang ledger pada indeks adalah byte UTF-8 per baris termasuk pemisah baris. Pada pembacaan streaming, hitung byte baris secara kumulatif dan cocokkan dengan `ledger_offset_bytes` dan `ledger_length_bytes` setiap bab.

## 7. Ringkasan jumlah untuk uji konsistensi

1500 bab; 1500 heading `### BAB`; 18 Arc; 28 Stage; 108 fase; 10.000 baris floor plan; 5 baris carryover; 57 blocker; 30 skenario uji; 178 baris state changes; 4 death + 4 rebirth + 4 job grant; 128 entri manifest; 23 path allowlist; `actual_commits` = 0.

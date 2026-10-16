# Dataset — QLoRA training data for `Qwen/Qwen3.5-0.8B-Base`

Matlamat: setiap domain dibina satu demi satu menjadi `*_expanded.jsonl` — bilingual (Melayu+English),
boleh-tulis + boleh-jalankan program melalui tools (`TOOL_CALL`/`OBSERVATION`), kualiti "perfect" (jelas,
lengkap, ralat + solusi). Latihan: QLoRA di Colab/Kaggle GPU. Base asal tidak disentuh.

## Standard setiap fail expanded

- Gold-rewrite: output nipis/duplikat ditulis semula (bilingual + contoh runnable + jangkaan + ralat biasa + solusi).
- Penanda `Melayu:` / `English:`; fence ` ```bash|python `; `TOOL_CALL: {...}` mesti JSON-parseable.
- Parafrasa sahaja (w3schools © Refsnes, ss64 © 1999–2026, GNU FDL) — sumber asal tidak disalin bulat-bulat.
- Split `train/val` 95/5 (seed 42) + `*.report.json` (QC + nota). Semua intermediates di `_expanded/`.

## Siap (4/16) — 17,637 rows (deduped, tiada duplikat silang-domain)

| Domain | Expanded | Train / Val | QC |
|---|---|---|---|
| bash | `bash_expanded.jsonl` 5,987 | 5,688 / 299 | invalid 0, tool 133, err+sol 118 |
| python | `python_expanded.jsonl` 5,432 | 5,161 / 271 | invalid 0, tool 29 (JSON OK), fence 1,628 (`py_compile` 1,524/1,524), err+sol 1,604, runtime 107/108 (1 perlu rangkaian) |
| linux | `linux_expanded.jsonl` 1,403 | 1,333 / 70 | invalid 0, tool 10 (JSON OK), fence 1,363, err+sol 1,359, bilingual 1,403/1,403 |
| sql | `sql_expanded.jsonl` 4,815 | 4,574 / 241 | invalid 0, exact-dupe 118 dibuang, bilingual 4,815/4,815, fence 4,765, tool 50 (JSON OK, sqlite OBS real), err+sol 4,815, sqlite 4,815/4,815 |

Sumber tambahan: w3schools bash (~52 pg), ss64 bash/mac/nt/ps/vb, GNU bash manual, docs.python.org tutorial+library, w3schools python, geeksforgeeks sql-tutorial (checklist topik sahaja, parafrasa — tiada salin verbatim).

## Belum buat (raw, jumlah baris)

- `javascript.jsonl` 4,573 · `docker.jsonl` 1,582 · `nodejs.jsonl` 1,348 · `php.jsonl` 1,302 · `general.jsonl` 1,284 · `html.jsonl` 1,223 · `css.jsonl` 1,222 · `mysql.jsonl` 1,222 · `laravel.jsonl` 1,212 · `tailwind.jsonl` 1,118 · `postgresql.jsonl` 1,120 · `typescript.jsonl` 1,125 (`sql.jsonl` siap sebagai `sql_expanded.jsonl`; 9 baris off-topik dipindah ke `_expanded/sql_dropped_offtopic.jsonl`)

## Wajib sebelum latihan (dicatat dalam report)

1. `bash -n` + `shellcheck` ke atas semua fence bash — belum jalan (hos Windows: tiada WSL distro, `bash.exe` stub hang).
2. Live-run `OBSERVATION` tool rows (bash 133, python 29, linux 10) di hos Linux/Colab — sebahagian dilabel bentuk-jangkaan.
3. Domain ke-5 (cadangan: `postgresql.jsonl` 1,120 atau `mysql.jsonl` 1,222 — kecil, tulen, kunci pola sqlite sebelum domain besar `javascript`).

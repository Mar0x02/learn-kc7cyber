# Notes - Section 4: Connecting the Dots

## Schema tables
- `AuthenticationEvents` (8 kolom)
- `Email` (7 kolom)
- `Employees` (9 kolom)
- `FileCreationEvents` (7 kolom)
- `InboundNetworkEvents` (5 kolom)
- `OutboundNetworkEvents` (5 kolom)
- `PassiveDns` (3 kolom)
- `ProcessEvents` (8 kolom)
- `SecurityAlerts` (5 kolom)

## Fakta penting dari narasi
- Alamat email `valdorias_best_recruiter@gmail.com` ketahuan ngirim email ke Ronnie (Editorial Intern) dan beberapa karyawan lain — kemungkinan besar pola phishing mass-mailing yang sama kayak insiden Nene Leaks (Section 1) & Sonia Gose (Section 3).
- Total 18 email terkirim dari `valdorias_best_recruiter@gmail.com` ke banyak recipient berbeda (peter_parket, kathleen_hopper, nene_leaks, mark_zuckerberg, larry_page, dan_rather, carl_bernstein, barbara_walters, bob_woodward, seymour_hersh, ronnie_mclovin, bill_gates, tyler_byers, tom_brokaw, dll) dalam beberapa gelombang (2024-01-03, 01-05, 01-09, 01-10) — konfirmasi ini mass-phishing campaign, bukan serangan tertarget 1 orang.
- ⚠️ Domain link phishing ke Ronnie: `promotionrecruit.org` — beda dari domain yang diklik Sonia di Section 3 (`promotionrecruit.com`). Attacker kemungkinan daftar beberapa domain mirip (typosquatting pattern) buat kampanye yang sama.
- Hostname Ronnie: `A37A-DESKTOP`, username `romclovin`.
- ⚠️ Di `FileCreationEvents` mesin Ronnie, ada baris tanggal 2024-01-13 dengan username `editinte_local_admin` — beda dari `romclovin`, kemungkinan akun local admin baru yang dibuat attacker buat persistence/privilege escalation. Perlu ditelusuri lebih lanjut.
- Username (`$had0w`) & password (`thruthW!llS3tUfree`) plink SAMA PERSIS di semua korban (Nene, Sonia, Ronnie) — cuma IP C2 yang beda-beda tiap korban (193.224.154.119 / 136.130.190.181 / 168.57.191.100). Satu aktor/toolkit, infrastruktur di-rotate.
- 🔥 **Clue penting:** di mesin Ronnie juga ketemu download `fakestory.docx` dari `https://hire-recruit.org/files/fakescandal/2024/fakestory.docx` — path-nya literally "fakescandal", kemungkinan besar ini DRAFT ARTIKEL PALSU yang akhirnya dipublish. Domain `hire-recruit.org` juga mirip `hirerecruit.org` yang dipake di Section 1 (serangan ke Nene Leaks).
- 🔥🔥 **Titik temu terbesar sejauh ini:** `fakestory.docx` dibuat di mesin Ronnie tanggal **2024-01-31 09:47:51** — hari yang SAMA dengan email "URGENT: Final OpEd Draft Edits..." dari akun Ronnie ke Clark Kent (Section 2 Q5). Attacker yang udah kuasain mesin Ronnie sejak 2024-01-10 (RCE via plink) kemungkinan pakai akses itu buat nulis & ngirim draft artikel palsu ke Clark Kent.
- ✅ **Confirmed:** `fakestory.docx` di-rename jadi `C:\Users\romclovin\Documents\OpEdFinal_to_print.docx` — nama file ini PERSIS SAMA dengan attachment di email ke Clark Kent (Section 2). Rantai buktinya lengkap: attacker RCE ke mesin Ronnie → download `fakestory.docx` → rename jadi `OpEdFinal_to_print.docx` → kirim dari akun Ronnie ke Clark Kent → Clark Kent print → artikel palsu terbit.
- Family domain phishing yang dipake attacker (varian typosquatting): `hirerecruit.org` (Section 1), `promotionrecruit.com` (Section 3/Sonia), `promotionrecruit.org` (Section 4/Ronnie email awal), `hire-recruit.org` (Section 4/fakestory.docx), `hirerecruit.com` (dark web alert Ronnie), `hirejob.com` (exfil upload portal).
- Data yang dicuri dari mesin Ronnie: `DankMemes.7z`, `MyStolenDataFromDocuments.7z`, `MyStolenDataFromDesktop.7z` — semuanya di-encrypt pake password sama persis kayak plink (`thruthW!llS3tUfree`), terus di-upload via `curl` ke `https://hirejob.com/exfil_processor/upload.php`.

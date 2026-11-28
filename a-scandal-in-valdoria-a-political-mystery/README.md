# A Scandal in Valdoria: A Political Mystery

## Tentang Game Ini
Sebuah koran ternama, The Valdorian Times 🗞️, menerbitkan artikel tanpa izin yang menuduh kandidat politik terkemuka melakukan korupsi! Tapi apakah ini hasil hack, orang dalam, atau sesuatu yang lain?

Investigasi dilakukan dengan analisis log dan jejak digital. Di cybersecurity, suspect yang paling obvious belum tentu pelakunya — harus ngikutin evidence dan test competing hypotheses, karena di investigasi real gak bakal tau siapa pelakunya sampai semua data udah diperiksa.

## Kenapa Module Ini Penting
Kasus full kill-chain pertama. IR harus bisa reconstruct exact sequence of events — module ini melatih timeline reconstruction lintas email, endpoint, dan network logs, dengan C2 dan exfiltration jadi scope-nya.

Featured in: Incident Responder, Threat Hunter, Security Analyst I

## Tujuan Pembelajaran
- Menguasai syntax dasar KQL, termasuk `where`, `take`, dan `distinct`
- Filter data secara efektif buat fokus ke informasi relevan
- Terapkan filter berbasis waktu buat investigasi event di sekitar suspicious indicator
- Correlate event buat construct comprehensive attack timeline
- Interpretasi script PowerShell dan command berbahaya yang dijalankan attacker

## Topik
`data-exfiltration` `phishing` `influence-operations`

## Daftar Section
- [x] [Section 1: Welcome to Valdoria](./section-1-welcome-to-valdoria/README.md) — 12 question, done
- [x] [Section 2: The Investigation Begins](./section-2-the-investigation-begins/README.md) — 8 question, done
- [x] [Section 3: A Tip From Sonia](./section-3-a-tip-from-sonia/README.md) — 26 question, done
- [x] [Section 4: Connecting the Dots](./section-4-connecting-the-dots/README.md) — 34 question, done

## Summary: Gimana Hacker-nya Nyerang (End-to-End)
1. **Initial access (mass-phishing):** Attacker ngirim puluhan email phishing ke karyawan The Valdorian Times lewat beberapa domain "hire/recruit" palsu (`hirerecruit.org/.com`, `promotionrecruit.org/.com`, `hire-recruit.org`) — nyamar jadi lowongan kerja/recruiter. Minimal 3 karyawan (Nene Leaks, Sonia Gose, Ronnie McLovin) klik link-nya.
2. **Execution & persistence (identik di semua korban):** Link phishing nge-drop file `.docx` yang bikin file PowerShell `C:\ProgramData\hacktivist_manifesto.ps1` muncul di disk → persistence lewat `schtasks` (run tiap 5 jam) → script dijalanin.
3. **C2 / RCE:** Script-nya ngejalanin `plink.exe` buat bikin reverse SSH tunnel, pake kredensial yang **sama persis di semua korban** (`$had0w` / `thruthW!llS3tUfree`), cuma IP C2-nya yang beda-beda tiap korban — kemungkinan buat ngaburin korelasi antar insiden.
4. **Discovery:** Di tiap mesin yang kekompromi, attacker ngejalanin 5 command discovery yang sama (`whoami`, `ipconfig`, `arp -a`, `tasklist /svc`, `net view`) buat ngerti lingkungan korban.
5. **Dead end vs. target utama:** Nene & Sonia cuma sampe tahap discovery. **Ronnie McLovin** ternyata jadi target utama — mesinnya dipakai attacker buat:
   - Download `fakestory.docx`, rename jadi `OpEdFinal_to_print.docx`, lalu kirim dari akun Ronnie ke Clark Kent (Newspaper Printer) tanggal 2024-01-31, nyamar jadi draft OpEd asli yang emang ditunggu Clark. **Inilah cara artikel fitnah soal Luffy bisa kepublish** — bukan hack ke sistem percetakan, tapi social-engineered insider document delivery lewat akun yang udah di-takeover.
   - Exfiltrate data pribadi Ronnie (Documents, Desktop, bahkan folder Meme-nya 😂) jadi 3 file `.7z` terenkripsi, di-upload lewat `curl` ke `hirejob.com/exfil_processor/upload.php`.
6. **Cliffhanger:** Investigasi ditutup dengan email misterius dari sosok yang nyebut diri "ShadowTruth" — kemungkinan identitas di balik username `$had0w` yang konsisten muncul di semua RCE, jadi benang merah buat module KC7 selanjutnya.

**Pelajaran utama:** Ini bukan kasus single-target hack — ini **mass-phishing campaign** di mana attacker nyebar jaring ke banyak karyawan, tapi cuma ngelakuin "hands-on-keyboard" lanjutan (publish artikel palsu + exfiltration) di 1 korban yang paling strategis (Ronnie, yang emailnya dipercaya Clark Kent). Pola yang konsisten (nama file `hacktivist_manifesto.ps1`, kredensial plink, command discovery) jadi kunci buat connecting the dots antar korban yang kelihatannya gak berhubungan.

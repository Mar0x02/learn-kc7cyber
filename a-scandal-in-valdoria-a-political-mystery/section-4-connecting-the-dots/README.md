# Section 4: Connecting the Dots

- Tanggal mulai: 2026-11-28
- Role: Cyber Incident Responder
- Status: done (34/34 question)

## Narasi
Terapkan apa yang udah dipelajari dari investigasi insiden Sonia buat nemuin korban lain dari insiden ini. Muncul alamat email mencurigakan lain, `valdorias_best_recruiter@gmail.com`, yang ketahuan ngirim email ke Editorial Intern Ronnie dan beberapa orang lain.

## Tables yang dipakai
Lihat [notes.md](./notes.md)

## Summary: Gimana Hacker-nya Nyerang
1. Attacker ngejalanin mass-phishing campaign ke banyak karyawan The Valdorian Times pake 2+ domain "hire" (`hirerecruit.org/.com`, `promotionrecruit.org/.com`, `hire-recruit.org`) — 18 email terkirim total, dan dari bukti outbound traffic, setidaknya 3 karyawan (Nene Leaks, Sonia Gose, Ronnie McLovin) terbukti klik link-nya.
2. Pola initial access-nya **identik di semua korban**: klik link phishing → download `.docx` (kemungkinan macro-enabled) → `.docx` itu drop file `C:\ProgramData\hacktivist_manifesto.ps1` → persistence via `schtasks` (jalan tiap 5 jam) → script dijalanin → RCE lewat `plink.exe` reverse SSH tunnel (kredensial sama persis di semua korban: `$had0w` / `thruthW!llS3tUfree`, cuma IP C2 yang beda-beda) → 5 discovery command (`whoami`, `ipconfig`, `arp -a`, `tasklist /svc`, `net view`).
3. Nene & Sonia "mentok" di tahap discovery doang — gak ada tindak lanjut yang ketemu di mesin mereka.
4. **Ronnie beda** — mesinnya dipakai buat 2 aksi tambahan:
   - Attacker download `fakestory.docx` dari `hire-recruit.org`, rename jadi `OpEdFinal_to_print.docx`, lalu kirim dari akun Ronnie ke Clark Kent (Newspaper Printer) tanggal 2024-01-31, nyamar jadi draft OpEd asli. Clark Kent print artikel itu tanpa curiga → **inilah cara artikel palsu soal Luffy bisa kepublish**.
   - Attacker exfiltrate data dari mesin Ronnie: compress 3 folder (`Documents`, `Desktop`, `Memes`) jadi 3 file `.7z` terenkripsi (password sama kayak plink), lalu upload via `curl` ke portal custom `hirejob.com/exfil_processor/upload.php`. Dicek ke semua device lain, cuma mesin Ronnie yang exfiltrate ke domain ini.
5. Di penghujung investigasi, muncul email misterius dari sosok "ShadowTruth" — kemungkinan identitas attacker (cocok sama username `$had0w` yang dipake di semua RCE) — jadi cliffhanger buat module selanjutnya.

**Pelajaran utama:** Satu mass-phishing campaign bisa punya banyak korban dengan treatment beda-beda — kebanyakan cuma "numpang lewat" (discovery doang), tapi 1-2 korban yang akunnya paling berguna (akses ke editor/printer) dipakai buat eksekusi tujuan akhir attacker (publish artikel palsu), sementara korban lain juga tetap kena data theft kalau ada kesempatan (meme Ronnie ikut kecuri 😂). Jangan berhenti investigasi di 1 korban "dead end" — pola yang konsisten (nama file, kredensial, timing) adalah kunci buat connecting the dots antar korban.

## Query yang dipakai
```kql
Email
| where sender contains "valdorias_best_recruiter@gmail.com"

Email
| where sender contains "valdorias_best_recruiter@gmail.com" and recipient == "ronnie_mclovin@valdoriantimes.news"

Employees
| where name contains "ronnie"

OutboundNetworkEvents
| where src_ip contains "10.10.0.19"
| where url contains "promotionrecruit.org"

FileCreationEvents
| where hostname == "A37A-DESKTOP"

ProcessEvents
| where hostname == "A37A-DESKTOP"
| where timestamp >= todatetime("2024-01-10 08:55:51+00:00")

FileCreationEvents
| where hostname == "A37A-DESKTOP"
| where filename contains ".docx"

OutboundNetworkEvents
| where src_ip contains "10.10.0.19"
| where url contains ".docx"

ProcessEvents
| where hostname == "A37A-DESKTOP"
| where timestamp >= todatetime("2024-01-31 09:47:51+00:00")

ProcessEvents
| where hostname == "A37A-DESKTOP"

Email
| where sender contains "ronnie_mclovin@valdoriantimes.news"
| where recipient has "Clark"

ProcessEvents
| where timestamp between (datetime(2024-01-21 07:00:00) .. datetime(2024-01-21 12:00:00))
| where hostname == "A37A-DESKTOP"
| order by timestamp asc

ProcessEvents
| where timestamp >= datetime(2024-01-31 11:44:58+00:00)
| where hostname == "A37A-DESKTOP"
| order by timestamp asc

ProcessEvents
| where process_commandline contains "curl"
```

## Daftar question
- [x] [Q1](./q1.md)
- [x] [Q2](./q2.md)
- [x] [Q3](./q3.md)
- [x] [Q4](./q4.md)
- [x] [Q5](./q5.md)
- [x] [Q6](./q6.md)
- [x] [Q7](./q7.md)
- [x] [Q8](./q8.md)
- [x] [Q9](./q9.md)
- [x] [Q10](./q10.md)
- [x] [Q11](./q11.md)
- [x] [Q12](./q12.md)
- [x] [Q13](./q13.md)
- [x] [Q14](./q14.md)
- [x] [Q15](./q15.md)
- [x] [Q16](./q16.md)
- [x] [Q17](./q17.md)
- [x] [Q18](./q18.md)
- [x] [Q19](./q19.md)
- [x] [Q20](./q20.md)
- [x] [Q21](./q21.md)
- [x] [Q22](./q22.md)
- [x] [Q23](./q23.md)
- [x] [Q24](./q24.md)
- [x] [Q25](./q25.md)
- [x] [Q26](./q26.md)
- [x] [Q27](./q27.md)
- [x] [Q28](./q28.md)
- [x] [Q29](./q29.md)
- [x] [Q30](./q30.md)
- [x] [Q31](./q31.md)
- [x] [Q32](./q32.md)
- [x] [Q33](./q33.md)
- [x] [Q34](./q34.md)

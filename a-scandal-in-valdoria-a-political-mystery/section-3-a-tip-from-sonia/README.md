# Section 3: A Tip From Sonia

- Tanggal mulai: 2026-11-27
- Role: Cyber Incident Responder
- Status: done (26/26 question)

## Narasi
Mampir ke kantor The Valdorian Times dan ketemu beberapa staff. Setelah meeting, salah satu karyawan, Sonia Gose, dateng dan bilang dia mungkin punya sesuatu yang bisa bantu investigasi.

## Tables yang dipakai
Lihat [notes.md](./notes.md)

## Summary: Gimana Hacker-nya Nyerang
1. Sonia Gose (Senior Editor) ngasih tau dia pernah terima email mencurigakan dari `newspaper_jobs@gmail.com`, isinya link ke `promotionrecruit.com/.../Valdorian_Times_Editorial_Offer_Letter.docx`.
2. Sonia beneran klik link itu (2024-01-05 10:23:17) — 47 detik kemudian file docx-nya kedownload ke `C:\Users\sogose\Downloads\`.
3. 28 detik setelah itu, file `C:\ProgramData\hacktivist_manifesto.ps1` ditulis ke disk — **nama & path-nya identik** sama yang kejadian di mesin Nene Leaks (Section 1).
4. ~1 jam kemudian, attacker bikin persistence lewat `schtasks` (jalan tiap 5 jam), lalu beneran ngejalanin script PowerShell-nya.
5. Script-nya ngejalanin `plink.exe` buat bikin reverse SSH tunnel ke IP eksternal `136.130.190.181` (username `$had0w`).
6. Setelah tunnel-nya kebentuk, attacker ngelakuin 5 discovery command manual (hands-on-keyboard): `whoami`, `ipconfig`, `arp -a`, `tasklist /svc`, `net view`.
7. Triase sisa log di mesin Sonia gak nemu aktivitas berbahaya lain — kesimpulannya attacker kemungkinan bukan nargetin Sonia secara spesifik, cuma numpang lewat buat discovery doang.

⚠️ **Temuan penting:** Di Section 1 kita udah tau ada ~9 IP karyawan berbeda yang sempat ngakses domain phishing yang sama (`hirerecruit.org`) — artinya serangan ini **gak cuma nargetin 1 orang**, tapi mass-phishing ke banyak karyawan sekaligus. Pola serangannya identik di tiap korban (download docx → `hacktivist_manifesto.ps1` → schtasks persistence → plink RCE), tapi **IP C2/attacker beda-beda per korban** (Section 1: `193.224.154.119`, Section 3/Sonia: `136.130.190.181`) — kemungkinan attacker pakai beberapa IP/proxy berbeda buat tiap target biar gak gampang dikorelasiin satu sama lain.

**Pelajaran utama:** Satu korban yang "dead end" (gak ada hasil lanjutan) bukan berarti investigasi selesai — kalau pola serangannya mass-phishing, korban lain yang kena infeksi yang sama bisa jadi punya clue yang beda, dan attacker bisa sengaja pakai infrastruktur berbeda-beda per korban buat ngaburin korelasi.

## Query yang dipakai
```kql
Employees
| where name contains "sonia"

Email
| where recipient contains "sonia_gose@valdoriantimes.news"
| where sender contains "newspaper_jobs"

OutboundNetworkEvents
| where src_ip == "10.10.0.3"
| where url contains "promotionrecruit"

FileCreationEvents
| where hostname == "UL0M-MACHINE"

ProcessEvents
| where hostname == "UL0M-MACHINE"
| where timestamp >= todatetime("2024-01-05 10:24:32+00:00")
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

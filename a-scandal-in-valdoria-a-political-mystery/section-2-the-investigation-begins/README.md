# Section 2: The Investigation Begins

- Tanggal mulai: 2026-11-27
- Role: Cyber Incident Responder
- Status: done (8/8 question)

## Narasi
Investigasi resmi dimulai. Langkah pertama, kita hubungi Nene Leaks (Editorial Director) buat minta info lebih lanjut soal insiden artikel palsu yang kepublish.

## Summary: Gimana Hacker-nya Nyerang
1. Nene Leaks (Editorial Director) ngarahin kita ke Clark Kent, Newspaper Printer.
2. Clark Kent cuma nge-print artikel yang dikirim ke emailnya — dia bilang itu final draft dari Ronnie McLovin, Editorial Intern yang baru di-hire 2024-01-02 (sehari sebelum insiden phishing di Section 1).
3. Pas ditanya, Ronnie ngaku dia gak pernah ngirim final draft itu — dia ketiduran dan draft aslinya gak jadi dikirim.
4. Ditelusuri, ternyata beneran ada email "URGENT: Final OpEd Draft Edits..." yang terkirim ke Clark Kent tanggal 2024-01-31, dari akun asli `ronnie_mclovin@valdoriantimes.news` (verdict CLEAN, bukan spoof), isinya link ke `OpEdFinal_to_print.docx` di sharepoint internal.
5. Karena Ronnie ngotot gak pernah ngirim, dan dia juga gak sadar ada aktivitas aneh di akunnya, kesimpulan sementara: akun emailnya kemungkinan di-takeover attacker buat ngirim draft palsu yang isinya tuduhan korupsi ke Luffy — tanpa sepengetahuan Ronnie sendiri.

**Pelajaran utama:** Insider threat gak selalu berarti karyawan yang jahat — bisa juga karyawan yang gak bersalah tapi akunnya dikuasai orang lain (account takeover). Investigasi harus tetap jalan walau pelaku "resmi"-nya udah ketemu, karena korelasi akun ≠ korelasi pelaku sebenarnya.

## Tables yang dipakai
Lihat [notes.md](./notes.md)

## Query yang dipakai
```kql
Employees
| where name contains "clark kent"

Employees
| where role contains "editorial"

Email
| where recipient contains "clark_kent@valdoriantimes.news"

Email
| where timestamp between (todatetime("2024-01-31 00:00:00+00:00") .. todatetime("2024-02-01 00:00:00+00:00"))
| where recipient contains "clark_kent@valdoriantimes.news"
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

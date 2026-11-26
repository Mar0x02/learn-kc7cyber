# Section 2: Less Beef, More Phish

- Tanggal mulai: 2026-11-26
- Role: Security Analyst, OWL Records
- Status: done (23/23 question)

## Narasi
Setelah insiden Section 1, OWL Records deliberasi private soal kejadian itu (Dwake ikut, masih kesel). Setelah 6 jam, perusahaan declare "settled issue" tapi gak pernah spesifik gimana cara nanganinnya. Besoknya, hacker random di dark web ngancem bakal release info berbahaya soal Present (si rapper lawan Dwake) kalau dia gak umumin pensiun dalam 30 hari.

Alur investigasinya:
1. Gak ada bukti management OWL Records terlibat post dark web itu. Homie kasih hint samar: retaliation kemungkinan lewat phishing ([Q1](./q1.md)-[Q2](./q2.md)).
2. IP adversary yang sama dari Section 1 (`18.66.52.227`) dicek di `PassiveDns`, ketemu domain `betterlyrics4u.com` ([Q3](./q3.md)).
3. Cek table `Email`, filter kolom `link` yang mengandung domain itu — ketemu 13 email phishing ([Q4](./q4.md)-[Q5](./q5.md)), dikirim dari dua address: `ghostwritersanonymous@protonmail.com` dan `wemakebeatz@gmail.com` ([Q6](./q6.md)-[Q7](./q7.md)).
4. Cross-reference recipient email ke table `Employees` nunjukin phishing ini ngincer role `Rapper` dan `Lead Rapper` — Lead Rapper-nya adalah Dwake Audrey, dengan IP `10.10.0.5` ([Q8](./q8.md)-[Q11](./q11.md)).
5. Email yang dikirim ke Dwake subjectnya "[EXTERNAL] RE: Need a ghostwriter for your next hit?", link-nya `http://betterlyrics4u.com/share/online/published/enter`, dan lolos dari email security (verdict `CLEAN`) ([Q12](./q12.md)-[Q14](./q14.md)). Isi emailnya nyamar jadi tawaran ghostwriter, signed "GhostWriter" ([Q15](./q15.md)).
6. Dwake klik link itu (`OutboundNetworkEvents`, 2024-04-15 12:03:12+00:00) dan masuk ke fake login page ("Login to speak with GHOST WRITER") — kemungkinan besar masukin credentials-nya di situ ([Q16](./q16.md)-[Q17](./q17.md)).
7. Adversary pake credentials itu buat login ke akun Dwake (`AuthenticationEvents`), berhasil 1 jam kemudian: 2024-04-15 13:03:12+00:00 ([Q18](./q18.md)-[Q19](./q19.md)).
8. Setelah masuk, adversary browsing akun Dwake (`InboundNetworkEvents`, 10 hits) nyari info sensitif, dan akhirnya nyuri/export data lewat file `DwakesDirtySecrets.zip` ([Q20](./q20.md)-[Q21](./q21.md)).
9. Dwake & Present sama-sama punya dirt sekarang — akhirnya gencatan senjata ([Q22](./q22.md)-[Q23](./q23.md)).

**Pelajaran:** phishing yang kelihatan meyakinkan + fake login page bisa lolos email security dan nipu user buat kasih credentials sendiri — social engineering tetep jadi jalur masuk paling efektif.

## Tables yang dipakai
Lihat [notes.md](./notes.md)

## Query yang dipakai
Cek domain yang diasosiasikan IP adversary ([Q3](./q3.md)):
```kql
PassiveDns
| where ip == "18.66.52.227"
```

Kenalan sama table `Email` ([Q4](./q4.md)):
```kql
Email
| take 10
```

Cari email phishing yang mengandung domain itu ([Q5](./q5.md)-[Q7](./q7.md)):
```kql
Email
| where link has "betterlyrics4u.com"
```

Lihat sender-nya aja ([Q6](./q6.md)-[Q7](./q7.md)):
```kql
Email
| where link has "betterlyrics4u.com"
| project sender
```

Cross-reference recipient ke data employee buat liat role yang ditarget ([Q8](./q8.md)-[Q11](./q11.md)):
```kql
let _targets = Email
| where link has "betterlyrics4u.com"
| distinct recipient;
Employees
| where email_addr in (_targets)
```

Filter ke email yang dikirim spesifik ke Dwake ([Q12](./q12.md)-[Q15](./q15.md)):
```kql
Email
| where link has "betterlyrics4u.com"
| where recipient has "dwake_audrey"
```

Cek apakah Dwake klik link phishing-nya ([Q16](./q16.md)):
```kql
OutboundNetworkEvents
| where url == "http://betterlyrics4u.com/share/online/published/enter"
| where src_ip == "10.10.0.5"
```

Liat semua data Employees ([Q9](./q9.md)-[Q11](./q11.md)):
```kql
Employees
```

Cek login attempt adversary ke akun Dwake ([Q18](./q18.md)-[Q19](./q19.md)):
```kql
AuthenticationEvents
| where username == "dwaudrey"
| where src_ip == "18.66.52.227"
```

Cek aktivitas adversary di dalam akun Dwake ([Q20](./q20.md)-[Q21](./q21.md)):
```kql
InboundNetworkEvents
| where src_ip == "18.66.52.227"
| where url has "dwaudrey"
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

# Section 2: Inside the Clout Breach 🐾

- Tanggal mulai: 2026-11-27
- Role: Security Analyst, CloutHaus
- Status: done (13/13 question)

## Narasi
Lihat overview investigasi di [README utama](../README.md).

1 jam setelah Afomiya klik link phishing di Section 1, ada login mencurigakan ke email kerjanya — device beda, negara beda, pakai Internet Explorer versi lama. Mission: pake log Authentication, Network, Email buat lacak apa yang dilakuin intruder, apa yang dia mau, dan apa yang mungkin ilang.

Alur investigasinya:
1. Dari log authentication ketemu IP yang dipake buat login sukses ke akun Afomiya, `182.45.67.89` ([Q1](./q1.md)).
2. Pivot IP itu ke `PassiveDns`, ketemu domain `influencer-deals.net` — satu infrastruktur attacker yang sama ([Q2](./q2.md)).
3. User-Agent login-nya pake browser ancient, `Windows NT 5.2` — tanda attacker nyamarin device/browser ([Q3](./q3.md)).
4. Geolocation IP nunjukin login berasal dari Jinan, China — jauh banget dari aktivitas normal Afomiya ([Q4](./q4.md)).
5. Dari `InboundNetworkEvents` filter IP yang sama, ketemu puluhan "search" recon ke website CloutHaus: nyari **lokasi** Afomiya lewat Instagram story, **home address**, transaction history **Venmo**, sampe rencana fake **birthday party** buat lure — semuanya OSINT buat profiling & social engineering lanjutan ([Q5](./q5.md)-[Q8](./q8.md)).
6. Setelah akses email, attacker masang auto-forward rule yang ngirim email keluar ke `noreply@influencer-deals.net` (domain yang sama dari Q2) — exfiltration ([Q9](./q9.md)).
7. Email yang di-forward isinya dokumen sensitif: direct deposit form (bank info), passport scan, dan tax document year-end summary — bahan lengkap buat identity theft & financial fraud ([Q10](./q10.md)-[Q12](./q12.md)).
8. Kesimpulan: tujuan attacker bukan cuma satu hal, tapi gabungan — identity theft, financial fraud, espionage, dan long-term access ([Q13](./q13.md)).

**Pelajaran:** sekali attacker dapet akses ke email (dari Section 1), dia bisa pivot ke OSINT lanjutan + auto-forward rule buat exfiltrate data sensitif secara stealthy tanpa perlu login berulang — makanya penting cek forwarding rule & login anomaly (device/geo/user-agent) begitu curiga ada compromise.

## Tables yang dipakai
Lihat [notes.md](./notes.md)

## Query yang dipakai
Cari data Afomiya & cek login sukses ([Q1](./q1.md)):
```kql
Employees
| where username == "afstorm"

AuthenticationEvents
| where username == "afstorm"
| where result contains "Successful"
```

Cari domain terkait IP mencurigakan ([Q2](./q2.md)):
```kql
PassiveDns
| where ip contains "182.45.67.89"
| distinct domain
```

Cari aktivitas recon/search dari IP yang sama ([Q3](./q3.md)-[Q8](./q8.md)):
```kql
InboundNetworkEvents
| where src_ip == "182.45.67.89"
```

Cari recipient eksternal yang paling sering nerima forward dari akun Afomiya ([Q9](./q9.md)):
```kql
let _email = Employees
| distinct email_addr;
Email
| where recipient !in (_email)
| where sender contains "afomiya_storm@clouthaus.com"
| summarize count() by recipient
| order by count_ desc
```

Lihat semua subject yang di-forward ke address attacker ([Q10](./q10.md)):
```kql
Email
| where recipient == "noreply@influencer-deals.net"
| project subject
```

Cari subject spesifik soal passport ([Q11](./q11.md)):
```kql
Email
| where recipient == "noreply@influencer-deals.net"
| where subject contains "passport"
```

Cari subject spesifik soal tax document ([Q12](./q12.md)):
```kql
Email
| where recipient == "noreply@influencer-deals.net"
| where subject contains "tax"
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

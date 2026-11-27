# Section 1: Got Clout? 🤳

- Tanggal mulai: 2026-11-27
- Role: Security Analyst, CloutHaus
- Status: done (21/21 question)

## Narasi
Lihat overview investigasi di [README utama](../README.md).

Alur ceritanya:
1. Afomiya Storm, influencer fashion & lifestyle, pake email pribadi di bio Instagram buat brand deal — meski kolom email di `Employees` nunjukin email kerja CloutHaus-nya, bio Instagram-nya sendiri nyebut email pribadi ([Q1](./q1.md)).
2. Afomiya bikin Q&A session di Instagram Stories, gak sadar beberapa pertanyaan follower sebenernya security question buat akun email/social media-nya — attacker pake **social engineering** & **OSINT** (info publik dari Q&A/post) buat ngumpulin jawaban (termasuk "Lalibela") ([Q2](./q2.md)-[Q4](./q4.md)).
3. Attacker coba reset password email pribadi Afomiya pake jawaban itu, tapi gagal karena ada **MFA** (kode ke HP) ([Q5](./q5.md)).
4. Afomiya resmi gabung CloutHaus, datanya mulai kecatet di internal log (Azure Data Explorer/KQL): email kerja `afomiya_storm@clouthaus.com`, role `Influencer Partner`, dan ternyata MFA di akun CloutHaus-nya **belum aktif** ([Q6](./q6.md)-[Q8](./q8.md)).
5. Email phishing nyamar jadi "Dior" masuk ke inbox kerjanya — sender-nya `collabs@dior-partners.com` (bukan domain Dior asli), subject `[EXTERNAL] Exclusive Partnership Opportunity with Dior`, linknya `https://super-brand-offer.com/login` ([Q9](./q9.md)-[Q11](./q11.md)).
6. Afomiya klik link itu (`OutboundNetworkEvents`, 2025-04-03 11:20:00+00:00) dan masukin username (`afstorm`) ke fake login page clone Dior ([Q12](./q12.md)-[Q13](./q13.md)).
7. Domain phishing-nya resolve ke IP `198.51.100.12` (`PassiveDns`), dan IP itu ternyata nge-host 3 domain mencurigakan lain juga — indikasi infrastruktur attacker ([Q14](./q14.md)-[Q15](./q15.md)).
8. Karena reuse password, credentials yang dicuri dipake buat bobol Instagram-nya juga. Attacker DM follower minta "investasi" (phishing scam), dan pura-pura jadi brand buat nipu soal alamat apartment — ditemuin lewat reverse image search dari foto rooftop di postingan Afomiya ("The Apartments at CityCenter") ([Q16](./q16.md)-[Q18](./q18.md)).
9. Bahkan mailbox fisiknya kebobol gara-gara foto close-up kunci yang dia post — classic information leakage ([Q19](./q19.md)).
10. Afomiya sadar, langsung ganti semua password & aktifin MFA di semua akun — jadi lesson tentang cyber resilience ([Q20](./q20.md)-[Q21](./q21.md)).

**Pelajaran:** oversharing di social media (lokasi, jawaban security question, bahkan foto kunci) + password reuse + MFA yang gak konsisten di semua akun = kombinasi yang bikin satu klik phishing bisa nyebar ke banyak akun sekaligus.

## Tables yang dipakai
Lihat [notes.md](./notes.md)

## Query yang dipakai
Kenalan sama table `Employees` ([Q1](./q1.md)):
```kql
Employees
| take 10
```

Cari data Afomiya ([Q1](./q1.md), [Q6](./q6.md)):
```kql
Employees
| where name contains "afomiya"
```

Cek login attempt attacker ke akun Instagram-nya ([Q4](./q4.md)-[Q5](./q5.md)):
```kql
AuthenticationEvents
| where username == "afstorm"
```

Cari role Afomiya di CloutHaus ([Q7](./q7.md)):
```kql
Employees
| where name contains "afomiya"
| distinct role
```

Cari email phishing "Dior" ([Q9](./q9.md)-[Q11](./q11.md)):
```kql
Email
| where recipient == "afomiya_storm@clouthaus.com"
| where subject contains "exclusive"
```

Cek apakah Afomiya klik link phishing-nya ([Q12](./q12.md)):
```kql
OutboundNetworkEvents
| where url contains "super-brand-offer"
```

Cari domain lain yang satu infrastruktur sama IP phishing ([Q15](./q15.md)):
```kql
PassiveDns
| where ip contains "198.51.100.12"
| distinct domain
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

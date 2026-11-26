# Section 1: Enough Beef for a Burger

- Tanggal mulai: 2026-11-25
- Role: Security Analyst, OWL Records
- Status: done (13/13 question)

## Narasi
Dwake (OWL Records) dan Present (Dollar Currency Records) lagi musical feud. Dwake rilis verse ngejek Present, tapi gak sadar keceplosan bocorin info pribadi di liriknya sendiri — dan ini yang berujung jadi insiden keamanan.

Alur ceritanya:
1. Present sakit hati, minta labelnya (DCR) cari dirt soal Dwake. Ada tip dari "homie" di cyber underground: DCR diduga hire hacker yang pake IP `18.66.52.227` buat poke around website OWL Records awal April ([Q3](./q3.md)).
2. Investigasi mulai dari cek `Employees` table buat kenalan sama data perusahaan — nemu CEO OWL Records, Sean Crater ([Q4](./q4.md)).
3. Query `InboundNetworkEvents` dengan filter IP tadi nemu 19 hits browsing activity ke website di tanggal tersebut ([Q5](./q5.md)).
4. Dari kolom `url` hasil browsing itu, ketauan si hacker (operator) nyari-nyari info soal Dwake, termasuk ngetik ejekan soal musik Dwake ("trash") ([Q6](./q6.md), [Q7](./q7.md)) — dan akhirnya nemu email Dwake, `dwake_audrey@owl-records.com`, lalu nyoba reset password pake email itu ([Q8](./q8.md)).
5. Reset password di OWL Records butuh jawab security questions (mother's maiden name & childhood pet's name). Masalahnya, Dwake nyebutin dua-duanya di versenya sendiri — "Washington" (nama keluarga ibunya) dan "Fluffy" (peliharaan masa kecil) ([Q9](./q9.md)-[Q11](./q11.md)).
6. Operator pake dua jawaban itu buat berhasil reset password Dwake, kebukti dari full URL reset-password-nya di log ([Q12](./q12.md)).
7. Setelah kuasai email, operator juga ambil alih Instagram Dwake dan posting gambar memalukan ([Q13](./q13.md)).

**Pelajaran:** info yang keliatan sepele/personal (nama ibu, nama peliharaan) sering jadi jawaban security question — jangan diumbar di tempat publik (termasuk lirik lagu).

## Tables yang dipakai
Lihat [notes.md](./notes.md)

## Query yang dipakai
Cari CEO OWL Records ([Q4](./q4.md)):
```kql
Employees
| where role == "CEO"
```

Lihat url yang diakses operator dari IP mencurigakan ([Q5](./q5.md)-[Q8](./q8.md)):
```kql
InboundNetworkEvents
| where timestamp between (datetime("2024-04-10T00:00:00") .. datetime("2024-04-11T00:00:00"))
| where src_ip has "18.66.52.227"
| project url
```

Cari aksi reset-password pake info yang bocor di verse ([Q12](./q12.md)):
```kql
InboundNetworkEvents
| where timestamp between (datetime("2024-04-10T00:00:00") .. datetime("2024-04-11T00:00:00"))
| where src_ip has "18.66.52.227"
| where url has_any("washington", "fluffy")
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

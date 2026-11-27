# A Rap Beef: An Intro to Security Investigations

Great for first time KC7 players! Gak butuh pengalaman cybersecurity buat module ini.

## About This Game
Two hip-hop artists lagi musical feud. Dari tensi lama antara mereka, mulai saling jab lewat musik.

Sebagai Security Analyst untuk OWL Records, tugasnya jaga info perusahaan aman biar artist-nya gak kebuka selama feud ini berlangsung.

## Learning Objectives
- Cara identifikasi elemen-elemen phishing campaign
- Cara interpretasi Security Logs buat identifikasi bukti aktivitas malicious
- Cara investigasi basic cybersecurity intrusion

## Topics
reconnaissance, dark-web, phishing

## Summary: Gimana Hacker-nya Nyerang
Attack chain dari awal sampe akhir (gabungan Section 1 & 2):

1. **Reconnaissance lewat lirik lagu** — Dwake bikin diss track ke Present, gak sadar keceplosan bocorin info pribadi di liriknya sendiri: nama keluarga ibunya ("Washington") dan nama peliharaan masa kecil ("Fluffy").
2. **Initial foothold** — DCR (label Present) diduga hire hacker yang poke around website OWL Records pake IP `18.66.52.227`, nemu email Dwake lewat browsing activity di website.
3. **Account takeover via security question** — hacker pake info dari lirik buat jawab security question pas reset password email Dwake, berhasil masuk, lanjut takeover Instagram-nya juga, posting gambar memalukan.
4. **Retaliasi berujung phishing campaign** — OWL Records "settle" kejadian itu secara private, tapi besoknya ada ancaman dark web ke Present. Hacker (IP yang sama) kirim phishing campaign nargetin role `Rapper`/`Lead Rapper` di OWL Records, nyamar jadi brand/ghostwriter lewat domain `betterlyrics4u.com`.
5. **Credential phishing & exfiltration** — Dwake (lagi-lagi) klik link phishing-nya, masuk fake login page, credentials-nya dipake buat login lagi ke akunnya, nyari & nyuri data sensitif (`DwakesDirtySecrets.zip`) dari emailnya.
6. **Resolusi** — Dwake & Present sama-sama punya "dirt" satu sama lain, akhirnya setuju gencatan senjata.

**Pelajaran utama:** satu kebocoran info kecil (jawaban security question di lirik lagu) bisa jadi entry point buat rangkaian serangan yang terus eskalasi — dari account takeover, ke phishing campaign yang lebih luas, sampe data exfiltration. Dan hacker yang sama bisa balik lagi berkali-kali selama celah (reused info, gak ada MFA di titik-titik tertentu) belum ditutup.

## Daftar section
- [section-1-enough-beef-for-a-burger](./section-1-enough-beef-for-a-burger/README.md) — 13 question, done
- [section-2-less-beef-more-phish](./section-2-less-beef-more-phish/README.md) — 23 question, done

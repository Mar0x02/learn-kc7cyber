# Learn KC7 Cyber

Dokumentasi & write-up progress belajar cybersecurity (threat hunting) lewat [KC7 Cyber](https://kc7cyber.com) — platform training gratis berbasis skenario intrusi realistis, di mana kita investigasi "kasus" dengan query data (gaya Kusto/KQL) buat jawab pertanyaan per section.

## Struktur
Tiap "game"/investigation KC7 punya folder sendiri di root, isinya 1+ section:

```
<nama-investigation>/
  README.md                  # overview: about this game, learning objectives, daftar section
  <nama-section>/
    README.md                # overview section: narasi/skenario, status, daftar question
    notes.md                 # schema table & fakta penting khusus section ini
    q1.md                    # 1 question = 1 file: pertanyaan, cara penyelesaian, jawaban
    q2.md
    ...
```

## Daftar investigation
- [rap-beef-intro-to-security-investigations](./rap-beef-intro-to-security-investigations/README.md) — 2 section, done
- [clouthaus-social-media-compromise](./clouthaus-social-media-compromise/README.md) — 2 section, done

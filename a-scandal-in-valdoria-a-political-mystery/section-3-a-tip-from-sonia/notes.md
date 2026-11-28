# Notes - Section 3: A Tip From Sonia

## Schema tables
- `AuthenticationEvents` (8 kolom)
- `Email` (7 kolom)
- `Employees` (9 kolom)
- `FileCreationEvents` (7 kolom)
- `InboundNetworkEvents` (5 kolom)
- `OutboundNetworkEvents` (5 kolom)
- `PassiveDns` (3 kolom)
- `ProcessEvents` (8 kolom)
- `SecurityAlerts` (5 kolom)

## Fakta penting dari narasi
- Sonia Gose (Senior Editor) ngasih tip ke tim investigasi setelah meeting staff.
- ⚠️ Hostname Sonia ditulis `UL0M-MACHINE` (pakai angka nol, bukan huruf O) — perhatiin pas query biar gak miss.
- File `Valdorian_Times_Editorial_Offer_Letter.docx` muncul di `C:\Users\sogose\Downloads\` via proses `edge.exe` tanggal 2024-01-05 10:24:04, cuma ~47 detik setelah Sonia klik link phishing-nya.
- 28 detik kemudian (10:24:32), `C:\ProgramData\hacktivist_manifesto.ps1` ditulis via `explorer.exe` — nama file & path sama persis kayak di Section 1 (insiden Nene Leaks), indikasi aktor/toolkit serangan yang sama dipakai ke beberapa korban.
- Timeline eksekusi di mesin Sonia: file ditulis (10:24:32) → persistence via `schtasks` dibuat ~1 jam kemudian (11:22:44) → script beneran dijalankan 33 menit setelahnya (11:55:22). Ada jeda waktu antar tahap, beda sama Nene Leaks yang lebih cepat berturut-turut.

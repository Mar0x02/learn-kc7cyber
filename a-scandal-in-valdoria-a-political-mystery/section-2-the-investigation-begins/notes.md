# Notes - Section 2: The Investigation Begins

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
- Investigasi resmi dimulai dengan menghubungi Nene Leaks (Editorial Director) buat dapet info awal.
- Editorial Intern (Ronnie McLovin) baru di-hire 2024-01-02 — sehari sebelum insiden phishing (2024-01-03). ⚠️ Mencurigakan, kemungkinan karyawan baru yang belum ke-vet atau jadi entry point.
- ⚠️ Ronnie ngaku gak pernah ngirim final draft (ketiduran), tapi Clark Kent ngaku terima email final draft dari Ronnie tanggal 31 Januari 2024 — ini gak sinkron sama timeline insiden (phishing/compromise terjadi 2024-01-03). Kemungkinan salah satu pihak bohong, atau ada email palsu yang ngaku-ngaku dari Ronnie.
- Email "URGENT: Final OpEd Draft Edits..." ke Clark Kent (2024-01-31) terkirim beneran dari `ronnie_mclovin@valdoriantimes.news` (bukan spoof/external, verdict CLEAN), lampirannya link sharepoint `.../files/rmclovin/2024/OpEdFinal_to_print.docx`. Karena Ronnie ngaku gak pernah ngirim ini, kemungkinan besar akun emailnya di-takeover attacker.

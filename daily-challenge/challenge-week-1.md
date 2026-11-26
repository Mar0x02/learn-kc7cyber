# Daily Challenge - Week 1

## Q1: Process state `D` di `ps`
**Pertanyaan:** What is the name of the process state represented by D in ps, often associated with waiting for disk I/O?

**Jawaban:** Uninterruptible Sleep

**Penjelasan singkat:**
Proses di state ini lagi nunggu operasi I/O level rendah (misal baca/tulis disk atau request ke hardware) yang gak boleh dipotong kernel di tengah jalan — kalau dipotong bisa bikin data korup atau device-nya jadi gak jelas kondisinya. Makanya proses "terkunci" sampai I/O-nya beneran selesai, gak bisa di-kill/interrupt pakai sinyal biasa selama itu. Biasanya cuma sekejap; kalau nyangkut lama di state `D`, itu tanda ada masalah (disk/storage error atau hang).

## Q2: Isolasi resource di Linux kernel
**Pertanyaan:** What Linux kernel mechanism isolates process views of resources such as process IDs, mount points, and network stacks?

**Jawaban:** Namespaces

**Penjelasan singkat:**
Mekanisme kernel yang bikin tiap proses/grup proses punya "pandangan" sendiri atas resource sistem, seolah terisolasi dari proses lain padahal masih di kernel yang sama. Beberapa tipe yang relevan: PID namespace (proses ID sendiri), mount namespace (filesystem view sendiri), network namespace (network stack sendiri). Ini fondasi yang dipakai container (Docker, dll) buat bikin tiap container berasa kayak mesin terpisah.

## Q3: System call ganti process image
**Pertanyaan:** What Linux system call takes a program pathname, an argument vector, and an environment vector to replace the current process image?

**Jawaban:** `execve()`

**Penjelasan singkat:**
Ngambil 3 argumen: path program, argument vector (`argv[]`), environment vector (`envp[]`) — terus ganti isi process yang lagi jalan (memory, code, data) dengan program baru, tanpa bikin process baru (PID tetep sama). Semua fungsi family `exec*()` di libc (`execl`, `execv`, `execvp`, dll) ujung-ujungnya manggil `execve()`. Biasanya dipasangin sama `fork()`: `fork()` bikin copy process, child-nya manggil `execve()` buat ganti dirinya jadi program lain — pola klasik shell pas jalanin command.

## Q4: Child process yang masih jalan setelah parent exit
**Pertanyaan:** What is a still-running child process traditionally called after its parent exits?

**Jawaban:** Orphan process

**Penjelasan singkat:**
Kebalikan dari zombie. Zombie = child udah exit tapi parent belum `wait()`-in. Orphan = child-nya masih jalan, tapi parent-nya duluan yang exit. Di Linux, orphan otomatis di-reparent ke `init`/PID 1 (atau subreaper kalau ada), jadi tetep ada yang nanggung buat `wait()`-in dia pas dia nanti selesai.

## Q5: File penyimpan password hash & aging info
**Pertanyaan:** What restricted-access file conventionally stores local users' password hashes and password-aging information?

**Jawaban:** `/etc/shadow`

**Penjelasan singkat:**
Nyimpen password hash tiap user lokal (bukan plaintext) plus info password-aging (kapan terakhir ganti, umur min/maks sebelum wajib ganti, masa tenggang sebelum akun dikunci, dll). Beda sama `/etc/passwd` yang readable semua orang (cuma nyimpen UID/GID/home dir/shell) — `/etc/shadow` dibatasin, biasanya cuma root yang bisa baca (permission `600`/`640`, owner root), karena isinya hash password yang kalau bocor bisa di-brute-force offline.

## Q6: Config file filesystem mount saat boot
**Pertanyaan:** What configuration file conventionally defines filesystems to mount at boot and their mount options?

**Jawaban:** `/etc/fstab`

**Penjelasan singkat:**
Daftar filesystem yang harus di-mount pas boot, tiap baris biasanya 6 kolom: device/UUID, mount point, filesystem type, mount options, dump flag, fsck order. Selain otomatis mount pas boot, file ini juga dipake `mount -a` buat mount ulang semua entry-nya, dan `mount <mountpoint>` (tanpa sebut device) bisa auto-lookup opsinya dari `fstab`.

## Q7: Mount option yang abaikan setuid/setgid
**Pertanyaan:** What mount option makes the kernel ignore set-user-ID and set-group-ID bits when executing files from that mount?

**Jawaban:** `nosuid`

**Penjelasan singkat:**
Kalau filesystem di-mount pakai `nosuid`, kernel ngabaikan bit setuid/setgid di executable yang ada di mount itu — meski file-nya punya permission setuid/setgid, pas dijalanin tetep jalan pakai permission user biasa (bukan privilege owner file). Dipake buat harden mount point yang gak trusted (removable media, `/tmp`, partisi upload user) biar gak bisa disalahgunain buat privilege escalation lewat binary setuid yang disusupin.

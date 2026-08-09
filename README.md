# Nmap

## Apa itu Nmap?

Nmap adalah tool untuk scanning jaringan. Fungsinya buat nemuin host yang hidup, port yang kebuka, service yang jalan, dan OS target.

## Istilah Dasar

Host, perangkat yang nyambung ke jaringan.
IP, alamat unik tiap perangkat di jaringan.
Port, jalur komunikasi di host. Port 22 SSH, 80 HTTP, 443 HTTPS, 3306 MySQL.
Open, port nerima koneksi.
Closed, port ada tapi gak ada service.
Filtered, port diblokir firewall, gak ada respon.

## Cara Kerja

Nmap ngirim paket khusus ke target dan analisis responnya. Kalau ada jawaban, host hidup dan port kebuka. Gak ada jawaban, berarti mati atau diblokir.

## Kapan Dipakai

- Cek host mana yang hidup di jaringan
- Cari port kebuka di target
- Identifikasi service dan versinya
- Identifikasi OS target

## Urutan Scan di CTF

Mulai dari scan host hidup, terus port, terus service, baru OS dan script.

Command lengkap ada di [docs/basic-command.md](docs/basic-command.md)

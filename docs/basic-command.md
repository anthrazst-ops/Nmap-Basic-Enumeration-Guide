# Basic Command

## Scan Dasar

Scan port biasa:

```bash
nmap target.com
```

Scan semua port (1 sampai 65535):

```bash
nmap -p- target.com
```

Scan port tertentu:

```bash
nmap -p 22,80,443 target.com
```

Scan port umum 1 sampai 1000:

```bash
nmap -p 1-1000 target.com
```

## Deteksi Service dan Versi

```bash
nmap -sV target.com
```

Milih tau service apa yang jalan dan versinya, buat nyari exploit yang cocok.

## Deteksi OS

```bash
nmap -O target.com
```

## Scan Agresif

```bash
nmap -A target.com
```

Gabungan deteksi service, OS, dan script default dalam satu scan.

## Host Discovery

Cek host mana yang hidup di jaringan:

```bash
nmap -sn 192.168.1.0/24
```

Skip host discovery, langsung scan port:

```bash
nmap -Pn target.com
```

Dipakai kalau target gak ngerespon ping tapi port-nya kebuka.

## Kecepatan Scan

```bash
nmap -T4 target.com
```

T1 paling pelan, T5 paling kenceng. T4 cukup buat kebanyakan kasus.

## Simpan Hasil

```bash
nmap -oN hasil.txt target.com
```

Format lain: -oG buat grepable, -oX buat XML, -oA buat semua format sekaligus.

## Keterangan Opsi

-p      port atau range port
-sV     deteksi service dan versi
-O      deteksi OS
-A      scan agresif, service, OS, script
-sn     host discovery, tanpa scan port
-Pn     skip host discovery
-T4     timing template, kecepatan scan
-oN     simpan hasil ke file

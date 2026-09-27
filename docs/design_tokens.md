KantinCepat Design Tokens

Project: KantinCepat
Version: 2.0.0
Platform: Mobile
Viewport: 390 × 844

Dokumen ini merupakan dokumentasi Design Tokens untuk aplikasi KantinCepat. Token digunakan sebagai acuan visual agar seluruh high-fidelity mockup memiliki warna, typography, spacing, radius, dan shadow yang konsisten serta mudah diterjemahkan ke dalam kode.

1. Color Tokens

1.1 Primary

Warna utama KantinCepat menggunakan palet hijau.

Token

HEX

Penggunaan

primary-50

#E8F5E9

Background sangat ringan / highlight

primary-100

#C8E6C9

Background ringan

primary-200

#A5D6A7

Highlight / elemen pendukung

primary-300

#81C784

Aksen

primary-400

#4CAF50

Aksen / elemen interaktif

primary-500

#2E7D32

Warna brand utama / CTA

primary-600

#1B5E20

Hover / pressed / emphasis

1.2 Semantic Colors

Token

HEX

Penggunaan

success

#10B981

Berhasil / status sukses

warning

#F59E0B

Peringatan

error

#EF4444

Error / validasi

info

#3B82F6

Informasi

1.3 Neutral Colors

Token

HEX

Penggunaan

background

#F8FAFC

Background utama aplikasi

surface

#FFFFFF

Card, modal, dan surface

border

#E2E8F0

Border dan divider

text-primary

#0F172A

Teks utama

text-secondary

#64748B

Teks sekunder

text-disabled

#94A3B8

Teks disabled

2. Typography Tokens

Font Family: Inter, sans-serif

Token

Font Size

Line Height

Weight

heading-1

24px

32px

700

heading-2

20px

28px

700

heading-3

16px

24px

600

body-large

16px

24px

400

body-medium

14px

20px

400

caption

12px

16px

500

3. Spacing Tokens

Gunakan hanya nilai spacing berikut agar layout tetap konsisten.

Token

Value

space-1

4px

space-2

8px

space-3

16px

space-4

24px

space-5

32px

space-6

48px

4. Border Radius Tokens

Token

Value

radius-sm

4px

radius-md

8px

radius-lg

12px

radius-xl

16px

radius-full

9999px

5. Shadow Tokens

Token

Value

shadow-sm

0 1px 2px 0 rgba(0, 0, 0, 0.05)

shadow-md

0 4px 6px -1px rgba(46, 125, 50, 0.15)

shadow-lg

0 10px 15px -3px rgba(0, 0, 0, 0.08)

6. User Flow

Alur utama aplikasi KantinCepat:

Login → tekan MASUK KE KANTIN → Home

Home → pilih Food Card / Tombol Pesan → Detail

Detail → tekan PESAN SEKARANG → Form

Form → tekan KONFIRMASI PESAN → Sukses

Sukses → tekan KEMBALI KE HOME → Home

Step

Screen

Trigger

Target

1

01_Login (High-Fidelity)

MASUK KE KANTIN

02_Home (High-Fidelity)

2

02_Home (High-Fidelity)

Food Card / Tombol Pesan

03_Detail (High-Fidelity)

3

03_Detail (High-Fidelity)

PESAN SEKARANG

04_Form (High-Fidelity)

4

04_Form (High-Fidelity)

KONFIRMASI PESAN

05_Sukses (High-Fidelity)

5

05_Sukses (High-Fidelity)

KEMBALI KE HOME

02_Home (High-Fidelity)

7. Developer / AI Usage Guidelines

Gunakan token, bukan nilai warna atau spacing acak.

Gunakan primary-500 sebagai warna CTA/brand utama.

Gunakan semantic tokens untuk status, bukan warna manual.

Gunakan typography tokens yang tersedia untuk seluruh teks.

Gunakan spacing space-1 sampai space-6.

Gunakan radius dan shadow tokens yang tersedia.

Pertahankan konsistensi visual antara Login, Home, Detail, Form, dan Sukses.

Saat menerjemahkan desain ke kode, buat token sebagai CSS variables, theme variables, atau konstanta sesuai framework yang digunakan.

Jangan menambahkan nilai visual baru tanpa alasan desain yang jelas.

8. Token Reference

Colors:
primary-50     #E8F5E9
primary-100    #C8E6C9
primary-200    #A5D6A7
primary-300    #81C784
primary-400    #4CAF50
primary-500    #2E7D32
primary-600    #1B5E20
success        #10B981
warning        #F59E0B
error          #EF4444
info           #3B82F6

Spacing:
space-1        4px
space-2        8px
space-3        16px
space-4        24px
space-5        32px
space-6        48px

Typography:
heading-1      24px/32px
heading-2      20px/28px
heading-3      16px/24px
body-large     16px/24px
body-medium    14px/20px
caption        12px/16px

Source: kantincepat_design_tokens_export.json — KantinCepat Design Tokens Export v2.0.0.

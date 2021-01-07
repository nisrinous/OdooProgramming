# Odoo Installation
*Materi instalasi Odoo*

## Prerequisites and Assumptions
Sebelum memulai materi, berikut adalah prasyarat dan asumsi yang harus dipahami oleh pembaca:
- Materi ini berisi cara instalasi Odoo pada server linux
- Pembaca sebelumnya telah memahami perintah dasar penggunaan CLI linux
- Pembaca sebelumnya sudah tidak asing dengan penggunaan database dan aksesnya

## Dependent Packages
File-file dependensi ini terdiri dari beberapa package python yang diperlukan untuk menjalankan Odoo
1. Lakukan instalasi pakcages dengan `sudo apt install python3-babel python3-dateutil python3-decorator python3-docutils python3-feedparser python3-gevent python3-html2text python3-libsass python3-lxml python3-mako python3-mock python3-ofxparse python3-passlib python3-pil python3-polib python3-psutil python3-psycopg2 python3-pydot python3-pyparsing python3-pypdf2 python3-qrcode python3-reportlab python3-tz python3-usb python3-vatnumber python3-vobject python3-werkzeug python3-xlsxwriter python3-zeep postgresql-client python3-suds python3-xlrd` 
2. Kemudian lakukan instalasi wkhtmltopdf_v.0.12.5 yang bisa di download pada https://github.com/wkhtmltopdf/wkhtmltopdf/releases/tag/0.12.5 
3. Pada link tersebut download file `.deb` dari wkhtmltopdf_v.0.12.5 sesuai kebutuhan arsitektur yang dimiliki
4. Lakukan `sudo dpkg -i nama_file.deb`
5. Apabila terdapat dependensi yang belum terpenuhi, lakukan `sudo apt --fix-broken install xfonts-75dpi xfonts-base xfonts-encodings xfonts-utils`
6. Lakukan kembali `sudo dpkg -i nama_file.deb` setelah *missing dependencies* sebelumnya telah terpenuhi
7. Setelah selesai, buat shortcut untuk **wkhtmltopdf** dan **wkhtmltoimage** dengan menjalankan:
`sudo ln -s /usr/local/bin/wkhtmltopdf /usr/bin`
`sudo ln -s /usr/local/bin/wkhtmltoimage /usr/bin`


**wkhtmltopdf** dan **wkhtmltoimage** sendiri digunakan pada Odoo untuk membangun report dalam bentuk PDF dari  yang dibentuk dari 

## Odoo

### Install from Yenthe
Cara installasi ini menggunakan metode dari Yenthe. Script Yenthe ini biasa digunakan untuk development, sumber yang di ambil dari repository Odoo langsung.


`Kelebihan: setiap keperluan Odoo telah disiapkan dalam satu perintah instalasi`


Sumber: [Yenthe666](https://github.com/Yenthe666/InstallScript)

1. Jalankan `sudo wget https://raw.githubusercontent.com/Yenthe666/InstallScript/14.0/odoo_install.sh`
2. Kemudian atur konfugurasi untuk penamaan service, user, dan port
3. Setelah itu, lakukan `sudo chmod +x odoo_install.sh`
4. Jalankan dengan `sudo ./odoo_install.sh`

### Install by Nightly Code
Cara ini telah terdokumentasi dengan baik pada Odoo.

1. Lakukan `wget -O - https://nightly.odoo.com/odoo.key | apt-key add -`
2. Kemudian `echo "deb http://nightly.odoo.com/13.0/nightly/deb/ ./" >> /etc/apt/sources.list.d/odoo.list`
3. Setelah itu, lakukan update kemudian install Odoo dengan `apt-get update && apt-get install odoo`

### Install by Odoo .deb file
Cara ini dilakukan untuk mendapatkan versi Odoo yang *stable*.
1. Buka halaman https://www.odoo.com/page/download
2. Isi form yang dibutuhkan pada link tersebut, kemudian download **Odoo Community** pada **Odoo 13**
3. Setelah selesai, lakukan `dpkg -i nama_file.deb`
4. Apabila terdapat *missing dependencies*, lakukan instalasi untuk dependensi yang belum terpenuhi dengan `apt-get install -f`
5. Lakukan kembali `dpkg -i nama_file.deb` setelah *missing dependencies* terpenuhi

## Postgresql

### Install Postgresql
*Postgresql dibutuhkan sebagai Database Engine, dan Setting user, juga untuk akses ke database*

Setiap versi Odoo, memiliki rekomendasi yang berbeda untuk versi postgresql yang dipakai. Namun hampir semua versi postgresql 9.5 keatas, sudah dapat digunakan untuk database engine Odoo.

1. Lakukan `apt install -y postgresql`
2. Setelah terinstall, kita dapat melihat status dari server postgresql dengan menjalankan `sudo service postgresql status`
```
root@laptop:/# sudo service postgresql status
10/main (port 5432): down
```
4. Kemudian jalankan server postgresql dengan `sudo service postgresql start`
```
root@laptop:/# sudo service postgresql start
 * Starting PostgreSQL 12 database server                           [ OK ]
root@laptop:/# sudo service postgresql status
10/main (port 5432): online
```
3. Selanjutnya, hal yang diperlukan adalah user akses untuk odoo ke postgresql. Buat user postgresql dengan menjalankan `sudo -u postgres createuser odoo -s`

## Configuration
*Konfigurasi Performa dan security*

- Konfigurasi odoo disimpan pada file `/etc/odoo/odoo.conf`
- Berbeda cara install Odoo, berbeda juga nama file config, tetapi tetap berada pada direktori `/etc` 
- Lakukan konfigurasi berikut:
```
[options]
# addons ini berisi module module default dari Odoo
# jika menginstall Odoo Enterprise deb, module enterprise juga akan terisi di direktori ini
addons_path = /usr/lib/python3/dist-packages/odoo/addons

# koneksi ke Database
db_user = odoo      # sesuai dengan user yang dibuat sebelumnya saat instalasi postgresql
db_host = False     # jika False, maka db_host = localhost
db_password = user_password
db_port = False     # jika False, maka db_port= 5432
```

## Akses Odoo
Setelah proses di atas dijalankan, selanjutnya Odoo dapat dijalankan

1. Lakukan `sudo service odoo status`
```
root@laptop:/# sudo service odoo status
Status of odoo: stopped
```
2. Jalankan odoo dengan `sudo service odoo start`
```
root@laptop:/# sudo service odoo start
Starting odoo: ok
root@laptop:/# sudo service odoo status
Status of odoo: running
```
3. Kemudian buka halaman `IPaddress:8069` atau `localhost:8069` pada browser
4. Isikan data diri yang dibutuhkan pada form yang tersedia
5. Centang bagian **Demo data** lalu **Create database**

## Checklist
- [ ] **Membaca Prerequisites dan Assumptions**
- [ ] **Menginstall Dependent Packages**
- [ ] **Menginstall Odoo**
- [ ] **Menginstall postgresql**
- [ ] **Melakukan konfigurasi performa dan security Odoo**
- [ ] **Mengakses Odoo**






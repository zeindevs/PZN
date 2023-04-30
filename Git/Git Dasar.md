# Git Dasar

## Agenda

- Pengenalan Version Control
- Pengenalan Git
- Repository
- Workflow
- The Three Tree
- Working Directory
- Staging Index
- Hash
- Commit
- Reset Commit
- Dain lain-lain

## #1 Pengenalan Version Control

### Sebelum ada Version Control System

- Saat kita mengerjakan pekerjaan, kita sering sekali melakkan revisi. Misal saja kita membuat dokumen proposal atau skripsi
- Biasanya kita akan simpan dokumen pertama dengan nama docoument_1, setelah mendapat revisi, kita akan simpan dengan nama document_2, jika masih ada revisi, kita akan simpan dengan nama document_3 dan seterusnya
- Kenapa kita lakukan hal tersebut? Agar kita bisa megetahui perubahan yang terjadi antar revisi document, dan jika sewaktu-waktu kita perlu menggunakan revisi yang sebelumnya, kita bisa menggunakannya dengan mudah

### Version Control System

- Version Control adalah sebuah system yang merekam perubahan pada file waktu ke waktu sehingga kita bisa melihat versi sebelumnya jika diinginkan
- Version Control sangat populer dikalangan programmer, karena programmer selalu membuat kode program dalam bentuk kode tulisan. Dengan version control, programmer bisa merekam semua perubahan yang terjadi, sehingga jika terjadi kesalahan, programmer bisa dengan mudah kembali ke version sebelumnya
- Tapi tidak hanya untuk file dalam bentuk text, jika misal kita adalah seorang Graphic Designer, kita juga bisa memanfaatkan Version Control untuk merekam peubahan dari file gambar atau layout, sehingga kita tidak perlu membackup tiap perubahan secara manual

### Tipe Version Control

Secara garis besar, version control dibagi menjadi 3 jenis

- Local Version Control
- Centralized Version Control
- Distributed Version Control

### Local Version Control

- Local version control merupakan version control yang berjalan hanya di local komputer
- Pendekatan ini bisa digunakan karana sederhana dan tidak butuh server, karena semua riwayat pekerjaan disimpan di local komputer
- Setiap perubahan versi yang terjadi pada file hanya disimpan di local komputer

### Diagram Local Version Control

![Gambar]()


### Centralized Version Control

- Masalah yang terjadi pad Local Version Control adalah, jika komputer rusak, maka seluruh data bisa hilang
- Selain itu agak susah untuk berkolaborasi dengan pengguna lain jika file hanya ada di satu komputer
- Untuk menangani masalah ini, kita bisa menggunakan Centralized Version Control
- Centralized Version Control menyimpan seluruh data revisi di Server
- Pengguna bisa mengakses data ke Server untuk melihat file
- Kekurangannya adalah, jika pengguna offline, merupakan tidak bisa melihat riwayat file karena semua riwayat hanya ada di Server
- Jika Server down, maka seluruh pengguna tidak bisa melakukan perubahan dan melihat revisi file
- Contoh Centralized Version Control adalah Subversion

### Diagram Centralized Version Control

![Gambar]()

### Distributed Version Control

- Distributed Version Control adalah alternatif lain dari Centralized Version Control
- Dalam DVCs, client tidak hanya mengambil file versi terakhir, namun seluruh riwayat revisi di copy seluruhnya
- Hal ini menjadikan jika terjadi masalah dengan Server, client masih tetap bekerja, manipulasi file, sampai melihat riwayat perubahan
- Bahkan dalam DVCs, Server bisa lebih dari satu, karena tiap server isinya sama, fill backup data
- Contoh DVCs adalah Git, Mercurial dan lain-lains

### Diagram Distributed Version Control

![Gambar]()

## #2 Pengenalan Git

### Sejarah Git

- Git muncul dengan latar belakang OpenSource project Linux Kernel
- tahun 1991-2002, Linux Kernel di develop hanya dengan memanfaatkan path dan archived files
- Tahun 2002, Linux Kernel mulai menggunakan DVCs bernama BitKeeper
- Di tahun 2005, hubungan antara perusahaan pemilik BitKeeper dengan komunitas Kinux Kernel kurang baik, sehingga pembuat Linux, Linus Torvalds mulai membuat DVCs sendiri
- Git pertama kali dikenalkan tahun 2005, semakin kesini Git semakin populer dan sekarang menjadi DVCs yang paling populer di dunia
- Git sangat cepat, ringan dan baik dalam me-manage project dengan ukuran besar

### Pengenalan Git

- Jadi, Git adalah salah satu DVCs yang ada
- Git tidak membutuhkan server untuk melakukan perubahan atau melihat riwayat revisi, hal ini dikarenakan dalam Git, semua riwayat project akan selalu di duplikasi, baik itu di server ataupun di local komputer
- Artinya sebenarnya Git juga bisa digunakan sebagai Local Version Control
- Setiap perubahan yang terjadi di Git akan selalu dibuat signature (tanda) nya menggunakan algoritma hashing SHA-1. Hal ini menjadikan perubahan sekecil apapun pasti bisa dideteksi oleh git
- Semua hal yang terjadi di git secara otomatis akan dicatat, hal ini menjadikan perunahan apapun di Git, pasti selalu bisa dikembalikan ke versi sebelumnya

### Menginstall Git

- Git adalah aplikasi OpenSource dan Gratis, kita bisa download aplikasi Git dengan bebas
- Git tersedia untuk berbagain sistem operasi, seperti Windows, Mac, dan Linux
- Kita bisa download Git di : https://git-scm.com/downloads

### Memastikan Git Berjalan

- Git merupakan aplikasi berbasis terminal / command line, oleh karena itu, untuk menggunakan Git, kita perlu membuat terminal / command line
- Untuk mengecek versi Git yang terinstall do local computer kita, kita bisa gunakan perintah: `git --version`

### Tool Pembantu

- Visual Studio Code : https://code.visualstudio.com/
- Install di PATH

## #3 Configuration

### Configuration

- Setelah selesai menginstall git, hal yang pertama kita lakukan adalah melakukan konfigurasi
- Yang paling utama yang perlu kita konfigurasi diawal adalah user name dan user email

```
git config --global user.name "Eko Kurniawan Khannedy"
git config --global user.email "echo.khannedy@gmail.com"
```

### Menggunakan Visual Studio Code

- Agar mempermudah, kita akan menjadikan Visual Studio Code sebagai default editor untuk Git dan default diff tool

```
git config --global core.editor "code --wait"
```

```
git config --global diff.tool "default-difftool"
git config --global difftool.default-difftool.cmd "code --wait --diff \$LOCAL \$REMOTE"
```

### Melihat Seluruh Configuration

```
git config --list --show-origin
```

## #4 Repository

### Repository

- Repository merupakan sebutan project di Git
- Kita bisa membuat folder kosong atau folder yang sudah berisi file, lalu membuatnya sebagai Git
- Atau kita bisa melakukan clone Git Repository yang sudah ada di Server Git

### Membuat Repository

- Untuk membuat repository, kita hanya perlu menggunakan perintah : `git init`
- Dan dilakukan dari dalam folder yang akan kita jadikan sebagai Git Repository
- Setelah membuat Git Repository, kita bisa lihat ada folder baru dengan nama `.git`
- `.git` merupakan folder yang berisikan database Git, jangan sampai kita mengubah data yang terdapat pada folder .git

### Kodde : Git Init

```
$ git init
```

### Kode : Git Status

```
$ git status
```

## #5 Workflow

### The Three States

- Harap diperhatikan, ini adalah hal utama yang wajib dimengerti di Git agar selanjutnya bisa mengerti dengan baik
- Git memiliki tiga state terhadap file kita: modified, staged and committed
- Modified artinya kita mengubah (menambah, mengedit, menghapus) file, namun belum disimpan secara permanen ke repository
- Staged artinya kita menandai modifikasi yang kita lakukan terhadap file akan disimpan secara permanen ke repository
- Committed artinya data sudah aman disimpan di repository

### Three Section

- Tiga state sebelumnya di dalam Git dilakukan di section yang berbeda-beda, yaitu Working Directory, Staging Area dan Repository
- Saat kita melakukan modifikasi file, itu dilakukan di working directory
- Staging Area merupakan section dimana file sudah disiapkan untuk disimpan secara permanan, di Staging Area semua informasi perubahan file akan disimpan
- Repository merupakan tempat dimana semua file dan database riwayat versi file disimpan

### Diagram Three Tree

![Gambar]()

### Workflow

- Sekarang kita sudah tahu tentang arsitektur Three Tree, sekarang peranyaannya, bagaimana alur kerja menggunakan Git
- Secara sederhana, setiap perubahan-akan kita lakuknan di working directory
- Jika ada yang mau kita siapkan untuk disimpan secara permanan, kita akan bawa perubahan tersebut ke staging index
- Selanjutnya, kita bisa melakukan penyimpanan versi baru secara permanen ke repository

### Diagram Workflow (1)

![Gambar]()

### Diagram Workflow (2)

![Gambar]()

## #6 Hash

### Snapshot

- Pada materi sebelumnya kita selaku menyebutkan versi pada perbahan file
- Sebenarnya perubahan yang dilakukan bisa juga dilakukan secara beramaan untuk beberapa file, hal in berarti sebenarnya tidak ada yang namanya versi file
- Semua perubahan yang terjadi akan direkam, dan kita sebut namanya adalah snapshot
- Snapshot berisikan semua perubahan yang terjadi di semua file yang kita commit
- Setiap snapshot akan menghasilkan hash

### Hash

- Setiap snapshot yang kita lakukan, semua akan menghasilkan hash sebagai identitas snapshot nya
- Hash menrupakan checksum untuk menghitung perubahan yang terjadi
- Git menggunakan algoritma SHA-1 untuk menghitung hash
- Hash dibutuhkan untuk menjaga data integrity, sehingga tiap snapshot yang sudah kita lakukan tidak bisa diubah, hal ini karena akan secara otomatis merusak hash yang sudah dibuat 
- Contoh hash Git : 30534abcdede891829182912891289bbd

### Diagram Snapshot

![Gambar]()

### Perhitungan Hash

- Perhitungan hash dilakukan tidak hanya dari perubahan file
- Namun juga dari parent, author dan message
- Artinya perubahan yang terjadi pada snapshot sebelumnya, maka akan menimbulkan efek berantai, karena semua hash selanjutnya akan berubah
- Oleh kareana itu, hal tersebut tidak bisa dilakukan di Git

### HEAD

- HEAD merupakan pointer menuju hash yang paling akhir
- Karena kadang sangat menyulitkan jika harus menuliskan hash value, jika kita akan menuju ke hash paling baru, kita bisa gunakan kata HEAD

### Diagram HEAD

![Gamar]()

## #7 Menambah File

### Menambah File

- Untuk menambah file baru ke Repository, kita cukup tambahkan file nya saja
- Secara otomatis file yang kita tambahkan awalnya akan berada di working directory
- Secara default saat menambah file baru, file tersebut tidak akan di track perubahannya
- Agar perubahan di track, kita harus pindahkan dari working directory ke staging index

### Kode : Untracked Files

```
$ git status
```

### Kode : Memindahkan ke Staging Index

```
$ git add file.txt
$ git status
```

### Kode : Commit ke Repository

```
$ git commit -m "Menambah file.txt"
$ git status
```

### Tugas

- Tambah file2.txt, lalu commit ke Repository
- Tambah file3.txt, lalu commmit ke Repository

## #8 Meengubah File

### Mengubah File

- Untuk melakuka nperubahan file kita, cukup lakukan perubahan file terhadap file yang sudah ada di Repository
- Secara otomatis git bisa mendeteksi perubahan
- Sama seperti dengan menambah file, jika perubahan ingin kita simpan secara permanen, kita bisa pindahkan ke staging index, lalu commit ke Repository

### Kode : Git Status

```
$ git status
```

![Gambar]()

### Melihat Perubahan File

- ketika kita melakukan perubahan Git, secara otomatis medeteksi bahwa file tersebut berubah
- Jika kita ingin melihat perubahan yang terjadi, kita juga bisa menggunakan Git untuk melihat perubahan nya dengan perintah : `git diff`

### Kode : Melihat Periubahan File

```
$ git diff
```

![Gamabr]()

### Tugas

- Commit perubahan file3.txt ke Repository
- Ubah file1.txt dan file2.txt secara bersamaan, lalu commit semuannya ke Repository

## #9 Menghapus File

## Menghapus File

- Untuk menhapus file di Repository, kita cikup lakukan delete file nya di folder nya
- Secara otomati Git akan mendeteksi file yang hilang
- Sama seperti menambah dan menghapus, jika ingin simpan secara permanan di Repository, kita harus menambahkan operasi terserbut ke Staging index, lalu commit ke Repository

### Kode : Git Status

![Gambar]()

### Kode : Git Commit

```
$ git add file3.txt
$ git status
$ git commit -m "menghapus file3.txt"
```

![Gambar]()

## #10 Membatalkan Perubahan

### Membatalkan Penambahan File

- Jika kita menambahkan fiel di Working Directory, lalu misal kita ingin membatalkan perubahannya
- Caranya cukup sederhana, kita hanya perlu menghapus file terseut, atau bisa menggunakan perintah : `git clean -f`


### Membatalkan Perubahan File

- Jika kita ingin membatalkan perubahan file yang telah kita lakukan, kita bisa menggunakan perintah : `git restore namafile`


### Membatalkan Penghapusan File

- Penghapusan file sebenarnya sama dengan perubahan file, jadi jika kita ingin membatalkan penghapusan file, kita bisa gunakan perintah yang sama dengan membatalkan perubahan file : `git restore namafile`

### Kode Git Restore

```
$ git status
$ git restore file1.txt
```

![Gambar]()

### Membatalkan dari Staging Index

- Perintah fit restore hanya bisa dilakukan untuk membatalkan perubahan yang terjadi Working Directory
- Artinya jika perubahan terlanjur kita masukkan ke Staging Index, maka untuk membatalkannya tidak bisa kita lakukan secara langsung dari Staging Index
- Kita perlu mengembalikan posisi dari Staging Index ke Working Directory terlebih dahulu, caranya kita bisa gunakan perintah : `git restore --staged namefile`

### Kode : Git Restore ke Working Directory

```
$ git status
$ git restore --staged file1.txt
$ git status
```

![Gambar]()

### Membatalkan Yang Sudah di Commit

- Bagaimana jika perubahan yang kita lakukan terlanjur di commit?
- Tidak ada cara yang bisa kita lakukan jika perubahan sudah terlanjur di commit
- Yang bisa kita lakukan adalah dengan dua cara, Rollback Commit atau Revert Commit
- Kedua cara tersebut akan kita bahas di materi sendiri-sendiri

## #11 Commit Log

## Commit Log

 - Git adalah distributed version control, artinya walaupun kita Repository di local komputer kita, semua riwayat perubahan disimpan di komputer kita
 - Kekurangan menjadi makin lama Repository akan semakin besar ukurannya, namun keuntungannya, kita bisa melihat semua riwayat commit, atau disebut Commit Log
 - Untuk melihat Commit Log, kita bisa gunakan perintah : `git log`

### Kode : Git Log

```
$ git log
```

![Gambar]()

### Log Sederhana

- Kadang kita hanya ingin melihat commit log message nya saja atau istilahnya adalah versi sederhananya saja
- Untuk melakukan itu, kita bisa gunakan perintah : `git log --oneline`

### Kode : Git Log Oneline

```
$ gti log --oneline
```

![Gambar]()

### Graph

- Saat nanti kita sudah belajar tantang Git Branching, kadang kita ingin melihat commit log dengan hubungannya dengan commit log sebelumnya
- Hal ini kita bisa lakukan menggunakan perintah : `git log --oneline --graph`

### Kode : Git Log Graph

```
$ git log --oneline --graph
```

![Gambar]()

### Kode : Contoh Git Graph Kompleks

![Gambar]()

### Melihat Detail Commit

- Kadang kita ingin melihat detail periubahan yang terjadi pada sebuah commit
- Untuk melakukan itu, kita bisa gunakan perintah : `git show hash`

### Kode : Detail Commit

```
$ git show ad2bc
```

![Gambar]()

## #12 Compare Commit

### Compare Commit

- Git memiliki fitur untuk membandingkan antara commit dengan commit lainnya
- Namun jangan sampai salah pengertian, membandingkan disini adalah membadingkan snapshot hasil commit, bukan perubahan yang terjadi antara commit
- Misa pada commit sebelumnya kita pernah menambah file3.txt, namun jika kita bandingkan antara commit pertama dan terakhir (HEAD), hasilnya hanyalah perbandingan antara file1 dan file2, tidak ada file3
- Hal ini dikarenakan membandingkan commit bukanlah membandingkan perubahan yang pernah terjadi, melainkan membandingkan hasil di commit
- Untuk membandingkan commit, kita bisa gunakan perintah: `git diff hash1 hash2`

### Kode : Git Diff

```
$ git diff 7453f5e HEAD
```

![Gambar]()

### Difftool

- Sebelumnya kita sudah melakukan pengaturan menggunakan Visual Studio Code untuk melihat diff
- Jika kita ingin menggunakan visual studio code untuk melihat perbedaan antar commit, kita bisa gunakan perintah : `git difftool hash1 hash2`

### Difftool Visual Studio Code

![Gambar]()

## #14 Rename File

### Rename FIle

- Hal yang paling menarik di Git adalah, Git bisa mendeteksi rename file
- Secar sederhana sebenarnya rename file merupakan operasi gabungan antara hapus file, lalu menambah file baru dengan isi yang sama
- Namun Git bisa otomatis mendetaksi jika terjadi perubahan nama file, karena isi file sebagaian besar masih sama

## Kode : Git Status

```
$ git status
```

![Gambar]()

### Kode : Git Status di Staging Index

```
$ git add file2.txt
$ git add file_2.txt
$ git status
```

![Gaambar]()

## #14 Reset Commit

### Reset Commit

- Sebelumnya kita suah tahu membatalkan perubahan, namun bagaimanay jika ternyata perubahan sudah terlanjur kita commit ke Repository?
- Untuk hal seperti itu, kita bisa melakukan reset commit
- Reset commit merupakan mekanisme dimana kita menggeser HEAD pointer ke posisi commit yang kita mau, artinya commit selanjutnya akan dilakukan pada posisi HEAD baru
- Untuk melakukan reset commit, kita bisa gunakan perintah : `git reset <mode> hash`
- Ada beberap mode pengaturan melakukan reset commit

### Diagram Git Reset

![Gambar]()

### Mmode Git Reset

- `--soft`, memindahkan HEAD pointer, namun tidak melakukan perubahan apapun di Staging Index dan Working Directory
- `--mixed (default)`, memindahkan HEAD pointer, mengubah Staging Index menjadi sama seperti dengan Repository, namun tidak mengubah apapun di Working Directory
- `--hard`, memindahkan HEAD pointer, dan mengubah Staging Index dan Working Directory sehingga sama dengan Repository

### Kode : Git Log

![Gambar]()

### Kode : Git Reset Soft

```
$ git reset --soft ae4158c
$ git status
```
![Gambar]()

### Rewrite Riwayat Commit

- Jika kita melakukan reset, namun kita belum membuat commit baru
- Kita masih bisa kembali maju lagi ke commit yang paling baru
- Namun jika kita membuat commit baru, secara otomatis commit lama akan ditimpa olah commit baru

### Kode : Git Reset Mixed

```
$ git reset --mixed ae4158c
$ git status
```
![Gambar]()

### Kode : Git Reset Hard

```
$ git reset --hard ae4158c
$ git status
```
![Gambar]()

## #15 Amend Commit

### Amend Commit

- Kadang saat sudah melakukan commit, mungkin ada beberapa hal yang terlupakan
- Biasanya kita akan lakukan reset soft ke commit sebelumnya, lalu tambahkan perubahan yang terlupakan, lalu kita lakukan commit ulang
- Hal tersetbut bisa dilakukan tanpa manual melakukan reset, caranya kita bisa menggunakan perintah : `git commot --amend`
- Perlu diingat, amend akan mengubah hash commit karena data perubahan yang dicommit bertambah

### Tugas

- Tambah file3.txt
- Commit file3.txt ke Repository
- Ubah file3.txt
- Commit dengan amend ke Repository : `git commit --amend`

### Kode : Git Commit Amend

```
$ git add file3.txt
$ git commit --amend -m "Add file3.txt"
```
![Gambar]()

## #16 Versi Sebelumnya

### Versi Sebelumnya

- Kadang kita sering mengalami masalah dengan file yang sudah kita commit ke Repository
- Git memiliki fitur dimana kita bisa melihat versi file pada commit sebelumnya
- Saat kita ambil versi file sebelumnya, file pada commit tersebut akan berada di Staging Index
- Untuk melakukannya, kita bisa gunakan perintah : `git checkout hash -- namafile`

### Kode : Git Status

Misal kita ingi melihat file1.txt sebelum terjadi perubahan di commit 1b5f564, maka kita bisa gunakan perintah : `git checkout 2114ac3 -- file1.txt`

![Gambar]()

### Kode : Git Checkout

```
$ git checkout 2114ac3 -- file1.txt
$ git status
```

![Gambar]()

## #17 Snapshot Sebelumnya

### Snapshot Sebelumnya

- Git juga memiliki fitur seperti mesin waktu, dimaan kita bisa kembali pada snapshot sebelumnya
- Kita bisa tentukan kemana tujuan snapshot kita hanya dengan menggunaakn hash commit
- Cara jika kita ingin menuju ke snapshot tertentu, cukup gunakan petintah : `git checkout hash`
- Jika ingin kembali ke paling awal, kita bisa gunakan perintah : `git checkout namabranch`

### Git Branch

- Materi branching akan dibahas pada course terpisah, namun secara default saat kita membuat Git Repository, maka secara otomatis Git akan membuat branch
- Untuk melihat nama branch saat ini, kita bisa gunakan perintah : `git branch --show-current`

![Gambar]()

### Kode : Git Checkout

```
$ git checkout 7265dca
```

![Gambar]()

### Kode : Kembali Ke Commit Terakhir

```
$ git checkout master
```

![Gambar]()

## #18 Revert Commit

### Revert Commit

- Git memiliki fitur revert commit, yaitu fitur untuk membatalkan commit yang sudah kita lakukan dengan cara membuat commit baru yang membatalkan commit sebelumya
- Misal kita sudah melakukan commit data perubahan dari text Eko menjadi Eka, jika kita revert, secara otomatis akan membuat commit baru dengan melakukan perubahan dari Eka ke Eko
- Untuk melakukan revert commit, kita bisa gunakan perintah : `git revert hash`

### Kode : Git Status

Misal kita akan revert commit melakukan rename dari file2.txt menjadi file_2.txt

![Gambar]()

### Kode : Git Revert

```
$ git revert b34dc9a
```

![Gambar]()

### Kode : Git Log Setelah Revert

![Gambar]()

## #18 Ignore

### Ignore

- Kadang saat membuat aplikasi, tidak semua file ingin kita track di Git, contoh seperti file log, hasil kompilasi, kadang itu tidak butuh di track di Git
- Git memiliki fitur ignore, dimana kita bisa meminta Git secara otomatis tidak mem-track file di Git
- Caranya kita bisa tambahkan file .gitignore di Repository
- Lalu kita bisa tambahkan tiap baris di file .gitignore berisikan file atau folder yang tidak kita ingin track

### Kode : File .gitignore

```
> .gitignore

# Ignore folder log
log/

# Ignore file dengan extension .backup
*.backup

# Ignore file
ignore.txt
```

![Gambar]()

### Kode : Git Status

![Gambar]()


## #19 Blame

### Blame 

- Saat membuat kode program kadang kta ingin tahu, siapa yang menambahkan baris kode program tersebut, dan apa saja yang ditambahkan
- Git memiliki fitur yang bernama blame, ini digunakan untuk mencari tahu, siapa yang menambah perubahan pada file dan juga mengetahui commit nya
- Caranya kita bisa gunakan perintah : `git blame namafile`

### Kode : Git Blame

```
$ git blame file1.txt
```

![Gambar]()

## #20 Alias

### Alias

- Git memiliki fitur yang bernama alias
- Dengan alias, kita bisa menambahkan nama perintah lain untuk perintah yang sudah ada di git
- Misal kita bisa menambah perintah co, komit untuk nama lain dari commit misalnya
- Atau misal menambah alias logline untk nama lain dari log --online

### Kode : Menambahkan Alias

```
$ git config --global alias.ko commit
$ git config --global alias.komit commit
$ git config --global alias.logone "log --oneline"
```

## #21 Materi Selanjutnya

### Materi Selanjutnya

- Git Branching
- Git Remote
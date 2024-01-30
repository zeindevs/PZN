# Docker Dasar

## Sebelum Belajar

- Mengerti tentang sistem operasi
- Mengerti cara menginstall aplikasi
- Mengerti cara menggunakan perintah di terminal / command line
- Mengerti tentang Virtual Machine

## Agenda

- Pengenalan Container
- Pengenalan Docker
- Menginstall Docker
- Arsitektur Docker
- Docker Image
- Docker Registry
- Docker Container
- Docker Volume
- Docker Network
- Dan lain-lain

## #1 Pengenalan Container

### Virtual Machine

- Dalam dunia Infrastructure, kita sudah terbiasa dengan yang namanya VM (Virtual Machine)
- Saat membuat sebuah VM, biasanya kita akan menginstall sistem operasi juga di VM nya
- Masalah ketika kita menggunakan VM adalah proses yang lambat ketika pembuatan VM nya, dan butuh waktu untuk boot sistem operasi di dalam VM tersebut ketika kita menjalankan VM atau me-restart VM tersebut

### Diagram Virtual Machine

![Diagram Virtual Machine](./imagesdocker-dasar-01.jpg)

### Container

- Berbeda dengan VM, Container sendiri berfokus pada sisi Aplikasi
- Container sendiri sebenarnya berjalan diatas aplikasi Container Manager yang berjalan di sistem operasi.
- Yang membedakan dengan VM adalah, pada Container, kita bisa mem-package aplikasi dan dependency-nya tanpa harus menggabungkan sistem operasi
- Container akan menggunakan sistem operasi host dimana Container Manager nya berjalan, oleh karena itu, Container akan lebih hemat resource dan lebih cepat jalan nya, karena tidak butuh sistem operasi sendiri
- Ukuran Container biasanya hanya hitungan MB, berbeda dengan VM yang bisa sampai GB karena di dalamnya ada sistem operasinya

### Diagram Container

![Diagram Container](./images/docker-dasar-02.jpg)

## #2 Pengenalan Docker

- Docker adalah salah satu implementasi Container Manager yang saat ini paling populer
- Docker merupakan teknologi yang masih baru, karena baru diperkenalkan sekitar tahun 2013
- Docker adalah aplikasi yang free dan Open Source, sehingga bisa kita gunakan secara bebas
- <https://www.docker.com/>

## #3 Arsitektur Docker

- Docker menggunakan arsitektur Client-Server
- Docker client berkomunikasi dengan Docker daemon (server)
- Saat kita menginstall Docker, biasanya didalamnya sudah terdapat Docker Client dan Docker Daemon
- Docker Client dan Docker Daemon bisa berjalan di satu sistem yang sama
- Docker Client dan Docker Daemon berkomunikasi menggunakan REST API

### Diagram Docker Architecture

![Diagram Docker Architecture](./images/docker-dasar-03.jpg)

## #4 Menginstall Docker

- Docker bisa di install hampir disemua sistem operasi
- Untuk menginstall di Windows dan Mac, kita bisa menggunakan Docker Desktop
- <https://docs.docker.com/get-docker/>
- Untuk Linux, kita bisa install dari repository sesuai distro linux masing-masing
- <https://docs.docker.com/engine/install/>

### Mengecek Docker

- Untuk mengecek apakah Docker Daemon sudah berjalan, kita bisa gunakan perintah :
- `docker version`

## #5 Docker Registry

- Docker Registry adalah tempat kita menyimpan Docker Image
- Dengan menggunakan Docker Registry, kita bisa menyimpan Image yang kita buat, dan bisa digunakan di Docker Daemon dimanapun selama bisa terkoneksi ke Docker Registry

### Diagram Docker Registry

![Diagram Docker Registry](./images/docker-dasar-04.jpg)

### Contoh Docker Registry

- Docker Hub : <https://hub.docker.com/>
- Digital Ocean Container Registry : <https://www.digitalocean.com/products/container-registry/>
- Google Cloud Container Registry : <https://cloud.google.com/container-registry>
- Amazon Elastic Container Registry : <https://aws.amazon.com/id/ecr/>
- Azure Container Registry : <https://azure.microsoft.com/en-us/services/container-registry/>

## #6 Docker Image

- Docker Image mirip seperti installer aplikasi, dimana di dalam Docker Image terdapat aplikasi dan dependency
- Sebelum kita bisa menjalankan aplikasi di Docker, kita perlu memastikan memiliki Docker Image aplikasi tersebut

### Melihat Docker Image

- Untuk melihat Docker Image yang terdapat di dalam Docker Daemon, kita bisa menggunakan perintah :
- `docker image ls`

### Kode: Melihat Docker Image

```sh
docker image ls
```

![Melihat Docker Image](./images/docker-dasar-05.jpg)

### Download Docker Image

- Untuk download Docker Image dari Docker Registry, kita bisa gunakan perintah :
- `docker image pull namaimage:tag`
- Kita bisa mencari Docker Image yang ingin kita download di <https://hub.docker.com/>

### Kode: Download Docker Image

```sh
docker image pull redis:latest
```

![Kode: Download Docker Image](./images/docker-dasar-06.jpg)

### Menghapus Docker Image

- Jika kita tidak ingin menggunakan Docker Image yang sudah kita download, kita bisa gunakan perintah :
- `docker image rm namaimage:tag`

### Kode: Menghapus Docker Image

```sh
docker image rm redis:latest
```

![Kode: Menghapus Docker Image](./images/docker-dasar-07.jpg)

## #7 Docker Container

- Jika Docker Image seperti installer aplikasi, maka Docker Container mirip seperti aplikasi hasil installernya
- Satu Docker Image bisa digunakan untuk membuat beberapa Docker Container, asalkan nama Docker Container nya berbeda
- Jika kita sudah membuat Docker Container, maka Docker Image yang digunakan tidak bisa dihapus, hal ini dikarenakan sebenarnya Docker Container tidak meng-copy isi Docker Image, tapi hanya menggunakannya isinya saja

### Status Container

- Saat kita membuat container, secara default container tersebut tidak akan berjalan
- Mirip seperti ketika kita menginstall aplikasi, jika tidak kita jalankan, maka aplikasi tersebut tidak akan berjalan, begitu juga container
- Oleh karena itu, setelah membuat container, kita perlu menjalankannya jika memang ingin menjalankan container nya

### Melihat Container

- Untuk melihat semua container, baik yang sedang berjalan atau tidak di Docker Daemon, kita bisa gunakan perintah :
- `docker container ls -a`
- Sedangkan jika kita ingin melihat container yang sedang berjalan saja, kita bisa gunakan perintah :
- `docker container ls`

### Kode: Melihat Docker Container

```sh
docker container ls -a
```

![Kode: Melihat Docker Container](./images/docker-dasar-08.jpg)

### Membuat Container

- Untuk membuat container, kita bisa gunakan perintah :
- `docker container create --name namacontainer namaimage:tag`

### Kode: Membuat Container

```sh
docker container create --name contohredis redis:latest
```

![Kode: Membuat Container](./images/docker-dasar-09.jpg)

### Menjalankan Container

- Untuk menjalankan container yang sudah kita buat, kita bisa gunakan perintah :
- `docker container start containerId/namacontainer`

### Kode: Menjalankan Container

```sh
docker container start contohredis
```

![Kode: Menjalankan Container](./images/docker-dasar-10.jpg)

### Menghentikan Container

- Untuk menghentikan container, kita bisa gunakan perintah :
- `docker container stop containerId/namacontainer`

### Kode: Menghentikan Container

![Kode: Menghentikan Container](./images/docker-dasar-11.jpg)

### Menghapus Container

- Untuk menghapus container yang sudah berhenti, kita bisa gunakan perintah :
- `docker container rm containerId/namacontainer`

### Kode: Menghapus Container

```sh
docker container rm contohredis
```

![Kode: Menghapus Container](./images/docker-dasar-12.jpg)

## #8 Container Log

- Kadang saat terjadi masalah dengan aplikasi yang terdapat di container, sering kali kita ingin melihat detail dari log aplikasinya
- Hal ini dilakukan untuk melihat detail kejadian apa yang terjadi di aplikasi, sehingga akan memudahkan kita ketika mendapat masalah

### Melihat Container Log

- Untuk melihat log aplikasi di container kita, kita bisa menggunakan perintah :
- `docker container logs containerId/namacontainer`
- Atau jika ingin melihat log secara realtime, kita bisa gunakan perintah :
- `docker container logs -f containerId/namacontainer`

### Kode: Melihat Container Log

```sh
docker container logs contohredis
```

![Kode: Melihat Container Log](./images/docker-dasar-13.jpg)

## #9 Container Exec

- Saat kita membuat container, aplikasi yang terdapat di dalam container hanya bisa diakses dari dalam container
- Oleh karena itu, kadang kita perlu masuk ke dalam container nya itu sendiri
- Untuk masuk ke dalam container, kita bisa menggunakan fitur Container Exec, dimana digunakan untuk mengeksekusi kode program yang terdapat di dalam container

### Masuk ke Container

- Untuk masuk ke dalam container, kita bisa mencoba mengeksekusi program bash script yang terdapat di dalam container dengan bantuan Container Exec :
- `docker container exec -i -t containerId/namacontainer /bin/bash`
- `-i` adalah argument interaktif, menjaga input tetap aktif
- `-t` adalah argument untuk alokasi pseudo-TTY (terminal akses)
- Dan `/bin/bash` contoh kode program yang terdapat di dalam container

### Kode: Container Exec

```sh
docker container exec -i -t contohredis /bin/bash
```

![Kode: Container Exec](./images/docker-dasar-14.jpg)

## #10 Container Port

- Saat menjalankan container, container tersebut terisolasi di dalam Docker
- Artinya sistem Host (misal Laptop kita), tidak bisa mengakses aplikasi yang ada di dalam container secara langsung, salah satu caranya adalah harus menggunakan Container Exec untuk masuk ke dalam container nya.
- Biasanya, sebuah aplikasi berjalan pada port tertentu, misal saat kita menjalankan aplikasi Redis, dia berjalan pada port `6379`, kita bisa melihat port apa yang digunakan ketika melihat semua daftar container

### Port Forwarding

- Docker memiliki kemampuan untuk melakukan port forwarding, yaitu meneruskan sebuah port yang terdapat di sistem Host nya ke dalam Docker Container
- Cara ini cocok jika kita ingin mengekspos port yang terdapat di container ke luar melalui sistem Host nya

### Melakukan Port Forwarding

- Untuk melakukan port forwarding, kita bisa menggunakan perintah berikut ketika membuat container nya :
- `docker container create --name namacontainer --publish posthost:portcontainer image:tag`
- Jika kita ingin melakukan port forwarding lebih dari satu, kita bisa tambahkan dua kali parameter --publish
- `--publish juga bisa disingkat menggunakan -p`

## #11 Kode: Melakukan Port Forwarding

```sh
docker container create --name contohredis --publish 8080:80 nginx:latest
```

![Kode: Melakukan Port Forwarding](./images/docker-dasar-15.jpg)

## #12 Container Environment Variable

- Saat membuat aplikasi, menggunakan Environment Variable adalah salah satu teknik agar konfigurasi aplikasi bisa diubah secara dinamis
- Dengan menggunakan environment variable, kita bisa mengubah-ubah konfigurasi aplikasi, tanpa harus mengubah kode aplikasinya lagi
- Docker Container memiliki parameter yang bisa kita gunakan untuk mengirim environment variable ke aplikasi yang terdapat di dalam container

### Menambah Environment Variable

- Untuk menambah environment variable, kita bisa menggunakan perintah `--env` atau `-e`, misal :
- `docker container create --name namacontainer --env KEY=”value” --env KEY2=”value” image:tag`

### Kode: Menambah Environment Variable

```sh
docker container create --name contohredis --publish 27017:27017  --env MONGO_INITDB_ROOT_USERNAME=eko --env MONGO_INITDB_PASSWORD=eko mongo:latest
```

![Kode: Menambah Environment Variable](./images/docker-dasar-16.jpg)

## #13 Container Stats

- Saat menjalankan beberapa container, di sistem Host, penggunaan resource seperti CPU dan Memory hanya terlihat digunakan oleh Docker saja
- Kadang kita ingin melihat detail dari penggunaan resource untuk tiap container nya
- Untungnya docker memiliki kemampuan untuk melihat penggunaan resource dari tiap container yang sedang berjalan
- Kita bisa gunakan perintah :
- `docker container stats`

### System Monitoring Mac

![System Monitoring Mac](./images/docker-dasar-17.jpg)

### Container Stats

![Container Stats](./images/docker-dasar-18.jpg)

## #14 Container Resource Limit

- Saat membuat container, secara default dia akan menggunakan semua CPU dan Memory yang diberikan ke Docker (Mac dan Windows), dan akan menggunakan semua CPU dan Memory yang tersedia di sistem Host (Linux)
- Jika terjadi kesalahan, misal container terlalu banyak memakan CPU dan Memory, maka bisa berdampak terhadap performa container lain, atau bahkan ke sistem host
- Oleh karena itu, ada baiknya ketika kita membuat container, kita memberikan resource limit terhadap container nya

### Memory

- Saat membuat container, kita bisa menentukan jumlah memory yang bisa digunakan oleh container ini, dengan menggunakan perintah `--memory` diikuti dengan angka memory yang diperbolehkan untuk digunakan
- Kita bisa menambahkan ukuran dalam bentu `b` (bytes), `k` (kilo bytes), `m` (mega bytes), atau `g` (giga bytes), misal `100m` artinya `100 mega bytes`

### CPU

- Selain mengatur Memory, kita juga bisa menentukan berapa jumlah CPU yang bisa digunakan oleh container dengan parameter `--cpus`
- Jika misal kita set dengan nilai `1.5`, artinya container bisa menggunakan satu dan setengah CPU core

### Kode: Menambahkan Resource Limit

```sh
docker container create --name smallnginx --publish 8081:80 --memory 100m --cpus 0.5 nginx:latest

docker container start smallnginx
```

![Kode: Menambahkan Resource Limit](./images/docker-dasar-19.jpg)

### Kode: Container Stats

```sh
docker container stats
```

![Kode: Container Stats](./images/docker-dasar-20.jpg)

## #15 Bind Mounts

- Bind Mounts merupakan kemampuan melakukan mounting (sharing) file atau folder yang terdapat di sistem host ke container yang terdapat di docker
- Fitur ini sangat berguna ketika misal kita ingin mengirim konfigurasi dari luar container, atau misal menyimpan data yang dibuat di aplikasi di dalam container ke dalam folder di sistem host
- Jika file atau folder tidak ada di sistem host, secara otomatis akan dibuatkan oleh Docker
- Untuk melakukan mounting, kita bisa menggunakan parameter `--mount` ketika membuat container
- Isi dari parameter `--mount` memiliki aturan tersendiri

### Parameter Mount

| Parameter    | Keterangan                                                                        |
| ------------ | --------------------------------------------------------------------------------- |
| type         | Tipe mount, bind atau volume                                                      |
| source       | Lokasi file atau folder di sistem host                                            |
| desctination | Lokasi file atau folder di container                                              |
| readonly     | Jika ada, maka file atau folder hanya bisa dibaca di container tidak bisa ditulis |

### Melakukan Mounting

- Untuk melakukan mounting, kita bisa menggunakan perintah berikut :
- `docker container create --name namacontainer --mount "type=bind,source=folder,destination=folder,readonly" image:tag`

### Kode: Melakukan Mounting

```sh
docker container create --name mongodata --mount "type=bind,source=/Users/khannedy/data/db" --publish 27017:27017 --env MONGO_INITDB_ROOT_USERNAME=eko --env MONGO_INITDB_PASSWORD=eko mongo:latest

docker container start mongodata
```

![Kode: Melakukan Mounting](./images/docker-dasar-21.jpg)

## #16 Docker Volumme

- Fitur Bind Mounts sudah ada sejak Docker versi awal, di versi terbaru direkomendasikan menggunakan Docker Volume
- Docker Volume mirip dengan Bind Mounts, bedanya adalah terdapat management Volume, dimana kita bisa membuat Volume, melihat daftar Volume, dan menghapus Volume
- Volume sendiri bisa dianggap storage yang digunakan untuk menyimpan data, bedanya dengan Bind Mounts, pada bind mounts, data disimpan pada sistem host, sedangkan pada volume, data di manage oleh Docker

### Melihat Docker Volume

- Saat kita membuat container, dimanakah data di dalam container itu disimpan, secara default semua data container disimpan di dalam volume
- Jika kita coba melihat docker volume, kita akan lihat bahwa ada banyak volume yang sudah terbuat, walaupun kita belum pernah membuatnya sama sekali
- Kita bisa gunakan perintah berikut untuk melihat daftar volume :
- `docker volume ls`

### Kode: Melihat Volume

```sh
docker volume ls
```

![Kode: Melihat Volume](./images/docker-dasar-22.jpg)

### Membuat Volume

- Untuk membuat volume, kita bisa gunakan perintah :
- `docker volume create namavolume`

### Kode: Membuat Volume

```sh
docker volume create mongovolume
```

![Kode: Membuat Volume](./images/docker-dasar-23.jpg)

### Menghapus Volume

- Volume yang tidak digunakan oleh container bisa kita hapus, tapi jika volume digunakan oleh container, maka tidak bisa kita hapus sampai container nya di hapus
- Untuk menghapus volume, kita bisa gunakan perintah :
- `docker volume rm namavolume`

### Kode: Menghapus Volume

```sh
docker volume rm mongovolume

docker volume ls
```

![Kode: Menghapus Volume](./images/docker-dasar-24.jpg)

## #17 Container Volume

- Volume yang sudah kita buat, bisa kita gunakan di container
- Keuntungan menggunakan volume adalah, jika container kita hapus, data akan tetap aman di volume
- Cara menggunakan volume di container sama dengan menggunakan bind mount, kita bisa menggunakan parameter `--mount`, namun dengan menggunakan type volume dan source nama volume

### Kode: Container Volume

```sh
docker volume create mongodata

docker container create --name mongovolume --mount "type=mount,source=mongodata,destination=/data/db" --publish 27017:27017 --env MONGO_INITDB_ROOT_USERNAME=eko --env MONGO_INITDB_PASSWORD=eko mongo:latest

docker container start mongovolume
```

![Kode: Container Volume](./images/docker-dasar-25.jpg)

## #18 Backup Volume

- Sayangnya, sampai saat ini, tidak ada cara otomatis melakukan backup volume yang sudah kita buat
- Namun kita bisa memanfaatkan container untuk melakukan backup data yang ada di dalam volume ke dalam archive seperti zip atau `tar.gz`

### Tahapan Melakukan Backup

- Matikan container yang menggunakan volume yang ingin kita backup
- Buat container baru dengan dua mount, volume yang ingin kita backup, dan bind mount folder dari sistem host
- Lakukan backup menggunakan container dengan cara meng-archive isi volume, dan simpan di bind mount folder
- Isi file backup sekarang ada di folder sistem host
- Delete container yang kita gunakan untuk melakukan backup

### Kode: Membuat Backup Container

```sh
docker container create --name nginxbackup --mount "type=bind,source=/Users/khannedy/backup,destination=/backup" --mount "type=mount,source=mongodata,destination=/data" nginx:latest

docker container start nginxbackup

docker container exec -i -t nginxbackup /bin/bash

root@31837183/# tar cvf /backup/backup.tar.gz /data
```

![Kode: Membuat Backup Container](./images/docker-dasar-26.jpg)

### Menjalankan Container Secara Langsung

- Melakukan backup secara manual agak sedikit ribet karena kita harus start container terlebih dahulu, setelah backup, hapus container nya lagi
- Kita bisa menggunakan perintah run untuk menjalankan perintah di container dan gunakan parameter `--rm` untuk melakukan otomatis remove container setelah perintahnya selesai berjalan

### Kode: Backup Dengan Container Run

![Kode: Backup Dengan Container Run](./images/docker-dasar-27.jpg)

## #19 Restore Volume

- Setelah melakukan backup volume ke dalam file archive, kita bisa menyimpan file archive backup tersebut ke tempat yang lebih aman, misal ke cloud storage
- Sekarang kita akan coba melakukan restore data backup ke volume baru, untuk memastikan data backup yang kita lakukan tidak corrupt

### Tahapan Melakukan Restore

- Buat volume baru untuk lokasi restore data backup
- Buat container baru dengan dua mount, volume baru untuk restore backup, dan bind mount folder dari sistem host yang berisi file backup
- Lakukan restore menggunakan container dengan cara meng-extract isi backup file ke dalam volume
- Isi file backup sekarang sudah di restore ke volume
- Delete container yang kita gunakan untuk melakukan restore
- Volume baru yang berisi data backup siap digunakan oleh container baru

### Kode: Restore Backup

```sh
docker volume create mongodatabackup

docker container run --rm --name ubuntu --mount "type=bind,source=/Users/khannedy/backup,destination=/backup" --mount "type=volume,source=mongodatabackup,destination=/data" ubuntu:latest bash -c "cd /data && tar xvf /backup/backup.tar.gz --strip 1"
```

![Kode: Restore Backup](./images/docker-dasar-28.jpg)

## #20 Docker Network

- Saat kita membuat container di docker, secara default container akan saling terisolasi satu sama lain, jadi jika kita mencoba memanggil antar container, bisa dipastikan bahwa kita tidak akan bisa melakukannya
- Docker memiliki fitur Network yang bisa digunakan untuk membuat jaringan di dalam Docker
- Dengan menggunakan Network, kita bisa mengkoneksikan container dengan container lain dalam satu Network yang sama
- Jika beberapa container terdapat pada satu Network yang sama, maka secara otomatis container tersebut bisa saling berkomunikasi

### Network Driver

- Saat kita membuat Network di Docker, kita perlu menentukan driver yang ingin kita gunakan, ada banyak driver yang bisa kita gunakan, tapi kadang ada syarat sebuah driver network bisa kita gunakan.
- `bridge`, yaitu driver yang digunakan untuk membuat network secara virtual yang memungkinkan container yang terkoneksi di bridge network yang sama saling berkomunikasi
- `host`, yaitu driver yang digunakan untuk membuat network yang sama dengan sistem host. host hanya jalan di Docker Linux, tidak bisa digunakan di Mac atau Windows
- `none`, yaitu driver untuk membuat network yang tidak bisa berkomunikasi

### Melihat Network

- Untuk melihat network di Docker, kita bisa gunakan perintah :
- `docker network ls`

### Kode: Melihat Network

```sh
docker network ls
```

### Membuat Network

- Untuk membuat network baru, kita bisa menggunakan perintah :
- `docker network create --driver namadriver namanetwork`

### Kode: Membuat Network

```sh
docker network create --driver bridge contohnetwork

docker network ls
```

![Kode: Membuat Network](./images/docker-dasar-29.jpg)

### Manghapus Network

- Untuk menghapus Network, kita bisa gunakan perintah :
- `docker network rm namanetwork`
- Network tidak bisa dihapus jika sudah digunakan oleh container. Kita harus menghapus container nya terlebih dahulu dari Network

### Kode: Menghapus Network

```sh
docker network rm contohnetwork

docker network ls
```

![Kode: Menghapus Network](./images/docker-dasar-30.jpg)

## #21 Container Network

- Setelah kita membuat Network, kita bisa menambahkan container ke network
- Container yang terdapat di dalam network yang sama bisa saling berkomunikasi (tergantung jenis driver network nya)
- Container bisa mengakses container lain dengan menyebutkan hostname dari container nya, yaitu nama container nya

### Membuat Container dengan Network

- Untuk menambahkan container ke network, kita bisa menambahkan perintah `--network` ketika membuat container, misal :
- `docker container create --name namacontainer --network namanetwork image:tag`

### Kode: Membuat Container Dengan Network

```sh
docker network create mongonetwork

docker container create --name mongodb --network mongonetwork --env MONGO_INITDB_ROOT_USERNAME=eko --env MONGO_INITDB_PASSWORD=eko mongo:latest

docker container create --name mongodbexpress --network mongonetwork --publish 8081:8081 --env ME_CONFIG_MONGODB_URL="mongodb://eko:eko@mongodb:27017/" mongo-express:latest

docker container start mongodb

docker container start mongodbexpress
```

![Kode: Membuat Container Dengan Network](./images/docker-dasar-31.jpg)

### Menghapus Container dari Network

- Jika diperlukan, kita juga bisa menghapus container dari network dengan perintah :
- `docker network disconnect namanetwork namacontainer`

### Kode: Manghapus Container dari Network

```sh
docker network disconnect mongonetwork mongodb
```

### Menambah Container ke Network

- Jika containernya sudah terlanjur dibuat, kita juga bisa menambahkan container yang sudah dibuat ke network dengan perintah :
- `docker network connect namanetwork namacontainer`

### Kode: Menambahkan Container ke Network

```sh
docker network connect mongonetwork mongodb
```

## #22 Inpect

- Setelah kita men-download image, atau membuat network, volume dan container. Kadang kita ingin melihat detail dari tiap hal tersebut
- Misal kita ingin melihat detail dari image, perintah apa yang digunakan oleh image tersebut? Environment variable apa yang digunakan? Atau port apa yang digunakan?
- Misal kita juga ingin melihat detail dari container, Volume apa yang digunakan? Environment variable apa yang digunakan? Port forwarding apa yang digunakan? dan lain-lain
- Docker memiliki fitur bernama inspect, yang bisa digunakan di image, container, volume dan network
- Dengan fitur ini, kita bisa melihat detail dari tiap hal yang ada di Docker

### Menggunakan Inspect

- Untuk melihat detail dari image, gunakan : `docker image inspect namaimage`
- Untuk melihat detail dari container, gunakan : `docker container inspect `namacontainer
- Untuk melihat detail dari volume, gunakan : `docker volume inspect namavolume`
- Untuk melihat detail dari network, gunakan : `docker network inspect namanetwork`

### Kode: Menggunakan Inspect

```sh
docker image inspect redis:latest
```

![Kode: Menggunakan Inspect](./images/docker-dasar-32.jpg)

## #23 Prune

- Saat kita menggunakan Docker, kadang ada kalanya kita ingin membersihkan hal-hal yang sudah tidak digunakan lagi di Docker, misal container yang sudah di stop, image yang tidak digunakan oleh container, atau volume yang tidak digunakan oleh container
- Fitur untuk membersihkan secara otomatis di Docker bernama `prune`
- Hampir di semua perintah di Docker mendukung prune

### Perintah Prune

- Untuk menghapus semua container yang sudah stop, gunakan : docker` container prune`
- Untuk menghapus semua image yang tidak digunakan container, gunakan : `docker image prune`
- Untuk menghapus semua network yang tidak digunakan container, gunakan : `docker network prune`
- Untuk menghapus semua volume yang tidak digunakan container, gunakan : `docker volume prune`
- Atau kita bisa menggunakan satu perintah untuk menghapus container, network dan image yang sudah tidak digunakan menggunakan perintah : `docker system prune`

### Kode: Menggunakan Prune

```sh
docker image prune

docker network prune

docker volume prune
```

![Kode: Menggunakan Prune](./images/docker-dasar-33.jpg)

## #24 Materi Selanjutnya

- Docker Dockerfile
- Docker Compose

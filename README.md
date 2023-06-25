![alt text](https://github.com/RakhaFe21/HackathonProject/blob/master/arsitektur%20otomatisasi.jpeg?raw=true)

# HackathonProject

Nama saya Rakha Febryza Rasendriya. Asal sekolah SMK Telkom Purwokerto. Disini saya mengambil project tema 1

# Introduction
1. Langkah pertama yang saya lakukan adalah saya membuat roadmap alur kerja terlebih dahulu secara oret-oretan pada sebuah buku.
2. Saya menemukan sebuah arsitektur automation kurang lebih seperti pada gambar berikut:
![image.png]( {https://github.com/RakhaFe21/HackathonProject/blob/master/arsitektur%20otomatisasi.jpeg} )

# Step by step
1. Sesuai dengan gambar diatas, langkah pertama saya melakukan clonning source code file yang telah di push oleh tim developer yaitu pada app1:       https://github.com/islamyakin/semesta-app1 dan app2 : https://github.com/islamyakin/semesta-app2 
2. Setelah saya cloning kedalam repository local saya, selanjutnya saya membuat dockerfile untuk membuat image dari source code yang telah diberikan menggunakan Visual Studio Code.
3. Selanjutnya yaitu langkah building pada Jenkins. Disini saya menggunakan layanan free tier EC2 dari AWS dengan membuat ubuntu 22.04 sebagai server Jenkins dan server docker
4. Install plugin yang dibutuhkan seperti plugin git,go, publish overssh,discord notifier, dsb.
5. Setting publish over ssh seperti pada gambar berikut:
![image.png]( {https://github.com/RakhaFe21/HackathonProject/blob/master/Screenshot%20from%202023-06-25%2008-08-20.png} )
6. Create new job, disini saya membuat 2 job terpisah dengan nama APP1 dan APP2
7. Job APP1:
   -Description : untuk deploy app1
   - Source code management : git
   - Repository URL :
     
           https://github.com/RakhaFe21/semesta-app1.git
   - Branch : */master
   - Build Triggers : Githubhook trigger for GITscm polling (check). Disini saya menambahkan webhhok di github dengan ip Jenkins dengan tujuan ketika nanti developers commit perubahan di repository, maka Jenkins akan otomatis membuat build baru.
   - Build environtment : set up Go programming language tools.
     
           Go version : 1.20.5
   - Post Build Actions
     
        -Discord notifier: webhook url
   ![image.png]( {https://github.com/RakhaFe21/HackathonProject/blob/master/discordchat.png} ) 

     Bertujuan sebagai sumber informasi kepada tim apabila salah satu ada yang memulai build
   
       -Send build artifacts over SSH
   
	        Name : docker_host1

            	Transfer set
                 Source file : **/*.go
                 Remove prefix : *kosong
                 Remote directory: /opt/semesta-app1
                 EXEC command:
                 cd /opt/semesta-app1/
                 docker build --tag go1 .
                 docker network create go-network
                 docker run -d -p 3000:3000 --name go1 --network go-network go1
            		
8. Untuk Job app2, saya melakukan copy terhadap app1. Berikut konfigurasi job app2:
   
     Job APP2:
   
       Description : untuk deploy app2
       Source code management : git
       Repository URL : https://github.com/RakhaFe21/semesta-app2.git
       Branch : */main
   
       -Build Triggers : Githubhook trigger for GITscm polling (check).
   
![image.png]( {link gambar} )

       Webhook url menuju pada repository ini
   
       -Post Build Actions
   
         Discord notifier: webhook url
         
![image.png]( {link gambar} )

         Disini saya membuat channel berbeda dengan app1 agar user dapat dengan mudah mengidentifikasi build
   
         Send build artifacts over SSH
   
             Name : docker_host2

            	Transfer set
               Source file : **/*.go
               Remove prefix : *kosong
               Remote directory: /opt/semesta-app2
               EXEC command:
                 cd /opt/semesta-app2/
                 docker build --tag go2 .
                 docker network create go-network
                 docker run -d -p 3000:3000 --name go2 --network go-network go2

10. Triger build kedua job tersebut dan hasilnya :

    a. APP1
    
![image.png]( {link gambar} )

    b. APP2

![image.png]( {link gambar} )

12. Pemanggilan melalui browser 
    a. APP1

![image.png]( {link gambar} )

    b. APP2

![image.png]( {link gambar} )
![image.png]( {link gambar} )

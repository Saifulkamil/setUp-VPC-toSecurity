# Setup-VPC-to-Security

## setup vpc (yang perlu diisi)
    - isi kan nama
    - centang custom 
    - pilih region 
    - isi kan ipv4 range yaitu subnet nya sontoh 10.2.0.0/28
    create

## firewall ssh ke server main
buat untuk ssh dari public // (yang perlu di isi)

    - name
    - pilih network sesuai yang di buat sebelumnya
    - pilih spesifik target => target tags harus sama dengan di vm Instance misal web
    - - ipv4 isikan network public 
    - centang tcp -22, 9000, 5000
    create

## buat untuk ssh ke server internal
   
    - name
    - pilih network sesuai yang di buat sebelumnya
    - pilih spesifik target => target tags harus sama dengan di vm Instance misal web
    - - ipv4 isikan network public 
    - centang tcp - 0-65535, UDP -0-65535 , other isi kan icmp 
    create


## membuat secret key pada laptop
   
    - ssh-keygen -t rsa -b 4096 -C <komentar keynya terserah ini perlu untuk indentitas key>
    - selanjutnya atur foledernya, key masuk di buat di folder mana contoh E:GCP\key\sepolkeyproject\sepolkey (terserah di mana tapi harus ingat)
    - masukan password nya 2x
    - ada 2 file yang pertama privat dan publik 
    - public di masukan ke server target yang mau di ssh
    - privat di gunakan di konmputer sumber
    ssh-keygen -t rsa -b 4096 -C sepolkey


## membuat secret key server main (biar bisa ssh ke server internal)
   
    - ssh-keygen -t rsa -b 4096 -C <komentar keynya terserah ini perlu untuk indentitas key>
    - selanjutnya atur foledernya, key masuk di buat di folder mana contoh E:GCP\key\sepolkeyproject\sepolkey (terserah di mana tapi harus ingat)
    - masukan password nya 2x
    - ada 2 file yang pertama privat dan publik 
    - public di masukan ke server target yang mau di ssh
    - privat di gunakan di konmputer sumber
    ssh-keygen -t rsa -b 4096 -C sepolkeyinternal


## setting key pada compute engine
   
    - pilih di compute engine 
        - scrol pilih settig
        - klik metadata
        - pilih ssh key
        - add key/ edit
        - masukan public key yang sudah dibuat sebelumnya
        -save



## membuat server main di compute engine(yang harus di isi)
   
    - nama
    -region (pilih sesuai vpc)
    - pilih type mesin
    -klik advance options
        - network
        - isi kan tags yang sesuai dengan firewall
        - network interface
            -network pilih vpc yang dibuat sebelunya
            - done
    create

 
## membuat server internal di compute engine(yang harus di isi)
   
    - nama
    -region (pilih sesuai vpc)
    - pilih type mesin
    -klik advance options
        - network
        - isi kan tags yang sesuai dengan firewall
        - network interface
            -network pilih vpc yang dibuat sebelunya
            - ipv4 external => none
            - done
    - klik security
    - klik manage access
     - + add Item
     masukan publik key yang di buat di server mainsupaya bisa ssh dari server main ke server internal
    create


## cara ssh di cmd
   
    - ssh -i E:GCP\key\sepolkeyproject\sepolkey <nama key/comment contoh sepolkey>@<ipv4 public server main>
    - contoh ssh -i E:GCP\key\sepolkeyproject\sepolkey sepolkey@34.121.159.255

## cara ssh di terminal linux
   
    - ssh -i /home/sepolkey/.ssh/id_rsa_sepolkey <nama key/comment contoh sepolkey>@<ipv4 public server main>
    - ssh -i /home/sepolkey/.ssh/id_rsa_sepolkey sepolkeyinternal@10.128.0.9
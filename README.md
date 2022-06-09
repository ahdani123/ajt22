# Tugas Akhir Arsitektur Jaringan Terkini

## Tugas 1: Pembuatan Instance Ubuntu, Instalasi Mininet, Ryu, Flowmanager

Screenshot :

![image](https://user-images.githubusercontent.com/80938624/172859080-dcf3140b-7aa5-4ef4-ba65-a14ff131da61.png) <br/> &nbsp;
OS Images dan Instance Type <br/>

![image](https://user-images.githubusercontent.com/80938624/172859181-cdce2d34-b9a0-43d0-ad49-2350d0211101.png) <br/> Network Settings<br/>

![image](https://user-images.githubusercontent.com/80938624/172859121-ec6bda2c-0b6b-4446-84df-0077b6475ef1.png) <br/> Storage dan Tag<br/>

![image](https://user-images.githubusercontent.com/80938624/172858854-1711013a-f373-48c7-a4cb-40aca3d38d23.png) <br/> Key Pair
<br/><br/>

## Tugas 2: Custom Topology Mininet

Langkah-langkah percobaan:<br/>
Menjalankan mininet dengan custom topo yang telah dibuat, custom3sw3h.py
```
sudo mn --custom mininet/custom/custom3sw3h.py --topo mytopo --mac --arp
```
Secara manual menulis flow pada masing-masing switch
```
mininet> sh ovs-ofctl add-flow s1 -O OpenFlow13 "in_port=1,action=output:2,3,4"
mininet> sh ovs-ofctl add-flow s1 -O OpenFlow13 "in_port=2,action=output:1,3,4"
mininet> sh ovs-ofctl add-flow s1 -O OpenFlow13 "in_port=3,action=output:1,2,4"
mininet> sh ovs-ofctl add-flow s1 -O OpenFlow13 "in_port=4,action=output:1,2,3"
```
begitu juga untuk s2 dan s3 <br/>

Cek Konektivitas 
```
mininet> pingall
mininet> h2 ping -c2 h5
```

Screenshot : <br/>

![image](https://user-images.githubusercontent.com/80938624/172867807-c97ef483-0a9a-4f55-a314-ed2b2ba63ad1.png) <br/>
menjalankan custom topo<br/>

![image](https://user-images.githubusercontent.com/80938624/172868213-1fcd2d63-e0b9-4d01-80eb-485af5f4b49d.png) <br/>
cek link dan nodes<br/>

![image](https://user-images.githubusercontent.com/80938624/172869712-87fbf928-3ea7-463f-8997-5153cf873b12.png) <br/>
membuat flow<br/>

![image](https://user-images.githubusercontent.com/80938624/172869939-97c0c0e2-2931-4e9a-9fc7-ae0210af5818.png) <br/>
pingall (ping ke semua host)<br/>

![image](https://user-images.githubusercontent.com/80938624/172870631-90a78165-a4e1-45d9-bbee-cf11405bbf5c.png) <br/>
![image](https://user-images.githubusercontent.com/80938624/172870579-f33b9599-87cd-4f76-9354-08fe75bebbd8.png) <br/>
ping h2 ke h5 (terjadi duplikat)
<br/><br/>

## Tugas 3: Aplikasi Ryu Load Balancer

Mulai mininet dengan topologi standar
```
sudo mn --controller=remote --topo single,4 --mac
```

Tentukan virtual ip server: 10.0.0.100
h1 sebagai client web untuk mengakses bisa gunakan:
```
mininet> h1 curl 10.0.0.100
```

Algoritme untuk berganti-ganti ke server web h2 - h4 cukup gunakan round-robin

h2 s/d h4 sebagai web server: semisal dijalankan dengan perintah dalam console
```
mininet> h2 python3 -m http.server 80 & 
```
Hal yg sama untuk h3 dan h4 <br/> <br/>

Screenshot:<br/>
![image](https://user-images.githubusercontent.com/80938624/172874218-0cb4d7be-ecd2-43c3-bc3f-8adcaa427c69.png) <br/>
memulai mininet dan menjalankan ryu llb.py<br/>

![image](https://user-images.githubusercontent.com/80938624/172874272-5ed85eee-bb8f-470b-a4c5-7f89856ca385.png) <br/>
h2 s/d h4 sebagai web server<br/>

![image](https://user-images.githubusercontent.com/80938624/172874306-453de119-06f4-4cf8-a9f9-d6e35b0021a4.png) <br/>
h1 sebagai client web
<br/><br/>

## Tugas 4

Pada Terminal Console 1 jalankan:
```
ryu-manager --observe-links dijkstra_Ryu_controller.py
```
Pada Terminal Console 2 jalankan:
```
sudo python3 topo-spf_lab.py
```
Lanjutkan dengan cek semua konektivitas, misalnya
```
$ mininet> h1 ping -c 4 h4
```
```
$ mininet> h5 ping -c 4 h6
```
<br/>
Screenshot: <br/>

![image](https://user-images.githubusercontent.com/80938624/172874443-5712fd9c-af13-4b6f-b5e9-309f39ff16f0.png)<br/>
memulai mininet dan ryu<br/>

![image](https://user-images.githubusercontent.com/80938624/172874505-5ecc861e-a4f3-4ce3-920c-444ddd19f998.png)<br/>
pingall<br/>

![image](https://user-images.githubusercontent.com/80938624/172874543-a5380abf-136c-49e8-9511-53676f02758b.png)<br/>
ping h5 ke h6<br/>

![image](https://user-images.githubusercontent.com/80938624/172874585-4e274b6a-5b37-4ce3-a4f4-baca0b696e7f.png)<br/>
ping h1 ke h4


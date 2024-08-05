# GNS3 ile VLAN ve Trunk Portları İçeren Ağ Simülasyonu

Bu projede, GNS3 kullanarak çeşitli VLAN'lar, trunk portları, Fortigate güvenlik duvarı, yönlendirici (router), anahtarlar (switch) ve bilgisayarları içeren karmaşık bir ağ yapılandırması oluşturdum. Bu topoloji, ağ trafiğinin yönetimini ve güvenliğini sağlamayı amaçlayan bir senaryoyu temsil eder.

## Genel Bakış

Ağ yapımda, farklı VLAN'ları içeren bir yapı oluşturup, VLAN'lar arasındaki trafiği yönlendiren çeşitli cihazlar kullandım. Trunk portları, VLAN'lar arasında veri taşıyan önemli bileşenlerdir. İşte topolojinin detayları:

![1 odev-Topoloji](https://github.com/user-attachments/assets/fc2dc948-d814-40eb-b25f-d38e5f5c85ff)


### Ağ Elemanları ve Yapılandırmalar

#### 1. Fortigate Güvenlik Duvarı (FORTIGATE-1)



![forrr](https://github.com/user-attachments/assets/f2a2df33-a41c-49c4-aab2-6b3d0f3f2231)



IP Adresi: 100.0.0.1/24 (Management VLAN10)
VLAN'lar:
vlan210: 10.0.0.254/24
vlan220: 20.0.0.254/24
vlan230: 30.0.0.254/24

Trunk Port: Fortigate ile yönlendirici arasındaki bağlantı üzerinden tüm VLAN trafiği taşınır.

#### 2. Yönlendirici (Router)



![3 L3-ip_route](https://github.com/user-attachments/assets/3e70f846-422e-4bde-96a8-4e5216930534)




IP Adresi: Çeşitli VLAN'lar için yönlendirme yapar.
Trunk Portlar: Yönlendirici ve switch'ler arasındaki trunk portlar, farklı VLAN'lardan gelen trafiği taşır.


#### 3. Anahtarlar (Switch'ler)

Switch1: PC1 (10.0.0.10/24 - VLAN 210)
Switch2: PC3 (30.0.0.30/24 - VLAN 230)
Switch3: PC2 (20.0.0.20/24 - VLAN 220)
Switch4: PC4 (40.0.0.10/24 - VLAN 310)
Switch5: PC5 (50.0.0.10/24 - VLAN 320)
Switch6: PC6 (60.0.0.10/24 - VLAN 330)



![4 L3-port_trunk](https://github.com/user-attachments/assets/03f80fde-b19a-4331-abcd-f5516fd8d148)



#### 4. Bilgisayarlar (PC'ler)

PC1: 10.0.0.10/24 (VLAN 210)
PC2: 20.0.0.20/24 (VLAN 220)
PC3: 30.0.0.30/24 (VLAN 230)
PC4: 40.0.0.10/24 (VLAN 310)
PC5: 50.0.0.10/24 (VLAN 320)
PC6: 60.0.0.10/24 (VLAN 330)


#### 5. NAT Cihazı (NAT1)

NAT (Network Address Translation) işlevi görür ve dış ağlara erişimi sağlar.


VLAN'lar:
vlan100: 100.0.0.254/24
vlan310: 40.0.0.254/24
vlan320: 50.0.0.254/24
vlan330: 60.0.0.254/24


![2 L3-ip_interface](https://github.com/user-attachments/assets/c5e93377-d602-4009-808e-24fe9a61dc6b)



#### 6. Sunucular (Servers)

SERVER3-1: Yönetim VLAN'ında (Management VLAN10 - 100.0.0.10/24) yer alır.

#### 7. DevOps Cihazları

###### DevOps_UBN1-1: 192.168.2.155/24 (Farklı bir ağda yer alıyor)


Static IP ataması yaptım (Burada önemli olan noktalardan biri gateway!):



![11 ubuntu1-ip_config](https://github.com/user-attachments/assets/220b7e2b-442b-473b-b201-222638bfd29c)




###### DevOps_UBN2-1: 60.0.0.12/24 (VLAN 330)


Aynı şekilde bu cihaza da static IP ataması yaptım (yine aynı şekilde en önemli nokta gateway!):



![12 ubuntu2-ip_config](https://github.com/user-attachments/assets/045b283e-3bc8-4186-ab6a-508c278db16f)




## Fortigate Yönetimi

Bu projede Fortigate güvenlik duvarını yönetmek için Windows Server 2019 kullanarak aşağıdaki adımları gerçekleştirdim:


### 1. Network Interface Ayarları

Fortigate güvenlik duvarının network interface ayarlarını yapılandırdım. Bu adım, Fortigate'in farklı VLAN'lara ve ağlara doğru şekilde bağlanmasını sağlar.

Adımlar:

Fortigate arayüzüne giriş yaptım.
Network > Interfaces menüsüne giderek ağ arayüzlerini görüntüledim.
Her bir arayüz için uygun IP adreslerini ve subnet maskelerini tanımladım.
Her arayüzün VLAN yapılandırmalarını kontrol ettim ve gerekli değişiklikleri yaptım.


![5 fortigate-network_interfaces+subInterfaces](https://github.com/user-attachments/assets/34cacae6-22d2-4638-87de-ae770f5b6545)



### 2. Static Routing Atamaları
Fortigate üzerinde static routing yapılandırmasını tamamladım. Bu adım, VLAN'lar arasında doğru yönlendirme yapılmasını sağlar.

Adımlar:

Network > Static Routes menüsüne giderek static routing ayarlarına eriştim.
Yeni bir route ekleyerek hedef ağ, gateway ve diğer gerekli bilgileri belirledim.
Routing tablolarını kontrol ederek atamaların doğru yapıldığını doğruladım.


![6 fortigate-static_routes](https://github.com/user-attachments/assets/9ad8b620-4016-4cd0-8604-4d74fd9754b3)



### 3. Fortigate Kuralları Düzenleme
Fortigate güvenlik duvarı üzerinde gerekli kuralları oluşturdum ve düzenledim. Bu adım, ağ güvenliğini sağlamak ve trafiği kontrol etmek için önemlidir.

Adımlar:

Policy & Objects > IPv4 Policy menüsüne giderek mevcut güvenlik politikalarını inceledim.
Yeni kurallar ekleyerek veya mevcut kuralları düzenleyerek trafik izinlerini ve yasaklarını belirledim.
Kuralların doğru şekilde uygulandığını test etmek için ilgili trafik akışlarını kontrol ettim.



![7 fortigate-all_rules](https://github.com/user-attachments/assets/c4b9c601-d304-4e57-b36c-da357d55dbd9)



### 4. Virtual IP (VIP) Oluşturma

SSH erişimi için DevOps_UBN2 cihazı için bir Virtual IP (VIP) oluşturdum.

VIP, dış ağdan gelen isteklerin iç ağdaki belirli bir cihaza yönlendirilmesini sağlar.

Adımlar:

Policy & Objects > Virtual IPs menüsüne giderek yeni bir VIP ekledim.
VIP için uygun bir ad belirledim ve dış IP ile iç IP arasındaki ilişkiyi tanımladım.
VIP ayarlarını kaydettim ve ilgili kuralları güncelleyerek VIP'yi kullanıma açtım.



![8 fortigate-ssh_virtual_IP](https://github.com/user-attachments/assets/505c0329-7eb5-49c3-b70b-c9318392e747)




Yeni kurallar ekleyerek veya mevcut kuralları düzenleyerek trafik izinlerini ve yasaklarını belirledim:



![10 fortigate-all_rules1](https://github.com/user-attachments/assets/e922641d-0c2a-4d8d-908e-c10d2528389b)




NOT: Fortigate eğitim amaçlı kullandığım için ücretsiz versiyonunda yalnızca 5 kurala izin veriyor.


### 5.  DevOps_UBN2 cihazına dış ağdan SSH bağlantısı başarılı bir şekilde gerçekleştirdim:



![13 ssh_ok](https://github.com/user-attachments/assets/443c96e6-6978-4475-a5a3-f6b2f8c644be)




## Ağ İletişimi ve Trafik Yönetimi 

VLAN'lar Arası Trafik: L3 yönlendirici ve Fortigate, VLAN'lar arası trafiği yönlendirir.  

Dış Ağ Erişimi: NAT1 cihazı aracılığıyla dış ağlara erişim sağlanır.

Güvenlik Duvarı: Fortigate, trafiği güvenli hale getirir ve zararlı trafiği filtreler.

Trunk Portlar: Tüm switch'ler ile yönlendirici arasındaki trunk portlar, tüm VLAN'lar üzerinden gelen trafiği yönlendiriciye taşır.


## Topolojinin Amacı

Bu topoloji, çeşitli VLAN'lar arasındaki trafiği yönetmek ve güvenliğini sağlamak için tasarlanmıştır. Fortigate güvenlik duvarı ve NAT cihazı, güvenliği artırırken, L3 cihazı ağın etkin bir şekilde çalışmasını sağlar.
Ayrıca birden çok VLAN'ın bulunduğu ve her VLAN'ın belirli bir amacı veya kullanıcı grubunu temsil ettiği karmaşık bir ağ yapısını simüle etmek için kullanılabilir. Örneğin, bu topoloji büyük bir ofis ağı,
veri merkezi veya eğitim amaçlı bir laboratuvar ortamı olabilir.


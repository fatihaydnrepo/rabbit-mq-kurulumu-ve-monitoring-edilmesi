# RabbitMQ Kurulumu Ve Zabbix İle Monitoring Edilmesi.

# RabbitMQ nedir ?
RabbitMQ bir mesaj kuyruğu sistemidir.  Amacı herhangi bir kaynaktan alınan bir mesajın, bir başka kaynağa sırası geldiği anda iletilmesidir.  Ama burada yapılacak işler bir sıraya alınmaktadır.  RabbitMQ çoklu işletim sistemine destek vermesi ve açık kaynak kodlu olması da en büyük tercih sebeplerinden birisidir. Bazı işlemlerin anlık yapılmasına ihtiyaç yoktur. Örnek vermek istenir ise sisteme yeni bir haber girildiğinde, ya da var olan bir haberin güncellenmesi anında cache’in düşürülmesidir.

## Gereksinimler :

•	Ubuntu Server 20.04 LTS

## RabbitMQ server kurulumu
Öncelikle paket listelerini güncellemeliyiz. Bunun için şu kodları kullanıyoruz
> -Sudo apt-get update

Ardından rabbitmq-server’ı indiriyoruz.
>-sudo apt-get install rabbitmq-server

ve servisin durumunu kontrol ediyoruz.
>-sudo rabbitmqctl status

RabbitMQ mesajlarını görüntüleyebilmek için bir plugin geliştirilmiştir. Bu plugin sayesinde web arayüzü üzerinden mesajlaşma trafiğini izleyebilirsiniz. Plugin adı **rabbitmq_management** şeklindedir. Bu plugin aktif edilmeden kullanılamaz. Aktif etmek için kullanılacak komut:

>-**cd /usr/lib/rabbitmq/lib/rabbitmq_server-3.8.2/sbin** adresine giderek **“ rabbitmq-plugins enable rabbitmq_management”** komutunu çalıştırıyoruz.

Şimdi RabbitMQ servisini restart edelim.

>-sudo service rabbitmq-server restart 

## Web Arayüzü
RabbitMQ kurduğunuz ortama aşağıdaki linkten erişebilirsiniz.
>http://ip_adresiniz:15672

<img src="https://github.com/ReptilianusBileciktus/zabbix-rabbitmq-kurulumu-ve-monitoring-/blob/3bd7fc125ea42b4c2f8271a17fac9f652a4b5d3b/1.png" width="auto">

Ardından yeni bir kullanıcı oluşturmalıyız Bunun için şu komutu kullanacağız:
>-rabbitmqctl add_user juda juda

 juda kullanıcı adında ve juda parolası ile giriş yapabileceğimiz bir kullanıcı oluşturduk.
Şimdi bu kullanıcıyı Administrator olarak etiketleyip tüm yetkilere sahip olmasını sağlayalım.
>-rabbitmqctl set_user_tags juda administrator


>-rabbitmqctl set_permissions -p / juda "." "." ".*"

juda yerine kullanıcı adınızı girmeniz gerekiyor ardından kullanıcınız ile giriş yapabilirsiniz.

<img src="https://github.com/ReptilianusBileciktus/zabbix-rabbitmq-kurulumu-ve-monitoring-/blob/3bd7fc125ea42b4c2f8271a17fac9f652a4b5d3b/2.png" width="auto">



## Zabbix İle RabbitMQ Monitoring İşlemi

Şimdi Zabbix ile RabbitMQ uygulamasını monitoring edeceğiz. Zabbix ile RabbitMQ uygulamasının tüm detaylarını izleyip anlık bildirim alabileceğiz.

## Export a file

You can export the current file by clicking **Export to disk** in the menu. You can choose to export the file as plain Markdown, as HTML using a Handlebars template or as a PDF.


# Gereksinimler
•	Aktif RabbitMQ Kurulumu

## RabbitMQ Monitoring

Zabbix arayüzünden yeni bir host ekleme işlem yapmalıyız. Aşağıdaki adımları izlemelisiniz.

<img src="https://github.com/ReptilianusBileciktus/zabbix-rabbitmq-kurulumu-ve-monitoring-/blob/590b72e7df54132c6de7d95b7757ff8a48d19247/3.jpg" width="auto">

Burada ilgili template'leri seçin ve agent kısmına RabbitMQ adresinizi yazmalısınız . Buradaki hostname ifadesini nereden aldığımı bir sonraki görselde görebilirsiniz:

<img src="https://github.com/ReptilianusBileciktus/zabbix-rabbitmq-kurulumu-ve-monitoring-/blob/3bd7fc125ea42b4c2f8271a17fac9f652a4b5d3b/4.png" width="auto">

RabbitMQ arayüzünden giriş yaptıktan sonra sağ tarafta cluster name yazmaktadır. Bir önceki görselde hostname kısmına rabbit@ ifadesinden sonraki ismi yazmalısınız.

<img src="https://github.com/ReptilianusBileciktus/zabbix-rabbitmq-kurulumu-ve-monitoring-/blob/3bd7fc125ea42b4c2f8271a17fac9f652a4b5d3b/5.png" width="auto">

Ardından makrolarda user ve password girmelisiniz ve cluster name olarak az önceki görselde gördüğünüz rabbit@ ifadesindeki rabbit ifadesini  yazmalısınız. 

<img src="https://github.com/ReptilianusBileciktus/zabbix-rabbitmq-kurulumu-ve-monitoring-/blob/3bd7fc125ea42b4c2f8271a17fac9f652a4b5d3b/6.png" width="auto">

Ardından host eklenmiş olacaktır ve belirli bir aradan sonra item'ler çekilecektir.

<img src="https://github.com/ReptilianusBileciktus/zabbix-rabbitmq-kurulumu-ve-monitoring-/blob/3bd7fc125ea42b4c2f8271a17fac9f652a4b5d3b/7.png" width="auto">

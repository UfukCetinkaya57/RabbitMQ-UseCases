# RabbitMQ Watermark Uygulaması

Bu proje, RabbitMQ kullanarak resimlere filigran ekleme işlemi gerçekleştiren bir .NET 8.0 uygulamasıdır.

## Kullanılan Teknolojiler
- .NET 8.0
- RabbitMQ
- Docker
- Entity Framework Core (In-Memory Database)
- ASP.NET Core MVC

## Başlangıç

### 1. RabbitMQ'yu Docker ile Çalıştırma

```
docker run -d -p 5672:5672 -p 15672:15672 --name rabbitmqcontainer rabbitmq:4.0-management
```
Bu komut RabbitMQ'yu başlatır. 5672 portu iletişim için, 15672 portu ise yönetim arayüzü için kullanılır.

### 2. Projeyi Çalıştırma

1. Repository'yi klonlayın:
```
git clone https://github.com/your-repo/rabbitmq-watermark-app.git
```
2. Uygulamayı çalıştırın:
```
dotnet restore
dotnet build
dotnet run
```
Uygulama çalıştıktan sonra `https://localhost:7231` adresinden erişilebilir ve aksiyon için `https://localhost:7231/products` urli kullanılabilir.


# RabbitMQ Create Excel Projesi

Bu proje, RabbitMQ kullanarak dosya oluşturma servisi sağlar ve Docker ile RabbitMQ’yu çalıştırarak mesajları kuyruğa ekler. Ayrıca AdventureWorks2022 veritabanı kullanılarak projeyi veritabanı ile entegre çalıştırabilirsiniz.

## Özellikler

- RabbitMQ üzerinden mesaj kuyruğu yönetimi.
- Docker ile RabbitMQ’nun çalıştırılması.
- AdventureWorks2022 veritabanı ile bağlantı.
- Excel dosyalarının kuyruktaki verilere göre oluşturulması.

## Gereksinimler

- Docker
- .NET 8 veya üstü
- RabbitMQ.Client kütüphanesi
- AdventureWorks2022 veritabanı (bak dosyasını indirip SQL Server’a eklemeniz gerekmektedir)

## Kurulum ve Çalıştırma

### 1. RabbitMQ’nun Docker ile Çalıştırılması

RabbitMQ’yu Docker üzerinden çalıştırmak için aşağıdaki adımları izleyin. Docker yüklü olduğundan emin olun.

```
docker run -d -p 5672:5672 -p 15672:15672 --name rabbitmqcontainer rabbitmq:4.0-management
```


Bu komut RabbitMQ’yu 5672 (AMQP) ve 15672 (yönetim arayüzü) portlarında çalıştırır. Yönetim paneline erişmek için tarayıcınızda `http://localhost:15672` adresini kullanabilirsiniz. Varsayılan kullanıcı adı ve şifre `guest` olacaktır.

### 2. AdventureWorks2022 Veritabanının Eklenmesi

1. AdventureWorks2022.bak dosyasını indirin.
2. SQL Server Management Studio (SSMS) ile SQL Server’a bağlanın.
3. Yedek dosyasını (bak) geri yükleyin.
4. Projede veritabanı bağlantısı için ilgili connection string’i ayarlayın.

### 3. Projenin Çalıştırılması

1. RabbitMQ.ExcelCreate projesinde gerekli migration işlemini yapmak için: Package Manager Console açıp sonrasında sırasıyla
```
add-migration initial
update-database
```
komutlarını kullanmanız gerekmektedir. Sonrasında RabbitMQ.ExcelCreate projesinin ihtiyacı olduğu veritabınını başarıyla yüklenmiş olmalıdır.

2. Gerekli bağımlılıkları yüklemek için aşağıdaki komutu kullanın:

```
dotnet restore
```

Projede iki ana servis bulunmaktadır: FileCreateWorkerService ve RabbitMQ.ExcelCreate. Normal şartlarda her iki servisin aynı anda çalıştırılması gerekmektedir. Ancak, ilk kez çalıştırırken aşağıdaki adımları takip etmeniz gerekir:

Öncelikle RabbitMQ.ExcelCreate projesini çalıştırın:
Bu projeyi çalıştırdıktan sonra ilgili aksyiona giderek tetiklemeniz gerekiyor. RabbitMQ'da gerekli olan kuyruğu oluşturacak ve Excel dosyasının kuyruğa alınmasını sağlayacaktır. Eğer bu adımı atlar ve direkt olarak FileCreateWorkerService projesini çalıştırırsanız, ilgili kuyruk oluşturulmadığı için uygulama hata verecektir.

Sonrasında iki projeyi aynı anda çalıştırın:
İlk adım tamamlandıktan sonra hem RabbitMQ.ExcelCreate hem de FileCreateWorkerService projelerini aynı anda çalıştırmanız gerekecek. Bu aşamada FileCreateWorkerService, RabbitMQ kuyruğunda bekleyen Excel dosyalarını işleyerek çalışacaktır.


2. Projeyi başlatmak için:

```
dotnet run
```


Bu adımlardan sonra RabbitMQ kuyruğu çalışacak ve mesajları işleyebileceksiniz.

## Kullanılan Teknolojiler

- RabbitMQ
- Docker
- SQL Server
- .NET 8
- RabbitMQ.Client Kütüphanesi
- Entity Framework Core

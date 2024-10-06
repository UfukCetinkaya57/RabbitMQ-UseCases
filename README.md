# RabbitMQ Watermark Uygulaması

Bu proje, RabbitMQ kullanarak resimlere filigran ekleme işlemi gerçekleştiren bir .NET 6.0 uygulamasıdır.

## Kullanılan Teknolojiler
- .NET 6.0
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



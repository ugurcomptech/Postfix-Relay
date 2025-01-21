# Postfix Relay Yapılandırması

Bu döküman, Postfix e-posta sunucusunda relayhost yapılandırmasını nasıl yapacağınızı ve e-postalarınızı farklı bir Postfix sunucusu üzerinden nasıl yönlendireceğinizi adım adım açıklamaktadır. Relay ayarları, özellikle şirket içi e-posta trafiğini yönetmek, dış sunucular üzerinden e-posta çıkışı sağlamak veya yük dengelemesi gibi senaryolarda kullanışlıdır.


## Gereksinimler
- 2 Adet SMTP sunucusu (Postfix)


## Relay Tanımı

İlk öncelikle main mail sunucumuz üzerinde `/etc/postfix/main.cf` yazıyoruz ve aşağıdaki gibi yapılandırıyoruz:

```
#Relay
relayhost = [sunucu_ip]:25
```

Kaydedip çıkıyoruz ve postfixi yeniden başlatıyoruz.

```
systemctl restart postfix
```

## Relay Sunucu Üzerinde Yapılandırma

Aynı şekilde postfixin yapılandırma dosyasına girip aşağıdaki gibi yapılandırıyoruz:

```
mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128 1.1.1.1 # main mail server ip
default_destination_rate_delay = 100  # Hedef sunucuya her e-posta gönderimi arasındaki gecikme süresi (saniye cinsinden). Trafik kontrolü sağlar.
default_destination_concurrency_limit = 100  # Aynı anda bir hedef sunucuya yapılacak maksimum eşzamanlı bağlantı sayısı. Bağlantı limiti belirler.
```

Kaydedip çıkıyoruz ve postfixi yeniden başlatıyoruz.

```
systemctl restart postfix
```

Başarılı bir şekilde relay yapılandırmasını tamamladık. Testlerinizi gerçekleştirebilirsiniz.

## Önemli

Relay yapılandırması, bir Postfix sunucusundan gelen e-postaların farklı bir Postfix sunucusu üzerinden yönlendirilmesini sağlar. Bu yapılandırma, merkezi e-posta yönetimi, IP itibarının korunması, yük dengeleme, gelişmiş güvenlik ve spam koruması gibi avantajlar sunar. Ayrıca, SMTP bağlantısının doğrudan yapılmasının kısıtlandığı ağlarda, relay sunucusu bu kısıtlamaları aşmak için kullanılabilir. Özellikle yüksek hacimli e-posta trafiği olan sistemlerde, e-postaları birden fazla sunucu arasında dağıtarak performansı artırmak ve e-posta trafiğini güvenli hale getirmek amacıyla relay kullanımı tercih edilir.


## Kaynaklar

- [Postfix SMTP relay and access control](https://www.postfix.org/SMTPD_ACCESS_README.html)


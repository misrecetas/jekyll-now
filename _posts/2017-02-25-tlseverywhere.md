---
layout: post
title: Pasar web a HTTPS en 2 minutos
published: true
---

Muchas tiendas online, compañías ferroviarias, compañías eléctricas y otras webs que trabajan con datos personales siguen funcionando sin cifrado HTTPS, a pesar de las recomendaciones que dan organismos públicos como la AEPD y la [OSI](https://www.osi.es/es/actualidad/blog/2012/03/26/compra-por-el-movil-de-forma-segura) desde hace años.

Ahora tener HTTPS en una web y que muestre el candadito en la barra de direcciones de los navegadores sin ningún aviso de precaución no tiene por qué suponer un gasto, los certificados de Let's Encrypt son gratuitos y el proceso para crearlos, instalarlos y renovarlos supera en sencillez a cualquier otro sistema.

* Muchos servicios de [alojamiento](https://community.letsencrypt.org/t/web-hosting-who-support-lets-encrypt/6920) que no dan acceso total al usuario dan la posibilidad de utilizar los certificados de Let's Encrypt a golpe de ratón.

Para crear los certificados y configurar un servidor web Nginx en Raspbian Jessie podemos utilizar los paquetes de Certbot que hay en el repo de Raspbian testing, siguiendo estos pasos en una terminal:


```sudo su -```

```echo "deb http://mirrordirector.raspbian.org/raspbian/ stretch main contrib non-free rpi" >> /etc/apt/sources.list.d/stretch.list```

```echo -e "Package:  *\nPin:  release n=jessie\nPin-Priority:  600" >> /etc/apt/preferences.d/preferences```

```apt-get update && apt-get install -t stretch python-certbot-nginx```

```tar zcvf backup_conf_nginx.tgz /etc/nginx```

```certbot --nginx```


Al lanzar Certbot el asistente preguntará para qué dominio queremos el certificado, que puede ser perfectamente un [subdominio](https://community.letsencrypt.org/t/will-you-issue-certificates-for-third-level-domains-too/1962). Certbot es un software que se mejora cada poco tiempo así que lo mejor para resolver cualquier duda es ir a la [web oficial](https://certbot.eff.org/docs/using.html). Hay otros softwares, ya que Let's Encrypt ha creado un protocolo abierto para comunicarse con sus servidores, pero este tiene la ventaja de automatizarlo todo al máximo y el paquete Debian que hemos instalado tiene un script de Cron que renueva automáticamente los certificados cuando llega la fecha.

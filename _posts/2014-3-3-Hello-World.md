---
layout: post
title: Cómo evitar suplantación de identidad en correo electrónico
---

El email se inventó hace muchos años y desde entonces han ido apareciendo diversos RFC con mejoras para adaptarlo a los nuevos tiempos. Una de estas mejoras es DMARC, junto con SPF y DKIM.

Desde los orígenes del email siempre se han enviado mensajes desde otros servidores diferentes al del campo “From:” ya fuese por comodidad, por ahorro de recursos… o porque somos una empresa malvada que spamea a sus clientes con un boletín de publicidad enviado desde un servidor externo. Para que otras personas no puedan enviar mensajes con las cuentas de nuestro dominio se crearon los tres sistemas mencionados.

SPF. Es el más sencillo de entender e implementar. Con él se le dice a los servidores de los destinatarios desde qué servidores se pueden enviar los emails de un dominio. Se hace mediante un registro DNS que contiene como este:

![_config.yml]({{ site.baseurl }}/images/config.png)

Esta línea dice que todo lo que no se envíe desde las IP de los registros MX o desde los otros servidores de evil-corp-usa no cumple la política del dominio, lo que quiere decir que seguramente vaya a la carpeta Spam.
Si no termina en ~all o en -all lo más seguro es que esté mal configurado, hay algunos servidores que lo tienen en ?all que no tiene ningún sentido más allá de verificar en el rato que dediquemos a implementarlo que la línea no tiene una coma donde no debe o cosas así.

DKIM. Puede ser imposible de implementar si usamos un servicio de hosting ya que su funcionamiento se basa en que nuestro servidor de correo firme los mensajes y los servidores destinatarios comprueben que están firmados con una clave pública que hay en un registro DNS.

DMARC. Su principal uso es recibir reportes de los servidores destinatarios que nos dirán si hay envíos que incumplen las políticas SPF y DKIM que tengamos. Para eso está la política de DMARC “none”, que no invalida lo que ya hacen SPF y DKIM.
Aparte de publicar la política se debería implementar el envío de reportes DMARC, si nadie enviase reportes, DMARC no serviría de nada.

"Mail Receivers [SHOULD](https://tools.ietf.org/html/rfc7489) also implement reporting instructions of DMARC"


Conclusiones:
SPF es muy sencillo de implementar para cualquier administrador de un dominio, su uso está muy extendido y por si solo debería reducir bastante el problema del [spoofing en email](https://en.wikipedia.org/wiki/E-mail_spoofing), aunque todavía hay servidores que no lo usan.
DKIM asegura que un email con una firma válida no ha sido modificado, mientras se confie en el servidor de origen (que se lo digan a Hillary).
DMARC puede ayudar a detectar además de spoofing real políticas SPF o DKIM mal implementadas dando una pista de que está saliendo el correo por servidores no incluidos en la política.

Independientemente de las medidas aplicadas por los servicios de email los usuarios pueden firmar sus mensajes con [OpenPGP](http://openpgp.org) o un certificado S/MIME (por ejemplo FNMT), con el inconveniente de que no hay un sistema automático que indique la política a aplicar por las máquinas receptoras cuando un email no va firmado por el usuario.

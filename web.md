# Seguretat web

[Què és la seguretat web?](https://www.fortinet.com/lat/resources/cyberglossary/what-is-web-security)

## OWASP

## Dades i paràmetres associats a la seguretat web

### Cookie de sessió

## WAF - Web Application Firewall

[Article de cloudfare](https://www.cloudflare.com/application-services/products/waf/)

## CSRF / XSRF - Cross Site Request Forgery

[Article de cloudfare](https://www.cloudflare.com/es-es/learning/security/threats/cross-site-request-forgery/)

[Article de Fortinet](https://www.fortinet.com/lat/resources/cyberglossary/csrf)

Moltes vegades passa que un lloc web al que estem visitant accedeix a un altre lloc web al qual tenim una sessió activa. Per exemple, un lloc web que té incrustat un video provinent d'una plataforma de vídeos en *streaming*. Si l'usuari té una sessió activa en aquesta segona plataforma el vídeo es podrà visualitzar.

**Per a que funcioni és necessari que l'usuari tingui una sessió activa al lloc web objectiu?**
Sí.

**Com veure les dades de sessió activa que tinc en el meu navegador?**

**Què podem fer a nivell usuari en un navegador?**

**SECRET_KEY FLASK PARAM**

### Variants - CSRF emmagatzemat

El codi maliciós està dintre del lloc web (etiqueta `img` o `iframe`).

### Eines de protecció

#### Política d'execució d'script només si prové del mateix origen

Les versions més recents dels navegadors les implementen: Es basa en el fet de que un navegador només pot permetre que s'executin scripts d'una primera pàgina per realitzar accions en una segona pàgina només si tots dos llocs provenen del mateix origen.

#### Tokens de sessió

Es genera un *token* aleatori (dada única i aleatòria) per sessió en el costat de servidor que s'envia posteriorment al client per a que aquest, cada vegada que hagi de fer una sol·licitd al lloc web, incrusti en l'acapçalament HTTP el valor d'aquest *token*, de tal manera que es pugui comparar amb el token emmagatzemat al servidor.

Depenent de com s'implementi aquesta mesura de seguretat pot tenir vulnerabilitats associades.

#### Incloure camps extra als formularis

L'atacant ha de saber construir la petició (ja sigui `GET` o `POST`) a enviar al lloc web objectiu. Per tant, ha de passar tots els paràmetres i valors de manera correcta en la petició. Aquesta mesura el que fa és incloure un camp ocult al formulari que s'envia al client amb un valor (*token aleatori*) que l'atacant no pot conéixer.

#### Fer ús de cookies de sessió amb indicador SameSite

Això farà que la cookie només es pugui fer servir si és per sol·licituds que provinguin del mateix lloc. El problema ara només pot venir si l'atacant té accés al lloc web objectiu.

## Injecció SQL

## XSS - Cross Site Scripting
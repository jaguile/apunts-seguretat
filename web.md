# Seguretat web

[Què és la seguretat web?](https://www.fortinet.com/lat/resources/cyberglossary/what-is-web-security)

## OWASP

## Dades i paràmetres associats a la seguretat web

### Cookies

[Article de Mozzilla](https://developer.mozilla.org/es/docs/Web/HTTP/Guides/Cookies)

Una *cookie* és un fitxer que s'envia des d'una aplicació web cap al client que fa una petició. Tenen diferents usos i poden ser temporals (mentre dura la sessió) o permanents (amb data d'expiració). Alguns usos:

* Recuperar dades d'accés (Gestió de la sessió)
* Millorar l'experiència de l'usuari (Personalització)
* Recordar hàbits teus de navegació (Rastreig). Les aplicacions web poden incloure cookies de tercers per a poder saber més de tu o inclús identificar-te segons dades com la IP o els teus hàbits de navegació.

Com s'acaba de dir, la *cookie* s'emmagatzema en el navegador i després s'envia en les diferents peticions `HTTP` que fa l'usuari cap al servidor. 

#### Com es crea una cookie i com s'envien les dades

S'envien en encapçalaments `HTTP` de tipus `Set-Cookie` (costat servidor) i `Cookie` (costat client). Es pot delimitar l'ús d'una cookie per temps, per domini o per rutes.

Exemple de `Set-Cookie` per enviar dades cap al client. Diem al client que emmagatzemi la següent cookie:

```javascript
Set-Cookie: <nombre-cookie>=<valor-cookie>
```

```javascript
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[page content]
```

Exemple des del client:

```javascript
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

Les *cookies* també es poden crear des de `javascript`.

Una *cookie*, resumint, tindrà la següent informació: 

* Un parell **clau=valor**
* Una sèrie de directives associades (Èxpires`, ...)

Una cookie s'identifica pel nom de la clau, pel domini de qui la creat i el path.

#### Cookies de sessió vs cookies permanents

Les de sessió s'esborren quan el client es tanca. Les cookies permanents són aquelles que tenen una data d'expiració (directiva `Expires` o `Max-Age` dintre d'un encapçalament `Set-Cookie`):

```javascript
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
```

#### Com es gestionen les cookies en una aplicació web

O bé des del client amb javascript (`Document.cookie`) o bé des del servidor (`php`, `python`, ...).

#### Seguretat en les cookies

**Directiva Secure** - Només s'envien cookies en peticions `HTTPS`.
**Directiva HttpOnly** - Les cookies no són accessibles amb l'api de javascript. Per exemple, útil si es llegeixen i es gestionen des del servidor.

```javascript
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly
```

**Directiva Domain i Path** - En aquests paràmetres pots especificar quins dominis (i subdominis) poden llegir la cookie i quines rutes són `URL` són vàlides per enviar la *cookie* una altra vegada des del client. Per exemple:

```javascript
Set-Cookie: sessio=abc123; Path=/app/
```

**Directiva SameSite** - Una *cookie* només serà enviada al mateix lloc web. Els possibles valors són `strict` i `lax`. Amb `lax` la cookie viatja de vegades quan és cap a un altre lloc web. No viatja si és quan es carrega una imatge o un `iframe` d'un altre lloc web en el que ha creat la cookie. Tampoc si es fa un *submit* amb petició `POST` en un formulari cap a un altre lloc web. Sí que viatje si és un enllaç (petició `GET`). Exemple:

```javascript
Set-Cookie: sessio=abc123; SameSite=Lax
```

1. Un usuari a https://altresite.com i fa clic a un enllaç cap a https://elteusite.com/panel - La cookie sí viatja.

2. Un formulari de https://altresite.com fa un POST a https://elteusite.com/panel - La cookie **no viatja**.

3. https://altresite.com carrega un `<img src="https://elteusite.com/panel">` - La cookie **no viatja**.

Per defecte, viatja sempre. 

Només s'enviarà la cookie a les `URL` que comencin per `/app/`. Per defecte, el valor de `Domain` serà el valor del domini que ha creat la *cookie* (excloent subdominis). Anàlogament per `Path`.

**Vulnerabilitats associades a l'ús de cookies** - Segrest de sessió, XSS, CSRF.

#### Gestió de la sessió d'usuari mitjançant cookies

Tenim dues maneres de fer-ho: Basades en el client o basades en el servidor.

**Basades en el servidor** - Les dades de la sessió s'emmagatzemen en el servidor. El client només té una **cookie* amb un identificador de la sessió que el client envia a cada petició.

**Basades en el client** - Les dades de la sessió s'emmagatzemen en una *cookie* al client. El servidor només valida i interpreta la informació de la sessió enviada pel client.

#### Com veure les cookies en el navegador

[Storage Inspector a Firefox](https://firefox-source-docs.mozilla.org/devtools-user/storage_inspector/index.html)

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

Serveix per xifrar les dades de sessió en una aplicació *Flask*. Important tenir aquest paràmetre desat en un fitxer de configuració o en un fitxer `.env` i que tingui un valor llarg i *força* aleatori.

Es genera la mateixa SECRET_KEY per totes les sessions que naveguen a una mateixa aplicació.

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

## Bones pràctiques

* [DEV] - Tenir una forta `SECRET_KEY` i protegir-la tenint-la en un fitxer de configuració o en un fitxer `.env`.
* [DEV] - Formularis - Afegir camp ocult amb *token aleatori*.
* [DEV] - Cookies - No guardar mai informació sensible.
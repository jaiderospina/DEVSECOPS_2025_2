# Grupo 6
LAB_ING_REVERSA - G6
Repositorio Docker: https://hub.docker.com/r/aven93/hatsh/tags
Repositorio GitHub: https://github.com/Aven93/G6
#
<br>```Escaneo de vulnerabilidades```<br>

![Escaneo de vulnerabilidades](https://i.imgur.com/xN1zCdY.png)

<br>```Edicion del archivo YML y subida del container```<br>
![Edicion del archivo YML y subida del container](https://i.imgur.com/2hqtLVd.png)

<br>```CVE mas Critico escaneado```<br>
![CVE mas Critico escaneado](https://i.imgur.com/gA9CIWA.png) 


# Para poner en el .YML



```yaml
services:
  hatsh:
    image: ghcr.io/aven93/hatsh:latest
    container_name: hatsh
    ports:
      - "3991:80"
    restart: unless-stopped
```
# Editar imagen central con una personalizada
Se agrego otra imagen para reemplazar el icono con la imagen vista anterior, solo modificando la imagen que se encuentra en la ruta: hatsh\src\assets\images (se debe dejar con el mismo nombre para no tener errores) quedaria algo asi:

![icono](https://imgur.com/psts44E.png)


imagen utilizada:
![imagen de referencia](https://imgur.com/evA1dv8.png)

```
Se debe agregar el link de la imagen a la estructura main class del sitio WEB en este caso en la ruta donde este guardado el index del aplicativo dentro del body:

<img src="/assets/images/logo.png" alt="Logo" style="height: 200px; display: block; margin: 0 auto;">
```
#

<p align="center">
  <a href="#" rel="noopener">
 <img src="https://i.imgur.com/8b0GE2B.png" width="180"></a>
</p>

<a href="https://hat.sh" style="color:#000"><h3 align="center">hat.sh</h3></a>

<div align="center">

[![Status](https://img.shields.io/badge/status-active-success.svg)](#)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](#)
[![CodeQL](https://github.com/sh-dv/hat.sh/actions/workflows/codeql-analysis.yml/badge.svg)](https://github.com/sh-dv/hat.sh/actions/workflows/codeql-analysis.yml)
[![Node.js CI](https://github.com/sh-dv/hat.sh/actions/workflows/node.js.yml/badge.svg?branch=master)](https://github.com/sh-dv/hat.sh/actions/workflows/node.js.yml)
[![Snyk](https://github.com/sh-dv/hat.sh/actions/workflows/snyk.yml/badge.svg)](https://github.com/sh-dv/hat.sh/actions/workflows/snyk.yml)

</div>

---





## Usage

![how-to-use-gif](https://i.imgur.com/NbAZOgP.gif)

<br>

## Features

### Security

- XChaCha20-Poly1305 - for symmetric encryption.
- Argon2id - for password-based key derivation.
- X25519 - for key exchange.

The libsodium library is used for all cryptographic algorithms.

### Privacy

- The app runs locally in your browser.
- No data is ever collected or sent to anyone.​

### Functionality

- Secure multiple file encryption/decryption with passwords or keys.
- Secure random password generation.
- Asymmetric key pair generation.
- Authenticated key exchange.
- Password strength estimation.

<br>

## Offline Use

The app can be easily self hosted, please follow the [installation](https://hat.sh/about/#installation) instructions.

<br>

## Browser Compatibility

We officially support the last two versions of every major browser. Specifically, we test on the following

- **Chrome** on Windows, macOS, and Linux , Android
- **Firefox** on Windows, macOS, and Linux
- **Safari** on iOS and macOS
- **Edge** on Windows

Safari and Mobile browsers are limited to single 1GB files, due to lack of support with server-worker fetch api.

<br>

## Official running instances of the app

| #   | URL                                       |
| --- | ----------------------------------------- |
| 1   | [hat.sh](https://hat.sh/)                 |
| 2   | [hat.now.sh](https://hat.now.sh/)         |
| 2   | [hat.vercel.app](https://hat.vercel.app/) |

<br>

## Donations

The project is maintained in my free time. Donations of any size are appreciated :

<br>

<div>

<strong>Crypto</strong>

  <table>
    <tr>
      <th></th>
      <th>Coin</th>
      <th>Address</th>
    </tr>
    <tr>
      <td><img src="https://i.imgur.com/utSCHpB.png" /></td>
      <td>Monero</td>
      <td style="word-break: break-word">
        <code
          >84zQq4Xt7sq8cmGryuvWsXFMDvBvHjWjnMQXZWQQRXjB1TgoZWS9zBdNcYL7CRbQBqcDdxr4RtcvCgApmQcU6SemVXd7RuG</code
        >
      </td>
    </tr>
    <tr>
      <td><img src="https://i.imgur.com/imvYFLR.png" /></td>
      <td>Bitcoin</td>
      <td><code>bc1qlfnq8nu2k84h3jth7a27khaq0p2l2gvtyl2dv6</code></td>
    </tr>
    <tr>
      <td><img src="https://i.imgur.com/a4vLbjm.png" /></td>
      <td>Ethereum</td>
      <td><code>0xF6F204B044CC73Fa90d7A7e4C5EC2947b83b917e</code></td>
    </tr>
  </table>

  <br>
  
  <strong>Kofi</strong>

[https://ko-fi.com/shdvapps](https://ko-fi.com/shdvapps)

<strong>Open Collective</strong>

[https://opencollective.com/hatsh](https://opencollective.com/hatsh)

</div>

<br>
<br>

## Social

- [Reddit](https://reddit.com/r/hatsh)

<br>

## Acknowledgements

- Everyone who supported the project.
- [Samuel-lucas6](https://github.com/samuel-lucas6) from the [Kryptor](https://github.com/samuel-lucas6/Kryptor) project for being helpful and doing a lot of beta testing.
- [stophecom](https://github.com/stophecom) from the [Scrt.link](https://scrt.link/) project for translating to German.
- [bbouille](https://github.com/bbouille) for translating to French.
- [qaqland](https://github.com/qaqland) for translating to Chinese.
- [Ser-Bul](https://github.com/Ser-Bul) for translating to Russian.
- [matteotardito](https://github.com/matteotardito) for translating to Italian.
- [t0mzSK](https://github.com/t0mzSK) for translating to Slovak.
- [Xurdejl](https://github.com/Xurdejl) for translating to Spanish.
- [Franatrtur](https://github.com/Franatrtur) for translating to Czech.
- [darkao](https://github.com/darkao) for translating to Turkish.
- [Frank7sun](https://github.com/Frank7sun) for translating to Japanese.

<br>

## Credits

[libsodium.js](https://github.com/jedisct1/libsodium.js)

[next.js](https://nextjs.org/)

[material-ui](https://material-ui.com/)

<br>

## License

[Copyright (c) 2022 sh-dv](https://github.com/sh-dv/hat.sh/blob/master/LICENSE)

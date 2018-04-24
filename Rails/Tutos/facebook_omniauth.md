## Integrar autenticación Devise+Facebook

### Luego coloco el resto xD lo importante es la nota

> Nota: Para poder probar en local es necesario forzar SSL, por lo tanto en config/enviroment/development.rb
> colocar config.force_ssl = true. Ademas es necesario configurar openssl en nuestro local para que el servidor
> no se vuelva loco intentando ingresar al localhost. Colocar 'openssl req -x509 -sha256 -nodes -newkey rsa:2048 -days 365 -keyout localhost.key -out localhost.crt'
> en la carpeta de tu eleccion, luego de instalar el certificado se necesita arrancar el servidor con binding a este
> 'rails s -b 'ssl://localhost:3000?key=/direccion/a/localhost.key&cert=/direccion/a/localhost.crt' '
> colocar https://localhost/ dentro del dominio de facebook y https://localhost:3000/users/auth/facebook/callback en la URI
> de redireccionamiento Oauth validos.

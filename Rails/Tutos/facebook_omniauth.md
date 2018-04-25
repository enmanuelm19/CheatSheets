## Integrar autenticación Devise+Facebook

### Luego coloco el resto xD lo importante es la nota

> Nota: Para poder probar en local es necesario forzar SSL, por lo tanto en config/enviroment/development.rb
> colocar config.force_ssl = true. Ademas es necesario configurar openssl en nuestro local para que el servidor
> no se vuelva loco intentando ingresar al localhost. Colocar 'openssl req -x509 -sha256 -nodes -newkey rsa:2048 -days 365 -keyout localhost.key -out localhost.crt'
> en la carpeta de tu eleccion, luego de instalar el certificado se necesita arrancar el servidor con binding a este
> 'rails s -b 'ssl://localhost:3000?key=/direccion/a/localhost.key&cert=/direccion/a/localhost.crt' '
> colocar https://localhost/ dentro del dominio de facebook y https://localhost:3000/users/auth/facebook/callback en la URI
> de redireccionamiento Oauth validos.


### Hacer mock para probar con rspec y cabypara
```ruby
module SessionHelpers
  def sign_in_with(email, password)
    visit new_user_session_path
    fill_in 'user[email]', with: email
    fill_in 'user[password]',	with: password
    click_button 'Iniciar Sesión'
  end

  def sign_in_with_facebook(user)
    opts = {
      'provider': 'facebook',
      'uid': '999999999',
      'info': {
        'email': user.email, 
        'name': user.name
      },
      'credentials': {
        'token': 'random',
        'expires_at': 1529779526,
        'expires': true
      },
      'extra': {
        'raw_info': {
          'name': user.name,
          'email': user.email,
          'id': '123123'
          }
        }
    }
    visit new_user_session_path
    set_omniauth(opts)
    click_link_or_button 'log_in_facebook'
  end

  def set_omniauth(opts = {})
    params = JSON.parse(opts.to_json, object_class: OpenStruct)
    OmniAuth.config.test_mode = true
    OmniAuth.config.mock_auth[:facebook] = params
  end

  def set_invalid_omniauth(opts = {})
    credentials = { :provider => :facebook,
                    :invalid  => :invalid_crendentials
                   }.merge(opts)
    OmniAuth.config.test_mode = true
    OmniAuth.config.mock_auth[credentials[:provider]] = credentials[:invalid]
  end 
end
```

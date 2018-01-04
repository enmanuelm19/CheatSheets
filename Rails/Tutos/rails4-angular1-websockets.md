### Integrar Websockets a Rails 4 con Angular como front

> En mi experiencia integre websockets en Rails 4.2, trabajando con angular dentro de los assets de Rails, para ver como integrar websockets en Rails 4 teniendo angular totalmente separado seguir este enlace: https://medium.com/@karimbutt/integrating-websocket-rails-with-angularjs-or-other-js-frameworks-256729e95a86

El stack con el que trabaje fue el siguiente:
* Rails 4.2
* Ruby 2.3.1
* Angular 1.6

Para poder integrar websockets en Rails 4 utilice la gema `faye-websocket`. Por ello necesitamos agregar las siguientes gemas al *Gemfile*
```
gem 'faye-websocket'
gem 'puma'
```

Ahora crearemos un *Rack middleware* para encapsular la logica de detras de la implementación del websocket. Creamos una clase `app/middleware/notification_middleware.rb` donde colocaremos nuestro codigo como se muestra a continuación:

```Ruby
require "faye/websocket"

class NotificationMiddleware
  KEEPALIVE_TIME = 45

  def initialize(app)
    @app = app
    @clients = []
  end

  def call(env)
    if Faye::WebSocket.websocket?(env)
      ws = Faye::WebSocket.new(env, nil, { ping: KEEPALIVE_TIME })

      ws.on :open do |event|
        @clients << ws
      end

      ws.on :message do |event|
        @clients.each { |client| client.send(event.data) }
      end

      ws.on :close do |event|
        @clients.delete(ws)
        ws = nil
      end
      ws.rack_response
    else
      @app.call(env)
    end
  end
end

```

Como tengo el aplicativo montado en *Heroku* el cual tumba los dynos cada 60 segundos, se coloca un tiempo estimado para que no se caiga la conexion (*KEEPALIVE_TIME*)

Despues de creada nuestra logica necesitamos que sea reconocida por rails, agregamos lo siguiente en `config/application.rb`

```Ruby
require_relative '../app/middleware/notification_middleware'

Bundler.require(*Rails.groups)

module AppName
  class Application < Rails::Application
    config.middleware.use NotificationMiddleware
  end
end
```

Ya con estas configuraciones podemos hacer uso de este protocolo dentro de nuestro aplicativo, en mi caso cree un *Factory* en angular que se hiciera cargo de la conexión

```Javascript
(function () {
  'use strict';

  angular.module('App.services.notification', []).factory('Notification', notification);

  function notification() {
    var scheme = "ws://"
    var uri = scheme + window.document.location.host + "/";
    var ws = new WebSocket(uri);
    return ws;
  }
})();
```

Esto abre la conexión desde el cliente, para enviar data desde el servidor cree un metodo en `helpers/application_helper.rb`

```Ruby
def send_notification(data)
  EM.run {
        ws = Faye::WebSocket::Client.new(ENV['SOCKET'])
        ws.on :open do |event|
          ws.send(data.to_json)
        end
  }
end
```

Como podemos observar hacemos uso del *EventMachine* libreria que viene integrada dentro de Rails. Recordar que el valor de `ENV['SOCKET']` tiene que ser el mismo de la uri especificada en el cliente.

Y listo, solo hace falta usar el metodo *send_notification* en donde quiras dentro del aplicativo para comunicarte a traves de websockets con el cliente.

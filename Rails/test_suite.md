## Test Suite

Estas notas especifican la manera de configurar tu entorno de pruebas, en donde se incluyen las siguientes gemas:

* Rspec: Gema casi obligatoria para los desarrolladores en Rails, crea un entorno para hacer prubas a modelos, controladore, vistas, APIs, etc.

* FactoryBot: Anteriormente FactoryGirl es una gema para contruir objetos y reproducirlos en las pruebas.

* Shoulda-matchers: Es una gema que tiene como finalidad hacer mas intuitivas las instrucciones para realizar las pruebas.

* FFaker: Gema que cuenta con la capacidad de reproducir data de prueba para nuestros objetos.

* Database-cleaner: Esta gema como su nombre lo indica, se encarga de limpiar la BD al momento de correr nuestras pruebas.

* rails-controller-testing: Esta gema solo se tiene que instalar si se trabaja con la version 5 de Rails, debido a que viene por defecto configurada en antiguas versiones

#### Instalación

Como cualquier otra gema, para tenerla disponible en una aplicación en Rails es necesario agregarlas al archivo `Gemfile` y correr el comando `bundle install`

```Ruby
group :test do
  gem 'rspec-rails'
  gem 'Factory_bot_rails'
  gem 'ffaker'
  gem 'shoulda-matchers'
  gem 'rails-controller-testing'
  gem 'database-cleaner'
end
```

#### Configuración

Para la instalación de `Rspec` y el resto de gemas es necesario correr ciertos comandos:

> rails generate rspec:install

Este comando crea la carpeta `spec/` en la raiz del proyecto, dentro de esta siguiendo las convenciones de la gema se deben crear las carpetas `models/`,`controllers/`,`factories/`*,`features/` para realizar las respectivas pruebas.

Para configurar la integración de estas gemas en el archivo `spec/rails_helper.rb` se deben colocar las siguientes lineas de codigo

```Ruby
RSpec.configure do |config|
  .
  .
  # Solo si es Rails 5, integrar aspectos como assigns en los specs de controladores (rails-controller-testing)
  [:controller, :view, :request].each do |type|
    config.include ::Rails::Controller::Testing::TestProcess, :type => type
    config.include ::Rails::Controller::Testing::TemplateAssertions, :type => type
    config.include ::Rails::Controller::Testing::Integration, :type => type
  end
  .
  .
  # Para integración con FactoryBot
  config.include FactoryBot::Syntax::Methods
end

Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    with.test_framework :rspec
    with.library :rails
  end
end
```

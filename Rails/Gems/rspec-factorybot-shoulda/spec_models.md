### Spec models

En esta sección esta detallado que y como he probado el codigo de los modelos. Cabe recordar como fue configurado nuestra suite de pruebas. Como hacemos uso de la gema **Shoulda-matcher** y sus ventajas. Las pruebas de modelos en **Rspec** tienen la siguiente estructura:

```Ruby
require 'rails_helper'

RSpec.describe Model, type: :model do
  # spec code
end
```

#### Relaciones
Descibiremos las relaciones que tiene nuestros modelos.

```Ruby
describe 'Relationships' do
    it { should have_many(:models) }
    it { should have_many(:model).dependent(:destroy) }
    it { should belong_to :model }
    it { should have_and_belong_to_many :models }
    it { should have_one :model }
  end
```
Podemos observar las diferentes relaciones y la descriptiva manera que **Shoulda** nos da para describir las relaciones de un modelo dado.

#### Validaciones
Ahora realizaremos **specs** de las validaciones del modelo, tanto las que vienen con **ActiveRecord** como las podemos crear.

Como ejemplo tenemos que un **User** tiene que tener **email**, **password** de por lo menos 10 caracteres y no puede ser **male** y de categoria 1 al mismo tiempo.

> validate :cannot_be_male_and_category_one

```Ruby
describe "Validations" do
  it { should validate_presence_of :email }
  it { should validate_length_of(:password).is_at_least(8)}
  it "should not be category 1 and male" do
    user = FactoryBot.create(:user, sex: 'male', category: 1)
    expect(user.valid?).to be false
  end
end
```

Puedes ver [Aqui](http://www.rubydoc.info/github/thoughtbot/shoulda-matchers/Shoulda/Matchers) mas ejemplos que tiene disponible **Shoulda-matcher**

#### Callbacks
Si nuestro modelo corren **callbacks** como **after_create**,**before_create**,**after_save**,**before_save**,**etc**. Puedes saber mas [Aqui](http://guides.rubyonrails.org/active_record_callbacks.html#available-callbacks)

Coloquemos ahora un ejemplo, supongamos que tenemos **Post** que posee un atributo llamado **category**, y dependiendo la cantidad de caracteres del **titulo** del **Post** este pasa a tener categoria 1 o 2 se realiza lo siguiente en el modelo:

> before_save :set_category_according_length_of_title

donde **set_category_according_length_of_title** es un metodo que asigna el valor de category a 1 si el tamaño del titulo es menor a 30 y 2 si pasa lo contrario. Realizaremos un **Spec** como el siguiente:

```Ruby
  describe "Callbacks" do
    context "when post title is 40 characters long" do
      it "should set category to 2" do
        post = FactoryBot.create(:post, title: '40+ title ...')
        expect(post.category).to eq(2)
      end
    end

    context "when post title is 30 characters long" do
      it "should set category 1" do
        post = FactoryBot.create(:post, title: '30- title ...')
        expect(post.category).to eq(1)
      end
    end
  end
```

Como se puede observar se tiene que crear un objeto que cumpla con las caracteristicas necesarias y que cumpla lo descrito, en el ejemplo dado deberiamos tener un **factory** que cree un objeto de tipo post, de lo contrario hacer lo siguiente:

> post = Post.create(title: '30 o 40 characters long', attribute: value, .....)

#### Metodos
Para realizar **specs** de los metodos de una clase podemos hacer lo siguiente:

Supongamos que para el modelo **User** tenemos un metodo llamado **full_name** que consiste basicamente en concatenar el nombre y el apellido del usuario, se realiza el siguiente **spec**.

```Ruby
  describe "#full_name" do
    let(:user) { FactoryBot.create(:user) }
    it "should return user full name" do
      expect(user.full_name).to eq("#{user.first_name} #{user.last_name}")
    end
  end
```

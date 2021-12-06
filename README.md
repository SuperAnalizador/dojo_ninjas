# README

# antonio_soto@yahoo.com
# 1. Empieza un nuevo proyecto (el nombre del proyecto debe ser 'dojo_ninjas').
rails new dojo_ninjas
# 2. Crea las tablas/modelos apropiados.
rails g model Dojo name:string city:string state:string

rails g model Ninja first_name:string last_name:string dojo:references

rails db:migrate
# 3. Usando la consola de Ruby
# Cree 3 dojos (ingrese algunos datos en blanco solo para asegurarse que le permite ingresar datos en blanco).
Dojo.create(name: "CodingDojo Silicon Valley", city: "Mountain View", state: "CA")

Dojo.create(name: "CodingDojo Seattle", city: "Seattle", state: "WA")

Dojo.create(name: "CodingDojo New York", city: "New York", state: "NY")
# 4. Cambia tu modelo para que haga las siguientes validaciones:
# En dojo.rb 
class Dojo < ApplicationRecord
# 5. Asegúrate que el modelo ninja pertenece a dojo
has_many :ninjas
# 4.1 Para dojo, requerir nombre, ciudad, estado. También haga que el estados sea de 2 caracteres de longitud.
    validates :name, :city, :state, presence: true
    validates :state, length: { is: 2 }
end

# En ninja.rb
class Ninja < ApplicationRecord
# 5. Asegúrate que el modelo ninja pertenece a dojo
belongs_to :dojo
# 4.2 Para ninja, requerir nombre y apellido.
    validates :first_name, :last_name, presence: true
end
# 6. Usando la consola de Ruby
# 6.1. Elimine los 3 dojos que creó (ej. Dojo.find(1).destroy, también consulte el método destroy_all).
Dojo.destroy_all
# 6.2. Cree 3 dojos adicionales usando el comando Dojo.new.
dojo1 = Dojo.new(name: "CodingDojo Seattle", city: "Seattle", state: "WA")

dojo1.save

dojo2 = Dojo.new(name: "CodingDojo Silicon Valley", city: "Mountain View", state: "CA")

dojo2.save

dojo3 = Dojo.new(name: "CodingDojo New York", city: "New York", state: "NY")

dojo3.save
# 6.3.  Intente crear algunos dojos adicionales pero sin especificar algunos de los campos requeridos. Asegúrese que las validaciones están funcionando y que le devuelve los mensajes de error.
dojo = Dojo.new(name: "CodingDojo Los Angeles", city: "Los Angeles")

dojo.save

dojo.errors.full_messages # => ["State can't be blank", "State is the wrong length (should be 2 characters)"]

dojo = Dojo.new(name: "CodingDojo Los Angeles", state: "CA")

dojo.save

dojo.errors.full_messages # => ["City can't be blank"]

dojo = Dojo.new(city: "Los Angeles", state: "CA")

dojo.save

dojo.errors.full_messages # => ["Name can't be blank"]

# 6.4. Cree 3 ninjas que pertenezcan al primer dojo que creó.
Dojo.first.ninjas.create(first_name: "Juan", last_name: "Rojas")

Dojo.first.ninjas.create(first_name: "Juana", last_name: "Rojas")

Dojo.first.ninjas.create(first_name: "Julieta", last_name: "Rojas")
 
# 6.5. Cree 3 ninjas más que pertenezcan al segundo dojo que creó.
Dojo.second.ninjas.create(first_name: "Miguel", last_name: "Ruz")

Dojo.second.ninjas.create(first_name: "Maria", last_name: "Ruz")

Dojo.second.ninjas.create(first_name: "Michael", last_name: "Ruz")

# 6.6. Cree 3 ninjas más que pertenezcan al tercer dojo que creó.
Dojo.third.ninjas.create(first_name: "Andres", last_name: "Lopez")

Dojo.third.ninjas.create(first_name: "Antonio", last_name: "Lopez")

Dojo.third.ninjas.create(first_name: "Ana", last_name: "Lopez")

# 6.7. Asegúrate de entender cómo obtener todos los ninjas para cualquier dojo (ej. obtener todos los ninjas para el primer dojo, segundo dojo, etc).
Dojo.first.ninjas

Dojo.second.ninjas

Dojo.third.ninjas

# 6.8. ¿Cómo recuperar solo el nombre de los ninjas que pertenecen al segundo dojo y ordenar el resultado por created_at en orden descendiente?
Ninja.where(dojo: Dojo.second).select(:id, :first_name).order(created_at: :desc)

# 6.9. Elimine el segundo dojo. ¿Cómo podrías ajustar tu modelo para que automáticamente elimine todos los ninjas asociados con ese dojo?
class Dojo < ApplicationRecord
  # agregado dependent: :destroy
  has_many :ninjas, dependent: :destroy
  validates :name, :city, :state, presence: true
  validates :state, length: { is: 2 }
end

Dojo.find(2).destroy
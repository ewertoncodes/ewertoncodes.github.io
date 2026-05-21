---
layout: post
title: Compartilhando comportamento usando Mixins 
date: 2026-05-12 22:16:00.000000000 -03:00
categories: ruby
tags:
  - mixins
  - ruby

canonical_url: https://ewertoncodes.github.io/post/mixins-ruby
---

# Mixins no Ruby

Quando queremos compartilhar um comportamento entre classes geralmente usamos herança. Mas nem sempre a herança faz sentido.

```ruby
class Product
  def initialize(name, price, weight_kg)
    @name   = name
    @price  = price
    @weight = weight_kg
  end

  def calculate_shipping
    puts "Shipping for #{@name}: $#{(@weight * 8).round(2)}"
  end

  def apply_discount(pct)
    puts "#{@name} at #{pct}% off: $#{(@price * (1 - pct / 100.0)).round(2)}"
  end
end

class Book < Product
  def initialize(name, price, weight_kg, author)
    super(name, price, weight_kg)
    @author = author
  end
end

class Vinyl < Product
  def initialize(name, price, weight_kg, artist)
    super(name, price, weight_kg)
    @artist = artist
  end
end

class Ebook < Product
  def initialize(name, price)
    super(name, price, 0)
  end

  def calculate_shipping
    puts "Ebooks don't have shipping!"
  end
end

book  = Book.new("Sapiens", 19.90, 0.6, "Harari")
vinyl = Vinyl.new("Kind of Blue", 34.90, 0.3, "Miles Davis")

book.calculate_shipping    # => Shipping for Sapiens: $4.8
book.apply_discount(10)   # => Sapiens at 10% off: $17.91
vinyl.calculate_shipping   # => Shipping for Kind of Blue: $2.4
```
Não faz sentido o Ebook herdar o método calculate_shipping de Product.

Nesses casos, podemos usar Mixins.
```ruby
module Discountable
  def apply_discount(pct)
    discounted = (@price * (1 - pct / 100.0)).round(2)
    puts "#{@name} at #{pct}% off: $#{discounted}"
  end
end

module Shippable
  def calculate_shipping
    puts "Shipping for #{@name}: $#{(@weight * 8).round(2)}"
  end
end

module Stockable
  def check_stock
    puts "#{@name}: #{@stock} units in stock"
  end
end

class Book
  include Discountable
  include Shippable
  include Stockable

  def initialize(name, price, weight_kg, stock, author)
    @name   = name
    @price  = price
    @weight = weight_kg
    @stock  = stock
    @author = author
  end
end

class Vinyl
  include Discountable
  include Shippable
  include Stockable

  def initialize(name, price, weight_kg, stock, artist)
    @name   = name
    @price  = price
    @weight = weight_kg
    @stock  = stock
    @artist = artist
  end
end

class Ebook
  include Discountable

  def initialize(name, price, author)
    @name   = name
    @price  = price
    @author = author
  end
end

book  = Book.new("Sapiens", 19.90, 0.6, 15, "Harari")
vinyl = Vinyl.new("Kind of Blue", 34.90, 0.3, 4, "Miles Davis")
ebook = Ebook.new("Sapiens Digital", 9.90, "Harari")

book.calculate_shipping   # => Shipping for Sapiens: $4.8
book.check_stock          # => Sapiens: 15 units in stock
book.apply_discount(10)  # => Sapiens at 10% off: $17.91

vinyl.calculate_shipping  # => Shipping for Kind of Blue: $2.4
vinyl.check_stock         # => Kind of Blue: 4 units in stock

ebook.apply_discount(20) # => Sapiens Digital at 20% off: $7.92
# ebook.calculate_shipping  # => NoMethodError
```
## Module

Você deve ter visto que usamos module ao invés de class na definição dos mixins.
Módulos no Ruby são usados para agrupar métodos e constantes que se relacionam logicamente. Eles não podem ser instanciados.  Também podemos usá-los para namespace e para definir métodos de classe que serão compartilhados entre classes. 

```ruby
# warehouse/order.rb
class Order
  def initialize(customer, items) 
    @customer = customer
    @items = items
  end

  def to_s
    "Order for #{@customer}: #{@items.join(", ")}"
  end
end

#A
# storefront/order.rb
class Order
  def initialize(sku, quantity)
    @sku = sku
    @quantity = quantity
  end

  def to_s
    "Order for #{@sku}: #{@quantity}"
  end
end
```
O codigo acima são dois tipos de orders diferentes e um acaba sobrescrevendo o outro. Pra resolver isso, podemos usar módulos.

```ruby
# storefront/order.rb
module Storefront
  class Order
    def initialize(sku, quantity)
      @sku = sku
      @quantity = quantity
    end

    def to_s
      "Order for #{@sku}: #{@quantity}"
    end
  end
end

# warehouse/order.rb
module Warehouse
  class Order
    def initialize(customer, items) 
      @customer = customer
      @items = items
    end

    def to_s
      "Order for #{@customer}: #{@items.join(", ")}"
    end
  end
end


sf_order = Storefront::Order.new("GR-86", 3)
wh_order = Warehouse::Order.new("Alice", ["Book: Sapiens", "Vinyl: Kind of Blue"])

puts sf_order # => Order for GR-86: 3
puts wh_order # => Order for Alice: Book: Sapiens, Vinyl: Kind of Blue
```

## Inlude, Extend e Prepend

Nos vimos uma forma de adicionar esses métodos do mixin dentro da classe usando o include. Mas ainda temos mais duas que sao o extend e o prepend.

```include`` Adiciona os métodos do módulo como métodos de instância.

```ruby
module Discountable
  def apply_discount(pct)
    puts "#{@name} at #{pct}% off: $#{(@price * (1 - pct / 100.0)).round(2)}"
  end
end

class Book
  include Discountable 

  def initialize(name, price)
    @name  = name
    @price = price
  end
end

book = Book.new("Sapiens", 19.90)
book.apply_discount(10)  # => Sapiens at 10% off: $17.91

Book.apply_discount(10)  # => NoMethodError


Book.ancestors
# => [Book, Discountable, Object, Kernel, BasicObject]
#           ^^^^^^^^^^^^ 
```
Book → Discountable → Object → Kernel → BasicObject
```extend``` Adiciona os métodos do módulo como métodos de classe (na classe singleton). Instâncias não os recebem.

```ruby

module Searchable
  def find_by_name(name)
    puts "Querying database for '#{name}'..."
  end

  def all
    puts "Fetching all records..."
  end
end

class Book
  extend Searchable   
  def initialize(name)
    @name = name
  end
end

Book.find_by_name("Sapiens")  
Book.all 

book = Book.new("Sapiens")
book.find_by_name("Sapiens")

vinyl = Object.new
vinyl.extend(Searchable)
vinyl.find_by_name("Kind of Blue") 
```
Book (singleton) → Searchable → Class → Object

No ```prepend``` o módulo é colocado na frente da classe na hierarquia de métodos.


```ruby
module Loggable
  def display
    puts "--- Iniciando Log ---"                # 1. Executa antes
    super                                       # 2. Chama o método original
    puts "--- Finalizando Log ---"              # 3. Executa depois
  end
end

class Report
  prepend Loggable                              # Prependendo o módulo

  def display
    puts "Conteúdo do Relatório"
  end
end

report = Report.new
report.display
# Saída:
# --- Iniciando Log ---
# Conteúdo do Relatório
# --- Finalizando Log ---

# Verificando a cadeia de ancestrais
p Report.ancestors
# Saída: [Loggable, Report, Object, Kernel, BasicObject]
``` 


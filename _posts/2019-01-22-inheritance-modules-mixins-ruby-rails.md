---
type: single
title: 'Classes, inheritance, modules and mixins in ruby on rails'
permalink: /classes-inheritance-modules-and-mixins-ruby-on-rails/
excerpt: "Take a look at the patterns and tools available to us in Ruby on Rails."
header:
  image: '/assets/images/rails.jpg'
  teaser: '/assets/images/rails.jpg'
author_profile: false
comments: true
---

Good organisation helps make code easier to read and easier to maintain. Also encourages separation of concerns and a scalable codebase, making future development more efficient.

Rails gives us a directory structure to help with this upfront, yet when projects grows larger, it can be difficult to keep the DRY principle (Do not repeat yourself) when you find yourself with dozens of models and controllers packed full of logic.

A basic inheritance pattern in ruby:

```ruby
class Child < Parent

  attr_accessor :x

  def initialize(x, p)
    super(p)  
    @x = x
  end

  def child_instance
    "child instance"
  end

  def self.child_class_method
    "child class method"
  end
end

class Parent

  attr_accessor :p

  def initialize(p)
    @p = p
  end

  def self.parent_class_method
    puts "parent class method"
  end

end
```

Many Ruby developers prefer using a pattern known as the 'mix-in' pattern over class inheritance. In this method we define a module and then include it into our class. The module basically passes its functionality to the class which included it. You can include the module in as many classes as you want or a single class can include as many modules as you want. Mixin pattern is a powerful yet flexible way of sharing functionality.

A module can be created as a sort of self contained library containing functionality that can be passed to be used elsewhere by using `require`. Unlike classes, modules are not instantiated but can define methods on themselves using `self`. They are the perfect place to store lengthy complex business logic. For example if you were to be writing a program that uploads a file, parses it, does some complex logic and returns some data. Your program can become quite large so you decide to move the parsing functionality into a module.

```ruby
module ParseFile
  def self.parse(file)
    return 'ABCDEFG'
  end
end
```

In your main program, just require the module you've created and give it any file to parse. In Rails, modules live in the `lib` folder.

You can create large madule `libraries` consisting of many nested modules and classes. Here is a more complex example module used for processing data.

```ruby
module DataProcessingModule

  def self.process_all_data(dataObj)
    a = DataProcessingOne.new(dataObj.slice(:keyA, :keyB))
    b = DataProcessingTwo.new(dataObj.slice(:keyC, :keyD))

    HelperModule.combine_data(a.process_data, b.process_data)
  end

  module HelperModule
    def self.combine_data(dataOne, dataTwo)
      return dataOne + dataTwo
    end
  end

  class DataProcessingOne
    def initialize(dataGroupOne)
      @data = dataGroupOne
    end

    def process_data
      puts "data A is #{@data}"
      return @data[:keyA] + @data[:keyB]
    end
  end

  class DataProcessingTwo
    def initialize(dataGroupTwo)
      @data = dataGroupTwo
    end

    def process_data
      puts "data B is #{@data}"
      return @data[:keyC] + @data[:keyD]
    end
  end

end
```

After `require`ing the module in your program, just instantiate the class DataProcessingOne as follows:

```ruby
DataProcessingModule::DataProcessingOne.new({keyA: 6, keyB: 8})
```

Modules can be used this way to define simple namespaces which can be used to encapsulate your objects/classes/data/functions etc, or to group together related classes/functions/data etc, for organisation purposes.

Here is a module that is used to group together three related classes:

```ruby
module Mod
  class A
  end

  class B
  end

  class C
  end
end
```

Class `A` is accessible via `Mod::A`. Classes `B`, `C` are similar and are grouped together with class `A` within module `Mod`.

Modules can be included in other classes to achieve the `mixin` functionality. All methods in a module can be either added to another Class's class methods or instance methods, by either using `include` or `extend`.

Lets see how this works by defining two modules and a class:

```ruby
class MyClass
  include A
  extend B
end

module A
  def method_a
    puts "abc"
  end
end

module B
  def method_b
    puts "def"
  end
end
```

Any instance of `MyClass` can call `method_a` because of the `include A` defined in the class. The class `MyClass` itself can now call `method_b` because module `B` is `extend`ed and not included like `A`.

Modules can also instantiate new data/variables, although not typically used in this way. To achieve this its best to define a distinct method in the module that will instantiate the data we want when called.

```ruby
class MyClass
  include A
end

module A

  attr_accessor :var1, :var2

  def initialize_module_data
    @var1 = "one"
    @var2 = "two"
  end
end

myclass = MyClass.new
myclass.initialize_module_data

myclass.var1
--> "one"
myclass.var2
--> "two"
```

To encapsulate more functionality lets create one module that both extended class methods and adds new instance methods, instead of using one module for each.

```ruby
class MyClass
  include A
end

module A

  def self.included base
    base.send :include, InstanceMethods
    base.send :extend, ClassMethods
  end

  module InstanceMethods

    def my_instance_method
      "module A instance method"
    end
  end

  module ClassMethods

    def my_class_method
      "module B class method"
    end
  end
end
```

This way when module `A` is included it calls the include and extend methods of the base object with wither the `InstanceMethods` module or the `ClassMethods` module.


## Namespaces and Modules

Let's put the `Child` and `Parent` classes from earlier together in their own subdirectory within the models directory, call the folder `test_space`.

```ruby
class MyClass
  include A
end

module A

  def self.included base
    base.send :include, InstanceMethods
    base.send :extend, ClassMethods
  end

  module InstanceMethods

    def my_instance_method
      "module A instance method"
    end
  end

  module ClassMethods
    def my_class_method
      "module B class method"
    end
  end
end
```

Since we moved the `parent.rb` and `child.rb` into our `test_space` subdirectory, we need to define our classes in the namespace `TestSpace` as rails convention. It may seem strange that we are defining ruby classes here instead of ActiveRecord `models`. In actuality it can be very helpful to leverage ruby classes and modules for organization and separation of concerns throughout a Rails project. Since we are on the topic of ActiveRecord models, we need to create some. Lets namespace our models in a directory called `test_space_two` with the following command:

```ruby
rails g model TestSpaceTwo::ChildModel name:string
```

In addition to creating the model, rails will create a file in the models directory called `test_space_two.rb`. This is Rails magic going on, look at the associated migration file:

```ruby
class CreateTestSpaceTwoChildModels < ActiveRecord::Migration
  def change
    create_table :test_space_two_child_models do |t|
      t.string :name

      t.timestamps null: false
    end
  end
end
```

The table Rails created was named `test_space_two_child_models`. Rails automatically matches the naming convention of our tables to our namespace. Lets take a look at `test_space_two.rb`.

```ruby
module TestSpaceTwo
  def self.table_name_prefix
    'test_space_two_'
  end
end

end
```

Rails creates a module that matches our directory. ChildModel and every other model within the `test_space_two` folder becomes namespaced to this module. By defining the `table_name_prefix` method will ensure every table within the module will have the prefix `test_space_two`.

If you would like to organise your models without needing to worry about namespaces, just add this line to `config/application.rb`. This will tell Rails to load models in a particular subfolder without needing to define a module/namespace:

```ruby
config.autoload_paths += Dir[Rails.root.join('app', 'models', 'sub_folder_name')]
```

Or tell rails to load models in all model subfolders:

```ruby
config.autoload_paths += Dir[Rails.root.join('app', 'models', '{*/}')]
```

## Mix-Ins and Concerns

Lets add some functionality to our `ChildModel` by using mix-ins. Create the file `talk.rb`in the `test_space_two` directory.

```ruby
module Talk

  def self.included base
    base.send :include, InstanceMethods
    base.send :extend, ClassMethods
  end

  module InstanceMethods

    def say_name
      return self.name
    end
  end

  module ClassMethods
    def identify
      return "I am a talking class"
    end
  end
end
```

Include this module in `ChildModel`:

```ruby
class TestSpaceTwo::ChildModel
  include TestSpaceTwo::Talk

  #  name :string
end
```

This way our child model can `say_name` and `identify`.

We can push the mixin functionality further by defining validations and associations in our module. For our module, anything that 'talks' should have a name so lets add some name validation.

```ruby
module Talk

  def self.included base
    base.send :include, InstanceMethods
    base.send :extend, ClassMethods

    base.class_eval do
      validate :name, presence: true
      belongs_to :teacher
    end
  end

  module InstanceMethods

    def say_name
      return name
    end
  end

  module ClassMethods

    def identify
      return "I am a talking class"
    end
  end

end
```

The module adds the validation and rails association by evaluating those methods in the context of the base class.

The above syntax can be some confusing. Rails defines its own version of these mix-in modules known as `concerns`.

You could move the above into the default `concerns` folder located within `models` directory. The syntax for the above module would then be written as follows:

```ruby
module Talk

  extend ActiveSupport::Concern

  included do
    validate :name, presence: true
    belongs_to :teacher
  end

  def say_name
    return name
  end

  class_methods do
    def identify
      return "I am a talking class"
    end
  end

end
```

Easier to read and write. There is a concerns folder within controllers as well. Concerns for controllers are used in mostly the same way as described here. A good way to start is by breaking up some of the methods in ApplicationController into a more organised concern.

> When writing your controller concerns, remember that rails instantiates a new controller object matching the route for every request.
> Concerns also have some other nice features that you can read about here: [http://api.rubyonrails.org/classes/ActiveSupport/Concern.html](http://api.rubyonrails.org/classes/ActiveSupport/Concern.html).

## Service Objects and Helpers

Service onjects in Rails are objects whose main purpose is to handle some sort of complext logic. The difference between this and modules is that you are using an actual class where you can instantiate an service object and define and manipulate your own state within this object.

They are more closely related to a specific model whereas modules in the `lib` directory tend to be larger in scope, making use of models and modules across different areas of a project.

Lets suppose you have a document model. This model has fields such as `name`, `date`, `category` etc. The main part of the document model is the content field that is generated via some complex logic. Your document model is already packked full of validations, associations, scopes and other methods.

We could move the logic for generating content to a service object:

```ruby
class MyNamespace::Document < ActiveRecord::Base

  before_save :generate_content

  def generate_content
    content_helper = MyNamespace::DocumentHelper.new(self.field1, self.field2, self.field3)
    content = content_helper.generate_content
    self.update_column(content, content)
  end
end

class MyNamespace::DocumentHelper
  def initialize(field1, field2, field3)
    @x = 1
    @field1 = field1
    @field2 = field2
    @field3 = field3
  end

  def generate_content
    # logic for generating content
  end
end
```

Try to keep your controllers small. Handle data and business logic at the model level whenever possible. For controllers it is best to either use concerns or inherit shared logic from another controller, for example `ApplicationController`.

Similarly try to keep your views simple. To move logic out of your views you could wither define helper methods in the corresponding controller or make use of Rails helpers.

In the helpers folder, create a module following the Rails naming convention to match the corresponding controller. Methods defined in this module can be used in the corresponding views. Rails creates corresponding helpers for you automatically when using the `generate` command.

## Inheritance in Rails, Abstract Class and Single Table Inheritance

We have been through that we can use inheritance as normal with ruby classes. Also that inheritance is straightforward for our controllers. What about Rails models?

Two main options:

1. Inherit the parent models table via single table inheritance.
2. Inherit from the parent model in the usual way, keeping our own table via abstract class.

In single table inheritance all of our models share the top level parent's table. Define a 'type' column on the top level parent model and rails will automatically use this field to store and look up the correct class names of your models. This makes sense when the classes share much of the same data, otherwise your table will contain records with many nil values. In this case you might consider representing the relationship via a `belongs_to` association.

The other option is to declare the parent class as an abstract class. This way you can still make use of the inheritance pattern while keeping your own table:

```ruby

class ChildModel < ParentModel
end

class ParentModel < ActiveRecord::Base
  self.abstract_class = true
end
```

There are no absolute rules for how to organise your code. Hopefully the patterns and tools presented here can give some ideas for better/cleaner code structure for when your project is starting to feel unwieldy.

By John Stemler
Ref: https://www.webascender.com/blog/tutorial-classes-inheritance-modules-mixins-ruby-rails/

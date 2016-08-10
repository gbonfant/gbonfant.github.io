---
layout: post
title: "Assigning date attributes without ActiveRecord"
date: 2013-05-14 16:13:26 +0200
category: blog
description: "How to generate and assign date attributes to a plain ruby class without ActiveRecord."
---

When straying away from "the rails path" we may find ourselves missing some nifty functionality the Rails modules provide us.

The most ubiquitous of those, ActiveRecord, is without a doubt a behemont of nifty functionalities, in this case seemingly instantiating and assigning date object to our models date attributes is a feature we give for granted.

However, if you are building Rails applications without ActiveRecord you might find yourself in a predicament when submitting a form with dates.

Assuming the following class:

~~~ ruby
class Person < BaseModel
  # For the sake of simplicity, BaseModel will add all the boilerplate
  # needed for making our class behave like an AR model class.
  attr_accessor :name, :birth_date

  def initialize(attributes = nil)
    super
  end
end
~~~

When submitting a form with the following parameters, ``name`` will be automatically assigned but ``birth_date`` won't.

~~~ js
{
  "name": "James Bond",
  "birth_date(3i)": "11",
  "birth_date(2i)": "11",
  "birth_date(1i)": "1920"
}
~~~

The reason is simple, neither of the remaining parameters are attributes of our class. On ActiveRecord land, however, the remaining parameters will magically be assigned to our class attribute due to ``assign_attribute`` method call, which automatically assigns multi parameter attributes. This works by iterating over the passed attributes, selecting those that are of the type multi parameter, extract their values and assign them to a new hash which will ultimately be assigned to the parent class.

By poking into ActiveRecord's internals we can extract this logic into our own method:

~~~ ruby
class Person < BaseModel
  attr_accessor :name, :birth_date

  def initialize(attributes = nil)
    super

    assign_dates(attributes) if attributes
  end

  protected

  def assign_dates(attributes)
    new_attributes            = attributes.stringify_keys
    multiparameter_attributes = extract_multiparameter_attributes(new_attributes)

    multiparameter_attributes.each do |multiparameter_attribute, values_hash|
      set_values = (1..3).collect{ |position| values_hash[position].to_i }

      self.send("#{multiparameter_attribute}=", Date.new(*set_values))
    end
  end

  def extract_multiparameter_attributes(new_attributes)
    multiparameter_attributes = []

    new_attributes.each do |k, v|
      if k.include?('(')
        multiparameter_attributes << [k, v]
      end
    end

    extract_attributes(multiparameter_attributes)
  end

  def extract_attributes(pairs)
    attributes = {}

    pairs.each do |pair|
      multiparameter_name, value = pair
      attribute_name             = multiparameter_name.split('(').first
      attributes[attribute_name] = {} unless attributes.include?(attribute_name)

      attributes[attribute_name][find_parameter_position(multiparameter_name)] ||= value
    end

    attributes
  end

  def find_parameter_position(multiparameter_name)
    multiparameter_name.scan(/\(([0-9]*).*\)/).first.first.to_i
  end
end
~~~

Diving into ActiveRecord's source is a rewarding exercise that greatly helps to understand the "magic" behind Rails nifty features.

*Update: It looks like this functionality [will get extracted](https://github.com/rails/rails/pull/8189) into ActiveModel.*

# Covariance and Contravariance





I would like to explain why covariance and contravariance are important, and why they apply to return types and parameter types respectively, and not the other way around.

Covariance is probably easiest to understand, and is directly related to the Liskov Substitution Principle. Using the above example, let&apos;s say that we receive an `AnimalShelter` object, and then we want to use it by invoking its `adopt()` method. We know that it returns an `Animal` object, and no matter what exactly that object is, i.e. whether it is a `Cat` or a `Dog`, we can treat them the same. Therefore, it is OK to specialize the return type: we know at least the common interface of any thing that can be returned, and we can treat all of those values in the same way.

Contravariance is slightly more complicated. It is related very much to the practicality of increasing the flexibility of a method. Using the above example again, perhaps the &quot;base&quot; method `eat()` accepts a specific type of food; however, a _particular_ animal may want to support a _wider range_ of food types. Maybe it, like in the above example, adds functionality to the original method that allows it to consume _any_ kind of food, not just that meant for animals. The &quot;base&quot; method in `Animal` already implements the functionality allowing it to consume food specialized for animals. The overriding method in the `Dog` class can check if the parameter is of type `AnimalFood`, and simply invoke `parent::eat($food)`. If the parameter is _not_ of the specialized type, it can perform additional or even completely different processing of that parameter - without breaking the original signature, because it _still_ handles the specialized type, but also more. That&apos;s why it is also related closely to the Liskov Substitution: consumers may still pass a specialized food type to the `Animal` without knowing exactly whether it is a `Cat` or `Dog`.

  

#

[Official documentation page](https://www.php.net/manual/en/language.oop5.variance.php)

**[To root](/README.md)**
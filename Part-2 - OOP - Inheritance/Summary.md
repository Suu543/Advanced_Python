## Wrapping Things Up

Here is a quick summary of inheritance with a minimalistic example

```python
class Small:
    pass

class Bigger:
    pass
```

In the above example, class `Bigger` inherits from class `Small`.
We can also say:

- `Bigger` subclasses `Small`
- `Bigger` extends `Small`
- `Bigger` is the child. `Small` is the parent.
- Don't forget, `Bigger` has access to all the methods of `Small`

## Practice Inheritance

The paint shop decide it's a good idea to apply paint discounts for special occasions.
Therefore, your new task as a programmer is to incorporate a new type of object
`DiscountedPaint`.

Your exact task is to:
1. Create a `DiscountedPaint` class below the existing code. The class should inherit from `Paint`
2. Add a `discounted_price` method to that class. The method should have a `discount_percentage` parameter. There is no need to create an `__init__` method.
3. The `discounted_price` method should calculate and return a discounted price based on the output of `self.total_price()` and the value of `discount_percentage`. You could do this by first calculating the total price, then calculate the total discount, then subtract the total discount from the total price to get the discounted price and return it.

```python
class Paint:
    
    def __init__(self, buckets, color):
        self.color = color
        self.buckets = buckets
    
    def total_price(self):
        if self.color == "white":
            return self.buckets * 1.99
        else:
            return self.buckets * 2.19

class DiscountedPaint(Paint):
    
    def discounted_price(self, discount_percentage):
        price = self.total_price()
        discount = price * discount_percentage / 100
        return price - discount
```
# Fluent Python

## Chapter 9 on pytest

Chapter 9 of the Fluent Python book, by Luciano Ramalho, presents a Vector2d class.

It's a good reminder and example for experienced developers that are learning python. For instance, I can remember lots of concepts at once and compare with my other known programming languages, like Ruby.

### Directory Structure suggestion

I also converted to the follow structure, recommended for a isolated component/module in python. I was used to Rails framework conventions with a different autoloader, so this reminds me that I need to create this structure by myself in python way.

And yes, as a seasoned Rubist, it's weird to put `__init__` everywhere. Don't forget that.

```text
+ fluentpy
|- __init__.py
|+ chapter9
 |- __init__.py
 |- vector2d.py
|+ tests
 |- __init__.py
 |+ chapter9
  |- __init__.py
  |- test_vector2d.py
```

### Tests, converted to pytest

The original tests Ramalho presents on the book are the also widely used `doctest`.
Buy the book and see there the original. The book is worthy every penny.
I just converted (and added some bits) to pytest to make most of my learning and to remember in the future.

The coverage is 100% of the original code.

In the file `fluentpy/tests/chapter9/test_vector2d.py`:

```python
import math
import pytest

import fluentpy.chapter9.vector2d as v2d

class TestVector2d:
    v1 = v2d.Vector2d(3, 4)

    def test_init_int(self):
        x, y = self.v1
        assert (x, y) == (3.0, 4.0), 'Must return a tuple'

    def test_clone_eval(self):
        v1_clone = eval('v2d.' + repr(self.v1))
        assert self.v1 == v1_clone, \
            'repr must be evaluated'

    def test_str(self):
        assert str(self.v1) == '(3.0, 4.0)', \
            'str conversion need to print a tuple'

    def test_bytes(self):
        assert bytes(self.v1) == b'd\x00\x00\x00\x00\x00\x00\x08@\x00\x00\x00\x00\x00\x00\x10@', \
            'byte representation must match'

    def test_abs(self):
        assert abs(self.v1) == 5.0, \
            'abs must be working'

    def test_bool(self):
        assert (bool(self.v1), bool(v2d.Vector2d(0, 0))) == (True, False), \
            'bool conversion must evaluate False on null vectors'

    def test_from_bytes(self):
        v1_clone = v2d.Vector2d.frombytes(bytes(self.v1))
        assert self.v1 == v1_clone

    def test_format(self):
        assert format(self.v1) == '(3.0, 4.0)', \
            'format without params'
        assert format(self.v1, '.2f') == '(3.00, 4.00)', \
            'format 2 decimal places'
        assert format(self.v1, '.3e') == '(3.000e+00, 4.000e+00)', \
            'format cientific exp mode'

    def test_angle(self):
        assert v2d.Vector2d(0, 0).angle() == 0.0, \
            'angle on null vector is zero'
        assert v2d.Vector2d(1, 0).angle() == 0.0, \
            'angle with y component zero is zero'
        epsilon = 10 ** -8
        assert abs(v2d.Vector2d(0, 1).angle() - math.pi / 2) < epsilon, \
            'angle must be valid on circle'
        # interesting reading: https://www.euclideanspace.com/maths/geometry/trig/inverse/index.htm
        assert abs(v2d.Vector2d(1, 1).angle() - math.pi / 4) < epsilon

    def test_format_polar(self):
        v_1_1 = v2d.Vector2d(1, 1)
        assert format(v_1_1, 'p') == '<1.4142135623730951, 0.7853981633974483>', \
            'vector in polar format must match'
        assert format(v_1_1, '.3ep') == '<1.414e+00, 7.854e-01>', \
            'vector in polar format, with 3 decimals and exponential rest must match'
        assert format(v_1_1, '0.5fp') == '<1.41421, 0.78540>', \
            'vector in polar format, with 5 decimals, truncated rest must match'

    def test_x_y_handling(self):
        # x and y must be read only
        with pytest.raises(AttributeError):  # as e_info:
            self.v1.x = 1
        with pytest.raises(AttributeError):
            self.v1.y = 2

    def test_hashing(self):
        v2 = v2d.Vector2d(3.1, 4.2)
        assert hash(self.v1) == 7, \
            'hash of vector (3, 4) is 7'
        assert hash(v2) == 384307168202284039, \
            'hash of a non integer components must work'
        assert hash(self.v1) != hash(v2), \
            'hash of (3, 4) must not match to (3.n, 4.n)'
        assert hash(self.v1) == hash(v2d.Vector2d(3, 4)), \
            'two different instance of vectors with same components must match'

    def test_set(self):
        assert len({self.v1, v2d.Vector2d(0, 0)}) == 2, \
            'converting to a set must work, with two different vectors'
        assert len({self.v1, self.v1}) == 1, \
            'converting to set must work, with two equivalent vectors'

```

### The original code to be tested

File: `fluentpy/chapter9/vector2d.py`

```python
from array import array
import math


class Vector2d:
    type_code = 'd'

    def __init__(self, x, y):
        self.__x = float(x)
        self.__y = float(y)

    @property  # getter
    def x(self):
        return self.__x

    @property  # getter
    def y(self):
        return self.__y

    def __iter__(self):
        return (i for i in (self.x, self.y))

    def __repr__(self):
        class_name = type(self).__name__
        return "{}({!r}, {!r})".format(class_name, *self)

    def __str__(self):
        return str(tuple(self))

    def __bytes__(self):
        return (bytes([ord(self.type_code)])) + \
               bytes(array(self.type_code, self))

    def __eq__(self, other):
        return tuple(self) == tuple(other)

    def __hash__(self):
        return hash(self.x) ^ hash(self.y)

    def __abs__(self):
        return math.hypot(self.x, self.y)

    def __bool__(self):
        return bool(abs(self))

    def angle(self):
        return math.atan2(self.y, self.x)

    def __format__(self, format_spec=''):
        if format_spec.endswith('p'):
            format_spec = format_spec[:-1]
            coords = (abs(self), self.angle())
            outer_format = '<{}, {}>'
        else:
            coords = self
            outer_format = '({}, {})'
        components = (format(c, format_spec) for c in coords)
        return outer_format.format(*components)

    @classmethod
    def frombytes(cls, octets):
        type_code = chr(octets[0])
        memv = memoryview(octets[1:]).cast(type_code)
        return cls(*memv)

```

In this code we have lots of methods that are used by common python functions. For instance `str(something)` converts the something to string. The something needs to respond to `__str__`, that will be called by `str()`. It's the equivalent in ruby to implement `to_s` in a class/module and use the `something.to_s`, that is a convetion to string conversions in Ruby.

Another example is `__iter__` that must be implemented to make the object compatible with splat/unpack syntax `*something` or "matching" with `x, y = something`.

Every detail here is important for a reason. You will not use everything always.

# Properties
  
---

1. **Enqueue followed by Dequeue gives back the same element**

```math
\forall x, Q: \quad \text{enqueue}(Q, x) \Rightarrow \text{dequeue}(\text{enqueue}(Q, x)) = x
```

---

2. **Queue maintains FIFO (First In, First Out) order**

```math
\forall x, y, Q: \quad \text{enqueue}(\text{enqueue}(Q, x), y) \Rightarrow \text{dequeue}(\text{dequeue}(Q)) = x
```

---

3. **Dequeue on an empty queue throws an exception (or returns a special value)**

```math
\forall Q: \quad \text{isEmpty}(Q) \Rightarrow \text{dequeue}(Q) = \text{exception} \quad \text{or} \quad \text{None}
```

---

4. **Queue is empty after all elements have been dequeued**

```math
\forall Q: \quad \text{isEmpty}(\text{dequeue}(\text{dequeue}(\ldots \text{dequeue}(Q)))) = \text{True}
```

---

5. **Enqueueing multiple elements does not change their relative order**

```math
\forall Q, x_1, x_2, \ldots, x_n: \quad \text{enqueue}(Q, x_1), \text{enqueue}(Q, x_2), \ldots, \text{enqueue}(Q, x_n)
```
```math
\Rightarrow \quad \text{dequeue}(\text{dequeue}(\ldots \text{dequeue}(Q))) = (x_1, x_2, \ldots, x_n)
```

---

6. **Queue size increases after enqueue and decreases after dequeue**

```math
\forall Q, x: \quad \text{size}(\text{enqueue}(Q, x)) = \text{size}(Q) + 1
```
```math
\forall Q: \quad \text{size}(\text{dequeue}(Q)) = \text{size}(Q) - 1 \quad \text{if} \ \text{size}(Q) > 0
```

---

7. **Queue remains unchanged after enqueueing and immediately dequeuing the same element**

```math
\forall x, Q: \quad \text{enqueue}(\text{dequeue}(\text{enqueue}(Q, x)), x) = Q
```

---

8. **Queue behaves like a sequence, meaning it has an invariant length through enqueues and dequeues**

```math
\forall Q, x: \quad \text{size}(\text{enqueue}(Q, x)) \leq \text{size}(Q) + 1
```
```math
\forall Q: \quad \text{size}(\text{dequeue}(Q)) \geq \text{size}(Q) - 1
```

---

9. **The queue should not accept `None` or invalid values (if applicable)**

```math
\forall Q: \quad \text{enqueue}(Q, \text{None}) = \text{exception} \quad \text{or} \quad \text{enqueue}(Q, \text{invalid}) = \text{exception}
```

---

10. **Queue is immutable in terms of size after it is emptied and not refilled**

```math
\forall Q: \quad \text{isEmpty}(Q) \Rightarrow \text{dequeue}(Q) = \text{None}
```

---

## Example Property Tests

To convert the above properties into Python code using **property-based testing** with the **Hypothesis** library, we will:

- Define a **Queue class** with basic operations: `enqueue`, `dequeue`, `is_empty`, and `size`.
- Use **Hypothesis** to generate random data and test the properties.
  
Below is a Python implementation using **Hypothesis** to test the queue properties.

```python
from hypothesis import given
from hypothesis.strategies import integers
import pytest

class Queue:
    def __init__(self):
        self.items = []
    
    def enqueue(self, x):
        self.items.append(x)
    
    def dequeue(self):
        if self.is_empty():
            raise IndexError("dequeue from empty queue")
        return self.items.pop(0)
    
    def is_empty(self):
        return len(self.items) == 0
    
    def size(self):
        return len(self.items)


# Property 1: Enqueue followed by Dequeue gives back the same element
@given(integers())
def test_enqueue_dequeue_same_element(x):
    q = Queue()
    q.enqueue(x)
    assert q.dequeue() == x


# Property 2: Queue maintains FIFO (First In, First Out) order
@given(integers(), integers())
def test_fifo_order(x, y):
    q = Queue()
    q.enqueue(x)
    q.enqueue(y)
    assert q.dequeue() == x
    assert q.dequeue() == y


# Property 3: Dequeue on an empty queue throws an exception (or returns a special value)
def test_dequeue_empty_queue():
    q = Queue()
    with pytest.raises(IndexError):
        q.dequeue()


# Property 4: Queue is empty after all elements have been dequeued
@given(integers())
def test_empty_after_dequeue(x):
    q = Queue()
    q.enqueue(x)
    q.dequeue()
    assert q.is_empty()


# Property 5: Enqueueing multiple elements does not change their relative order
@given(integers(), integers(), integers())
def test_multiple_enqueue(x, y, z):
    q = Queue()
    q.enqueue(x)
    q.enqueue(y)
    q.enqueue(z)
    assert q.dequeue() == x
    assert q.dequeue() == y
    assert q.dequeue() == z


# Property 6: Queue size increases after enqueue and decreases after dequeue
@given(integers())
def test_size_increase_decrease(x):
    q = Queue()
    initial_size = q.size()
    q.enqueue(x)
    assert q.size() == initial_size + 1
    q.dequeue()
    assert q.size() == initial_size


# Property 7: Queue remains unchanged after enqueueing and immediately dequeuing the same element
@given(integers())
def test_enqueue_dequeue_identity(x):
    q = Queue()
    q.enqueue(x)
    q.dequeue()
    q.enqueue(x)
    q.dequeue()
    assert q.is_empty()


# Property 8: Queue behaves like a sequence, meaning it has an invariant length through enqueues and dequeues
@given(integers())
def test_size_invariant(x):
    q = Queue()
    initial_size = q.size()
    q.enqueue(x)
    assert q.size() == initial_size + 1
    q.dequeue()
    assert q.size() == initial_size


# Property 9: The queue should not accept `None` or invalid values (if applicable)
def test_enqueue_invalid_value():
    q = Queue()
    with pytest.raises(ValueError):
        q.enqueue(None)


# Property 10: Queue is immutable in terms of size after it is emptied and not refilled
def test_empty_immutable_size():
    q = Queue()
    q.enqueue(1)
    q.dequeue()
    assert q.size() == 0
    with pytest.raises(IndexError):
        q.dequeue()


# Run the tests
if __name__ == "__main__":
    pytest.main()
```

### Explanation:

1. **Queue class**: 
   - `enqueue(x)`: Adds an element `x` to the queue.
   - `dequeue()`: Removes and returns the front element of the queue. Raises `IndexError` if the queue is empty.
   - `is_empty()`: Returns `True` if the queue is empty, otherwise `False`.
   - `size()`: Returns the number of elements currently in the queue.

2. **Hypothesis property-based tests**:
   - **`@given` decorator**: Hypothesis generates random input values for the properties being tested. For example, `@given(integers())` generates random integers for the test.
   - **Test properties**: Each property corresponds to a different test function. For instance, `test_enqueue_dequeue_same_element` verifies that enqueueing and then dequeueing an element returns the same value.

3. **Test cases**:
   - **Property 1**: Verifies that enqueueing and then dequeueing the same element returns that element.
   - **Property 2**: Ensures the queue maintains the FIFO (First In, First Out) order.
   - **Property 3**: Ensures that dequeuing from an empty queue raises an exception.
   - **Property 4**: Ensures that a queue becomes empty after all elements have been dequeued.
   - **Property 5**: Verifies that enqueuing multiple elements does not change their relative order.
   - **Property 6**: Verifies that the queue's size increases after enqueueing and decreases after dequeueing.
   - **Property 7**: Ensures that the queue behaves correctly when enqueueing and dequeuing the same element.
   - **Property 8**: Verifies that the queue behaves like a sequence, maintaining an invariant size.
   - **Property 9**: Ensures that invalid values (like `None`) are rejected.
   - **Property 10**: Verifies that the queue is empty after all elements have been dequeued and doesn't allow further dequeues.

### Dependencies:
To run this code, you will need the following Python packages:

- `pytest`
- `hypothesis`

You can install them using:

```bash
pip install pytest hypothesis
```

### Running the Tests:

You can run the tests using `pytest`:

```bash
pytest queue_properties_test.py
```

Hypothesis will automatically generate test cases and check the properties for various inputs. If any property fails, Hypothesis will provide a minimal failing case for you to debug.


## Getting Last N Lines From Text File

#### Read N lines from the the file using queue 
- link: https://www.online-python.com/0t8LYjwvEH

```python
# last_lines_from_file.py
# link: https://www.online-python.com/0t8LYjwvEH

from collections import deque
import io


def last_lines_from_file(file_object, last_lines_count):
    q = deque()
    count = 0
    for line in file_object:
        q.append((count, line.strip("\n")))
        count = count + 1
        if len(q) > last_lines_count:
            q.popleft()
    while q:
        print(q.popleft())


def test_last_lines_from_file(test_file_n, last_lines_count):
    file_object = io.StringIO("")
    for idx in range(test_file_n):
        file_object.write("line_%s\n" % idx)
    file_object.seek(0)
    last_lines_from_file(file_object, last_lines_count)
    print()
```

Testing last lines from the file using queue 

```bash
test_last_lines_from_file(1, 5)
test_last_lines_from_file(10, 5)
test_last_lines_from_file(1000, 5)
test_last_lines_from_file(78239, 5)
```

----

#### Read N lines from the the file by seeking the positions

- link: https://www.online-python.com/Bi4SXOPngp 

```python
# link: https://www.online-python.com/Bi4SXOPngp
from collections import deque
import io


def last_lines_from_file_v2(file_object, last_lines_count):
    file_object.seek(0, 2)
    last_offset = file_object.tell()
    reverse_offset = 2
    curr_line_end_pos = last_offset - reverse_offset + 1
    line_count = 0
    lines = []
    while reverse_offset <= last_offset:
        file_object.seek(-1 * reverse_offset, 2)
        b_piece = file_object.read(1)
        if b_piece.decode() == "\n":
            bytes_to_read = curr_line_end_pos - file_object.tell()
            curr_line_end_pos = file_object.tell()
            line = file_object.read(bytes_to_read)
            lines.append(line.decode().strip("\n"))
            line_count = line_count + 1

        if line_count == last_lines_count:
            break
        reverse_offset = reverse_offset + 1
        # handle edge case
        if line_count < last_lines_count and reverse_offset > last_offset:
            file_object.seek(0)
            bytes_to_read = curr_line_end_pos - file_object.tell()
            line = file_object.read(bytes_to_read)
            lines.append(line.decode().strip("\n"))

    for line in enumerate(list(reversed(lines))):
        print(line)
    return list(reversed(lines))


def test_last_lines_from_file_v2(test_file_n, last_lines_count):
    file_object = io.BytesIO(b"")
    for idx in range(test_file_n):
        file_object.write(bytes("line_%s\n" % idx, "utf-8"))
    last_lines_from_file_v2(file_object, last_lines_count)
    print()

```

Testing N lines from the the file by seeking the positions

```bash
test_last_lines_from_file_v2(1, 5)
test_last_lines_from_file_v2(3, 5)
test_last_lines_from_file_v2(5, 5)
test_last_lines_from_file_v2(7, 5)
test_last_lines_from_file_v2(73482, 5)
test_last_lines_from_file_v2(73482, 15)

```


Testing N lines from the the file by seeking the positions in large files

```python
def test_last_lines_from_file_v3(huge_file, last_lines_count):
    with open(huge_file, "rb") as file_object:
        last_lines_from_file_v2(file_object, last_lines_count)
```

Testing N lines from the the file by seeking the positions in very large files

```bash

test_last_lines_from_file_v3("test_file.txt", 47)
test_last_lines_from_file_v3("test_file.txt", 35)
test_last_lines_from_file_v3("test_file_1.txt", 2)
test_last_lines_from_file_v3("test_file_1.txt", 5)
test_last_lines_from_file_v3("test_file_1.txt", 15)

```

--------

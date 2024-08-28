## Python 便捷模块记录

[TOC]

#### 代码风格

[trick](https://pythonguidecn.readthedocs.io/zh/latest/writing/style.html)

* **Optional**  : Optional 的作用是可选类型，作用几乎和带默认值的参数等价。不同的是使用Optional会告诉你的IDE或者框架：这个参数除了给定的默认值外还可以是None，而且使用有些静态检查工具如mypy时，对 `a: int =None`这样类似的声明可能会提示报错，但使用 `a :Optional[int] = None`不会。

* **dataclass** : Python的`dataclass`是一个类装饰器，用于自动创建一个包含成员变量、构造函数和其他实用方法的类。

  在`dataclass`中，可以通过`field()`函数指定每个成员变量的类型、默认值、是否可变、默认值工厂函数等选项。这个函数可以作为装饰器参数传递给`dataclass`，也可以在类定义内部直接调用。

* **lamba**:lambda 函数是一种小型、匿名的、内联函数，它可以具有任意数量的参数，但只能有一个表达式。匿名函数不需要使用 **def** 关键字定义完整函数。

  lambda 函数通常用于编写简单的、单行的函数，通常在需要函数作为参数传递的情况下使用，例如在 map()、filter()、reduce() 等函数中。

  ```
  lambda arguments: expression
  ```

  

#### Print

* **format格式**

  `format()` 和 `format_map()` 都是 Python 中用于字符串格式化的方法，但它们在使用方式和功能上有所不同。

  **format() 方法**:

  - `format()` 是 `str` 类的内置方法，用于格式化字符串。
  - 它允许你在字符串中使用占位符（例如 `{}`），并通过在 `format()` 方法中提供相应的参数来替换这些占位符。
  - `format()` 方法支持多种格式化选项，可以通过位置（使用索引）或名称（使用关键字）来指定替换占位符的值。
  - 你还可以使用 `format()` 方法的第二个参数传入一个字典或类似字典的对象，该对象的键对应于字符串中的占位符，但这种方式并不是 `format_map()` 那样的直接映射。

  **format_map() 方法**:

  - `format_map()` 是 `format()` 方法的一个变体，它专门用于将字符串中的占位符替换为一个映射（如字典）中的值。
  - 与 `format()` 方法不同，`format_map()` 不需要额外的参数列表。它直接使用作为参数传入的字典或类似字典的对象来查找和替换字符串中的占位符。
  - `format_map()` 方法提供了一种更简洁和直接的方式来使用字典数据格式化字符串。

#### Parser

[Parser](https://docs.python.org/zh-cn/3/library/argparse.html)链接，该模块用于方便的创建参数解释

* **argparse.ArgumentParser**

  用于创建parser实例：

  ```python
  parser = argparse.ArgumentParser(
                      prog='ProgramName',
                      description='What the program does',
                      )
  ```

* **add_argument**

  将单个参数规格说明关联到解析器:

  ```python
  parser.add_argument(
          "--dataset_name",
          type=str,
          default=None,
          help="The name of the dataset to use (via the datasets library).",
      )
  ```

  1. `name or flags`:  一个命名或者一个选项字符串的列表，例如 `foo` 或 `-f, --foo`。
  2. `default`: 当参数未在命令行中出现并且也不存在于命名空间对象时所产生的值。
  3. `type`:命令行参数应当被转换成的类型。
  4. `required` : 此命令行选项是否可省略 （仅选项可用）。
  5. `help` :  一个此选项作用的简单描述。

* **parser_args**

  方法运行解析器并将提取的数据放入parser对象:

  ```python
  args = parser.parse_args()
  print(args.filename, args.count, args.verbose)
  ```

  

#### logging

[logging](https://docs.python.org/zh-cn/3/library/logging.html)链接，实现了灵活的事件日志系统的函数与类，所有的 Python 模块都可能参与日志输出，包括你自己的日志消息和第三方模块的日志消息。

惯例用法：

```python
import logging
import mylib
logger = logging.getLogger(__name__)

def main():
    logging.basicConfig(filename='myapp.log', level=logging.INFO)
    logger.info('Started')
    mylib.do_something()
    logger.info('Finished')

if __name__ == '__main__':
    main()
```

* **getLogger**

  记录器对象，创建一个实例化的记录器。记录器的名字分级类似 Python 包的层级，如果您使用建议的结构 `logging.getLogger(__name__)` 在每个模块的基础上组织记录器，则与之完全相同。这是因为在模块里， `__name__` 是该模块在 Python 包命名空间中的名字。

  ```python
  logger = logging.getLogger(__name__)
  ```

* **basicConfig**

  通过使用默认的 [`Formatter`](https://docs.python.org/zh-cn/3/library/logging.html#logging.Formatter) 创建一个 [`StreamHandler`](https://docs.python.org/zh-cn/3/library/logging.handlers.html#logging.StreamHandler) 并将其加入根日志记录器来为日志记录系统执行基本配置。

  1. `format` : 使用指定的格式字符串作为处理器。

* **log level**

  | 级别                 | 数值 | 何种含义 / 何时使用                                          |
  | -------------------- | :--- | :----------------------------------------------------------- |
  | logging.**NOTSET**   | 0    | 当在日志记录器上设置时，表示将查询上级日志记录器以确定生效的级别。 如果仍被解析为 `NOTSET`，则会记录所有事件。 在处理句柄上设置时，所有事件都将被处理。 |
  | logging.**DEBUG**    | 10   | 详细的信息，通常只有试图诊断问题的开发人员才会感兴趣。       |
  | logging.**INFO**     | 20   | 确认程序按预期运行。                                         |
  | logging.**WARNING**  | 30   | 表明发生了意外情况，或近期有可能发生问题（例如‘磁盘空间不足’）。 软件仍会按预期工作。 |
  | logging.**ERROR**    | 40   | 由于严重的问题，程序的某些功能已经不能正常执行               |
  | logging.**CRITICAL** | 50   | 严重的错误，表明程序已不能继续执行                           |

  

#### 文件操作

* **检索文件夹下的文件**

  当你调用 `os.listdir(data_path)` 时，它会返回一个列表，其中包含了指定目录下所有文件和子目录的名称。这个列表不会包含子目录内的文件，仅包含顶层的文件和目录。返回的文件名不会考虑任何排序，因此每次调用可能会得到不同的顺序。

  ```python
  filepaths = [os.path.join(data_path, filename)  
               for filename in os.listdir(data_path) if filename.endswith('.jsonl')]
  ```

* **解析json（jsonl）格式的字符串**

  `json.loads(line)` 是 Python 标准库中 `json` 模块提供的一个函数，用于将 JSON 格式的字符串解析为 Python 对象。这个函数是 `json` 模块的 `loads` 方法的便捷别名，它接受一个 JSON 格式的字符串作为输入，并返回一个对应的 Python 数据结构，通常是字典、列表、字符串、整数、浮点数、布尔值或者 `None`。

  `json.load(file)` 是 Python 标准库中 `json` 模块提供的一个函数，用于从一个文件中读取 JSON 格式的数据。与 `json.loads` 不同，`json.load` 直接从一个文件对象中读取数据，而不是从一个字符串中解析 JSON。这使得 `json.load` 特别适合用于处理存储在文件中的 JSON 数据。

* **写json文件**

  `json.dump` 是 Python 标准库中 `json` 模块提供的一个函数，用于将 Python 对象编码成 JSON 格式的字符串，并将其写入到一个文件中。

  ```python
  with open('data.json', 'w') as file:
      # 将 Python 字典写入到文件中，使用 JSON 格式
      json.dump(data, file, indent=4)
  ```

  

  `json.dumps` 是 Python 标准库中 `json` 模块提供的一个函数，用于将 Python 对象编码成 JSON 格式的字符串。与 `json.dump` 不同，`json.dumps` 仅用于生成 JSON 格式的字符串，而不是将其写入文件。

  1. 可以通过 `indent` 参数来指定输出 JSON 字符串的缩进，以提高可读性。
  2. 可以通过 `sort_keys` 参数来指定是否对字典的键进行排序。
  3. 可以通过 `ensure_ascii` 参数来指定是否将非 ASCII 字符转换为 ASCII 编码的 Unicode 转义序列。
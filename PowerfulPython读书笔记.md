## PowerfulPython
* Iterators: 
    * yield can appear multiple times in one iterator
    * iterator can be passed as an argument, just like all other functions, classes, and so on
    * a lot of built-in data types are iterators: dict.items()/keys()/values(), filter obj, map obj, zip obj
    * try to break the whole function into diffirent subroutines, like a parser function can be divided into: source + filtering + mapping + sink
* List Comprehensions:
    * ```[EXPR for VAR in SEQ if CON]```
    * can be used for nested loops (parse from left to right): ```[n for range in ranges for n in range]```
    * can be used for dict, set, tuple: ```{k:v for k in ... for v in ...}, set(...), tuple(...)```
    * directly using () gives a iterator: (...)
* Advanced Functions:
    * variable arguments: \*args, \*\*kargs
        * accepting(packing): 
            * positional: ```func(\*args): print(type(args); func(1,2,3) >>> <class: 'tuple'>```
            * keywords: ```func(\*\*kargs): print(type(kargs); func(a=1,b=2,c=3) >>> <class: 'dict'>```
        * passing(unpacking): 
            * positional: ```func(a, b, c): print(a,b,c); nums = [1,2,3]; func(\*nums) >>> 1 2 3```
            * keyword: ```func(a, b, c): print(a,b,c); nums = ["a":1, "b":2, "c":3]; func(\*\*nums) >>> 1 2 3```
    * Key Functions (funcs are also objs!):
        * ```max(nums, key = lambda x: int(-x))```
        * ```sorted(student\_list, key = get\_gpa)```
* Decorators:
    * def decorator:
        * no args: decorator(function)
        * with args: outer(decorator(function), args)
    * class decorator:
        * no args: The secret to decorating with classes is the magic method \_\_call__.
        * with args: simply adapt the decorator function itself to \_\_init\_\_, and wrapper to \_\_call__.
    * preserving the Wrapped Function: functools.wraps(func.\_\_name\_\_)
* Exceptions:
    * try + multiple except + finally
    * keep in mind that except should always follow a specific exception type
    * Exception is a object with args attibutes: e.g., KeyError.args[0] = KeyName
    * it is possible to actively raise or reraise an Exception: reraise means ```try: sth(); except ExceptionClass as err: handle_exception(); raise err```
* Classes and Objects:
    * @property: makes a func an attribute (a getter)
        * @\_\_func__.name.setter
    * @classmethod: makes a func an alternative \_\_init__ (an alternative constructor)
    * @abc.abstractmethod: an abstract class with abstract methods can't instantiated
    * Observer Pattern: Publisher-Subscriber
        * Publisher = Observable: register/unregister/notify
        * Subscriber = Observer: update
* Automated Testing and TDD (Test-Driven Development):
    * Unit Test: make a new file ==> import unittest; class TestClass(unittest.TestCase): def test\_method(self): self.assertXXX() ... ==> python3 -m unittest test\_file
    * every method must start with the string "test".
    * assertEqual, assertTrue and assertFalse are the most common assertion methods
    * setUp() ==> test1 ==> setUp() ==> test2() ==> tearDown()
    * with self.subTest() makes it possible to use for loops in test methods
* String Formatting:
    * Ordering: ```"{1}, {0}".format("Yes", "No") >>> No, Yes```
    * Numbering: 
        * ```"{:,d}".format (10**9) >>> 1 ,000 ,000, 000```
        * ```"{:.2f}".format (4) >>> 4.00```
        * ```"{:.2E}".format (10**9) >>> 1.00E+9```
    * Width, Alignment, and Fill:
        * ```"{:->8}". format("A") >>> -------A```
        * ```"{:->8}". format("A") >>> A-------```
        * ```"{:-^8}". format("A") >>> ---A----```
* Logging:
    * A tipical way of creating logging:
        * create a dir storing 3 logging files: log, warning, info
        * ```def get_logger(): 
                 logger = logging.getLogger
                 while logger.handlers():
                        logger.handlers.pop()
                 logger.setLevel(logging.INFO)
                 file_handler_info = logging.FileHandler(log_info_path, "a")
                 file_handler_info.setLevel(logging.INFO)
                 file_handler_info.setFormatter(logging.Formatter("%(asctime)s %(levelname)s %(module)s %(lineno)s %(message)s"))
                 logger.addHandler(file_handler_info)
                 # same for file_handler_warn, file_handler_err
                 return logger```

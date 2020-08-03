## pdb — The Python Debugger

* The typical usage: 
    * insert *pdb.set_trace()* in the code
    * use pdb.runcall(function_name, *args, **kwargs)
    * use magic command *%pdb* in ipython (run -d xxx.py进入单步调试模式)
    * use command like *python -m pdb xxx.py* in terminal

* Most used commands in debugger:
    * p = print （+变量，打印）
    * n = next （下一行）
    * c = continue （继续到下一个断点）
    * b = break （+行号，加断点）
    * l = list （查看附近代码）
    * q = quit （强制退出）
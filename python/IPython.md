# IPython

## 自动加载模块

在 IPython 中，有时候我们需要自动加载改动的一些模块，`autoreload` 提供了这种功能。

	# 在 Ipython 中引入自动加载模块
	%load_ext autoreload
	
	# 制定自动加载的级别
	# autoreload 2
	
	from foo import some_function
	
	some_function()
	=> 42
	
	# 修改 some_function() 的返回值为 43
	
	some_funtion()
	=> 43

详细的用法参考官方文档：[autoreload — IPython 3.2.1 documentation](https://ipython.org/ipython-doc/3/config/extensions/autoreload.html "autoreload — IPython 3.2.1 documentation")

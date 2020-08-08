## `micro`工程现状

- 根据`micro`的依赖情况, 整理出了一个隔离操作系统的`micro`文件夹, 目前`micro`工程已经通过所有测试, 可以成功编译.
- 目前的版本中并不存在多个测试文件.
- 目前工程执行的是`examples/micro_speech/micro_features` 中的`micro_features_generator_test.cc` 中的测试`main`函数, 实现一个特征生成的测试用例. 编译运行并装载到树莓派板子之后, 可以看到输出`ALL TEST PASSED` 的字样.
- `examples` 文件夹中包含了所有官方自带测试用例, 目前只有`micro_speech`语音用例
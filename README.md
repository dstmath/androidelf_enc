# 简单介绍

elf 加密项目，功能有：1）字符串加密（.rodata），2）函数加密

加密算法PRESENT（分组密码算法，速度快，实现简单。但可以通过差分分析破解，折中考虑），密钥由blake244生成（单向哈希，SHA-3备选，速度快）

目前支持Linux x86与ARM（由于elf解析使用的32位结构体）

# 使用

encrypt 是加密器，payload 是测试demo，首先在main函数中解密.rodata，亦可放到init_array中实现，loader 负责运行时函数加解密操作的具体实现

其中 payload 的编写要符合以下规范：

  +  需要加密的函数，以 `_encrypt_` 前缀声明，如 `void _encrypt_helloworld(void)`

  +  使用 `decrypt_and_call(func_name)` 调用加密的函数（注意多线程问题）

  +  加密函数页对齐`__attribute__((aligned(PAGE_SIZE))`

  +  使用 `-fpie` 编译

# 编译

  + Linux： `make`.

  + Android： `make android`

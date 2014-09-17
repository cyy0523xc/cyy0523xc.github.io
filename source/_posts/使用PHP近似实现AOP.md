title: 使用PHP近似实现AOP
date: 2014-09-16 14:26:15
tags:
- PHP 
- AOP 
- 设计模式

---

## AOP介绍

**面向切面编程** ：Aspect Oriented Programming(AOP)，是Spring框架中的一个重要内容。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

- AOP是OOP的延续。
- 主要的功能是：日志记录，性能统计，安全控制，事务处理，异常处理等等。
- 主要的意图是：将日志记录，性能统计，安全控制，事务处理，异常处理等代码从业务逻辑代码中划分出来，通过对这些行为的分离，我们希望可以将它们独立到非指导业务逻辑的方法中，进而改变这些行为的时候不影响业务逻辑的代码。

## PHP实现

现在版本的php并没有内置的AOP实现，不过我们还是可以通过一些魔术函数来近似实现。

```php
/** 
 * 业务逻辑类的包装类
 * @author cyy0523xc@gmail.com
 */
final class AOP
{
    private $instance;

    public function __construct($instance)
    {
        $this->instance = $instance;
    }

    public function __call($method, $params)
    {
        if (!$this->__methodExists($method)) {
            throw new Exception("Call undefinded method " . get_class($this->instance) . "::$method");
        }

        // 调用前置函数
        if ($this->__methodExists('before')) {
            $this->__callMethod('before');
        }

        // 调用业务函数
        $return = $this->__callMethod($method, $params);

        // 调用后置函数
        if ($this->__methodExists('after')) {
            $this->__callMethod('after');
        }
        
        return $return;
    }

    /**
     * 判断方法是否存在
     * 使用双下划线前缀主要是为了避免冲突
     * @param string $method 方法名
     * @return mixed
     */
    private function __methodExists($method)
    {
        return method_exists($this->instance, $method);
    }

    /**
     * 调用方法
     * @param string $method 方法名
     * @param array  $params 参数数组
     * @return mixed
     */
    private function __callMethod($method, array $params = null)
    {
        $callBack = array(
            $this->instance,
            $method
        );

        if (null === $params) {
            return call_user_func($callBack);
        } else {
            return call_user_func_array($callBack, $params);
        }
    }
}

```

完整代码见：https://github.com/cyy0523xc/code/raw/master/php/test/aop.php 


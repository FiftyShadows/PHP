## 全局异常处理的实现

- ExceptionHandler定义对异常的处理(render)，在config.php中设置'exception_handle'为ExceptionHandler

- 定义每个业务的异常处理类，例BannerMissException -> BaseException -> Exception



关闭app\_debug不会给出错误的详细信息

## 异常分类

* 由于用户行为导致的异常\(没有通过验证器，没查询到结果\)；通常不需要记录日志，需要向用户返回具体信息

* 服务器自身异常\(代码错误，调用外部接口错误\)；通常记录日志，不向客户端返回具体原因

## Exception分为think\Exception和HttpException和\Exception

* \Exception是think\Exception和HttpException的基类





## 在 ExceptionHandler 类中加入 recordErrorLog 方法

- Log::init

- Log::record($e->getErrorMessage(), 'error')



## 通过配置文件判断向客户端返回json还是错误页面

- Config::get('app_debug');    类静态方法

- config('app_debug')    助手函数

- 不要在config里保存数据




## app_debug开启时

- 性能差

- 暴露错误信息